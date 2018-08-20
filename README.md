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
