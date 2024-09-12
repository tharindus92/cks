# API Groups

* We learned about the Kube API server. Whatever operations we have done so far with the cluster, we've been interacting with the API server one way or the other, either through the Kube control utility or directly via REST.

<figure><img src="../.gitbook/assets/image (88).png" alt=""><figcaption></figcaption></figure>

* Say we want to check the version we can access the API server at the master notes address followed by the port, which is 6-4-4-3, by default, and the API version. It returns the version.
* &#x20;Similarly, to get a list of pods you would access the URL api/v1/pods. Our focus in this lecture is about these API pods the version, and the API.

<figure><img src="../.gitbook/assets/image (89).png" alt=""><figcaption></figcaption></figure>

* The Kubernetes API is grouped into multiple such groups based on their purpose, such as one for APIs, one for health, one for metrics and logs, et cetera.&#x20;
* The version API is for viewing the version of the cluster, as we just saw. The Metrics and Health API are used to monitor the health of the cluster. The logs are used for integrating with third-party logging applications.

<figure><img src="../.gitbook/assets/image (90).png" alt=""><figcaption></figcaption></figure>

These APIs are categorized into two; the core group and the named group.

<figure><img src="../.gitbook/assets/image (91).png" alt=""><figcaption></figcaption></figure>

The core group is where all core functionality exists, such as name, spaces, pods, replication controllers, events and points, nodes, bindings, persistent volumes, persistent volume claims, conflict maps, secrets, services, et cetera.

<figure><img src="../.gitbook/assets/image (92).png" alt=""><figcaption></figcaption></figure>

* The named group APIs are more organized and going forward, all the newer features are going to be made available through these named groups.&#x20;
* It has groups under it for apps, extensions, networking, storage, authentication, authorization, et cetera. Shown here are just a few.&#x20;
* Within apps, you have deployments, replica sets, stateful sets. Within networking, you have network policies.
* &#x20;Certificates have these certificate signing requests that we talked about earlier in the section.
* &#x20;So the ones at the top are API groups, and the ones at the bottom are resources in those groups.&#x20;
* Each resource in this has a set of actions associated with them; Things that you can do with these resources, such as list the deployments, get information about one of these deployments, create a deployment, delete a deployment, update a deployment, watch a deployment, et cetera. These are known as verbs.

<figure><img src="../.gitbook/assets/image (93).png" alt=""><figcaption></figcaption></figure>

The Kubernetes API reference page can tell you what the API group is for each object; select an object, and the first section in the documentation page shows it's group details, v1 core is just v1.

<figure><img src="../.gitbook/assets/image (94).png" alt=""><figcaption></figcaption></figure>

* You can also view these on your Kubernetes cluster. Access your Kube API server at port 6-4-4-3 without any path and it will list you the available API groups.&#x20;
* And then, within the named API groups it returns all the supported resource groups.

<figure><img src="../.gitbook/assets/image (95).png" alt=""><figcaption></figcaption></figure>

* A quick note on accessing the cluster API like that. If you were to access the API directly through cURL as shown here, then you will not be allowed access except for certain APIs like version, as you have not specified any authentication mechanisms.&#x20;
* So you have to authenticate to the API using your certificate files by passing them in the command line like this. An alternate option is to start a Kube control proxy client.

<figure><img src="../.gitbook/assets/image (96).png" alt=""><figcaption></figcaption></figure>

* The Kube control proxy command and uses credentials and certificates from your Kube config file to access the cluster.
* &#x20;That way, you don't have to specify those in the cURL command. Now you can access the Kube control proxy service from Kube config file to forward your request to the Kube API server. This will list all available APIs at root.

<figure><img src="../.gitbook/assets/image (97).png" alt=""><figcaption></figcaption></figure>

* So here are two terms that kind of sound the same; The Kube proxy and Kube control proxy. Well, they're not the same.
* &#x20;We discussed about Kube proxy earlier this course. It is used to enable connectivity between pods and services across different nodes in the cluster.
* &#x20;We discuss about Kube proxy in much more detail later in this course. Whereas Kube control proxy is an ACTP proxy service created by Kube control utility to access the Kube API server.
