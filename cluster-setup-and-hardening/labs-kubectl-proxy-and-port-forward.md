# Labs - Kubectl Proxy & Port Forward



{% hint style="info" %}
Now lets start the `kubectl proxy` on the controlplane node.

The proxy should use the default port and run in the background.



Run below command

```
kubectl proxy &
```
{% endhint %}



{% hint style="info" %}
Call the API endpoint `/version` of kubectl proxy using `curl` and redirect the output to the file: `/root/kube-proxy-version.json`

\


Tip: Open a new terminal if you didn't run `kubectl proxy` as a background service in the previous step.

\==========================



Run below command

```
curl 127.0.0.1:8001/version > /root/kube-proxy-version.json
```
{% endhint %}



{% hint style="info" %}
The kubectl proxy is running on the port `8001` by default. However, in some situations, this port may be used by another process in system. Let's fix that.

Run the `kubectl proxy` in the background on port `8002` instead.

\=========================



Run

```
kubectl proxy --port 8002 &
```
{% endhint %}



{% hint style="info" %}
What is the command to forward port `8000` on the host to port `5000` on the pod `app` ?

\================
{% endhint %}



{% hint style="info" %}
We deployed `nginx` app in default namespace. Wait few seconds for pod to spinup.

Forward port `8005` of localhost to port `80` of nginx pods\


Run port-forward process in background.

Try accessing port `8005` after port forwarding.

\====================

First get all resource names for `nginx` using the command:

```
kubectl get all
```

Run any of below commands with valid names

```
kubectl port-forward pods/{POD_NAME} 8005:80 &
```

OR

```
kubectl port-forward deployment/{DEPLOYMENT_NAME} 8005:80 &
```

OR

```
kubectl port-forward service/{SERVICE_NAME} 8005:80 &
```

OR

```
kubectl port-forward replicaset/{REPLICASET_NAME} 8005:80 &
```

then try `curl localhost:8005` to check nginx response

\========

kubectl port-forward deployment/nginx 8005:80 &
{% endhint %}
