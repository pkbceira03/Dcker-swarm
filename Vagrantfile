Vagrant.configure("2") do |config|
    config.vm.box = "ubuntu/bionic64"
    config.vm.provision "shell", inline: <<-SHELL
      sudo apt-get update
      sudo apt-get install -y apt-transport-https ca-certificates curl software-properties-common
      curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
      sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu bionic stable"
      sudo apt-get update
      sudo apt-get install -y docker-ce
    SHELL
  
    config.vm.define "master" do |master|
      master.vm.hostname = "master"
      master.vm.network "private_network", ip: "10.10.10.100"
      master.vm.provision "shell", inline: <<-SHELL
        sudo docker swarm init --advertise-addr 10.10.10.100
      SHELL
    end
  
    config.vm.define "node01" do |node01|
      node01.vm.hostname = "node01"
      node01.vm.network "private_network", ip: "10.10.10.101"
      node01.vm.provision "shell", inline: <<-SHELL
        sudo docker swarm join --token $(sudo docker -H 10.10.10.100:2377 swarm join-token -q worker) 10.10.10.100:2377
      SHELL
    end
  
    config.vm.define "node02" do |node02|
      node02.vm.hostname = "node02"
      node02.vm.network "private_network", ip: "10.10.10.102"
      node02.vm.provision "shell", inline: <<-SHELL
        sudo docker swarm join --token $(sudo docker -H 10.10.10.100:2377 swarm join-token -q worker) 10.10.10.100:2377
      SHELL
    end
  end
  