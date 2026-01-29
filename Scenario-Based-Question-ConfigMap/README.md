CKAD EXAM
Consume ConfigMap

Context:

You are tasked to create a ConfigMap and consume the ConfigMap in a pod using a volume mount. 
- Task
Please complete the following:
• Create a ConfigMap named another-config containing the key/value pair: key4/value3
• start a pod named nginx-configmap containing a single container using the
nginx image, and mount the key you just created into the pod under directory /also/a/path


1. Create Configmap
```bash
kubectl create cm another-config --from-literal=key4=value3
```

2.
```bash
kubectl get cm
```

3.
```bash
kubectl run nginx-configmap --image=nginx --dry-run=client -oyaml > 2.yaml
```

4. Edit file manifest
```bash
vi 2.yaml
```
Isi filenya, dengan config berikut
```bash
volumeMounts:
- name: myvol
  mountPath: /also/a/path
volumes:
- name: myvol
configMap:
  name: another-config
```
note: bisa dilihat di file `configmap-2.yaml`

5.
```bash
root@rouf:~# kubectl get po nginx-configmap
NAME              READY   STATUS    RESTARTS   AGE
nginx-configmap   1/1     Running   0          31s
```

6.
```bash
root@rouf:~# kubectl exec -it nginx-configmap -- ls /also/a/path
key4
```

7.
```bash
root@rouf:~# kubectl exec -it nginx-configmap -- cat /also/a/path/key4
value3root@rouf:~#
```

8. OPTIONAL
```bash
kubectl delete po nginx-configmap --force
kubectl delete cm another-config
rm 2.yaml
```
