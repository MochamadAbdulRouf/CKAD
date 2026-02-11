  # CKAD EXAM Fix Deprecated Deployment

Task:
- Fix any API deprecation issues in the manifest file /tmp/www.yaml so that this application can be deployed on cluster 
- Deploy the applications specified in the updated manifest file /tmp/www.yaml in namespace cobra
- Namespace cobra already exists in the system

## Troubleshoot

1. Try create Deployment 
```bash
k create -f www.yaml
```
error because deprecated API version
```bash
root@rouf:~# k create -f www.yaml
error: resource mapping not found for name: "test" namespace: "cobra" from "www.yaml": no matches for kind "Deployment" in version "extensions/v1beta1"
ensure CRDs are installed first
```

2. Checkout sample deployment
```bash
k create deployment test --image=nginx --dry-run=client -oyaml
```

3. Fix API version 
look at my file manifest `www.yaml`

4. Add selector and try, look at file `www.yaml`

5. Verify
```bash
k create -f www.yaml
```
