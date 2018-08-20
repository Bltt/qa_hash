# -*- mode: ruby -*-
# vi: set ft=ruby :
require 'yaml'
machines = YAML.load_file("config.yaml")

Vagrant.configure("2") do |config| 
  machines.each do |machine|
	config.vm.define "#{machine['name']}" do |new_vm|
		set_box(machine, new_vm)
		set_forwarded_ports(machine, new_vm)
		set_cpus_and_memory(machine, new_vm)	
		install_packages(machine, new_vm)	
		run_scripts(machine, new_vm)
	end
  end
end

def set_box(machine, new_vm)
	 new_vm.vm.box = "#{machine['box']}"
end

def set_forwarded_ports(machine, new_vm)
	machine['forwarded_ports'].each do |ports|
                        new_vm.vm.network "forwarded_port", guest: ports['guest'], host: ports['host']
                end
end

def set_cpus_and_memory(machine, new_vm)
	new_vm.vm.provider "virtualbox" do |vb|
                vb.cpus = "#{machine['cpus']}"
        	vb.memory = "#{machine['memory']}"
	end
end

def install_packages(machine, new_vm)
	machine['packages'].each do |pkg|
                        new_vm.vm.provision "shell", inline: <<-SHELL
                	sudo yum install -y "#{pkg}"
        	SHELL
	end
end

def run_scripts(machine, new_vm)
	machine['scripts'].each do |script|
                        new_vm.vm.provision "shell", path: "vagrant_scripts/#{script}"
        end
end
