Vagrant.configure("2") do |config|

  # MySQL Server
  config.vm.define "db01" do |db01|
    db01.vm.box = "bento/centos-stream-9"
    db01.vm.hostname = "db01"
    db01.vm.network "private_network", ip: "192.168.56.15"
    db01.vm.provider "vmware_desktop" do |vmware|
      vmware.memory = "512"
      vmware.cpus = "1"
    end
  end

  # Memcache Server
  config.vm.define "mc01" do |mc01|
    mc01.vm.box = "bento/centos-stream-9"
    mc01.vm.hostname = "mc01"
    mc01.vm.network "private_network", ip: "192.168.56.14"
    mc01.vm.provider "vmware_desktop" do |vmware|
      vmware.memory = "512"
      vmware.cpus = "1"
    end
  end

  # RabbitMQ Server
  config.vm.define "rmq01" do |rmq01|
    rmq01.vm.box = "bento/centos-stream-9"
    rmq01.vm.hostname = "rmq01"
    rmq01.vm.network "private_network", ip: "192.168.56.16"
    rmq01.vm.provider "vmware_desktop" do |vmware|
      vmware.memory = "512"
      vmware.cpus = "1"
    end
  end

  # Tomcat Server
  config.vm.define "app01" do |app01|
    app01.vm.box = "bento/centos-stream-9"
    app01.vm.hostname = "app01"
    app01.vm.network "private_network", ip: "192.168.56.12"
    app01.vm.provider "vmware_desktop" do |vmware|
      vmware.memory = "1024"
      vmware.cpus = "2"
    end
  end

  # Nginx Server
  config.vm.define "web01" do |web01|
    web01.vm.box = "bento/centos-stream-9"
    web01.vm.hostname = "web01"
    web01.vm.network "private_network", ip: "192.168.56.11"
    web01.vm.provider "vmware_desktop" do |vmware|
      vmware.memory = "512"
      vmware.cpus = "1"
    end
  end

end
