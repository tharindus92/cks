# Certificate Workflow & API

<figure><img src="../.gitbook/assets/image (75).png" alt=""><figcaption></figcaption></figure>

* I, as an administrator of the cluster, in the process of setting up the whole cluster, have set up a CA server and a bunch of certificates for various components.
* We then started the services using the right certificates and is all up and working. I'm the only administrator and user of the cluster, and I have my own admin certificate and key.
* A new admin comes into my team. She needs access to the cluster. We need to get her a pair of certificate and key pair for her to access the cluster. She creates her own private key, generates a certificate signing request, and sends it to me.
* Since I'm the only admin, I then take the certificate signing request to my CA server, gets it signed by the CA server using the CA server's private key and root certificate, thereby generating a certificate and then sends the certificate back to her.
* She now has her own valid pair of certificate and key that she can use to access the cluster. The certificates have a validity period. It ends after a period of time. Every time it expires, we follow the same process of generating a new CSR and getting it signed by the CA, so we keep rotating the certificate files.
* So we keep talking about the CA server. What is the CA server and where is it located in the Kubernetes setup? The CA is really just a pair of key and certificate files we have generated. Whoever gains access to these pair of files can sign any certificate for the Kubernetes environment.
* They can create as many users as they want with whatever privileges they want. So these files need to be protected and stored in a safe environment.
* Say we place them on a server that is fully secure. Now that server becomes your CA server. The certificate key file is safely stored in that server and only on that server. Every time you want to sign a certificate, you can only do it by logging into that server.

<figure><img src="../.gitbook/assets/image (76).png" alt=""><figcaption></figcaption></figure>

* As of now, we have the certificates placed on the Kubernetes master node itself. So the master node is also our CA server. The Kubeadm tool does the same thing. It creates a CA pair of files and stores that on the master node itself.
* So far, we have been signing requests manually, but as and when the users increase and your team grows, you need a better automated way to manage the certificate signing requests, as well as to rotate certificates when they expire.

<figure><img src="../.gitbook/assets/image (77).png" alt=""><figcaption></figcaption></figure>

* Kubernetes has a built-in certificates API that can do this for you. With the certificates API, you now send a certificate signing request directly to Kubernetes through an API call.
* This time, when the administrator receives a certificate signing request, instead of logging onto the master node and signing the certificate by himself, he creates a Kubernetes API object called CertificateSigningRequest.

<figure><img src="../.gitbook/assets/image (78).png" alt=""><figcaption></figcaption></figure>

* Once the object is created, all certificate signing requests can be seen by administrators of the cluster. The request can be reviewed and approved easily using kubectl commands.
* This certificate can then be extracted and shared with the user

<figure><img src="../.gitbook/assets/image (79).png" alt=""><figcaption></figcaption></figure>

* A user first creates a key, then generates a certificate signing request using the key with her name on it, then sends the request to the administrator.

<figure><img src="../.gitbook/assets/image (80).png" alt=""><figcaption></figcaption></figure>

* The administrator takes the key and creates a CertificateSigningRequest object. The CertificateSigningRequest object is created like any other Kubernetes object, using a manifest file with the usual fields.
* The kind is CertificateSigningRequest. Under the spec section... The request field is where you specify the certificate signing request sent by the user, but you don't specify it as plaintext. Instead, it must be encoded using the Base64 command.

<figure><img src="../.gitbook/assets/image (81).png" alt=""><figcaption></figcaption></figure>

* Then move the encoded text into the request field and then submit the request. Once the object is created, all certificate signing requests can be seen by administrators by running the kubectl get csr command.
* Then move the encoded text into the request field and then submit the request. Once the object is created, all certificate signing requests can be seen by administrators by running the kubectl get csr command.
* Identify the new request and approve the request by running the kubectl certificate approve command.

<figure><img src="../.gitbook/assets/image (82).png" alt=""><figcaption></figcaption></figure>

* Kubernetes signs the certificate using the CA key pairs and generates a certificate for the user. This certificate can then be extracted and shared with the user.
* View the certificate by viewing it in a YAML format. The generated certificate is part of the output, but as before, it is in a Base64 encoded format. To decode it, take the text and use the Base64 utility's decode option. This gives the certificate in a plaintext format.
* This can then be shared with the end user. Now that we have seen how it works, let's see who does all of this for us.

<figure><img src="../.gitbook/assets/image (83).png" alt=""><figcaption></figcaption></figure>

* All the certificate-related operations are carried out by the controller manager.
* If you look closely at the controller manager, you will see that it has controllers in it called as CSR-Approving, CSR-Signing, et cetera.
* They're responsible for carrying out these specific tasks. We know that if anyone has to sign certificates, they need the CA server's root certificate and private key.

<figure><img src="../.gitbook/assets/image (84).png" alt=""><figcaption></figcaption></figure>

* The Controller Manager service configuration has two options where you can specify these.

<figure><img src="../.gitbook/assets/image (85).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (86).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (87).png" alt=""><figcaption></figcaption></figure>
