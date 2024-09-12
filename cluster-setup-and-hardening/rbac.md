# RBAC

<figure><img src="../.gitbook/assets/image (108).png" alt=""><figcaption></figcaption></figure>

* So how do we create a role? We do that by creating a role object.&#x20;
* So, we create a role definition file with the&#x20;
  * API version set to our rback.authorization.k8s.io/V1 and
  * &#x20;kind said to role,&#x20;
  * we name the role developer as we are creating this role for developers,&#x20;
  * and then we specify rules. Each rule has three sections: API groups, resources, and works.&#x20;
  * The same things that we talked about in one of the previous lectures.&#x20;
  * For Core group you can leave the API group section as blank. For any other group, you specify the group name.&#x20;
  * The resources that we want to give developers access to are pods.&#x20;
  * The actions that they can take are list, get, create, and delete.&#x20;
  * Similarly, to allow the developers to create conflict maps, we add another rule to create config map. You can add multiple rules for a single role like this. Create the role using the cube control create roll command.

<figure><img src="../.gitbook/assets/image (109).png" alt=""><figcaption></figcaption></figure>

* The next step is to link the user to that role.&#x20;
* For this, we create another object called roll binding. The roll binding object links a user object to a role.&#x20;
* We will name it dev-user-to-developer-inding.&#x20;
* The kind is RoleBinding.&#x20;
* It has two sections. The subjects is where we specify the user details.&#x20;
* The rollRef section is where we provide the details of the roll we created.&#x20;
* &#x20;Also note that the roles and role bindings fall under the scope of name spaces.
* &#x20;So here the dev user gets access to pod and config maps within the default name space.
* &#x20;If you want to limit the dev user's access within a different name space then specify the name space within the metadata of the definition file while creating them.

```sh
kubectl get roles
kubectl get rolebindings
kubectl describe role developer
kubectl describe rolebinding devuser-developer-binding

kubectl auth can-i create deployments
kubectl auth can-i create deployments --as dev-user
kubectl auth can-i create pods --as dev-user
kubectl auth can-i create pods --as dev-user --namespace test


kubectl create role developer --namespace=default --verb=list,create,delete --resource=pods
kubectl create rolebinding dev-user-binding --namespace=default --role=developer --user=dev-user

kind: Role
apiVersion: rbac.authorization.k8s.io/v1
metadata:
  namespace: default
  name: developer
rules:
- apiGroups: [""]
  resources: ["pods"]
  verbs: ["list", "create","delete"]

---
kind: RoleBinding
apiVersion: rbac.authorization.k8s.io/v1
metadata:
  name: dev-user-binding
subjects:
- kind: User
  name: dev-user
  apiGroup: rbac.authorization.k8s.io
roleRef:
  kind: Role
  name: developer
  apiGroup: rbac.authorization.k8s.io
  
  
  kubectl edit role developer -n blue
  
```

<figure><img src="../.gitbook/assets/image (110).png" alt=""><figcaption></figcaption></figure>

* Well, a quick note on resource names.&#x20;
* We just saw how you can provide access to users for resources like pods within the name space. You can go one level down and allow access to specific resources alone.&#x20;
* For example, say you have five parts in namespace you wanna give access to a user to pods, but not all pods. You can restrict access to the blue and orange pod alone by adding a resource names field to the rule.



{% hint style="info" %}
`// get all api groups,resorces,verbs`

`kubectl api-resources -o wide`
{% endhint %}
