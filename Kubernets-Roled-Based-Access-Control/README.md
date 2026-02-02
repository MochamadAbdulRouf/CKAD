# Kubernetes Role Based Access Control 

Kubernetes Role-Based Access Control (RBAC) adalah fitur keamanan bawaan yang mengatur hak akses pengguna (manusia atau akun layanan/service accounts) ke sumber daya API cluster berdasarkan peran mereka. RBAC memungkinkan administrator menerapkan prinsip least privilege (hak akses minimal), memastikan pengguna hanya memiliki izin yang diperlukan untuk melakukan tugas spesifik (seperti membaca/menulis pod atau deployment) guna meminimalkan risiko keamanan.

## Implementation 

1. Set namespace
```bash
N=rbacdemo
```

2. Create namespace
```bash
k create ns $N
```

3. Create role khusus hanya dapat melihat resource Kubernetes
```bash
k -n $N create role readerrole --verb=get --verb=list --verb=watch --resource pod --resource deployment
```
```bash
k -n $N create rolebinding readerrolebinding --role readerrole --user rbacuser
```

4. Create private key menggunakan algoritma RSA dengan panjang kunci 2048 bit.
```bash
openssl genrsa -out rbacuser.key 2048
```

5. Create Certificate Signing Request (CSR) simplenya membuat permohonan tanda tangan agar kunci privat yg dibuat sebelumnya di akui sebagai identitas resmi oleh sistem kubernetes
```bash
openssl req -new -key rbacuser.key -out rbacuser.csr
```

6. Melihat file kunci yg dibuat
```bash
ls rbacuser*
```

7. Create file manifest for CSR
```bash
cat > csr.yaml << EOF
apiVersion: certificates.k8s.io/v1
kind: CertificateSigningRequest
metadata:
  name: rbacuser
spec:
  request: `cat rbacuser.csr | base64`
  signerName: kubernetes.io/kube-apiserver-client
  usages:
  - client auth
EOF
```

8. Lihat apakah isi file .csr berhasil masuk ke file manifest
```bash
vi csr.yaml
```
note: tampilannya akan seperti file `csr.yaml` yang aku buat di repo folder repo ini


9. Create CSR 
```bash
k create -f csr.yaml
```

10. Lihat csr apakah berhasil dibuat, jika iya kita perlu approve csr nya karena masih status denied
```bash
k get csr
```

11. Approve CSR
```bash
k certificate approve rbacuser
```

12. Lihat status CSR, wajib Approve
```bash
k get csr
```

13. Melihat detail objek CSR
```bash
k get csr rbacuser -oyaml
```

14. Mengambil isi CSR mengkodenya dari format base64, dan menyimpan menjadi file `rbacuser.crt`
```bash
k get csr rbacuser --output jsonpath={.status.certificate} | base64 -d > rbacuser.crt
```

15. Melihat isi CSR, verifikasi file sudah terisi
```bash
cat rbacuser.crt
```

16. Mendaftarkan user `rbacuser` ke sistem lokal, opsi `--embed-certs` memastikan isi file `.key` dan `.crt` langsung dimasukan dalam file config, sehingga file aslinya dihapus atau dipindah.
```bash
k config set-credentials rbacuser --client-key=rbacuser.key --client-certificate=rbacuser.crt --embed-certs
```

17. Menghubungkan user rbacuser dengan cluster tertentu, disini pakai default
```bash
k config set-context rbacuser --user=rbacuser --cluster=default
```
note: sebelum itu pastikan cluster pakai yg mana, berikut commandnya
```bash
root@rouf:~# k config get-contexts
CURRENT   NAME       CLUSTER   AUTHINFO   NAMESPACE
          default    default   default
*         rbacuser   default   rbacuser
```

18. gunakan roles yg udh dibuat, tadi rolesnya hanya aku izinin melihat pod dan deployment di namespace yang udh aku tentuin. jadi ketika dia ingin melihat resource lain ga akan bisa.
```bash
k config use-context rbacuser
```

19. verifikasi roles apakah bisa melihat resource di namespace yang di izinkan saja atau tidak
```bash
root@rouf:~# k auth can-i list pods -n default
no
root@rouf:~# k auth can-i list deployments -n default
no
root@rouf:~# k auth can-i list pods -n $N
yes
root@rouf:~# k auth can-i list pods -n rbacdemo
yes
root@rouf:~# k auth can-i list deployments -n rbacdemo
yes
root@rouf:~# k -n $N get deployments.apps
No resources found in rbacdemo namespace.
root@rouf:~# k -n $N get pods
No resources found in rbacdemo namespace.
root@rouf:~# k -n $N get secrets
Error from server (Forbidden): secrets is forbidden: User "rbacuser" cannot list resource "secrets" in API group "" in the namespace "rbacdemo"
root@rouf:~# k get pods
Error from server (Forbidden): pods is forbidden: User "rbacuser" cannot list resource "pods" in API group "" in the namespace "default"
root@rouf:~#
```

20. Switch ke role default
```bash
root@rouf:~# k config use-context default
Switched to context "default".
root@rouf:~# k config get-contexts
CURRENT   NAME       CLUSTER   AUTHINFO   NAMESPACE
*         default    default   default
          rbacuser   default   rbacuser
root@rouf:~#
```
note: role default tergantung sistem masing masing yaa, bisa dilihat dulu pakai command `k config get-contexts`
