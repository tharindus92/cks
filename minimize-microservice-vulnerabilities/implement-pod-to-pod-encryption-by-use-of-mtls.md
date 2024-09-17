# Implement pod to pod encryption by use of mTLS

<figure><img src="../.gitbook/assets/KodeKloud-Kubernetes-CKS-040-minimize-microservice-vulnerabilities_page-0140.jpg" alt=""><figcaption></figcaption></figure>

* In this lecture, we will learn how we can make use of mTLS to secure inter pod communication. In the previous lecture, we saw how two distinct organizations may need to securely communicate with each other. The pods in a Kubernetes cluster behave the same way. When one pod needs information from another pod running on a different server, it will send requests through the network connecting the two servers.
* However, by default, data transmitted between these pods is unencrypted. This opens up the same problems as before. An attackers sniffing on the network can see what the message is as it is sent in a plain text format. Now, this may potentially leak information that should probably remain private.
* Imagine a pod asking another pod for some private data such as a customer's phone number and address. Encryption ensures that pods in a cluster can communicate securely with each other.



<figure><img src="../.gitbook/assets/KodeKloud-Kubernetes-CKS-040-minimize-microservice-vulnerabilities_page-0141.jpg" alt=""><figcaption></figcaption></figure>

* Just like we saw in the example of the two businesses exchanging data using mTLS, we can also set up mutual TLS between our pods. When a pod A wants to send data to pod B, it will first request for pod B certificate. Pod B will then reply back with its own certificate but will also ask pod A for its certificate. After validating pod B certificate, pod A will send its own public certificate along with the symmetric key.
* Pod B will then validate pod A certificate and check if it's valid. All future communication will now be encrypted using the symmetric key.
* With mutual authentication, both pods prove to each other that they are indeed the real pods in the Kubernetes cluster and not some attacker trying to trick one of them into accepting fake data or fake commands.
* With the added encryption on top of this, we can make sure that the communication between the pods is very secure.

<figure><img src="../.gitbook/assets/KodeKloud-Kubernetes-CKS-040-minimize-microservice-vulnerabilities_page-0142.jpg" alt=""><figcaption></figcaption></figure>

* How is all this achieved within our cluster? Who manages the certificates and the keys? How is it managed when there are hundreds of pods schedule across several nodes, each of which tries to transfer data with several other pods? One way to do this is to have the applications running on the pods encrypt the messages themselves.

<figure><img src="../.gitbook/assets/KodeKloud-Kubernetes-CKS-040-minimize-microservice-vulnerabilities_page-0143.jpg" alt=""><figcaption></figcaption></figure>

* This is indeed done by some applications. MySQL, for example, does indeed encrypt data by default. However, it is not very convenient to use this universally. For example, let us consider a webapp1 pod which is running Apache. This may use a completely different encryption algorithm as compared to webapp2, which is developed by another team using Nginx.
* MySQL backend now would need to be able to support both these types of encryption, which adds complexity to the code and adding unnecessary work.
* Instead, the better approach is to let applications communicate normally using unencrypted format and use other ways to add encryption on top.

<figure><img src="../.gitbook/assets/KodeKloud-Kubernetes-CKS-040-minimize-microservice-vulnerabilities_page-0144.jpg" alt=""><figcaption></figcaption></figure>

* Instead of relying on applications to encrypt the data, we can leverage third-party programs, Istio and Linkerd are two such third-party programs that facilitate mTLS encryption between pods.

<figure><img src="../.gitbook/assets/KodeKloud-Kubernetes-CKS-040-minimize-microservice-vulnerabilities_page-0145.jpg" alt=""><figcaption></figcaption></figure>

* How do these programs implement mTLS? Well, both Istio and Linkerd allow secure service-to-service communication without depending on applications. Now, these solutions, however, are not just limited to encrypting or decrypting data.
* They do many more things all related to connecting multiple services together in a microservice architecture in what is technically named as service mesh.
* We will stick to the features relevant to our topic, which is implementing mTLS for our pods and we'll specifically look at Istio, which is perhaps the most popular service mesh in use today.

<figure><img src="../.gitbook/assets/KodeKloud-Kubernetes-CKS-040-minimize-microservice-vulnerabilities_page-0146.jpg" alt=""><figcaption></figcaption></figure>

* Let us turn our attention back to the webapp container running inside a pod on one node that needs to talk to the MySQL container running inside another pod on another node. When we implement mTLS with Istio, Istio inserts a sidecar container into the webapp pod as well as the MySQL pod.
* Now, when our webapp pod generates a message that has to be sent to the MySQL pod, the Istio sidecar intercepts the message before it leaves the network.
* It then encrypts the message and then sends it through to the network. Once the encrypted data reaches the other server, it is decrypted by the Istio sidecar container running inside the MySQL pod.
* The decrypted data is then passed on to the MySQL container.

<figure><img src="../.gitbook/assets/KodeKloud-Kubernetes-CKS-040-minimize-microservice-vulnerabilities_page-0147.jpg" alt=""><figcaption></figcaption></figure>

* Before we move on, let us take a look at the types of encryptions supported by Istio. One way is to allow the traffic to be encrypted most of the times but not always.
* &#x20;It will be done on a when possible basis. For example, let us consider another application that needs to be able to communicate to the MySQL pod.&#x20;
* Now, this application may not have support for mTLS, or perhaps it is an external service that is not part of the same cluster where we have implemented Istio for mTLS.
* In such cases, the app may only be able to transmit unencrypted data in plain-text format.
* While this is risky and may result in private data being leaked, it may be absolutely necessary for the application to work. Istio supports this mode where it allows both mTLS as well as plain-text traffic and it is known as permissive or opportunistic mode.

<figure><img src="../.gitbook/assets/KodeKloud-Kubernetes-CKS-040-minimize-microservice-vulnerabilities_page-0148.jpg" alt=""><figcaption></figcaption></figure>

* The other mode that we'll take a look at is called strict or enforced mode, which makes sure that only mTLS traffic is allowed between the pods every single time.&#x20;
* While this is very secure, it can also cause the applications to break. The external app which does not support mTLS will not work when we use this mode.
* &#x20;Well, that's it for this lecture. In the upcoming demo, we will see how to install Istio in an existing Kubernetes cluster and then make use of mTLS to secure pod-to-pod traffic.
