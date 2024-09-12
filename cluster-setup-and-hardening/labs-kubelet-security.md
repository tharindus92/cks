# Labs - Kubelet Security



{% hint style="info" %}


This shouldn't be allowed. Set the authorization mode to `Webhook` and restart kubelet.\
Then call the Pods API again using the command `curl -sk https://localhost:10250/pods`\


You must get a response like below as you removed authorization setting for anonymous user

```
Forbidden (user=system:anonymous, verb=get, resource=nodes, subresource=proxy)
====================
Set /var/lib/kubelet/config.yaml as below

authorization:
  mode: Webhook
then restart kubelet service using the command:

systemctl restart kubelet.service
.

Then check if you can still access pods endpoint with

curl -sk https://localhost:10250/pods
```
{% endhint %}



{% hint style="info" %}
Next, let's disable anonymous authentication mode. Once done, check again using the command `curl -sk https://localhost:10250/pods`\


You should get a response as `Unauthorized`

`============`



Set `/var/lib/kubelet/config.yaml` as below

```
authentication:
  anonymous:
    enabled: false 
```

then run below\
`systemctl restart kubelet.service`

Then check if you can still access pods endpoint with

```
curl -sk https://localhost:10250/pods
```
{% endhint %}



{% hint style="info" %}
Disable metrics endpoint on readOnlyPort.

After disabling check the metrics API again and verify that it does not display any results. Command: `curl -sk http://localhost:10255/metrics`

`==========================`



Set `readOnlyPort` as `0` in `/var/lib/kubelet/config.yaml` as below

```
readOnlyPort: 0
```

then run below

`systemctl restart kubelet.service`

Then check if you can still access metrics endpoint with

```
curl -sk http://localhost:10255/metrics
```
{% endhint %}

