# Intrusion Detection in a contianerized or Kubernetes environment

  	    	Kris Nova

	    	nova@sysdig.com
		github.com/kris-nova/public-speaking

---

# About Me

## Author

		 - Cloud Native Infrastructure
		 - Technical writing / blogs / demos

## Contributor

		 - Kubernetes 
		    - Kops, Kubicorn, Kubeadm, Cluster API
		 - Go
		 - Linux, FreeBSD
		 - Created own OSS efforts (kubicorn, loggers, lolgopher, etc)
 		 - Falco / Sysdig

## Advocate

		- Kubernetes
 		- Security
 		- Hacking
 		- Intrusion and Anomoly detection
 		
## Work

        - SoldFire/NetApp
        - Deis/Microsoft
        - Heptio/VMware
        - SysDig/? 

---

# Cgroups

Control  groups, usually referred to as cgroups, are a Linux kernel feature which allow processes to be organized into hierarchical groups
       whose usage of various types of resources can then be limited and monitored.  The kernel's cgroup interface is provided through a  pseudo-
       filesystem called cgroupfs.  Grouping is implemented in the core cgroup kernel code, while resource tracking and limits are implemented in
       a set of per-resource-type subsystems (memory, CPU, and so on).

```
# Where the cgroup data is
cd /sys/fs/cgroup
```


---

# Namespaces

A  namespace  wraps  a  global system resource in an abstraction that makes it appear to the processes within the namespace that they have
       their own isolated instance of the global resource.  Changes to the global resource are visible to other processes that are members of the
       namespace, but are invisible to other processes.  One use of namespaces is to implement containers.

       Linux provides the following namespaces:

       Namespace   Constant          Isolates
       Cgroup      CLONE_NEWCGROUP   Cgroup root directory
       IPC         CLONE_NEWIPC      System V IPC, POSIX message queues
       Network     CLONE_NEWNET      Network devices, stacks, ports, etc.
       Mount       CLONE_NEWNS       Mount points
       PID         CLONE_NEWPID      Process IDs
       User        CLONE_NEWUSER     User and group IDs
       UTS         CLONE_NEWUTS      Hostname and NIS domain name

---

Example of using `clone(2)`

```bash
  // -----------------------------------------------------------------------------
  // clone() calls
  //
  //int pid = clone(fn, pchild_stack + (1024 * 1024), SIGCHLD, NULL); // Same Pid, Same Disk
  //
  //int pid = clone(fn, pchild_stack + (1024 * 1024), CLONE_NEWPID | SIGCHLD, NULL); // Different Pid, Same Disk
  //
  int pid = clone(fn, pchild_stack + (1024 * 1024), CLONE_NEWPID | CLONE_NEWNET | CLONE_NEWNS | SIGCHLD, NULL); // Different Pid, Different Disk
  //
  // -----------------------------------------------------------------------------
```

Working code: github.com/falcosecurity/falco-kubernetes-workshop

---

# Other namespace tools

As well as various /proc files described below, the namespaces API includes the following system calls:

       clone(2)
              The  clone(2)  system call creates a new process.  If the flags argument of the call specifies one or more of
              the CLONE_NEW* flags listed below, then new namespaces are created for each flag, and the  child  process  is
              made a member of those namespaces.  (This system call also implements a number of features unrelated to name‐
              spaces.)

       setns(2)
              The setns(2) system call allows the calling process to join an existing namespace.  The namespace to join  is
              specified via a file descriptor that refers to one of the /proc/[pid]/ns files described below.

       unshare(2)
              The  unshare(2)  system call moves the calling process to a new namespace.  If the flags argument of the call
              specifies one or more of the CLONE_NEW* flags listed below, then new namespaces are created  for  each  flag,
              and  the calling process is made a member of those namespaces.  (This system call also implements a number of
              features unrelated to namespaces.)

       ioctl(2)
              Various ioctl(2) operations can be used to discover information about namespaces.  These operations  are  de‐
              scribed in ioctl_ns(2).

---

# MultiCloud

 - Auditing at the kernel level 
 - Kernel Modules and eBPF give us flexibility
 - We audit the same REGARDLESS of cloud 
 - We will show a demo
 
# How we can detect
 
 1) Linux system
 2) Container runtime
 3) Kubernetes runtime
 
# Why this is relevant 

 - Auditing application
 - Auditing infrastructure
    - This is software now!
 - Auditing the kernel

---

# Intrusion Detection with Containers

 - Using Linux Namespaces to manipulate a system
 - Enabled by confusing abstractions with container runtimes and Kubernetes
 - Auditng the system
   - Falco
   - Sysdig 
   - Syscall stream
   
---

# Intrusion detection with Kubernetes

 - Welcome the API server
 - Welcome the audit server
 - Welcome management with Kubernetes 
 
# Falco on Kubernetes

 - Running as DaemonSet
 - ConfigMap management 
 - Operator sets it up
 
---
 
# Take Away

_"Auditing at the Kernel level liberates you from concerning yourself which cloud you are running on"_

- Kris Nova

# Demo

 


