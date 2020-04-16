Instructions for setting up client prior to playbook execution

Short list: 

Identify one control node and any number of FAHClient Compute nodes.
Install latest ansible RPM on the Control node.
Download this Playbook (and affiliated if needed as it develops)
Hack up inventory file in the top level of the playbook
 - add FAHClient Compute nodes
 - add FAHControl nodes (soon)
 - add FAHView nodes (soon -if you want it)
 
 - Set the versions you want to run. At the moment 7.6.6 is good. 
   check around: https://download.foldingathome.org/releases/

 - Set variables down the bottom of the file: 
     chost      ( ip or hostname of your control node)
     cpass      ( password used by control node to ssh into compute nodes)
     username   ( you registered your account at F@H, right? 
     passkey    ( see https://foldingathome.org/support/faq/points/passkey/ )
     power full ( run it at full throttle)
     gpu false  ( for now, automation coming soon)
     smp true   ( for now)
     cause any  ( that's the channel for covid work loads)


=== Inventory file format ===============================================================

clients]
# add the list of hosts that you want to install the Folding-at-Home client on, here:
#localhost ansible_connection=local 
#box1  
#box2 

[controllers] 
# add the list of hosts that you want to install the Folding-at-Home controller on, here:
#localhost ansible_connection=local ansible_python_interpreter=/usr/bin/python3
# note: worst case, install the Compute nodes, then find the fahcontrol node app at: 
# https://download.foldingathome.org/releases/
#coffee

[viewers]
# add the list of hosts that you want to install the Folding-at-Home viewer on, here:

[clients:vars]
fahclient_ver= 7.6.6

[controllers:vars]
fahcontrol_ver= 7.6.6

[viewers:vars]
fahviewer_ver= 7.6.6

[all:vars]
chost='192.168.1.116'
cpass='redacted'
username='JohnWesterdale'
passkey='d7905408acb627b4d7905408acb627b4'
power=full
gpu=false
smp=true
cause=any

==========================================================================================

Folding@Home compute node install steps on RHEL8 
================================================

To setup a RHEL8 Compute node, the following steps are advised. (skip steps already taken)

1) Build server with minimal install. go thru the boot and configuration of the network. 
   1a) Hint: nmtui may be your friend to get NIC up asap.
2) Register with RHSM. Output of "subscription-manager status" should be  Current.
3) Setup access over ssh from control node to (username)@target, likely using ssh-copy-id.
    suggest:
       echo "(username ALL=(ALL) NOPASSWD: ALL" > /etc/sudoers.d/(username) 
4) download Ansible playbook:
   cd ; git clone  https://github.com/ajacocks/fah.git
5) prep inventory file:
   cd fah
   vi inventory  (add in details to your env)
           
6) Register system with RHSM
7) yum update if possible, reboot

8) if NVidia card:
  8a) Download NVidia driver from NVidia.com website. Similar to: NVIDIA-Linux-x86_64-440.82.run )
  8b) blacklist nouveau
  
      echo "blacklist nouveau" > /etc/modprobe.d/nouveau-blacklist.conf 
 
  8c) prepare all current kernels with Grubby line --all-kernels  

     Something like:

     grubby ––update-kernel=ALL ––args="rd.driver.blacklist=nouveau nouveau.modeset=0"
     mv /boot/initramfs-$(uname -r).img /boot/initramfs-$(uname -r).img.bak
     dracut /boot/initramfs-$(uname -r).img $(uname -r)

  8d) telinit 3 or boot multi-user (non graphical)
  8e) prepare DKMS for future kernel updates.
  8f) reboot to non-graphical mode
  8g) run the NVidia .run file (  Hint: `bash NVIDIA-Linux-x86_64-440.82.run` )
  8h) Run the playbook "ansible-playbook -i inventory main.yml"

Example System preparation
==========================

Register System: 
[root@XXXX ~]# subscription-manager register
Registering to: subscription.rhsm.redhat.com:443/subscription
Username: (RHSM login account)
Password: (RHSM password)
The system has been registered with ID: 27d-Redacted-523f6
The registered system name is: XXXX
[root@XXXX ~]# 
 
[root@zdog ~]# yum repolist
Updating Subscription Management repositories.
Last metadata expiration check: 0:00:02 ago on Wed 15 Apr 2020 04:33:35 PM EDT.
repo id                           repo name                                                   status
rhel-8-for-x86_64-appstream-rpms  Red Hat Enterprise Linux 8 for x86_64 - AppStream (RPMs)     8,820
rhel-8-for-x86_64-baseos-rpms     Red Hat Enterprise Linux 8 for x86_64 - BaseOS (RPMs)        3,801
[root@zdog ~]# 



