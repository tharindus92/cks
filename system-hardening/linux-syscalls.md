# Linux Syscalls

<figure><img src="../.gitbook/assets/image.png" alt=""><figcaption></figcaption></figure>

* In this lecture, we will learn about syscalls or system calls, and understand what happens under the hood when an application or a process runs.&#x20;
* Let’s start by taking a quick look at how an application or a process runs in Linux. To do that, let us first have a quick look at the fundamental concepts of the Linux operating system.
* &#x20;A kernel is the major component of an operating system, and it is the core interface between a computer’s hardware and its processes. It communicates between the two, managing resources as efficiently as possible.

<figure><img src="../.gitbook/assets/image (1).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (2).png" alt=""><figcaption></figcaption></figure>

* The kernel can be divided into two memory areas, known as the kernel space and the user space. Applications such as those written in C, Java, biotin, for example, and processes run by a user, are run inside the user space. The kernel itself runs inside the kernel space, which includes the kernel code, kernel extensions and device drivers.
* Let us now look at how programs running in the user space work. Say for example, an application wants to open and write data into a file, which is stored in memory or disk.

<figure><img src="../.gitbook/assets/image (3).png" alt=""><figcaption></figcaption></figure>

* Applications running in user space and get access to data on devices by making special requests to the kernel called system calls.

<figure><img src="../.gitbook/assets/image (4).png" alt=""><figcaption></figcaption></figure>

* For example, if you want to create an empty file called error.log in the /tmp file system, it will make several system calls.
* One of them is the execve system cal that executes the touch binary. A few other common examples of other system calls are, open, close, re-directory, string length, close directory, et cetera.

<figure><img src="../.gitbook/assets/image (5).png" alt=""><figcaption></figcaption></figure>

* Now, let’s take a look at some of the common ways we can use to trace syscalls used by a process. The first command that we are going to check out is the strace command. The strace utility is installed in most Linux distributions by default, and it is useful to trace the system calls used by an application, and the signals written back to it.&#x20;
* For example, to inspect the syscalls used when we create a file in the slash ten file system, simply add the strace command before the touch command. This command will display a lot of details in its output.

<figure><img src="../.gitbook/assets/image (6).png" alt=""><figcaption></figcaption></figure>

* Let us now inspect the first line of this result and understand what it means. Each line in this output provides information about the syscall name, the argument’s passed to the syscall, and the written status.&#x20;
* For example, in the first line execve is the name of the syscall used to execute a program with a specified array of arguments. The first argument here is the absolute part to the program that we executed, which in our case is the touch command.
* &#x20;This was followed by an array of strengths that are arguments, which are passed to the touch command, which in this case, as the file/temp/error.log. The 23 vars implies that 23 variables were inherited by the system call. We can verify this by checking the number of environment variables defined in the user shell by running the ENV command like this.

<figure><img src="../.gitbook/assets/image (7).png" alt=""><figcaption></figcaption></figure>

* To trace the syscalls use by a running process, we first need to determine the PID of the process. For example, to check the PID of the etcd process, use the pidof command with the etcd as the argument. Now, use the PID return by this command with the strace-p to attach the process.
* This command will now return all the future syscalls made by etcd. Once you’re done use the control+C to exit or detached from this command.

<figure><img src="../.gitbook/assets/image (8).png" alt=""><figcaption></figcaption></figure>

* This is just one of the syscalls used by this simple command. If you want to see a summary of all the syscalls used by the command, use the dash C flag like this. As you can see a simple command like touch can make use of multiple syscalls. An application, on the other hand, which would run several sub processes under the hood, can make use of hundreds and thousands of syscalls per second to the Linux kernel.
