# TLS in kubernetes

Let's look at the different components within the Kubernetes cluster and identify the various servers and clients and who talks to who.



* Let's start with a kube-apiserver. As we know already, the API server exposes an HTTPS service that other components, as well as external users, use to manage the Kubernetes cluster.
* So it is a server and it requires certificates to secure all communication with its clients. So we generate a certificate and key pair. We call it APIserver.cert and APIserver.key.
* Another server in the cluster is the etcd server. The etcd server stores all information about the cluster. So it requires a pair of certificate and key for itself. We will call it etcdserver.crt and etcdserver.key.
* The other server component in the cluster is on the worker nodes. They are the kubelet services. They also expose an HTTPS API endpoint that the kube-apiserver talks to to interact with the worker nodes. Again, that requires a certificate and key pair. We call it kubelet.cert and kubelet.key

<figure><img src="../.gitbook/assets/image (49).png" alt=""><figcaption></figcaption></figure>

Let's now look at the client components.

<figure><img src="../.gitbook/assets/image (50).png" alt=""><figcaption></figcaption></figure>

* The clients who access the kube-apiserver are us, the administrators through kubectl Arrest API. The admin user requires a certificate and key pair to authenticate to the kube-API server. We will call it admin.crt, and admin.key.
* The scheduler talks to the kube-apiserver to look for pods that require scheduling and then get the API server to schedule the pods on the right worker nodes. The scheduler is a client that accesses the kube-apiserver. So the scheduler needs to validate its identity using a client TLS certificate. So it needs its own pair of certificate and keys. We will call it scheduler.cert and scheduler.key.
* The Kube Controller Manager is another client that accesses the kube-apiserver, so it also requires a certificate for authentication to the kube-apiserver. So we create a certificate pair for it.
* The kube-proxy requires a client certificate to authenticate to the kube-apiserver, and so it requires its own pair of certificate and keys. We will call them kubeproxy.crt, and kubeproxy.key.
* The servers communicate amongst them as well. For example, the kube-apiserver communicates with the etcd server.
* In fact, of all the components, the kube-apiserver is the only server that talks to the etcd server. So as far as the etcd server is concerned, the kube-apiserver is a client, so it needs to authenticate. The kube-apiserver can use the same keys that it used earlier for serving its own API service. The APIserver.crt, and the APIserver.key files. Or, you can generate a new pair of certificates specifically for the kube-apiserver to authenticate to the etcd server.
* The kube-apiserver also talks to the kubelet server on each of the individual nodes. That's how it monitors the worker nodes for this. Again, it can use the original certificates, or generate new ones specifically for this purpose. So that's too many certificates.

<figure><img src="../.gitbook/assets/image (51).png" alt=""><figcaption></figcaption></figure>

* We will now see how to generate these certificates. As we know already, we need a certificate authority to sign all of these certificates.
* Kubernetes requires you to have at least one certificate authority for your cluster. In fact, you can have more than one. One for all the components in the cluster and another one specifically for etcd.

<figure><img src="../.gitbook/assets/image (52).png" alt=""><figcaption></figcaption></figure>

* In that case, the etcd servers certificates and the etcd servers client certificates, which in this case is the API server client certificate, will be all signed by the etcd server CA.
* For now, we will stick to just one CA for our cluster. The CA, as we know, has its own pair of certificate and key. We will call it CA.crt and CA.key. That should sum up all the certificates used in the cluster.
