# fah
Folding-at-home installer for Linux

This is a simple playbook to install and configure the Folding-at-Home client on Linux hosts, using systemd.

Right now, it is just tested on RHEL 8, Fedora 31, and Ubuntu 19.10, but I hope to add other platforms, soon.

To get started, just fill out the variables in the inventory, at minimum, the list of hosts to install on. All other variables are optional.

Please fork, send PRs, and open issues, as needed.

Instructions:

1) Install Ansible, on your platform of choice
2) Clone this repo:
   
   ```$ git clone https://github.com/ajacocks/fah.git```
   
3) Edit the file 'inventory' in the resulting 'fah' directory. All you need to do is to add the fully-qualified domain names of the hosts that you wish to install on, under the section '[servers]'. You can, optionally, define a control host, set a control password, add your username and passkey, and add your contributions to a team.
Be sure to set ```gpu=```  to your liking.

4) Run the playbook.
   1) If you need an SSH password, and a sudo password, on the managed server, run it, like this:
   
      ```$ ansible-playbook main.yml -k -K```

   2) Otherwise, it's simply:

      ```$ ansible-playbook main.yml```
5) Once the playbook completed successfully, point your browser at [http://localhost:7396/](http://localhost:7396/)

- Alex

## Notes

RHEL/CentOS 6 is currently unsupported as they are unable to verify the Folding At Home SSL Certificates due to their older versions of python

#### Compatibility matrix

|                 | RHEL/CentOS 6 | RHEL/CentOS 7 | RHEL/CentOS 8 | Amz 1 | Amz 2 | Fedora 31 | Fedora 30 | Debian 10 | Debian 9 | Ubuntu 18.04 |
|-----------------|---------------|---------------|---------------|-------|-------|-----------|-----------|-----------|----------|--------------|
| Local CPU only  | x             | ✓             | ✓             | ✓     | ✓     | -         | -         | ✓         | ✓        | ✓            |
| Local CPU+GPU   | x             | ✓             | ✓             | -     | -     | -         | -         | -         | -        | -            |
| Podman CPU only | -             | -             | -             | -     | -     | -         | -         | -         | -        | -            |
| Podman w/GPU    | -             | -             | -             | -     | -     | -         | -         | -         | -        | -            |
| Docker CPU only | -             | -             | -             | -     | -     | -         | -         | -         | -        | -            |
| Docker w/GPU    | -             | -             | -             | -     | -     | -         | -         | -         | -        | -            |

Key:

* `✓` Testing completed. Fully functioning
* '-' Testing not started or incomplete
* `x` Known not to work. May be supported in the future

Scaling up your operations - Getting started with multiple systems.

For setting up a group of servers, you will want one Control node, and then any number of Compute nodes. 
One server can run both functions if desired, but be sure to leave some CPU resources available for it.

Setup inventory for Control and Compute nodes, including the variables for compute and control.

The Control node will handle connections to the Folding@home (F@H) service, work unit handling, and post-install compute node configuration.

Initial setup for both control and compute nodes can performed by these Ansible playbooks. You could install RPMS manually, but Ansible automation will ensure consistency. 

The Ansible role for FAHControl (Control node) will setup connections to F@H and queuing of work units to and from the Compute nodes.  

The Ansible role for FAHClient (Compute node) may provide a first pass at configuring CPU and GPU loads.  It will also read configuration from ansible configuration files. (insert path)




Helpful bits: 
 
================================ 

Join F@H support forum for great information: https://foldingathome.org/support/

================================

Check Professor Bowmens video: https://www.youtube.com/watch?v=bjYS0UKA4dE
Twitter feed: https://twitter.com/drgregbowman 
Official install guides: https://foldingathome.org/support/faq/installation-guides/

================================ 
 
Its advised to use static or DHCP/reserved IP addresses for all systems involved, unless you run internal DNS.

================================

Items to experiment with: Hyperthreading, CPU-per-slot (-1 = auto), grouping of CPUs if you more than a few.

================================

Top is your friend: 

top | grep fahclient 
...
9044  fahclie+  39  19  735740 142532  13520 R 798.0   0.1 897:15.29 FahCore_a7                                               
19322 fahclie+  39  19  751448 145624  13680 R 798.0   0.1 686:46.79 FahCore_a7                                              
20453 fahclie+  39  19  715624  71784  13524 R 797.4   0.1  59:29.34 FahCore_a7                                             
...

   Above show 3 CPU groups running at ~8 cores
   
================================

Each GPU will saturate one core/thread. or.. performance will drop.

================================

Review GPU for Points per day, Cost of card, and cost of energy. 
Review this with whoever pays the bills!

Google is your friend.. search for trends. 

https://forums.anandtech.com/threads/folding-home-rtx-2060-performance.2559663/

GPU jobs seem to be 10x as rewarding as CPU jobs. every bit helps!

================================

Review PCIexpress lane negotiation: 

cat probe-nvidia.ksh
#!/bin/bash
if [ `whoami` != root ]; then
echo "Please run as root"
exit
fi
BAR="===================================================================================================="
for C in `lspci  | grep NVIDIA | grep -i geforce | cut -f1 -d" "`; do
echo $BAR
echo "Card: "`lspci -s $C`
lspci -vv -s $C | grep  -i gt
echo $BAR
done

sudo ./probe-nvidia.ksh
 ====================================================================================================
Card: 01:00.0 VGA compatible controller: NVIDIA Corporation TU106 [GeForce RTX 2060 Rev. A] (rev a1)
		LnkCap:	Port #0, Speed 8GT/s, Width x16, ASPM L0s L1, Exit Latency L0s <1us, L1 <4us
		LnkSta:	Speed 8GT/s (ok), Width x8 (downgraded)
		LnkCtl2: Target Link Speed: 8GT/s, EnterCompliance- SpeedDis-
 ====================================================================================================
====================================================================================================
Card: 02:00.0 VGA compatible controller: NVIDIA Corporation TU106 [GeForce RTX 2060 Rev. A] (rev a1)
		LnkCap:	Port #1, Speed 8GT/s, Width x16, ASPM L0s L1, Exit Latency L0s <1us, L1 <4us
		LnkSta:	Speed 8GT/s (ok), Width x8 (downgraded)
		LnkCtl2: Target Link Speed: 8GT/s, EnterCompliance- SpeedDis-
 ====================================================================================================

    Review the Link Capability. Here (8 Gigatransfer/sec) and 16 lanes, to the status: 8 GT/s and 8 lanes are shown.

 ================================

Day 2 operations: 

All nodes use config files in /etc/fahclient/config.xml on each server. 
This file is updated by the Control Node.  No need to manually edit it.
Changes made by the Control Node will be invoked immediately and automatically. 

If a CPU or GPU job hangs or is 0% for long, pause and restart or idle and un-idle it.

You can view the progress of One Thread by selecting the client in the FAHControl GUI, identifying the SlotID in the "Folding Slots" window and then selecting log tab, checking the Slot box and selecting which slot you are researching.

To run the FAHControl as Non-Root user over ssh, on Control node:

[root@coffee ~]# chmod 4755 /usr/bin/FAHControl
[root@coffee ~]# ls -l /usr/bin/FAHControl
-rwsr-xr-x 1 root root 2591 Mar  4  2014 /usr/bin/FAHControl

Then from another box, ssh with "-X" option:

╚═>ssh -X (Non-root-user)@coffee
(non-root-user)@coffee's password: 
...
[westerj@coffee ~]$ FAHControl


Control node variables  
username: 'F@Hname' - Account name to be associated to the workunits you complete. It's Visible on public Internet. 
passkey: '32char code' - A code you can get from https://foldingathome.org/support/faq/points/passkey/ 
         This passkey ensures your work is accredited to your username.

Compute node variables
chost: '192.168.1.123'  this is the control host IP of the Control Node ( hostname if resolvable )
cpass: 'XXXXXXXXXX' - password used for control node to connect to Compute Node.  The account is called fahclient and is set to nologin on the compute nodes. 

Compute node variables - initial set:
commandport: 36300   shouldnt need to change this
