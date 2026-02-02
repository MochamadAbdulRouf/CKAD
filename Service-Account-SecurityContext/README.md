# Service Account and Security Context

- Service Account adalah seperti identitas Pod. ini menentukan siapa yang dapat menjalankan pod tersebut
- Security Context adalah SOP atau aturan fisik. ini menentukan apa yang boleh dilakukan aplikasi tersebut terhadap sistem operasi container

## Meaning of Security Context
- runAsUser: 1000 = artinya jangan dijalankan sebagai root user. mencegah apabila hacker berhasil masuk ke container, mereka tidak otomatis menjadi root didalam container
- runAsGroup: 3000 = artinya set primary group proses menjadi 3000. semua file yang dibuat oleh proses didalam container akan dimiliki kepemilikan group
- fsGroup: 2000 = artinya kubernetes me mount volume lalu mengubah permissionnya menjadi dimiliki group 2000. bisa menulis file ke dalam folder /data/demo meskipun folder itu bukan miliknya secara langsung. karena user anggota group 2000.


## Implementation

1.
```bash
N=serviceaccountdemons
F=sademo.yaml
```

2.
```bash
k create ns $N
```

3.
```bash
k -n $N create deployment sademo --image busybox:1.28 \
--dry-run=client -oyaml > $F
```

4.
```bash
k -n $N create sa serviceaccountdemo
```

5.
```bash
vi $F
```
note: modify refrensi file `sademo.yaml`

6.
```bash
k -n $N create -f $F
```

7.
```bash
k -n $N get deploy
```

8.
```bash
k -n $N get pods
```

9.
```bash
PODNAME=`k -n $N get pods -o jsonpath='{.items[0].metadata.name}'`
```

10.
```bash
echo $PODNAME
```

11.
```bash
k -n $N describe pod  $PODNAME
```
