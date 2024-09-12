# Labs - Secure Kubernetes Dashboard



{% hint style="info" %}


To create the `Kubernetes dashboard` resources in the terminal run the following command.

```sh
kubectl apply -f https://raw.githubusercontent.com/kubernetes/dashboard/v2.7.0/aio/deploy/recommended.yaml
```
{% endhint %}



{% hint style="info" %}
Now run kubectl proxy with the command `kubectl proxy --address=0.0.0.0 --disable-filter &`

Click on `Kubernetes API` tab which is available at top right on the terminal and you will see API endpoints.

Append `/api/v1/namespaces/kubernetes-dashboard/services/https:kubernetes-dashboard:/proxy/#/login` to URL to check login UI

**Note:** From a security perspective do not use `--disable-filter` option as it can leave you vulnerable to XSRF attacks, when used with an accessible port. We have used this option to make our lab environment work with the kubernetes dashboard so you can access it through a browser. Ideally you would be accessing it through a kubectl proxy on your localhost only.\
So in actual environments do not use `--disable-filter` option as its a major security risk.
{% endhint %}



{% hint style="info" %}


Get the token of `admin-user` and login into UI. Check whether you can modify any of resources

Get token by running below and login to UI with that token: -\
\


```
kubectl get secrets -n kubernetes-dashboard admin-user -o go-template="{{.data.token | base64decode}}"
```
{% endhint %}



{% hint style="info" %}


Logout the `admin-user` from the Kubernetes Dashboard UI.\
And get the token from the secret called `readonly-user` we created just now and login into UI.\
\


Check whether you can modify any of the resources.

\-

Run the below command to get the token: -\
\


```
kubectl -n kubernetes-dashboard get secret readonly-user -o go-template="{{.data.token | base64decode}}"
```
{% endhint %}



{% hint style="info" %}
he `readonly-user` service account has too few privileges. Now let's create a `dashboard-admin` service account with access to the `kubernetes-dashboard` namespace only.\
\
\
Also, create a new secret called `dashboard-admin-secret` in the same namespace using the given YAML file `/root/dashboard-admin-secret.yaml`. It will create token credentials for the service account.

\


Use `admin` ClusterRole with RoleBinding so that it gives full control over every resource in the role binding's namespace, including the namespace itself.

Also assign `list-namespace` ClusterRole to only see namespaces.

Please use below names for bindings

RoleBinding name: `dashboard-admin-binding`

ClusterRoleBinding name: `dashboard-admin-list-namespace-binding`

`=====================================`



Run below command

```
cat <<EOF | kubectl apply -f -
apiVersion: v1
kind: ServiceAccount
metadata:
  name: dashboard-admin
  namespace: kubernetes-dashboard
EOF


# admin RoleBinding
cat <<EOF | kubectl apply -f -
apiVersion: rbac.authorization.k8s.io/v1
kind: RoleBinding
metadata:
  name: dashboard-admin-binding
  namespace: kubernetes-dashboard
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: admin
subjects:
- kind: ServiceAccount
  name: dashboard-admin
  namespace: kubernetes-dashboard
EOF


## list-namespace ClusterRoleBinding

cat <<EOF | kubectl apply -f -
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
  name: dashboard-admin-list-namespace-binding
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: list-namespace
subjects:
- kind: ServiceAccount
  name: dashboard-admin
  namespace: kubernetes-dashboard
EOF
```
{% endhint %}



{% hint style="info" %}


Get the token of `dashboard-admin` service account and login into UI. Confirm `dashboard-admin` can update/manage resources in the `kubernetes-dashboard` namespace only.

In the UI, in the `Select namespace` dialog box, select the `kubernetes-dashboard` from the dropdown to see its resources.\
Also, confirm that other namespace resources are not visible to this service account.

\-

Run the below command to get token as follows: -\
\


```
kubectl -n kubernetes-dashboard get secret dashboard-admin-secret -o go-template="{{.data.token | base64decode}}"
```
{% endhint %}
