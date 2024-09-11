# CIS benchmark for Kubernetes



* Register at the CIS website and download the CIS benchmarks for Kubernetes. The latest one as of this recording provides best practices for Kubernetes versions 1.16 to 1.18, and is intended for system and application administrators, security specialists, auditors and others who plan to develop, deploy, assess, or secure solutions with Kubernetes.
* The document contains hundreds of recommendations that cover control plane components, as well as worker node components. A few examples shown here are the recommendations about master node configuration files, such as setting the right file permissions for the API server pod specification file to 644, so that only administrators on the system can write to the file.
* The document also provides details on how to check the current status of the file and the necessary commands to remediate the situation. Some of the other notable recommendations are the command line arguments set on the Kube-API server during deployment, such as making sure anonymous authorization is disabled, basic and token auth files are not set, HTTPS and certificates are configured, et cetera.
* We looked at the CIS-CAT tool in the previous lecture that can help us in running automated assessments and generate reports in HTML format.&#x20;
* The Lite version is a free version that only supports selected benchmarks such as Windows, Ubuntu, Google Chrome, and macOS, and does not include Kubernetes.
* &#x20;The CIS-CAT Pro version supports automated assessments for many more benchmarks and version 4 includes support for Kubernetes.



{% hint style="info" %}
Download the CIS benchmark PDF’s from the below link:

[https://www.cisecurity.org/cis-benchmarks/#kubernetes](https://www.cisecurity.org/cis-benchmarks/#kubernetes)

Go to the **\`Server Software\`** section and click on the **\`Virtualization\`**.&#x20;

There you will see multiple items that fall into the virtualization category such as VMware, docker and kubernetes. Now, move on to the Kubernetes and expand it to see more options then download CIS Kubernetes Benchmark version 1.6.0.

Download the CIS CAT tool’ from the below link:&#x20;

[https://www.cisecurity.org/cybersecurity-tools/cis-cat-pro/cis-benchmarks-supported-by-cis-cat-pro/](https://www.cisecurity.org/cybersecurity-tools/cis-cat-pro/cis-benchmarks-supported-by-cis-cat-pro/)

Download the CIS CAT Lite tool from the below link:

[https://learn.cisecurity.org/cis-cat-lite](https://learn.cisecurity.org/cis-cat-lite)
{% endhint %}
