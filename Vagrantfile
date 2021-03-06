# -*- mode: ruby -*-
# vi: set ft=ruby :

VAGRANTFILE_API_VERSION = "2"

Vagrant.require_version ">= 1.7.4"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  config.vm.box          = "StefanScherer/windows_2016_docker"
  config.vm.communicator = "winrm"

  config.vm.provider "virtualbox" do |v|
    v.gui = true
    v.customize ["modifyvm", :id, "--memory", 2048]
    v.customize ["modifyvm", :id, "--cpus", 2]
    v.linked_clone = true
  end

  ["vmware_fusion", "vmware_workstation"].each do |provider|
    config.vm.provider provider do |v, override|
      v.gui = true
      v.vmx["memsize"] = "2048"
      v.vmx["numvcpus"] = "2"
      v.enable_vmrun_ip_lookup = false
    end
  end

  subnet = "192.168.36"

  config.vm.provider :vcloud do |vcloud, override|
    vcloud.vapp_prefix = "windows-docker-swarm-mode"
    vcloud.ip_subnet = "#{subnet}.1/255.255.255.0" # our test subnet with fixed IP adresses for everyone
    override.vm.usable_port_range = 2200..2999
    vcloud.memory = 4096
    vcloud.cpus = 2
  end

  config.vm.define "sw-lin-01" do |config|
    config.vm.hostname     = "sw-lin-01"
    config.vm.box          = "boxcutter/ubuntu1604"
    config.vm.communicator = "ssh"
    ["virtualbox", "vmware_fusion", "vmware_workstation"].each do |provider|
      config.vm.provider provider do |v, override|
        v.gui = false
      end
    end
    config.vm.network :private_network, ip: "#{subnet}.2", gateway: "#{subnet}.1"
    config.vm.network :forwarded_port, guest: 2375, host: 2375, id: "dockerinsecure", auto_correct: true
    config.vm.network :forwarded_port, guest: 8080, host: 8080, id: "visualizer", auto_correct: true
    config.vm.network :forwarded_port, guest: 8000, host: 8000, id: "whoami", auto_correct: true
    config.vm.network :forwarded_port, guest: 9000, host: 9000, id: "portainer", auto_correct: true

    config.vm.provision "shell", path: "scripts/install-docker.sh", privileged: false
    config.vm.provision "shell", path: "scripts/docker-swarm-init.sh", args: "-ip #{subnet}.2"
    config.vm.provision "shell", path: "scripts/run-visualizer.sh"
    config.vm.provision "shell", path: "scripts/portainer_create_service.sh"
    
  end

  config.vm.define "sw-lin-02" do |config|
    config.vm.hostname     = "sw-lin-02"
    config.vm.box          = "boxcutter/ubuntu1604"
    config.vm.communicator = "ssh"
    ["virtualbox", "vmware_fusion", "vmware_workstation"].each do |provider|
      config.vm.provider provider do |v, override|
        v.gui = false
      end
    end
    config.vm.network :private_network, ip: "#{subnet}.3", gateway: "#{subnet}.1"
    config.vm.provision "shell", path: "scripts/install-docker.sh", privileged: false
    config.vm.provision "shell", path: "scripts/docker-swarm-join.sh", args: "-managerip #{subnet}.2 -ip #{subnet}.3"

  end

  config.vm.define "sw-win-01" do |config|
    config.vm.hostname = "sw-win-01"
    config.vm.network :private_network, ip: "#{subnet}.4", gateway: "#{subnet}.1"
    config.vm.provision "shell", path: "scripts/fix-second-network.ps1", privileged: false, args: "#{subnet}.4"
    config.vm.provision "shell", path: "scripts/open-swarm-mode-ports.ps1", privileged: false
    config.vm.provision "shell", path: "scripts/update-docker-ce.ps1", privileged: false
    config.vm.provision "shell", path: "scripts/docker-swarm-join.ps1", privileged: false, args: "-managerip #{subnet}.2 -ip #{subnet}.4"
    # config.vm.provision "shell", path: "scripts/docker-swarm-init.ps1", privileged: false, args: "-ip #{subnet}.4"
    # at the moment we need to reboot the new swarm worker
    #config.vm.provision "reload"
  end

  config.vm.define "sw-win-02" do |config|
    config.vm.hostname = "sw-win-02"
    config.vm.network :private_network, ip: "#{subnet}.5", gateway: "#{subnet}.1"
    config.vm.provision "shell", path: "scripts/fix-second-network.ps1", privileged: false, args: "#{subnet}.5"
    config.vm.provision "shell", path: "scripts/open-swarm-mode-ports.ps1", privileged: false
    config.vm.provision "shell", path: "scripts/update-docker-ce.ps1", privileged: false
    config.vm.provision "shell", path: "scripts/docker-swarm-join.ps1", privileged: false, args: "-managerip #{subnet}.2 -ip #{subnet}.5"
    # at the moment we need to reboot the new swarm worker
    #config.vm.provision "reload"
  end
end
