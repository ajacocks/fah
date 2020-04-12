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

