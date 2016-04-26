# UOS 401 - Inspection tools

### Inspection tools 
Gerard Braad — 吉拉德

gerard@unitedstack.com


## Continued operation
Continued operation of an OpenStack environment is essential to our customers.

However, there are cases in which systems can react in unexpected or unforeseen
ways.

The following tools described can help you in pinpointing issues.


## Divide them into categories
  * General tools
  * Disk
  * Network
  * Debug


## General tools
These are tools that are used for general inspection, such as 

  * Process monitoring
  * Service control


## ps
`ps - report process status`

The `ps` program displays the currently-running processes. A related Unix utility named `top` provides a 


## ps - Command line options
Note that "ps -aux" is different than "ps aux".

  * UNIX options, which may be grouped and must be preceded by a dash ("-")
  * BSD options, which may be grouped and must not be used with a dash
  * GNU long options, which are preceded by two dashes ("--")


## ps - Basic examples
By default `ps` selects all processes with the same user ID and associated with the same terminal as the caller.

```
$ ps
```

    PID TTY          TIME CMD
    3327 pts/1    00:00:00 bash
    3328 pts/1    00:00:00 bash
    5882 pts/1    00:00:00 ps


## ps - Basic examples

To see every process running on the system, using the standard syntax:

```
$ ps -e
```

```
$ ps -ef
```


This does

  * select all processes
  * full listing


## ps - Basic examples

To see every process on the system, using BSD syntax:

```
$ ps ax
```


This lifts the

  * "only yourself" restriction
  * "must have a tty" restriction

Display user-oriented format

```
$ ps aux
```


## ps - Select the process you want
Combine using a `grep`

```
$ ps aux  | grep ssh
```

Note: this will also return the `grep` command.  


## ps - syntax
Try different options and see what works best for you.

```
$ man ps
```

```
$ info ps
```


## ps - process tree
Process trees show the relationship between commands, by graphing how they are
started.


```
$ ps axjf
```

```
$ ps -ejH
```

Alternative command is `pstree`.


## Alternatives process
`top` (table of processes) is a task manager program. Several variants exist,
like `htop`.

Especially `htop` is very detailed. Showing CPU core usage, free memory, etc.



## Memory usage
To check if the system has enough memory available to run processes, you can
check this with.

```
$ free -g
```

to display as gigabytes.

To update by interval

```
$ free -s 5
```

If the system is out-of-memory, strange behavior can occur, such as failing
commands and unable to start new processes.


## Alternatives free memory
The proc filesystem is a special filesystem in Unix-like operating systems that 
presents information about processes and other system information in a
hierarchical file-like structure.

```
$ cat /proc/meminfo
```

