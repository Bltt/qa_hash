# -*- mode: ruby -*-
# vi: set ft=ruby :
require 'yaml'
require 'json'

filename = "config"

if File.file? "#{filename}.json"
    machines = JSON.parse(File.read("#{filename}.json"))
else
    machines = YAML.load_file("#{filename}.yaml")
end

Vagrant.configure("2") do |config| 
  machines.each do |machine|
	config.vm.define "#{machine['name']}" do |new_vm|
		set_box(machine, new_vm)
		set_forwarded_ports(machine, new_vm)
		set_cpus_and_memory(machine, new_vm)	
		update_package_manager(machine, new_vm)
		install_packages(machine, new_vm)	
		set_shared_folders(machine, new_vm)
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

def update_package_manager(machine, new_vm)
	 if machine['package_manager'] == "apt-get"
	         new_vm.vm.provision "shell", inline: "sudo #{machine['package_manager']} upgrade -y"
	 else
		 new_vm.vm.provision "shell", inline: "sudo #{machine['package_manager']} update -y"
	 end

end

def install_packages(machine, new_vm)
	machine['packages'].each do |pkg|
                if machine['package_manager'] == "apk"
			new_vm.vm.provision "shell", inline: <<-SHELL
				sudo #{machine['package_manager']} add #{pkg}
			SHELL
		else
			new_vm.vm.provision "shell", inline: <<-SHELL
                		sudo #{machine['package_manager']} install -y "#{pkg}"
        		SHELL
		end
	end
end

def set_shared_folders(machine, new_vm)
	machine['shared_folders'].each do |dir|
		new_vm.vm.synced_folder dir['host'], dir['guest']
	end
end

def run_scripts(machine, new_vm)
	machine['scripts'].each do |script|
                        new_vm.vm.provision "shell", path: "vagrant_scripts/#{script}"
        end
end
