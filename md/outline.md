# Inspection and maintenance tools

Gerard Braad

gerard@unitedstack.com


## Continued operation
The following tools described can help you in pinpointing issues.

    Note: This is just a primer.


## Divide them into categories

  * General tools
  * Disk
  * Network
  * Debug


## General tools
These are tools that are used for general inspection, such as 

  * system state
  * process monitoring
  * service control


## ps
`ps - report process status`


## Memory usage
To check if the system has enough memory available to run processes


## Service management
Processes on Linux/Unix are owned by the `init` process, which primary role is
to start processes according to a script from `/etc/inittab` or `/etc/init.d`


## Service management
`systemctl` is a linux command to control the `systemd` system and service
manager.


## Service files
The files used by `systemd` to start a service process are located in
`/usr/lib/systemd/system`.


## Disk commands

  * basic commands
  * preparation tools
  * mapping
  * consistency check


## Disk free
`df - report file system disk space usage`


## Disk usage
`du - estimate file space usage`


## fdisk
Hard disks can be divided into one or more logical disks called partitions. This
division is described in the partition table found in sector 0 of the disk.

`fdisk` is a command-line utility that provides disk partitioning functions.


## LVM
The Logical Volume Manager (LVM) provides logical volume management for the
Linux kernel.


## Network

  * connectivity
  * diagnostic


## ifcfg-ethX
On CentOS / RHEL and Fedora systems you can configure networm interfaces using
a network configuration file.


## iproute2
`iproute2` is a collection of userspace utilities for controlling and monitoring
various aspects of networking in the Linux kernel, including routing, network
interfaces, tunnels, traffic control, and network-related device drivers.


## Persistent static routes
For CentOS / RHEL and Fedora a persistent static route can be configured via
a network configuration file.


## arping
`arping` is a tool for discovering and probing hosts on a computer network. It
probes hosts on the attached network link by sending Link Layer frames using ARP
request method addressed to a host identified by its MAC address of the network
interface.


## ping
`ping` is a small utitlity to measure the reachability and round-trip time for
a message to be sent and echoed back by a destination host. It uses a 
ICMP Echo Request message for this.


## traceroute (tracepath)
`traceroute` is a network diagnostic tool for displaying the route and measuring
transit delays of packets on an IP network. This can help identify incorrect
routing table definitions.


## tcpdump
`tcpdump` is a common command line packet analyzer. It allows the user to display
TCP/IP and other packets being transmitted or received.


## iftop
`iftop` is a command-line monitoring tool that shows a list of network
connections and their bandwidth usage.


## brctl
`brctl` is used to set up, maintain, and inspect the ethernet bridge
configuration in the linux kernel.


## lsof
`lsof - list open files`


## strace
`strace - trace system calls and signals`


## Debugging
Debugging is a the process of finding and resolving defects that prevent correct
operation of a computer process.


## pdb
`pdb` is an interactive source code debugger for Python programs, offered as a
module. 


## gdb
GNU Debugger. The standard debugger for the GNU operating system.
