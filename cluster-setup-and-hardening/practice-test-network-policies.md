# Practice Test - Network Policies



{% hint style="info" %}
Create a network policy to allow traffic from the `Internal` application only to the `payroll-service` and `db-service`.

\


Use the spec given below. You might want to enable ingress traffic to the pod to test your rules in the UI.

Also, ensure that you allow egress traffic to DNS ports TCP and UDP (port 53) to enable DNS resolution from the internal pod.

\-

Check

\-Next

Policy Name: internal-policy

Policy Type: Egress

Egress Allow: payroll

Payroll Port: 8080

Egress Allow: mysql

MySQL Port: 3306

\===========================================



Solution manifest file for a network policy `internal-policy` as follows:

```yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: internal-policy
  namespace: default
spec:
  podSelector:
    matchLabels:
      name: internal
  policyTypes:
  - Egress
  - Ingress
  ingress:
    - {}
  egress:
  - to:
    - podSelector:
        matchLabels:
          name: mysql
    ports:
    - protocol: TCP
      port: 3306

  - to:
    - podSelector:
        matchLabels:
          name: payroll
    ports:
    - protocol: TCP
      port: 8080

  - ports:
    - port: 53
      protocol: UDP
    - port: 53
      protocol: TCP
```

\


**Note:** We have also allowed `Egress` traffic to `TCP` and `UDP` port. This has been added to ensure that the internal DNS resolution works from the `internal` pod.

\


**Remember:** The `kube-dns` service is exposed on port `53`:

```sh
root@controlplane:~> kubectl get svc -n kube-system 
NAME       TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)                  AGE
kube-dns   ClusterIP   10.96.0.10   <none>        53/UDP,53/TCP,9153/TCP   18m

root@controlplane:~>
```

<img src="https://edf846a5ee7f40c4.labs.kodekloud.com/images/kubernetes-ckad-network-policies-9.jpg" alt="" data-size="original">
{% endhint %}
