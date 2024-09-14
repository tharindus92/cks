# Minimize IAM roles

<figure><img src="../.gitbook/assets/image (13).png" alt=""><figcaption></figcaption></figure>

* In this lecture, we will learn how to use the principle of least privilege when using identity and access management with public cloud platforms such as AWS.&#x20;
* In the previous lecture, we learned about the different accounts used in Linux. There's never a good idea to make use of the root user directly to carry out day-to-day operations.&#x20;
* Hence we learn how to harden the SSH access, prevent root users from logging in directly to the nodes, and make use of pseudo for running commands with administrative privileges when using normal user accounts.&#x20;

<figure><img src="../.gitbook/assets/image (1) (1).png" alt=""><figcaption></figcaption></figure>

* The root user account can be compared to the admin user in windows which has complete administrative privileges to carry out any task in the operating system.
* &#x20;In public cloud platforms such as AWS, it is equaling to the root account that is created when we sign up. When your AWS account is just set up, this is the only account that we can use to sign in to the management console.

<figure><img src="../.gitbook/assets/image (2) (1).png" alt=""><figcaption></figcaption></figure>

* We will use AWS as an example throughout this lecture. However, the concepts are generally applicable to most cloud platforms. Let us assume that a user called Mark with the email address, mark@example.com, creates a new AWS account.&#x20;
* After completing the signup process, Mark can now log into the AWS management console using the same credentials. This email address that was used to sign up to AWS is also known as a root account and it has complete administrative privileges.&#x20;
* Mark can now manage any service within AWS using this root account, but as we saw with the example of the root user account in Linux, this is not the recommended approach.

<figure><img src="../.gitbook/assets/image (3) (1).png" alt=""><figcaption></figcaption></figure>

&#x20;

* The root account can be used to create new users in AWS and assign them appropriate privileges. This should be the only purpose of using the root account.&#x20;
* Once the root users have been created, lock away the credential securely and do not use it unless it's absolutely necessary. To start off, let us creates a few new users called Lucy, Shiva, Abdul, and Anita.

<figure><img src="../.gitbook/assets/image (4) (1).png" alt=""><figcaption></figcaption></figure>

* As a standard, when a user is just created, cloud providers assign the least privileged permission to the user. What a user can or cannot do in AWS is decided by the permissions that are defined within IAM policies.&#x20;
* For example, let us consider that the users, Shiva, Abdul, and Anita are developers, and they need permission to create EC2 instances and be able to access the S3 buckets to develop their applications. To do this, we can attach policies to each user like this.&#x20;
* These policies should only allow the users with the bare minimum permissions to effectively develop their applications and deny access to everything else.

<figure><img src="../.gitbook/assets/image (5) (1).png" alt=""><figcaption></figcaption></figure>

* We can also group users with the same role into an IAM group. An IAM group is a collection of users which allows us to specify permissions to multiple users at once.&#x20;
* This way we can directly attach the same policies to the group rather than having to assign them at an individual level.

<figure><img src="../.gitbook/assets/image (6) (1).png" alt=""><figcaption></figcaption></figure>

* This allows the users which are part of the developer group permission to create EC2 instances and access S3 buckets.&#x20;
* The access provided so far has been for humans, people who want to carry out tasks either using the management console or programmatically by making use of access keys. What about other services?

<figure><img src="../.gitbook/assets/image (7) (1).png" alt=""><figcaption></figcaption></figure>

* Say an EC2 instance which should be able to access the S3 service. Would that work out of the box?&#x20;
* The answer is, no. Just like regular users, resources in AWS also do not have any permission to interact with other services by default.&#x20;
* They need to be granted access specifically. However, unlike regular users, we cannot attach an IAM policy to a service directly. Instead, in order to allow an EC2 instance to be able to interact with S3 buckets, we have to create a role. An IAM role allows us to securely grant access to an AWS service to act on other services in the AWS account.&#x20;
* In this example, we need to create a role, let's call it S3 access role, to be applied on EC2 service. In this example, we need to create a role, let's call it the S3 access role to be applied on the EC2 service. To the role, we attach the same IAM policy that we use for the developer group.&#x20;
* Once this role is attached to an EC2 instance, it now has access to the S3 service.
* Please note that there are other ways to give services such as EC2 access to other resources such as making use of programmatic access.&#x20;
* These can, however, be insecure. Wherever possible, always grant access to services using IAM roles. While creating the roles, adhere to the principle of least privilege and ensure that the services are granted the bare minimum privileges to do their job.

&#x20;

<figure><img src="../.gitbook/assets/image (8) (1).png" alt=""><figcaption></figcaption></figure>

* It is always a good idea to audit the policies and rules and refine them. For example, from time to time finding out if a permission within a policy is unused and removing them. Most popular cloud providers provide tools to do this out of the box.&#x20;
* For example, the AWS Trusted Advisor runs security checks in real-time and provides guidance on how to secure the systems based on the AWS best practices. Similarly, we have equaling tools such as the Security Command Centre in Google Cloud and the Azure Advisor.&#x20;
* Before we move on to the next lecture, it is worth pointing out that while we do not anticipate a question involving IAM for the actual exam, it is good to understand them at a high level.
