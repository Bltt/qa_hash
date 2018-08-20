# Vagrant tutorial #

## Monday: ##

Vagrantfile: created in ruby
Split into various functions to call as it provisions the box - options on config.yaml

#### Config.yaml #####

Contains various config options;

```yaml
---
- name: jenkins
  box: centos/7
  cpus: 1
  memory: 2048
  forwarded_ports:
  - guest: 9000
    host: 9000
  - guest: 9000
    host: 9001
  package_manager: yum
  packages:
  - git
  - vim
  scripts:
  - install_server
...
```
** name **            - Name of the server to spin up.
** box **             - os to use
** cpu **             - number of cores
** memory **          - ram to provide
** forward_ports **   - guest and host pairs for ports to forward that can be accessed through localhost:host listening on guest on the machine.
** package_manager ** - package manager to use, eg yum or apt-get
** packages **        - packages to download and install
** scripts **         - scripts to run after installation, stored in vagrant/scripts
