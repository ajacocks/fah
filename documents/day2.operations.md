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
