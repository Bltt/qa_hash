# Vagrant tutorial #

## Monday: ##

Vagrantfile: created in ruby
Split into various functions to call as it provisions the box - options on config.yaml

### Vagrantfile ###

```ruby
if File.file? "#{filename}.json"
    machines = JSON.parse(File.read("#{filename}.json"))
else
    machines = YAML.load_file("#{filename}.yaml")
end
```
Checks if the filename variable json exists (default "config") and uses that for vagrant
configuration, else uses a yaml.

Rest of the Vagrantfile loops through each machine, calling functions iteratively to set the various
options.


### Config ####

Contains various config options;
The following is in yaml, however there is the option to use the exact same options in .json (the
only difference is notation as opposed to naming)
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
   shared_folders:
  - host: ~/shared_vagrant
    guest: /home/vagrant/shared
  scripts:
  - install_server
...
```
* **name**            - Name of the server to spin up.
* **box**             - os to use
* **cpu**             - number of cores
* **memory**          - ram to provide
* **forward_ports**   - guest and host pairs for ports to forward that can be accessed through localhost:host listening on guest on the machine.
* **package_manager** - package manager to use, eg yum or apt-get
* **packages**        - packages to download and install
* **shared_folders**  - shared folder to add between the VM and host(windows)
* **scripts**         - scripts to run after installation, stored in vagrant/scripts
