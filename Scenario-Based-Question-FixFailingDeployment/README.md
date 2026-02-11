# Fix Failing Deployment 
 
Task: 
- A Deployment is falling on the cluster due to an incorrect image being specified.
- Locate the deployment, and fix the problem 
- Update deployment image to nginx:1.17.4

## Troubleshoot 

1. run this command for checking specific events on the deployment 
```bash
k get events 
```
or 
```bash
k get events -A 
```

2. Check correct pods on the namespace
```bash
k -n namespace get deploy 
```
then
```bash
k -n namespace edit deploy namedeploy
```

3. Alternate solution
```bash
k set image --help
```
then
```bash
k -n galaxy set image deploy/namedeploy nginx=nginx:1.17.4 
```

4. Verify
```bash
k get events -A
```
