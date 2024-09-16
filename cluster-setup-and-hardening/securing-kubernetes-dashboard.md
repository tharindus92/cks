# Securing Kubernetes Dashboard

<figure><img src="../.gitbook/assets/image (4) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

* In this lecture, we look at the different authentication mechanisms available for the Kubernetes Dashboard. When you access the Kubernetes Dashboard, you get two options to login, one with a token and another, with a kubeconfig file. To login with a token, you must create a user and give the user the necessary permissions using role based access control which we have discussed earlier in this course.&#x20;
* The Kubernetes Dashboard documentation page has instructions on creating a sample user. However, note that these instructions provide cluster admin level access to the users, so, you should be extremely cautious when performing this operation and only give the user the necessary level of permissions.

<figure><img src="../.gitbook/assets/image (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

* That could be limited to a particular namespace or specific resources within a namespace. These can be configured while creating roles and role bindings.&#x20;
* Once the user and necessary role bindings are created, inspect the secret created that holds the token and this is a token that can be used to login to the dashboard interface.&#x20;
* Alternatively, you can pass a kubeconfig file that has the required credentials to access the dashboard. In the upcoming labs, you will walkthrough how to access the Kubernetes Dashboard, you'll create users, roles and role bindings and then use the secret token to authenticate into the dashboard.
