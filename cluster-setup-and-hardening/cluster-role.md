# Cluster role

```
kubectl api-resources --namespaced=true
kubectl api-resources --namespaced=false

kubectl get clusterroles --no-headers  | wc -l 
kubectl get clusterroles --no-headers  -o json | jq '.items | length'

kubectl get clusterrolebindings --no-headers  | wc -l
```

<figure><img src="../.gitbook/assets/image (111).png" alt=""><figcaption></figcaption></figure>

* When we talked about roles and role bindings, we said that roles and role bindings are namespaced, meaning they are created within namespaces.&#x20;
* If you don't specify a namespace, they are created in the default namespace and control access within that namespace alone.

<figure><img src="../.gitbook/assets/image (112).png" alt=""><figcaption></figcaption></figure>

* But how do we authorize users to cluster-wide resources like nodes or persistent volumes. That is where you use cluster roles and cluster role bindings.&#x20;
* Cluster roles are just like roles, except they are for cluster-scoped resources.

<figure><img src="../.gitbook/assets/image (113).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (114).png" alt=""><figcaption></figcaption></figure>

* We said that cluster roles and binding are used for cluster-scoped resources, but that is not a hard rule. You can create a cluster role for namespaced resources as well. When you do that, the user will have access to these resources across all namespaces.
* Earlier, when we created a role to authorize a user to access pod, the user had access to pods in a particular namespace alone. With cluster roles, when you authorize a user to access the pods, the user gets access to all pods across the cluster.

```
---
kind: ClusterRole
apiVersion: rbac.authorization.k8s.io/v1
metadata:
  name: node-admin
rules:
- apiGroups: [""]
  resources: ["nodes"]
  verbs: ["get", "watch", "list", "create", "delete"]

---
kind: ClusterRoleBinding
apiVersion: rbac.authorization.k8s.io/v1
metadata:
  name: michelle-binding
subjects:
- kind: User
  name: michelle
  apiGroup: rbac.authorization.k8s.io
roleRef:
  kind: ClusterRole
  name: node-admin
  apiGroup: rbac.authorization.k8s.io
```
