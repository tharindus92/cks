# Remove Obsolete Packages and Services

<figure><img src="../.gitbook/assets/image (6) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

* Let us look at the software running on our nodes. It is always a good idea to keep the systems as lean as possible by making sure that only the required software is installed, and the ones that are installed are constantly updated to address security fixes.
* Now, this general rule is not just applicable for Kubernetes specifically but security as a whole.
* Ask yourself this, is apache really needed on the nodes of your Kubernetes cluster? Perhaps the operating system was installed on the node from a snapshot or perhaps from an image template that contains this additional software.
* In either case, software that is not needed should not be installed on the node. For one, you may never end up using it. Secondly, it also increases the complexity of the system with more moving parts that need to be maintained constantly.
* Besides this, new vulnerabilities are discovered every day. It's equally important to make sure that existing packages are updated in a timely manner.

<figure><img src="../.gitbook/assets/image (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

* Next, let's look at services. Services are used to start applications when the Linux system boots. In most modern-day Linux distributions, services are managed by the systemd process.
* The systemctl tool allows us to manage the systemd services, such as giving status, starting and stopping services, et cetera.
* For example, to check the status of the apache service running on the system, we can run the command systemctl status apache2.
* Here, we can see that the apache service is currently active and running in the system. The service configuration file is located at /lib/systemd/system/apache2.service. Most server's configuration files are also installed when the package is installed.
* However, we can also install services manually to start a process on the system.

<figure><img src="../.gitbook/assets/image (2) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

* Just like we did with software packages, it is essential that we identify and run only the services that are actually needed on the system.
* To see a list of all services installed in the system, run the command systemctl list units with the flag type with the value of service.
* If a service file is not needed, stop and disable the service using the systemctl command like this.

<figure><img src="../.gitbook/assets/image (3) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

* Now that the service has been stopped and disabled, to remove the apache package from the system, we can use the apt remote command like this.

<figure><img src="../.gitbook/assets/image (4) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

* Before we move on, you can check the section two of the CIS benchmarks for distribution independent Linux for additional reference. It provides recommendations about best practices to be followed while configuring services.
