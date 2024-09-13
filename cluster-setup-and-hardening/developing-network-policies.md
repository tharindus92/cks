# Developing network policies

* So let's get the other things out of our way so we can focus exactly on the required tasks. So we don't need to worry about the web pod or its port as we don't want to allow any traffic from any other sources other than the API pod. So let's get rid of that.
* We can also forget about the port on the API pod to which the web server connects as we don't care about that either.
* As we discussed by default, Kubernetes allows all traffic from all pods to all destinations. So as the first step, we want to block out everything going in and out of the database pod.
* So we do that by adding a pod selector field with the match labels option and by specifying the label on the DB pod, which happens to be set to role db and that associates the network policy with the database pod and blocks out all traffic.
* The API pod makes database queries and the database pods returns the results. So what about the results? Do you need a separate rule for the results to go back to the API pod? No, because once you allow incoming traffic, the response or reply to that traffic is allowed back automatically. We don't need a separate rule for that.
* you only need to be concerned about the direction in which the request originates, which is denoted by the straight line here, and you don't need to worry about the response which is denoted by the dotted line.

<figure><img src="../.gitbook/assets/image (150).png" alt=""><figcaption></figcaption></figure>

* This would create a policy that would block all traffic to the db-pod except for traffic from the API pod.

<figure><img src="../.gitbook/assets/image (151).png" alt=""><figcaption></figcaption></figure>

* Now what if there are multiple API pods in the cluster with the same labels but in different namespaces? So say, here we have different namespaces for dev, test, and prod environments, and we have the API pod with the same labels in each of these environments.
* The current policy would allow any pod in any namespace with matching labels to reach the database pod.
* We only want to allow the API pod in the prod namespace to reach the database pod. So how do we do that?

<figure><img src="../.gitbook/assets/image (152).png" alt=""><figcaption></figcaption></figure>

For this, we add a new selector called as the namespace selector property

<figure><img src="../.gitbook/assets/image (153).png" alt=""><figcaption></figcaption></figure>

* Now what if you only have the namespace selector and not the pod selector like this? In this case, all pods within the specified namespace will be allowed to reach the database pod such as the web pod that we had earlier, but pods from outside this namespace won't be allowed to go through.

<figure><img src="../.gitbook/assets/image (154).png" alt=""><figcaption></figcaption></figure>

* Let's look at another use case. Say we have a backup server somewhere outside of the Kubernetes cluster and we want to allow this server to connect to the database pod.&#x20;
* Now since this backup server is not deployed in our Kubernetes cluster, the pod selector and namespace selector fields that we use to define traffic from won't work because it's not a pod in the cluster.&#x20;
* However, we know the IP address of the backup server We could configure a network policy to allow traffic originating from certain IP addresses. For this, we add a new type of from definition known as the IP block definition.&#x20;
* IP block allows you to specify a range of IP addresses from which you could allow traffic to hit the database pod. So those are three supported selectors under the from section and ingress. And these are also applicable to the to section in egress.

<figure><img src="../.gitbook/assets/image (155).png" alt=""><figcaption></figcaption></figure>

* and we'll see that in a few minutes. We have podSelector to select pods by labels. We have namespaceSelector to select name spaces by labels and we have the ipBlock selector to select IP address ranges.&#x20;
* These can be passed in separately as individual rules or together as part of a single rule. In this example, under the from section, we have two elements. So these are two rules. The first rule has the podSelector and the namespaceSelector together. And the second rule has the ipBlock selector.
* &#x20;So this works like an OR operation. Traffic from sources meeting either of these criteria are allowed to pass through. However, within the first rule, we have to selectors part of it.
* That would mean traffic from sources must meet both of these criteria to pass through. So they have to be originating from pods with matching labels of API pod and those pods must be in the prod namespace. So it works like an AND operation.

<figure><img src="../.gitbook/assets/image (156).png" alt=""><figcaption></figcaption></figure>

* Now what if we were to separate them by adding a dash before the namespaceSelector like this? Now, they are two separate rules.&#x20;
* So this would mean that traffic matching the first rule is allowed, that is from any pod matching the label API pod in the same namespace And traffic matching the second rule is allowed, which is from any pod within the prod namespace that is either from the the pod web and, of course, along with the backup server as we have the ipBlock specification as well.&#x20;
* So now we have three separate rules and almost traffic from anywhere is allowed to the db-pod. So a small change like that can have a big impact.

<figure><img src="../.gitbook/assets/image (157).png" alt=""><figcaption></figcaption></figure>

* And we'll now look at egress. So say for example, instead of the backup server initiating a backup, say we have an agent on the db-pod that pushes backup to the backup server.&#x20;
* In that case, the traffic is originating from the database pod to an external backup server. For this, we need to have egress rule defined.&#x20;
* So we first add egress to the policy types and then we add a new egress section to define the specifics of the policy. So instead of from, we now have to under egress. So that's the only difference. Under to, we could use any of the selectors such as a pod, a namespace, or an ipBlock selector. And in this case, since the database server is external, we use ipBlock selector and provide the the CIDR block for the server.&#x20;
* So this rule allows traffic originating from the database pod to an external backup server at the specified address.
