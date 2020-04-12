Scaling up your operations - Getting started with multiple systems.

For setting up a group of servers, you will want one Control node, and then any number of Compute nodes. 
One server can run both functions if desired, but be sure to leave some CPU resources available for it.

Setup inventory for Control and Compute nodes, including the variables for compute and control.

The Control node will handle connections to the Folding@home (F@H) service, work unit handling, and post-install compute node configuration.

Initial setup for both control and compute nodes can performed by these Ansible playbooks. You could install RPMS manually, but Ansible automation will ensure consistency. 

The Ansible role for FAHControl (Control node) will setup connections to F@H and queuing of work units to and from the Compute nodes.  

The Ansible role for FAHClient (Compute node) may provide a first pass at configuring CPU and GPU loads.  It will also read configuration from ansible configuration files. (insert path)




