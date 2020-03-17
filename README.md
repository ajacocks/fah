# fah
Folding-at-home installer for Linux

This is a simple playbook to install and configure the Folding-at-Home client on Linux hosts, using systemd.

Right now, it is just tested on RHEL 8, Fedora 31, and Ubuntu 19.10, but I hope to add other platforms, soon.

To get started, just fill out the variables in the inventory, at minimum, the list of hosts to install on. All other variables are optional.

Please fork, send PRs, and open issues, as needed.

Instructions:

1) Install Ansible, on your playform of choice
2) Clone this repo:
   $ git clone https://github.com/ajacocks/fah.git
3) Edit the file 'inventory' in the resulting 'fah' directory. All you need to do is to add the fully-qualified domain names of the hosts that you wish to install on, under the section '[servers]'. You can, optionally, define a control host, set a control password, add your username and passkey, and add your contributions to a team.

- Alex
