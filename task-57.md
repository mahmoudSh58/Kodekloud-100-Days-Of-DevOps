## Day 57: Print Environment Variables

1- Create Pod to Print Environment Variables
> Create file `print-envars-greeting.yaml` using `vim print-envars-greeting.yaml` and add the following content:
``` yaml
kind: Pod
apiVersion: v1
metadata:
  name: print-envars-greeting
spec:
  containers:
    - name: print-env-container
      image: bash
      env:
        - name: GREETING
          value: "Welcome to"
        - name: COMPANY
          value: "xFusionCorp"
        - name: GROUP
          value: "Industries"
      command: ["/bin/sh", "-c", 'echo "$(GREETING) $(COMPANY) $(GROUP)"']
  restartPolicy: Never
```
> save and exit the file.
>
> Apply the Pod using the command:
```bash
kubectl apply -f print-envars-greeting.yaml
```
---
2- Verify the Pod Output
> Use the following command to check the Pod logs and verify the output:
``` bash
kubectl logs print-envars-greeting
```