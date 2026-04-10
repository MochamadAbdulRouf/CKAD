<p align="center">
  <img src="./images/CKAD-Credly.png" width="400">
</p>

# CKAD: Certified Kubernetes Application Developer
Mempelajari pehamahan tentang cara menghadapi ujian CKAD. Pertanyaan berbasis skenario yang dirancang sedemikan rupa agar realistis dan mirip dengan situasi dunia nyata, guna mempercepat tujuan menjadi profesional Certified Kubernetes Application Developer. Materi saya ambil dari mempelajari course di Udemy.

## Shortcut 
Gunakan shorcut untuk beberapa command di kubernetes, memudahkan dalam konfigurasi dan meminimalisir kesalahan typo dalam memasukan command. karena ketika ada kesalahan dalam memasukan command itu sangat krusial saat ujian CKAD bisa mengurangi poin. berikut list shorcut yang aku digunakan
```bash
alias k=kubectl
alias kgp='kubectl get pod'
alias kgs='kubectl get svc'
alias kdp='kubectl describe pod'
alias kdd='kubectl describe deployment'
alias kdn='kubectl describe nodes'
alias kgd='kubectl get deployment'
alias kgn='kubectl get ns'
alias kcgn='kubectl get nodes'
alias kgpan='kubectl get pods --all-namespaces'
alias kgdan='kubectl get deployments --all-namespaces'
```

## Membuat alias permanen

1. Masuk ke direktori root lalu edit bashrc nya
```bash
vi /root/.bashrc
```

2. Paste alias di atas, di dalam file `.bashrc`. example :
![ss-bashrc](./images/alias-bashrc.png)

3. Aktifkan perubahan bashrc
```bash
source /root/.bashrc
```

4. Kalau untuk user biasa juga bisa di setting, karena di atas hanya khusus untuk root user. example :
```bash
vi /home/rouf/.bashrc
```
atau
```bash
nano ~/.bashrc
```
note: jika sudah di paste silahkan di aktifnya dengan command berikut:
```bash
source ~/.bashrc
```

## How to create Kubernetes Yaml manifest quickly 

Install ektension vscode berikut ![ektension-kubernetes](https://code.visualstudio.com/docs/azure/kubernetes?ref=devopscube.com)

Atau bisa menggunakan cara berikut 

### Kubectl YAML Dry Run example

1. Create pod YAML
    - membuat sebuah pod YAML dengan nama myapp menggunakan ektension nginx:latest.
```bash
kubectl run myapp --image=nginx:latest --labels type=web --dry-run=client -oyaml > mypod.yaml
```

2. Create pod service YAML
    - membuat sebuah service untuk pod dengan nama myapp-svc, menggunakan type service NodePort untuk expose pod yg telah running 
```bash
kubectl expose pod myapp --port=80 --name=myapp-svc --type=NodePort --dry-run=client -oyaml > myapp-service.yaml
```

3. Create NodePort Service YAML
    - membuat service dengan menggunakan type NodePort, Port NodePort 30001 dengan service ke pod menggunakan TCP port mapping di port 80:80.
```bash
kubectl create service nodeport myapp --tcp=80:80 --node-port=30001 --dry-run=client -oyaml > svc-nodeport.yaml
```

4. Create Deployment YAML 
    - membuat deployment dengan nama deployapp with image Nginx
```bash
kubectl create deployment deployapp --image=nginx:latest --dry-run=client -oyaml > deployment.yaml
```

5. Create Deployment service YAML
    - membuat service NodePort YAML khusus untuk Deployment deployapp dengan port service 8080
```bash
kubectl expose deployment deployapp --type=NodePort --port=80 --name=deployment-svc --dry-run=client -oyaml > deployment-svc.yaml
```

6. Create Job YAML
  - membuat job dengan nama myjob menggunakan image nginx
```bash
kubectl create job myjob --image=nginx:latest --dry-run=client -oyaml > job.yaml
```

7. Create CronJob YAML
    - membuat cronjob dengan nama mycronjob menggunakan image nginx dan cron schedule 
```bash
kubectl create cj mycronjob --image=nginx:latest --schedule="* * * *" --dry-run=client -oyaml > cronjob.yaml
```
