Vagrant.configure(2) do |config|

  config.vm.box = "ubuntu-14.04"
  config.vm.box_url = "https://cloud-images.ubuntu.com/vagrant/trusty/current/trusty-server-cloudimg-amd64-vagrant-disk1.box"

  # CI Server box configurations
  config.vm.define "ci" do |ci|
    ci.vm.network "private_network", type: "dhcp"
    ci.vm.hostname = "ci-server"

    ci.vm.provider :virtualbox do |box|
      box.name = "ci-server"
      box.customize ["modifyvm", :id, "--memory", "2048"]
    end

    ci.vm.provision "shell", path: "install/docker.sh"
    ci.vm.provision "shell", path: "install/docker-compose.sh"

    ci.vm.synced_folder ".", "/data"
  end

  # Development box configurations
  config.vm.define "dev" do |dev|
    dev.vm.network "private_network", type: "dhcp"
    dev.vm.hostname = "development"

    dev.vm.provider :virtualbox do |box|
      box.name = "development"
      box.customize ["modifyvm", :id, "--memory", "2048"]
    end

    dev.vm.provision "shell", path: "install/docker.sh"
    dev.vm.provision "shell", path: "install/docker-compose.sh"

    dev.vm.synced_folder ".", "/data"
  end

  # Production box configurations
  config.vm.define "prod" do |prod|
    prod.vm.network "private_network", type: "dhcp"
    prod.vm.hostname = "production"

    prod.vm.provider :virtualbox do |box|
      box.name = "production"
      box.customize ["modifyvm", :id, "--memory", "2048"]
    end

    prod.vm.provision "shell", path: "install/docker.sh"
    prod.vm.provision "shell", path: "install/docker-compose.sh"

    prod.vm.synced_folder ".", "/data"
  end
end
