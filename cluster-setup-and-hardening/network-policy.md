# Network Policy

<figure><img src="../.gitbook/assets/image (144).png" alt=""><figcaption></figcaption></figure>

* Let us now look at network security in Kubernetes.&#x20;
* So we have a cluster with a set of nodes hosting a set of pods and services. Each node has an IP address and so does each pod as well as service.&#x20;
* One of the prerequisite for networking in Kubernetes is whatever solution you implement, the pods should be able to communicate with each other without having to configure any additional settings like routes.&#x20;
* For example, in this network solution, all pods are on a virtual private network that spans across the node in the Kubernetes cluster and they can all by default reach each other using the IPs or pod names or services configured for that purpose. Kubernetes is configured by default with an all allow rule that allows traffic from any pod to any other pod or services within the cluster.

<figure><img src="../.gitbook/assets/image (145).png" alt=""><figcaption></figcaption></figure>

* Let us now bring back our earlier discussion and see how it fits into Kubernetes. For each component in the application, we deploy a pod. One for the front end web server, for the API server and one for the database.
* We create services to enable communication between the as well as to the end user. Based on what we discussed in the previous slide, by default, all the three pods can communicate with each other within the Kubernetes cluster.
* What if we do not want the front end web server to be able to communicate with the database server directly?
* And network policy is another object in the Kubernetes namespace just like pods, replica sets or services, you link a network policy to one or more pods. You can define rules within the network policy.

<figure><img src="../.gitbook/assets/image (146).png" alt=""><figcaption></figcaption></figure>

* In this case, I would say only allow ingress traffic Once this policy is created, it blocks all other traffic to the pod and only allows traffic that matches the specified rule.
* Again, this is only applicable to the pod on which the network policy is applied. So how do you apply or link a network policy to a pod? We use the same technique that was used before to link replica sets or services to pod, labels and selectors.

<figure><img src="../.gitbook/assets/image (147).png" alt=""><figcaption></figcaption></figure>

* We label the pod and use the same labels on the port selector field in the network policy and then we build our rule.&#x20;
* Under policy types, specify whether the rule is to allow ingress or egress traffic or both. In our case, we only want to allow ingress traffic to the DB pod, so we add ingress.&#x20;
* Next, we specify the ingress rule. That allows traffic from the API pod and you specify the API pod again using labels and selectors.

<figure><img src="../.gitbook/assets/image (148).png" alt=""><figcaption></figcaption></figure>

* Let us put all that together. We start with a blank object definition file, and as usual, we have API version, kind, metadata and spec.&#x20;
* The API version is networking.kh.io/v1. The kind is network policy, we will name the policy DB-policy and then under the spec section, we will first move the pod selector to apply this policy to the DB pod.
* Then we will move the rule we created in the previous slide under it, and that's it. We have our first network policy ready.

<figure><img src="../.gitbook/assets/image (149).png" alt=""><figcaption></figcaption></figure>

* Now note that ingress or egress isolation only comes into effect if you have ingress or egress in the policy types. In this example, there's only ingress in the policy type which means only ingress traffic is isolated and all egress traffic is unaffected. Meaning the pod is able to make any egress calls and they're not blocked.
* Remember that network policies are enforced by the network solution implemented on Kubernetes cluster and not all network solutions support network policies.
* Also, remember, even in a cluster configured with a solution that does not support network policies, you can still create the policies but they will just not be enforced. You will not get an error message saying the network solution does not support network policies.
