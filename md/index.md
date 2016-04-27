# UOS 401 - Inspection and maintenance tools

### Inspection and maintenance tools 
Gerard Braad

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
Note that `ps -aux` is different than `ps aux`.

  * UNIX options, which may be grouped and must be preceded by a dash (`-`)
  * BSD options, which may be grouped and must not be used with a dash
  * GNU long options, which are preceded by two dashes (`--`)


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

  * `-c` produces a grand total
  * `-k` same as `block-size=1K`
  * `-s` summarize, total for each argument


## Disk

  * preparation tools
  * mapping
  * consistency check


## fdisk
Hard disks can be divided into one or more logical disks called partitions. This
division is described in the partition table found in sector 0 of the disk.

`fdisk` is a command-line utility that provides disk partitioning functions. It
doesn't understand GUID Partition Table (GPT) and it is not designed for large
partitions.


    Warning: Don’t delete, modify, or add partitions unless you know what you
    are doing. There is a risk of data loss!


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

Note: avoid running different commands while performing edits as they can have
differences in the way they write/store the data.


## List partitions
If you only need to list partitions, just doing

```
$ cat /proc/partitions
```

would probably be enough. This is also what we will be using in the remainder
of the slides when listing partitions.


## LVM
The Logical Volume Manager (LVM) provides logical volume management for the
Linux kernel.

It is implemented as a device mapper target.


## LVM usecases
  * Managing large hard disk farms by allowing disks to be added and replaced
    without downtime or service disruption, in combination with hot swapping.
  * On small systems (like a desktop at home), instead of having to estimate at
    installation time how big a partition might need to be in the future, LVM
    allows file systems to be easily resized later as needed.
  * Performing consistent backups by taking snapshots of the logical volumes.
  * Creating single logical volumes of multiple physical volumes or entire hard
    disks, allowing for dynamic volume resizing.


## Basic usage
Volume groups (VGs) can be resized online by absorbing new physical volumes
(PVs) or ejecting existing ones.

Logical volumes (LVs) can be resized online by concatenating extents onto them
or truncating extents from them.
LVs can be moved between PVs.


## Scenario

  * four SATA disks, each 250G
  * to be dedicated for storage
  * separated in fileshare (450G), database (50G), backup (500G)

Note: this is just an example scenario. It is not suggested to deploy this
layout in real as it provides no form of redundancy.


## Partition the disks
Using `fdisk` we will create a *n*ew *p*rimary partition of type '8e'. We do
this for each disk.

```
$ fdisk /dev/sdb
$ fdisk /dev/sdc
...
```


Note: we do not have to create partitions, but this is often what happens.


## Create physical volumes
And create the Physical Volumes for the partitions we just created

```
$ pvcreate /dev/sdb1 /dev/sdc1 /dev/sdd1 /dev/sde1
```

You can check the creation of the physical volumes using `pvdisplay`.

Another way to learn about the physical volumes is to issue a `pvscan`.


Note: `pvremove /dev/sdb1` will perform the opposite action.


## Create Volume Group
After this is done, we will create the Volume Group

```
$ vgcreate storage /dev/sd[b-e]1
```

You can check the creation of the volume group using `vgdisplay`.

Another way to learn about the volume groups is to issue a `vgscan`.


## Create Logical Volumes
Now we can create the actual logical volumes that represent the actual storage
locations.

```
$ lvcreate -n fileshare -L +450G storage
$ lvcreate -n database -L +50G storage
$ lvcreate -n backup -L +500G storage
```

You can check the creation of the logical volumes using `lvdisplay`.

Another way to learn about the logical volumes is to issue a `lvscan`.


You will see devices mappings such as `/dev/storage/fileshare`, etc.


## Modify Logical Volumes
Logical volumes can be modified using the following commands

  * `lvremove`
  * `lvreduce`
  * `lvextend`


## Create filesystems
Logical volumes are just mapped devices and need to be provisioned with a
filesystem. For example, you could use Ext3

```
$ mkfs.ext4 /dev/storage/fileshare
$ mkfs.ext4 /dev/storage/database
$ mkfs.ext4 /dev/storage/backup
```

After which we can mount our volumes

```
$ mkdir /media/fileshare /media/database /media/backup
$ mount /dev/storage/fileshare /media/fileshare
$ mount /dev/storage/database /media/database
$ mount /dev/storage/backup /media/backup
```


## Now what
You can inspect if the storage is available using `df -h`. But also make sure
you create the entries for these disks are in `/etc/fstab`.

    /dev/storage/fileshare  /media/fileshare  ext4  defaults  0 0
    /dev/storage/database   /media/database   ext4  defaults  0 0
    /dev/storage/backup     /media/backup     ext4  defaults  0 0


