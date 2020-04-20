Install and uninstall RPMS.

At the moment, to have an RPM set to a new version (up or down), remove the fahclient, and then run the playbook.

Since the RPM relies on initscripts, and playbook enforces systemd service management, the following is advised:

rpm -e --noscripts fahclient

RPM as of version 7.6.6 was like this.

[westerj@coffee fah]$ sudo rpm -e fahclient
/var/tmp/rpm-tmp.XCV46k: line 5: /etc/init.d/FAHClient: No such file or directory
error reading information on service FAHClient: No such file or directory
error: %preun(fahclient-7.6.6-1.x86_64) scriptlet failed, exit status 1
error: fahclient-7.6.6-1.x86_64: erase failed

[westerj@coffee fah]$ sudo rpm -e --noscripts fahclient
warning: file /etc/init.d/FAHClient: remove failed: No such file or directory

[westerj@coffee fah]$ sudo rpm -e --noscripts fahclient
error: package fahclient is not installed
[westerj@coffee fah]$
