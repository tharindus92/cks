# Cluster Upgrade Process

<figure><img src="../.gitbook/assets/image (130).png" alt=""><figcaption></figcaption></figure>

* The components can be at differen release version

<figure><img src="../.gitbook/assets/image (131).png" alt=""><figcaption></figcaption></figure>

* supported for 3 minor version
* update must do byone by one

<figure><img src="../.gitbook/assets/image (132).png" alt=""><figcaption></figcaption></figure>

## Upgrade with kubeadm

<figure><img src="../.gitbook/assets/image (133).png" alt=""><figcaption></figcaption></figure>

* Firstupgrade master node and then upgrade the worker nodes.
* while the master is being upgraded, the control plane component such as API server,scheduler and controller-managers, go down briefly.
* when master node goes down does not mean your worke nodes and application on the clustert are impacted. All workload hosted on the worker nodes continue to seve users as normal.
* All management funtions are down
* you cannt access the cluster using kubectl or otherkubenetes API
* You cannot deploy new applications,delet or modify exiting ones.
* The controller manager don't funtion either. If a pod was to fail, a new pod won't be automaticlly created.
* But as long as the nodes and th pods are up,your application should be up and users will not be impacted.



### Worker node upgrade strategy

<figure><img src="../.gitbook/assets/image (134).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (135).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (136).png" alt=""><figcaption></figcaption></figure>

## Master node

<figure><img src="../.gitbook/assets/image (137).png" alt=""><figcaption></figcaption></figure>

* kubeadm not upgrade the kubelet we have to update it manualy
* 1st we have to upgrde kubem tool with the same software vesion as kubenetes.

<figure><img src="../.gitbook/assets/image (138).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (139).png" alt=""><figcaption></figcaption></figure>

```
apt-get upgrade -y kubeadm=1.12.0-00
kubeadm upgrade apply v1.12.0
apt-get upgrade -y kubelet=1.12.0-00
systemctl restart kubelet
```

## Worker node

<figure><img src="../.gitbook/assets/image (140).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (141).png" alt=""><figcaption></figcaption></figure>

```
apt-get upgrade -y kubeadm=1.12.0-00
apt-get upgrade -y kubelet=1.12.0-00
kebeadm upgrade node config --kubelet-version v1.1.0
systemctl restart kubelet
```