## Changing a disk
For instance, a disk is not working as expected and needs to be removed.

Prepare the disk
```
$ pvcreate /dev/sdf1
```

Extend the volume group with the new disk and move data over
```
$ vgextend storage /dev/sdf1
$ pvmove /dev/sdb1 /dev/sdf1
```

And remove the defective disk from the volume group
```
$ vgreduce storage /dev/sdb1
$ pvremove /dev/sdb1
```

after which the physical disk can be removed.


## Unrecoverable disk
A disk has failed and a backup is not available. The data has to be considered
lost, but we want to recover the data in the remaining disks. 

Create the LVM meta data on the new disk using the old disk's UUID that
`pvscan` displays.

```
$ pvcreate --uuid 42oKek-5zLS-ckbD-e7vJ-gB42-YyQY-hwTRSu /dev/sdd1
```

Now the volume group can be restored

```
$ vgcfgrestore storage
$ vgscan
$ vgchange -ay storage
```

after which the volume group should be available. Note: run a filesystem
consistency check, such as `fsck.ext4`.


## Corrupted LVM meta data
This should not happen often, but when it happens the Logical Volumes should
also be considered as unreliable.

```
$ vgchange -ay storage
```

Will in this case it might show `Checksum error` and 

    Couldn't read volume group metadata.
    Volume group storage metadata is inconsistent
    
The volume group will not activate.


## Continued
If the disks are still inserted it would be possible to restore the metadata.
Check if the physical volumes can still be found in the metadata.

```
$ pvscan
```

If so, it would be possible to restore the volume group

```
$ vgcfgrestore storage
$ vgchange -ay storage
```

Now the volumegroup should be available again.


## Missing disk
If the `pvscan` does not find the disk.

    Couldn't find device with uuid '42oKek-5zLS-ckbD-e7vJ-gB42-YyQY-hwTRSu'.

It is possible to replace the disk and recreate it using the same UUID. The
solution is similar to replacing the disk as detailed earlier.

```
$ pvcreate --uuid 42oKek-5zLS-ckbD-e7vJ-gB42-YyQY-hwTRSu /dev/sdd1
$ vgcfgrestore storage
$ vgscan
$ vgchange -ay storage
```

After which you need to run a consistency check against the volume.


## Network
Connectivity


## ifcfg-ethX
On CentOS / RHEL and Fedora systems you can configure networm interfaces using
a network configuration file.

```
$ vi /etc/sysconfig/network-scripts/ifcfg-eth0
```

Be sure to not use `NetworkManager` in our case, and use `network`. You can do
this with:

```
$ systemctl disable NetworkManager
$ systemctl enable network
$ systemctl stop NetworkManager
$ systemctl start network
```

Verify this using

```
$ nmcli dev status
```


## iproute2
`iproute2` is a collection of userspace utilities for controlling and monitoring
various aspects of networking in the Linux kernel, including routing, network
interfaces, tunnels, traffic control, and network-related device drivers.

"Most network configuration manuals still refer to ifconfig and route as the
primary network configuration tools, but ifconfig is known to behave
inadequately in modern network environments." - [Source](http://www.linuxfoundation.org/collaborate/workgroups/networking/iproute2)


## Utilities provided by iproute2

| Legacy utility | Obsoleted by	             | Note                            |
|----------------|---------------------------|---------------------------------|
| `ifconfig`     | `ip addr`, `ip link`, `ip -s` | Address and link configuration|
| `route`        | `ip route`                | Routing tables                  |
| `arp`	         | `ip neigh`                | Neighbors                       |
| `iptunnel`     | `ip tunnel`               | Tunnels                         |
| `nameif`       | `ifrename`                | Rename network interfaces       |
| `ipmaddr`      | `ip maddr`                | Multicast                       |
| `netstat`      | `ip -s`, `ss`, `ip route` | Show various networking statistics|


Check `man ip` for more information about the possibilities.


## IP address
To check the IP address given to an interface, use the command

```
$ ip addr
```

or 

```
$ ip a
```

    1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN
        link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
        inet 127.0.0.1/8 scope host lo
           valid_lft forever preferred_lft forever
        inet6 ::1/128 scope host
           valid_lft forever preferred_lft forever
    2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP qlen 1000
        link/ether fa:16:3e:27:c7:10 brd ff:ff:ff:ff:ff:ff
        inet 10.1.22.69/24 brd 10.1.22.255 scope global dynamic eth0
           valid_lft 76420sec preferred_lft 76420sec
        inet6 fe80::f816:3eff:fe27:c710/64 scope link
           valid_lft forever preferred_lft forever


## Add/remove an IP address

Adding an IP address can be done using the command:

