# Labs - Admission Controllers



{% hint style="info" %}
The previous step failed because kubernetes have `NamespaceExists` admission controller enabled which rejects requests to namespaces that do not exist. So, to create a namespace that does not exist automatically, we could enable the `NamespaceAutoProvision` admission controller

\


Enable the `NamespaceAutoProvision` admission controller

_Note:_ Once you update `kube-apiserver yaml` file, please wait for a few minutes for the `kube-apiserver` to restart completely.

\==================

Add `NamespaceAutoProvision` admission controller to `--enable-admission-plugins` list to `/etc/kubernetes/manifests/kube-apiserver.yaml`\
It should look like below

```
    - --enable-admission-plugins=NodeRestriction,NamespaceAutoProvision
```

API server will automatically restart and pickup this configuration.
{% endhint %}



{% hint style="info" %}
Disable `DefaultStorageClass` admission controller

\


This admission controller observes creation of `PersistentVolumeClaim` objects that do not request any specific storage class and automatically adds a default storage class to them. This way, users that do not request any special storage class do not need to care about them at all and they will get the default one.

_Note:_ Once you update `kube-apiserver yaml` file then please wait few mins for the `kube-apiserver` to restart completely.

\================



Update `/etc/kubernetes/manifests/kube-apiserver.yaml` as below

```
   - --disable-admission-plugins=DefaultStorageClass
```
{% endhint %}

