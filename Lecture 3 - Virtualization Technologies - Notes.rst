
Warm-up
=======

Let's rehearse on the previous lectures...

What is cloud computing?
------------------------

Cloud computing is a model for enabling ubiquitous, convenient, on-demand network access
to a shared pool of configurable computing resources (e.g., networks, servers, storage, applications,
and services) that can be rapidly provisioned and released with minimal management effort or
service provider interaction.

Cloud traits
------------

* On-demand self-service
  – Focuses on delivering IT services driven by user requests
  – No human interaction with the cloud provider
  – Cloud computing provides a means of delivering computing services that makes the underlying
    technology, beyond the user device, almost invisible

* Broad network access
  – Focuses on delivering IT services anytime, anywhere, and through user-chosen devices

* Resource pooling
  - Computing resources merged into pools for better utilization

* Rapid elasticity
  – Resources can be dynamically allocated and contracted based on the
    requirements of the underlying workload and the usage characteristics

* Measured service
  – Focuses on delivering IT services that can be metered for usage and charged for (if needed) through
    pricing models including subscription, usage pricing – Service level agreements (SLAs)

Cloud service models
--------------------

* Software as a Service (SaaS)
* Platform as a Service (PaaS)
* Infrastructure as a Service (IaaS)

Cloud deployment models
-----------------------

* Public Cloud
* Private Cloud
* Hybrid Cloud
* Community Cloud

The history of virtualization
=============================

How old is virtualization?
--------------------------

Half a century!

The most successful computer of the time, S/360 mainframe system, did not
provide virtual memory and privilege separation at the CPU level. The
mainstream computing model of the time has been about non-interactive,
batched jobs.

The concepts of virtualization have not been researched and tried until late
sixties within the CP-40 project that eventually resulted in the first real
and full virtualization support which appeared in IBM S/370-67 in 1966.

The major driving factor behind time-sharing system development was that,
with batch processing, waiting for I/O was inefficient and program had to
wait for the queue before the programmer can get a failure.

Interestingly, it is the rise of the time-sharing concept that gave birth to
the computer security concerns.

Although the time-sharing technology was lost out to batch-processing
architecture in political battle of the time.

In in PC world, the motivation for time-sharing features was not
significant because PC CPUs were cheap enough to allocate to a single
person.

Many companies were coming up with desktop products offering virtualization
features. But they were not hugely practical (due to complexity and low
performance).

However the rise of the Internet brought the emergence of huge farms of
servers running applications like web sites. The load pattern of those
was apparently similarly bursty as with mainframes and terminals.
So running many different applications in parallel on the same system
made more sense again.

Around 2005 when PC CPU vendors introduced new CPU instructions supporting
virtualization - the Intel VT-x and AMD-V CPUs.

By this moment practical virtualization has become possible.

What exactly is virtualization?
===============================

Now, some people may feel a bit confused when it comes to the terminology
surrounding virtualization. Let's figure out what is what:

Multi-threading: execution environment allows each process to run multiple code
flows in parallel.

Multi-tasking: OS gives each task (process) the impression that it is the only
one running on the system and has full access to the system resources
(memory, I/O). The crucial HW component for VM to work is MMU.

Fun fact: at the batch-processing times, the alternative to multi-tasking was
multi-programming -- the dynamic queue of tasks that get off the CPU when
being blocked on I/O.

Virtualization: each instance of the OS has the impression that it is the
only OS running on the CPU/system and have full access to the system
resources (e.g. each OS thinks that it has its own CPUs, memory, IO).

Containers: give a set of tasks, the application, the impression that it is
the only one running within the OS. Yet, containers share the same OS kernel
instance.

Not directly relevant to the topic of virtualization, but the other
place where parallelism may be present is the CPU. With the advancements
in the manufacturing processes, we now have multiple CPUs built into
a single chip (multi-core CPU).

The other trick is to virtualize the CPU itself - the hyper-threading
technology presents multiple virtual CPU on top of a single physical
CPU.

How did virtualization work before 2005?
========================================

In fact, with Intel 80386+ CPUs virtualization was possible based on various
tricks. Like putting hypervisor at ring 0, VMs at ring 1 and apps at ring 2.
Though this was complicated from implementaton standpoint, has significant
limitations and it worked slowly.

Virtualization has become practical with better CPU support for VM
management which appeared in 2005 (PC CPUs).

Is it cloud already?
====================