```
$ ip addr add 192.168.50.5 dev eth0
```

And removing can be done using the command:

```
$ ip addr del 192.168.50.5/24 dev eth0
```


## Enable/disable a network interface
To enable a network interface use the command:

```
$ ip link set eth1 up
```

And disabling can be done using the command:

```
$ ip link set eth1 down
```

Manual page:

```
$ man ip link
```


## Routing table
Routing table management is done using the `ip route` command.

To see the current routing table use the command:

```
$ ip route
```

    default via 10.1.22.1 dev eth0  proto static  metric 100
    10.1.22.0/24 dev eth0  proto kernel  scope link  src 10.1.22.69  metric 100
    172.24.4.224/28 dev br-ex  proto kernel  scope link  src 172.24.4.225


## Add/remove static route
Static routes prevent traffic from passing through the default gateway. This
way the best way to reach a destination can be given.

To add a static route:
```
$ ip route add 10.10.20.0/24 via 192.168.50.100 dev eth0
```

And to remove a static route:
```
$ ip route del 10.10.20.0/24
```


## Persistent static routes
For CentOS / RHEL and Fedora a persistent static route can be configured via
a network configuration file.

For example:
```
$ vi /etc/sysconfig/network-scripts/route-eth0
```

    10.10.20.0/24 via 192.168.50.100 dev eth0


## Default gateway
Default gateways can be configured per interface or globally.

```
$ ip route add default via 192.168.50.100
```

and can be removed using:

```
$ ip route del default
```

Persistent gateway is configured in:

```
$ vi /etc/sysconfig/network
```

    GATEWAY=192.168.50.100


## See also

  * https://www.centos.org/docs/5/html/5.2/Deployment_Guide/s1-networkscripts-static-routes.html


## arping
Arping is a computer software tool for discovering and probing hosts on a computer network. Arping probes hosts on the attached network link by sending Link Layer frames using the Address Resolution Protocol request method addressed to a host identified by its MAC address of the network interface. The utility program may use ARP to resolve an IP 


## ping
`ping` is a small utitlity to measure the reachability and round-trip time for
a message to be sent and echoed back by a destination host. It uses a 
ICMP Echo Request message for this.

After just adding a default gateway for instance, you can verify whether the
route is working properly:

```
$ ping www.google.com
```

or using an IP address.


## What happened

  1. Query the DNS server to obtain the IP address of the destination host.
     (For example: 74.125.236.34)
  2. The destination address (74.125.236.34) is not within the network range.
     In Layer-3 (IP header) the DESTINATION IP will be set as `74.125.236.34`.
  3. In Layer-2, the DESTINATION MAC address will be the filled in with the
     MAC address of the default gateway (192.168.50.100’s MAC).

When the packet is sent out, the network switch (L2), sends the packet to the
default gateway since the destination MAC is that of the gateway.
Once the gateway receives the packet, based on its routing table, it will
forward the packets further.


## traceroute (tracepath)
In computing, traceroute is a computer network diagnostic tool for displaying the route and measuring transit delays of packets across an Internet Protocol network. The history of the route is



## iftop
iftop is a command-line system monitor tool that produces a frequently-updated list of network connections. By default, the connections are ordered by bandwidth usage, with only the "top" bandwidth …


## tcpdump
tcpdump is a common packet analyzer that runs under the command line. It allows the user to display TCP/IP and other packets being transmitted or received over a network to which the computer is attached. Distribut


## brctl
brctl is used to set up, maintain, and inspect the ethernet bridge configuration in the linux kernel.
An ethernet bridge is a device commonly used to connect different networks of ethernets together, so that these ethernets will appear as one ethernet to the participants.




## strace
strace is a diagnostic, debugging and instructional userspace utility for Linux. It is used to monitor interactions between processes and the Linux kernel, which include system calls, signal deliveries, and changes of process state. The operation of strace is made possible by the kernel feature known as ptrace.

systemtap


## lsof
lsof is a command meaning "list open files", which is used in many Unix-like systems to report a list of all open files and the processes that opened them. This open source utility was developed and supported by Victor A. Abell, the retired Associate Director of the Purdue University Computing Center. It works in and supports several Unix flavors.

```
$ lsof -i
```

## gdb
GNU Debugger. The standard debugger for the GNU operating system.


## pdb
The module pdb defines an interactive source code debugger for Python programs. It supports setting (conditional) breakpoints and single stepping at the source line level, inspection of stack frames, source code listing, and evaluation of arbitrary Python code in the context of any stack frame.

These libraries help you with Python development: the debugger enables you to step through code, analyze stack frames and set breakpoints etc., and the profilers run code and give you a detailed breakdown of execution times, 