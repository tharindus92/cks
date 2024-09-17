# Using Runtimes in Kubernetes



<figure><img src="../.gitbook/assets/KodeKloud-Kubernetes-CKS-040-minimize-microservice-vulnerabilities_page-0125 (1).jpg" alt=""><figcaption></figcaption></figure>

* In this lecture, we will see how to make use of specific runtime configuration for creating Kubernetes pods. In the previous lecture, we saw that gVisor makes use of the runsc runtime to create containers. Assuming that we already have gVisor installed on our Kubernetes nodes, let us now see how to instruct the pods to specifically make use of this runtime. To do this, we have to create a new Kubernetes object called RuntimeClass.
* This object has two significant fields, the RuntimeClass name which we have called gVisor, and secondly, the handler, which is the runtime to be used by the RuntimeClass. For gVisor, we will set this to runsc. Next, use the Kubectl create command to create the RuntimeClass object, like this. One thing to note here is that the name that we have used for the RuntimeClass can be anything. We just chose to call it gVisor, but you can call it whatever you want. The handler used, however, should be a valid runtime, such as runsc for gVisor or Kata for kata Containers.

<figure><img src="../.gitbook/assets/KodeKloud-Kubernetes-CKS-040-minimize-microservice-vulnerabilities_page-0126 (1).jpg" alt=""><figcaption></figcaption></figure>

* &#x20;Here we have a pod definition file to create an NGINX container. To specifically instruct the pod to be created using the gVisor runtime, we can make use of the field RuntimeClass name under the pod spec, like this. Here, we provide the name of the RuntimeClass that we just created. Creating the pod now will create the containers using the gVisor RuntimeClass.

<figure><img src="../.gitbook/assets/image (8).png" alt=""><figcaption></figcaption></figure>

* Our pod is now running the gVisor. To test that, the NGINX container is isolated from the Linux kernel, we can check if the NGINX process can be seen on the node which is running the pod. When we run the pgrep -a nginx on the node, the NGINX process seems to be missing.&#x20;
* This shows that gVisor is doing a good job at sandboxing that container, hiding what's going on inside from the rest of the system. We can also see that the runsc runtime is running on the node.