See also: [/proc/meminfo](https://www.centos.org/docs/5/html/5.2/Deployment_Guide/s2-proc-meminfo.html)


## Alternatives free memory
`vmstat` (virtual memory statistics) is a computer system monitoring tool that
collects and displays summary information about operating system memory,
processes, interrupts, paging and block I/O. 

```
$ vmstat -s
```


## Service management
Processes on Linux/Unix are owned by the `init` process, which primary role is to
start processes according to a script from `/etc/inittab` or `/etc/init.d`

```
$ service
```

is responsible for running a System V init script.

To get an overview of all the services and if they are enabled

```
$ service --status-all
```

Restarting a service can be down with

```
$ service network restart
```


## Enable services
Services are started according to their location in the runlevel folders.

To disable/enable a service

```
$ chkconfig disable NetworkManager
$ chkconfig enable network
```

It links the scripts in `/etc/rc?.d` accordingly.


## Service management
`systemctl` is a linux command to control the systemd system and service manager.

To show all services you have to limit to showing the service unit.

```
$ systemctl list-units -t service --all
```

Note: omitting `--all` will only show active services.

To look at the details of a specific service

```
$ systemctl status sshd.service
```

You can enable and disable with

```
$ systemctl enable sshd.service
```


## Disk commands

  * basic commands
  * fdisk


## Disk free
`df - report file system disk space usage`

`df` (disk free) is a standard Unix command used to display the amount of available disk space for file systems on which the invoking user has appropriate read access.

```
$ df -h
```

makes the output human-readable.


## Disk usage
`du - estimate file space usage`

`du` (disk usage) is a standard Unix command used to estimate file space usage—space used under a particular directory or files on a file system.

```
$ du -h
```

makes the output human-readable.


## Top 10 disk usage
Often just a few files hog your disk, such as log files or a large disk image.

```
$ du -a /var | sort -n -r | head -n 10
```

Hunt for hogs with ducks

```
$ alias ducks='du -cks * | sort -rn | head'
```

  * -c produces a grand total
  * -k same as `block-size=1K`
  * -s summarize, total for each argument


## fdisk
Hard disks can be divided into one or more logical disks called partitions. This
division is described in the partition table found in sector 0 of the disk.

`fdisk` is a command-line utility that provides disk partitioning functions. It
doesn't understand GUID Partition Table (GPT) and it is not designed for large
partitions.


    Warning: Don’t delete, modify, or add partitions if you don’t know what you
    are doing. You will lose data!


## fdisk - Basic usage
With the following command you can list all partitions.

```
$ fdisk -l
```

To modify partitions you can use the following command.

```
$ fdisk [device]
```


## Alternatives fdisk

  * sfdisk
  * cfdisk
  * parted


## LVM
The Logical Volume Manager (LVM) provides logical volume management for the
Linux kernel.

  * Managing large hard disk farms by allowing disks to be added and replaced without downtime or service disruption, in combination with hot swapping.
  * On small systems (like a desktop at home), instead of having to estimate at installation time how big a partition might need to be in the future, LVM allows file systems to be easily resized later as needed.
  * Performing consistent backups by taking snapshots of the logical volumes.
  * Creating single logical volumes of multiple physical volumes or entire hard disks, allowing for dynamic volume resizing.




## iftop
iftop is a command-line system monitor tool that produces a frequently-updated list of network connections. By default, the connections are ordered by bandwidth usage, with only the "top" bandwidth …


## tcpdump
tcpdump is a common packet analyzer that runs under the command line. It allows the user to display TCP/IP and other packets being transmitted or received over a network to which the computer is attached. Distribut


## ip
iproute2 is a collection of userspace utilities for controlling and monitoring various aspects of networking in the Linux kernel, including routing, network interfaces, tunnels, traffic control, and network-related device drivers.


## brctl
brctl is used to set up, maintain, and inspect the ethernet bridge configuration in the linux kernel.
An ethernet bridge is a device commonly used to connect different networks of ethernets together, so that these ethernets will appear as one ethernet to the participants.


## arping
Arping is a computer software tool for discovering and probing hosts on a computer network. Arping probes hosts on the attached network link by sending Link Layer frames using the Address Resolution Protocol request method addressed to a host identified by its MAC address of the network interface. The utility program may use ARP to resolve an IP 


## ping
Echo Request message in Internet Control Message Protocol (ICMP)
Ping is a computer network administration software utility used to test the reachability of a host on an Internet Protocol (IP) network. It measures the round-trip time for messages sent from the originating host to a destination computer and echoed back to the source. The name comes from active sonar terminology that sends a pulse of sound and listens for the echo to detect objects under water.[1] It is sometimes interpreted as a backronym as packet Internet groper.[2]


## traceroute (tracepath)
In computing, traceroute is a computer network diagnostic tool for displaying the route and measuring transit delays of packets across an Internet Protocol network. The history of the route is




## strace
strace is a diagnostic, debugging and instructional userspace utility for Linux. It is used to monitor interactions between processes and the Linux kernel, which include system calls, signal deliveries, and changes of process state. The operation of strace is made possible by the kernel feature known as ptrace.

systemtap


## lsof
lsof is a command meaning "list open files", which is used in many Unix-like systems to report a list of all open files and the processes that opened them. This open source utility was developed and supported by Victor A. Abell, the retired Associate Director of the Purdue University Computing Center. It works in and supports several Unix flavors.


## gdb
GNU Debugger. The standard debugger for the GNU operating system.


## pdb
The module pdb defines an interactive source code debugger for Python programs. It supports setting (conditional) breakpoints and single stepping at the source line level, inspection of stack frames, source code listing, and evaluation of arbitrary Python code in the context of any stack frame.

These libraries help you with Python development: the debugger enables you to step through code, analyze stack frames and set breakpoints etc., and the profilers run code and give you a detailed breakdown of execution times, 