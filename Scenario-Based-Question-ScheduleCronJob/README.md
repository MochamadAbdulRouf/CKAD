- Context:
Developers occasionally need to submit pods that run periodically.

- Task:
Follow the steps below to create a pod that will start at a predetermined time and which runs to completion only once each time it is started:

1. Create a YAML formatted Kubernetes manifest /opt/KDPD00301/periodic.yaml that runs the following shell command: date in a single busybox container.
2. The command should run every minute and must complete within 22 seconds or be terminated by Kubernetes.
3. The Cronjob name and container name should both be hello
4. Create the resource in the above manifest and verify that the job executes successfully at least once

- Implementation

1. Create Cronjob Template
```bash
 k create cronjob hello --image busybox --schedule "*/1 * * * *" --dry-run=client -oyaml > /opt/KDPD00301/periodic.yaml
 ```
 
 2. Modify Cronjob Template 
 ```bash
 vi /opt/KDPD00301/periodic.yaml
 ```
 Refrensi yang di modif bisa di lihat di file `cronjob.yaml`
 
 3. Create CronJob
 ```bash
 k create -f /opt/KDPD00301/periodic.yaml
 ```
 
 4. Verify 
 ```bash
 k get cj
 ```
 ```bash
 k get po
 ```
 ```bash
 k logs helo-
 ```

example:
```bash
root@rouf:~# k get cj
NAME    SCHEDULE      TIMEZONE   SUSPEND   ACTIVE   LAST SCHEDULE   AGE
hello   */1 * * * *   <none>     False     0        59s             4m4s
root@rouf:~# k get po
NAME                   READY   STATUS              RESTARTS   AGE
hello-29500216-bbdvz   0/1     Completed           0          3m2s
hello-29500217-6q2bj   0/1     Completed           0          2m2s
hello-29500218-8k8jk   0/1     Completed           0          62s
hello-29500219-kgnxc   0/1     ContainerCreating   0          2s
root@rouf:~# k logs hello-29500217-6q2bj
Mon Feb  2 06:17:04 UTC 2026
```

OPTIONAL:
```bash
k delete po --all
k delete cj hello
```
