Vagrant.configure("2") do |config|
	config.vm.synced_folder "C:/Users/crist/TFG", "/home/vagrant"
    config.timezone.value = "Europe/Madrid"

    config.vm.define "Jenkins" do |master|
      master.vm.box = "bento/ubuntu-20.04"
      master.vm.hostname = "Jenkins"
      master.vm.network "private_network", ip: "192.168.56.10"
      master.disksize.size = '20GB'
	  master.gui = true
      master.vm.provider "virtualbox" do |vb|
        vb.name = "Jenkins"
	      vb.memory = 4092
        vb.cpus = 8
      end
    end
end
