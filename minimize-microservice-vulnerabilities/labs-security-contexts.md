# Labs - Security Contexts



{% hint style="info" %}
Edit the pod `ubuntu-sleeper` to run the sleep process with user ID `1010`.

Note: Only make the necessary changes. Do not modify the name or image of the pod.

\===========

To delete the existing `ubuntu-sleeper` pod:

```sh
kubectl delete po ubuntu-sleeper 
```

After that apply solution manifest file to run as user `1010` as follows:

```yaml
---
apiVersion: v1
kind: Pod
metadata:
  name: ubuntu-sleeper
  namespace: default
spec:
  securityContext:
    runAsUser: 1010
  containers:
  - command:
    - sleep
    - "4800"
    image: ubuntu
    name: ubuntu-sleeper
```

Then run the command `kubectl apply -f <file-name>.yaml` to create a resource.

NOTE: TO delete the pod faster, you can run `kubectl delete pod ubuntu-sleeper --force`. This can be done for any pod in the lab or the actual exam. It is not recommended to run this in Production, so keep a note of that.
{% endhint %}