Once virtualization is in place, what it makes to become a cloud?

To provide cloud services we need:

- hypervisor(s) to control the lifecycle of the virtual machines
- higher-level virtualization management infrastructure and additional services

So why the thing is called "hypervisor"? It would probably be "supervisor"
if the term was not already allocated for the operating system (which
supervises the resources and the tasks). Thus, hypervisor which supervises
tenant operating systems.

Hypervisors
===========

A hypervisor is a software that creates and runs virtual machines.

A computer on which a hypervisor runs one or more virtual machines is called
a host machine, and each virtual machine is called a guest machine.

There exists two types of hypervisors:

* Type 1 or bare-metal or native
* Type 2 or hosted hypervisors

Native hypervisors
------------------

These hypervisors run directly on the host's hardware to control the hardware
and to manage guest operating systems.

The first hypervisors, which IBM developed in the 1960s, were native
hypervisors. Modern native hypervisors include: Xen, Oracle VM Server,
Microsoft Hyper-V and VMware ESX/ESXi.

Hosted hypervisors
------------------

These hypervisors run on a conventional OS just as other computer programs
do. A guest operating system runs as a process on the host. Type-2
hypervisors abstract guest operating systems from the host operating system
and vice versa.

Modern hosted hypervisors include:

* VMware
* VirtualBox
* Parallels Desktop
* QEMU

The distinction between these two types is not necessarily clear.

Linux's KVM and FreeBSD's bhyve are kernel modules that effectively convert
the host OS to a type-1 hypervisor.

At the same time, since Linux and FreeBSD are still general-purpose operating
systems, with other applications competing for VM resources, KVM and bhyve
can also be categorized as type-2 hypervisors.

Native vs hosted confusion
--------------------------

To add more confusion, the hypervisers sub-divide onto so-called full
virtualization and para-virtualization capabilities.

The latter involves modifying guest OS to call hypervisor's services
explicitly instead of letting the hypervisor emulate hardware interfaces to
the quest OS. Para-virtualization used to have more sense at the times when
hardware support for virtualization has not been fully implemented.

Why it's called Hypervisor?
===========================

The term hypervisor refers to the situation when you have a supervisor which
controls the kernel of an OS. Historically, OS kernel is also called
supervisor (controlling user applications). Thus - hypervisor.

Virtualization management
=========================

So far we end up having a way to invoke VMs on a host system. Trouble is that:

* Besides just firing up a VM users might need to deploy OS, configure
  networking, storage etc
* There are many different hypervisors around, users want a single UI to
  them

The libvirt project addresses the latter problem - the system offers a daemon
that manages guests, user-facing CLI tool to control the libvirtd daemon and
the API to let other programs manage guests.

The oVirt software addresses the problem at the data center level. It offers
a collection of virtual services normally present at the data center such as:

* [virtual] machines that are the basis of the compute nodes
* storage nodes
* networking

The user-facing GUI models a virtual data center where user can
point-and-click to build their computing infrastructure.

The OpenStack project offers similar services as oVirt, but at a way larger
scale, flexibility and extensibility. With OpenStack one can spawn hundreds
thousands of VMs scattered across the globe.

OpenStack is designed as an open-ended collection of web-services interacting
with each other to implement the workflow of VM lifecycle.

We will look into OpenStack at depth down this course.

Cloud features
==============

The virtual machines can be created different (in part of their "hardware"
capabilities) to reflect OS/application requirements on the hardware.

It is very easy to unroll a new machine and install OS onto it. It can be
done fully remotely and without human intervention.

Moreover, the virtual machines can be copied (or cloned) thus creating virtual
computers that are exactly the same, from their capabilities perspective as
well as the OS and apps running inside.

Once you have your virtual machine at rest, you can easily back it up
entirely. It can be just an application backup or it can also include the
entire memory and the state of the whole virtual machine. That can be used
for live migration of a running virtual machine.

That paves the ground for scaling up/down the computing resources at the
runtime, moving virtual machines across the data centers or geographical
locations.

The OS running inside virtual machine does not normally see the real
hardware of the host computer. Instead it is presented with some generic
virtual hardware which is mapped to the real hardware so the API of the
virtual hardware never changes. That makes it easier from OS maintenance
perspective as well as it easies the migration of the virtual machines
from one hardware to another.

If you are in software development or testing, the ability to make a copy of
potentially complicated gold-standard environment to re-use it later could
be a very powerful feature.
