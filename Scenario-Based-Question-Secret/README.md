CKAD EXAM
Consume Secret in a Pod

Context:

You are tasked to create a secret and consume the secret in a pod using environment variables as follow:
- Task
1. Create a secret named another-secret with a key/value pair; key1/value4
2. Start an nginx pod named nginx-secret using container image nginx,
    and add an environment variable exposing the value of the secret key key1, using COOL_VARIABLE as the name for the               environment variable inside the pod

1.
```bash
kubectl create secret generic another-secret --from-literal=key1=value4
```

2.
```bash
kubectl run nginx-secret --image nginx --dry-run=client -oyaml > 1.yaml
```

3.
```bash
vim 1.yaml
```
Edit file lalu tambahkan environment berikut:
```bash
env:
- name: COOL_VARIABLE
  valueFrom:
    secretKeyRef:
      name: another-secret
      key: key1
```
note: contoh sudah ada di file config `secret-1.yaml`

4.
```bash
kubectl create -f 1.yaml
```

5.
```bash
root@rouf:~# kubectl get pod -o wide
NAME           READY   STATUS    RESTARTS   AGE   IP            NODE   NOMINATED NODE   READINESS GATES
nginx-secret   1/1     Running   0          11s   10.42.0.208   rouf   <none>           <none>
```

6.
```bash
root@rouf:~# kubectl exec -it nginx-secret -- env
PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
HOSTNAME=nginx-secret
NGINX_VERSION=1.29.4
NJS_VERSION=0.9.4
NJS_RELEASE=1~trixie
ACME_VERSION=0.3.1
PKG_RELEASE=1~trixie
DYNPKG_RELEASE=1~trixie
COOL_VARIABLE=value4
KUBERNETES_PORT=tcp://10.43.0.1:443
KUBERNETES_PORT_443_TCP=tcp://10.43.0.1:443
KUBERNETES_PORT_443_TCP_PROTO=tcp
KUBERNETES_PORT_443_TCP_PORT=443
KUBERNETES_PORT_443_TCP_ADDR=10.43.0.1
KUBERNETES_SERVICE_HOST=10.43.0.1
KUBERNETES_SERVICE_PORT=443
KUBERNETES_SERVICE_PORT_HTTPS=443
TERM=xterm
HOME=/root
```

7. OPTIONAL
```bash
kubectl delete po nginx-secret --force
kubectl delete secret another-secret
rm 1.yaml
```
