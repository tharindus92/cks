# Verify platform binaries before deploying

<figure><img src="../.gitbook/assets/image (125).png" alt=""><figcaption></figcaption></figure>

* In this lecture, we'll look at how to verify platform binaries before deploying a cluster. The Kubernetes platform binaries are available at the Kubernetes GitHub release page. It is important to verify that the downloaded binaries are safe to use by comparing against the checksum hash found against each binary within this page.

<figure><img src="../.gitbook/assets/image (126).png" alt=""><figcaption></figcaption></figure>

* These binaries are served on the internet. When you download the binaries, they are downloaded from the hosted servers to your system. Now, it is possible that an attacker, who may have gained access to either your network or somewhere in between, can intercept your download request and return his or her own version of the file, which may contain malicious code. This may go unnoticed if the downloaded file is not verified against the checksum.

<figure><img src="../.gitbook/assets/image (127).png" alt=""><figcaption></figcaption></figure>

* Each file has a unique checksum or hash, and the smallest change in the file will generate a completely different hash. It is important that all downloaded files are verified against the hash available on the download page.

<figure><img src="../.gitbook/assets/image (128).png" alt=""><figcaption></figcaption></figure>

* Here, we run the curl command to download the Kubernetes binary, and then we use the shasum command line utility to generate our own hash of the downloaded file. We then verify the output of our command against the one that is available in the release page, and we make sure that they are exactly the same. In this case, we see that it's exactly the same.

<figure><img src="../.gitbook/assets/image (129).png" alt=""><figcaption></figcaption></figure>

* Note that the commands are different on different systems. On macOS, use the shasum command with the -a option to specify 512 or 256 bits. On Linux, use the sha512sum command to generate a 512-bit hash of the file. In the upcoming labs, you will walk through how to download and verify Kubernetes binaries using these commands.



