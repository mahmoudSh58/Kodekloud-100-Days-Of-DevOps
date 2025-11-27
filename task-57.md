> Create a Pod that prints a greeting message using environment variables
> 
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