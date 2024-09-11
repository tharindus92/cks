# Lab - Service Accounts



{% hint style="info" %}
Enter the access token in the UI of the dashboard application. Click `Load Dashboard` button to load Dashboard

Create an authorization token for the newly created service account, copy the generated token and paste it into the `token` field of the UI.\


To do this, run `kubectl create token dashboard-sa` for the `dashboard-sa` service account, copy the token and paste it in the UI.
{% endhint %}



{% hint style="info" %}
You shouldn't have to copy and paste the token each time. The Dashboard application is programmed to read token from the secret mount location. However currently, the `default` service account is mounted. Update the deployment to use the newly created ServiceAccount

\


Edit the deployment to change ServiceAccount from `default` to `dashboard-sa`

`==================================`

`U`se following `YAML` file:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: web-dashboard
  namespace: default
spec:
  replicas: 1
  selector:
    matchLabels:
      name: web-dashboard
  strategy:
    rollingUpdate:
      maxSurge: 25%
      maxUnavailable: 25%
    type: RollingUpdate
  template:
    metadata:
      creationTimestamp: null
      labels:
        name: web-dashboard
    spec:
      serviceAccountName: dashboard-sa
      containers:
      - image: gcr.io/kodekloud/customimage/my-kubernetes-dashboard
        imagePullPolicy: Always
        name: web-dashboard
        ports:
        - containerPort: 8080
          protocol: TCP
```

Then run the command: `kubectl apply -f <FILE-NAME>.yaml`
{% endhint %}

