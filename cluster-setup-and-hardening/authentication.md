# AUTHENTICATION

<figure><img src="../.gitbook/assets/image (53).png" alt=""><figcaption></figcaption></figure>

* Hello and welcome to this lecture on authentication in a Kubernetes cluster as we have seen already the kubernetes cluster consists of multiple nodes physical/virtual and various components that work together.&#x20;
* We have users like Administrators that access the cluster to perform administrative tasks The developers that access to cluster to test or deploy applications we have end users who access the applications deployed on the cluster and we have third party applications accessing the cluster for integration purposes.&#x20;
* Throughout this section we will discuss how to secure our cluster by securing the communication between internal components and securing management access to the cluster through authentication and authorization mechanisms. In this lecture our focus is on securing access to the kubernetes cluster with authentication mechanisms.

<figure><img src="../.gitbook/assets/image (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

* &#x20;so we talked about the different users that may be accessing the cluster.
* &#x20;security of end users who access the applications deployed on the cluster is managed by the applications themselves internally. So we will take them out of our discussion.
* &#x20;Our focus is on users access to the kubernetes cluster for administrative purposes. So we are left with two types of users.&#x20;
* Humans, such as the Administrators and Developers and Robots such as other processes/services or applications that requires access to the cluster.&#x20;
* Kubernetes does not manage user accounts natively it relies on an external source like a file with user details or certificates or a third party identity service like LDAP to manage these users.
* And so you cannot create users in a kubernetes cluster or view the list of users like this.

<figure><img src="../.gitbook/assets/image (2) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

* However in case of Service Accounts kubernetes can manage them.&#x20;
* You can create and manage service accounts using the Kubernetes API. We have a section on service accounts exclusively where we discuss and practice more about service accounts.

<figure><img src="../.gitbook/assets/image (3) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

* For this section we will focus on users in kubernetes. All user access is managed by the API server. Weather you are accessing the cluster through kubectl tool or the API directly, all of these requests go through the kube api server. The kube-api server authenticates the requests before processing it.

<figure><img src="../.gitbook/assets/image (4) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

* So how does the kube-api server authenticate? There are different authentication mechanisms that can be configured.&#x20;
* You can have a list of username and passwords in a static password file, or usernames and tokens in a static token file or you can authenticate using certificates. And another option is to connect to third party authentication protocols like LDAP, Kerberos etc

<figure><img src="../.gitbook/assets/image (5) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

* We will look at some of these next let's start with static password and token files as it is the easiest to understand.
* Let's start with the simplest form of authentication. You can create a list of users and their passwords in a csv file and use that as the source for user information.&#x20;
* The file has three columns password user name and user I.D.. We then pass the file name as an option to the kube-api server.&#x20;
* Remember the kube-api server service and the various options we looked at earlier in this course. That is where you must specify this option. You must then restart the kube-api server for these options to take effect.

<figure><img src="../.gitbook/assets/image (6) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

* If you setup your cluster using the kubeadm tool, then you must modify the kube-apiserver POD definition file. Kube-adm tool will automatically restart the kube-api server once you update the file.

<figure><img src="../.gitbook/assets/image (7) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>



* To authenticate using the basic credentials while accessing the API server specify the user and password in a curl command like this.&#x20;

<figure><img src="../.gitbook/assets/image (8) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

* In the csv file with the user details that we saw we can optionally have a fourth column with the group details to assign users to specific groups similarly instead of a static password file.&#x20;
* You can have a static token file here instead of password you specify a token pass the token file as an option token-auth-file to the kube-api server.

<figure><img src="../.gitbook/assets/image (9) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

* &#x20;While authenticating specify the token as an Authorization bearer token to your requests like this.

<figure><img src="../.gitbook/assets/image (10) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>
