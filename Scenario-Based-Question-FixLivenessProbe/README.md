CKAD EXAM
- Scenario Based Question
- Find Broken Pod
- Capture Error
- Fix Broken Pod

A user has reported an application is unreachable due to a failing livenessProbe
Task
Perform the following tasks:
- Find the broken pod and store its name and namespace to /opt/KDOB00401/broken.txt in the format:
<namaspace>/<pod>
- The output file has already been created
- Store the associated error events to a file /opt/KDOB00401/error.txt, The output file has already been created.
- You will need to use the -o wide output specifier with your command
- Fix the issue.

## Troubleshoot:

1.
```bash
kubectl get events
```

2.
```bash
kubectl get events -A
```

3.
```bash
kubectl get events -A | grep i liveness
```
example output:
```bash
root@rouf:~# kubectl get events -A | grep liveness
default       32m         Normal    Scheduled                           pod/nginx-pod-with-liveness-probe                            Successfully assigned default/nginx-pod-with-liveness-probe to rouf
default       13m         Normal    Pulling                             pod/nginx-pod-with-liveness-probe                            Pulling image "nginx"
default       32m         Normal    Pulled                              pod/nginx-pod-with-liveness-probe                            Successfully pulled image "nginx" in 2.491s (2.492s including waiting). Image size: 62870438 bytes.
default       13m         Normal    Created                             pod/nginx-pod-with-liveness-probe                            Created container: nginx-pod-with-liveness-probe
default       13m         Normal    Started                             pod/nginx-pod-with-liveness-probe                            Started container nginx-pod-with-liveness-probe
default       31m         Warning   Unhealthy                           pod/nginx-pod-with-liveness-probe                            Liveness probe failed: HTTP probe failed with statuscode: 404
default       14m         Normal    Killing                             pod/nginx-pod-with-liveness-probe                            Container nginx-pod-with-liveness-probe failed liveness probe, will be restarted
default       30m         Normal    Pulled                              pod/nginx-pod-with-liveness-probe                            Successfully pulled image "nginx" in 2.468s (2.468s including waiting). Image size: 62870438 bytes.
default       14m         Warning   Unhealthy                           pod/nginx-pod-with-liveness-probe                            Liveness probe failed: Get "http://10.42.0.242:80/live": context deadline exceeded (Client.Timeout exceeded while awaiting headers)
default       22m         Normal    Pulled                              pod/nginx-pod-with-liveness-probe                            Successfully pulled image "nginx" in 2.548s (2.55s including waiting). Image size: 62870438 bytes.
default       13m         Normal    Pulled                              pod/nginx-pod-with-liveness-probe                            Successfully pulled image "nginx" in 2.908s (2.908s including waiting). Image size: 62870438 bytes.
root@rouf:~# kubectl get events -A | grep -i liveness
default       32m         Normal    Scheduled                           pod/nginx-pod-with-liveness-probe                            Successfully assigned default/nginx-pod-with-liveness-probe to rouf
default       14m         Normal    Pulling                             pod/nginx-pod-with-liveness-probe                            Pulling image "nginx"
default       32m         Normal    Pulled                              pod/nginx-pod-with-liveness-probe                            Successfully pulled image "nginx" in 2.491s (2.492s including waiting). Image size: 62870438 bytes.
default       14m         Normal    Created                             pod/nginx-pod-with-liveness-probe                            Created container: nginx-pod-with-liveness-probe
default       13m         Normal    Started                             pod/nginx-pod-with-liveness-probe                            Started container nginx-pod-with-liveness-probe
default       31m         Warning   Unhealthy                           pod/nginx-pod-with-liveness-probe                            Liveness probe failed: HTTP probe failed with statuscode: 404
default       14m         Normal    Killing                             pod/nginx-pod-with-liveness-probe                            Container nginx-pod-with-liveness-probe failed liveness probe, will be restarted
default       31m         Normal    Pulled                              pod/nginx-pod-with-liveness-probe                            Successfully pulled image "nginx" in 2.468s (2.468s including waiting). Image size: 62870438 bytes.
default       14m         Warning   Unhealthy                           pod/nginx-pod-with-liveness-probe                            Liveness probe failed: Get "http://10.42.0.242:80/live": context deadline exceeded (Client.Timeout exceeded while awaiting headers)
default       22m         Normal    Pulled                              pod/nginx-pod-with-liveness-probe                            Successfully pulled image "nginx" in 2.548s (2.55s including waiting). Image size: 62870438 bytes.
default       14m         Normal    Pulled                              pod/nginx-pod-with-liveness-probe                            Successfully pulled image "nginx" in 2.908s (2.908s including waiting). Image size: 62870438 bytes.
kube-system   20h         Warning   Unhealthy                           pod/coredns-7f496c8d7d-4xccp                                 Liveness probe failed: Get "http://10.42.0.203:8080/health": context deadline exceeded (Client.Timeout exceeded while awaiting headers)
kube-system   17h         Warning   Unhealthy                           pod/coredns-7f496c8d7d-4xccp                                 Liveness probe failed: Get "http://10.42.0.218:8080/health": context deadline exceeded (Client.Timeout exceeded while awaiting headers)
kube-system   21h         Warning   Unhealthy                           pod/metrics-server-7b9c9c4b9c-j49ff                          Liveness probe failed: Get "https://10.42.0.186:10250/livez": net/http: request canceled (Client.Timeout exceeded while awaiting headers)
kube-system   20h         Warning   Unhealthy                           pod/metrics-server-7b9c9c4b9c-j49ff                          Liveness probe failed: Get "https://10.42.0.186:10250/livez": net/http: request canceled while waiting for connection (Client.Timeout exceeded while awaiting headers)
kube-system   17h         Warning   Unhealthy                           pod/metrics-server-7b9c9c4b9c-j49ff                          Liveness probe failed: Get "https://10.42.0.217:10250/livez": net/http: request canceled while waiting for connection (Client.Timeout exceeded while awaiting headers)
kube-system   17h         Warning   Unhealthy                           pod/metrics-server-7b9c9c4b9c-j49ff                          Liveness probe failed: Get "https://10.42.0.217:10250/livez": net/http: request canceled (Client.Timeout exceeded while awaiting headers)
kube-system   24m         Warning   Unhealthy                           pod/metrics-server-7b9c9c4b9c-j49ff                          Liveness probe failed: Get "https://10.42.0.226:10250/livez": net/http: request canceled while waiting for connection (Client.Timeout exceeded while awaiting headers)
kube-system   20h         Warning   Unhealthy                           pod/traefik-6f5f87584-8hl92                                  Liveness probe failed: Get "http://10.42.0.195:8080/ping": context deadline exceeded (Client.Timeout exceeded while awaiting headers)
monitoring    20h         Warning   Unhealthy                           pod/alertmanager-monitoring-stack-kube-prom-alertmanager-0   Liveness probe failed: Get "http://10.42.0.191:9093/-/healthy": context deadline exceeded (Client.Timeout exceeded while awaiting headers)
monitoring    20h         Warning   Unhealthy                           pod/monitoring-stack-grafana-5b77d89cfd-glh9x                Liveness probe failed: HTTP probe failed with statuscode: 503
monitoring    18h         Warning   Unhealthy                           pod/monitoring-stack-grafana-5b77d89cfd-glh9x                Liveness probe failed: Get "http://10.42.0.219:3000/api/health": dial tcp 10.42.0.219:3000: connect: connection refused
monitoring    20h         Warning   Unhealthy                           pod/monitoring-stack-kube-prom-operator-59f7df9897-pxpgx     Liveness probe failed: Get "https://10.42.0.190:10250/healthz": net/http: request canceled (Client.Timeout exceeded while awaiting headers)
monitoring    20h         Warning   Unhealthy                           pod/monitoring-stack-kube-prom-operator-59f7df9897-pxpgx     Liveness probe failed: Get "https://10.42.0.190:10250/healthz": net/http: request canceled while waiting for connection (Client.Timeout exceeded while awaiting headers)
monitoring    20h         Warning   Unhealthy                           pod/monitoring-stack-kube-prom-operator-59f7df9897-pxpgx     Liveness probe failed: Get "https://10.42.0.190:10250/healthz": context deadline exceeded
monitoring    20h         Warning   Unhealthy                           pod/monitoring-stack-kube-prom-operator-59f7df9897-pxpgx     Liveness probe failed: Get "https://10.42.0.190:10250/healthz": context deadline exceeded (Client.Timeout exceeded while awaiting headers)
monitoring    18h         Warning   Unhealthy                           pod/monitoring-stack-kube-prom-operator-59f7df9897-pxpgx     Liveness probe failed: Get "https://10.42.0.224:10250/healthz": context deadline exceeded
monitoring    17h         Warning   Unhealthy                           pod/monitoring-stack-kube-prom-operator-59f7df9897-pxpgx     Liveness probe failed: Get "https://10.42.0.224:10250/healthz": context deadline exceeded (Client.Timeout exceeded while awaiting headers)
monitoring    17h         Warning   Unhealthy                           pod/monitoring-stack-kube-prom-operator-59f7df9897-pxpgx     Liveness probe failed: Get "https://10.42.0.224:10250/healthz": net/http: request canceled (Client.Timeout exceeded while awaiting headers)
monitoring    49m         Warning   Unhealthy                           pod/monitoring-stack-kube-prom-operator-59f7df9897-pxpgx     Liveness probe failed: Get "https://10.42.0.233:10250/healthz": net/http: request canceled (Client.Timeout exceeded while awaiting headers)
monitoring    10m         Warning   Unhealthy                           pod/monitoring-stack-kube-prom-operator-59f7df9897-pxpgx     Liveness probe failed: Get "https://10.42.0.233:10250/healthz": net/http: request canceled while waiting for connection (Client.Timeout exceeded while awaiting headers)
monitoring    20h         Warning   Unhealthy                           pod/monitoring-stack-kube-state-metrics-5cf9965f97-npcbm     Liveness probe failed: Get "http://10.42.0.199:8080/livez": context deadline exceeded (Client.Timeout exceeded while awaiting headers)
monitoring    21h         Warning   Unhealthy                           pod/monitoring-stack-kube-state-metrics-5cf9965f97-npcbm     Liveness probe failed: HTTP probe failed with statuscode: 503
monitoring    20h         Warning   Unhealthy                           pod/monitoring-stack-prometheus-node-exporter-th546          Liveness probe failed: Get "http://192.168.101.85:9100/": context deadline exceeded (Client.Timeout exceeded while awaiting headers)
monitoring    17h         Warning   Unhealthy                           pod/monitoring-stack-prometheus-node-exporter-th546          Liveness probe failed: Get "http://192.168.101.85:9100/": context deadline exceeded (Client.Timeout exceeded while awaiting headers)
monitoring    5m44s       Warning   Unhealthy                           pod/monitoring-stack-prometheus-node-exporter-th546          Liveness probe failed: Get "http://192.168.101.85:9100/": context deadline exceeded (Client.Timeout exceeded while awaiting headers)
monitoring    20h         Warning   Unhealthy                           pod/prometheus-monitoring-stack-kube-prom-prometheus-0       Liveness probe failed: Get "http://10.42.0.194:9090/-/healthy": context deadline exceeded (Client.Timeout exceeded while awaiting headers)
```

## Save the answer 

1.
```bash
 mkdir -p /opt/KDOB00401
```

2.
```bash
echo production/liveness-exec > /opt/KDOB00401/broken.txt
```

3.
```bash
cat /opt/KDOB00401/broken.txt
```
output:
```bash
root@rouf:~# cat /opt/KDOB00401/broken.txt
production/liveness-exec
```

4.
```bash
echo pod/monitoring-stack-prometheus-node-exporter-th54 >> /opt/KDOB00401/broken.txt
```

## Error Events Solution

Example:
1.
```bash
kubectl -n production get event --field-selector involvedObject.name=liveness-exec
```

2.
```bash
kubectl -n production get event --field-selector involvedObject.name=liveness-exec \
-owide > /opt/KDOB00401/error.txt
```

3.
```bash
cat /opt/KDOB00401/error.txt
```
