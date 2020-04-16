Install and uninstall RPMS.

At the moment, to have an RPM set to a new version (up or down), remove the fahclient, and then run the playbook.

Since the RPM relies on initscripts, and playbook enforces systemd service management, the following is advised:

rpm -e --noscripts fahclient

RPM as of version 7.6.6 was like this.

