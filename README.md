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

## GPU notes

1) until you see your GPU in `clinfo` FAHClient has no chance to use it with OpenCL.

2) if `clinfo` shows nothing, then try again after installing `mesa-libOpenCL`.

3) check the most recent log file in `/var/lib/fahclient/logs/`, you want to see an `OpenCL Device` entry in the latest `…*** System ***…` block.