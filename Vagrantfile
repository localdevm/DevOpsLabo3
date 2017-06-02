Vagrant.configure("2") do |config|
  #Algemene box configuratie
  config.vm.provider "virtualbox" do |v|
      v.memory = 512
      v.cpus = 1
  end
  config.vm.box = "hashicorp/precise64"

  #Manager box
  config.vm.define "mgr" do |mgr|
    mgr.vm.hostname = "mgr.site"
    mgr.vm.network :private_network, ip: "192.168.16.10"
    mgr.vm.provision  "shell", path: "bootstrap.sh"
    config.ssh.insert_key = false
  end

   #Productie box
  config.vm.define "prod" do |prod|
    prod.vm.hostname = "prod.site"
    prod.vm.network :private_network, ip: "192.168.16.12"
    prod.vm.network  "forwarded_port", guest: 22, host: 2222, id: "ssh", disabled: true
    prod.vm.network  "forwarded_port", guest: 22, host: 6666, auto_correct: true
    prod.vm.provision  "shell", path: "bootstrap.sh"
    config.ssh.insert_key = false
  end

  #Load balancer
  config.vm.define "loadb" do |loadb|
      loadb.vm.hostname = "load.site"
      loadb.vm.network :private_network, ip: "192.168.16.14"
      loadb.vm.provision "shell", path: "bootstrap.sh"
      config.ssh.insert_key = false
  end

  #Database
  config.vm.define "dtb" do |dtb|
    dtb.vm.hostname = "dtb.site"
    dtb.vm.network :private_network, ip: "192.168.16.16"
    dtb.vm.provision "shell", path: "bootstrap.sh"
    config.ssh.insert_key = false
end

  #Web servers
  (1..2).each do |i|
      config.vm.define "web#{i}" do |node|
          node.vm.hostname = "web#{i}"
          node.vm.network :private_network, ip: "192.168.16.2#{i}"
          node.vm.network "forwarded_port", guest: 80, host: "808#{i}"
          node.vm.provision "shell", path: "bootstrap.sh"
          config.ssh.insert_key = false
      end
    end

end
