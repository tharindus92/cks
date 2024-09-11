# Kubernetes Security Primitives

<figure><img src="../.gitbook/assets/image (24) (1).png" alt=""><figcaption></figcaption></figure>



* Let's begin with the host that formed the cluster itself. Of course all access to these hosts must be secured, root access disabled, password based authentication disabled, and only SSH key based authentication to be made available.&#x20;
* And of course any other measures you need to take to secure your physical or virtual infrastructure that hosts kubernetes. Of course if that is compromised, everything is compromised.
* Our focus in this lecture is more on Kubernetes related security.
* What are the risks and what measures do you need to take to secure the cluster. As we have seen already, the kube-api server is at the center of all operations within kubernetes. We interact with it through the kubectl utility or by accessing the API directly and through that you can perform almost any operation on the cluster. So that's the first line of defense.
* Controlling access to the API server itself. We need to make two types of decisions who can access the cluster and what can they do.

<figure><img src="../.gitbook/assets/image (25) (1).png" alt=""><figcaption></figcaption></figure>



* Who can access the API server is defined by the Authentication mechanisms. There are different ways that you can authenticate to the API server.&#x20;
* Starting with user IDs and passwords stored in a static file, to tokens, certificates or even integration with external authentication providers like LDAP. Finally for machines we create service accounts.

<figure><img src="../.gitbook/assets/image (26) (1).png" alt=""><figcaption></figcaption></figure>



* Once they gain access to the cluster, What can they do is defined by authorization mechanisms. Authorization is implemented using Role Based Access Control, where users are associated to groups with specific permissions. In addition there are other authorization modules like Attribute based access control, Node Authorizers, webhooks etc.

<figure><img src="../.gitbook/assets/image (27) (1).png" alt=""><figcaption></figcaption></figure>



* Again we look at these in more detail in the upcoming lectures. All communication with the cluster, between the various components such as the ETCD cluster, kube controller manager, scheduler, api server, as well as those running on the worker nodes such as the kubelet and and kubeproxy is secured using TLS Encryption. We have a section entirely for this where we discuss and practice how to setup the certificates between the various components.

<figure><img src="../.gitbook/assets/image (28) (1).png" alt=""><figcaption></figcaption></figure>



* What about communication between applications within the cluster. By default all PODs can access all other PODs within the cluster. You can restrict access between them using Network Policies. We will look at how exactly that is done.
