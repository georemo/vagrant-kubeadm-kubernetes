NUM_WORKER_NODES=2
IP_NW="192.168.1."
IP_START=202

Vagrant.configure("2") do |config|
  config.vm.provision "shell", env: {"IP_NW" => IP_NW, "IP_START" => IP_START}, inline: <<-SHELL
      apt-get update -y
      echo "$IP_NW$((IP_START)) master-node" >> /etc/hosts
      echo "$IP_NW$((IP_START+1)) worker-node01" >> /etc/hosts
      echo "$IP_NW$((IP_START+2)) worker-node02" >> /etc/hosts
  SHELL

  config.vm.box = "bento/ubuntu-22.04"
  config.vm.box_check_update = true
  config.vm.network "public_network", bridge: "wlp2s0: Wi-Fi (AirPort)", auto_config: true

  config.vm.define "master" do |master|
    # master.vm.box = "bento/ubuntu-18.04"
    master.vm.hostname = "master-node"
    #master.vm.network "private_network", ip: IP_NW + "#{IP_START}"
    master.vm.network :public_network, ip: IP_NW + "#{IP_START}"
    master.vm.provider "virtualbox" do |vb|
        vb.memory = 4048
        vb.cpus = 2
    end
    # master.vm.provision "shell", path: "scripts/common.sh"
    master.vm.provision "file", source: "scripts/common.sh", destination: "common.sh"
    # master.vm.provision "shell", path: "scripts/master.sh"
    master.vm.provision "file", source: "scripts/master.sh", destination: "master.sh"
    master.vm.provision :shell, :inline => "sudo sh common.sh && sudo sh master.sh"
  end

  (1..NUM_WORKER_NODES).each do |i|

  config.vm.define "node0#{i}" do |node|
    node.vm.hostname = "worker-node0#{i}"
    # node.vm.network "private_network", ip: IP_NW + "#{IP_START + i}"
    node.vm.network :public_network, ip: IP_NW + "#{IP_START + i}"
    node.vm.provider "virtualbox" do |vb|
        vb.memory = 2048
        vb.cpus = 1
    end
    # node.vm.provision "shell", path: "scripts/common.sh"
    node.vm.provision "file", source: "scripts/common.sh", destination: "common.sh"
    # node.vm.provision "shell", path: "scripts/node.sh"
    node.vm.provision "file", source: "scripts/node.sh", destination: "node.sh"
    node.vm.provision :shell, :inline => "sudo sh common.sh && sudo sh node.sh"
  end

  end
end 
