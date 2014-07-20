VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  config.vm.box = "sandbox"
  config.vm.box_url = "http://cloud-images.ubuntu.com/vagrant/trusty/current/trusty-server-cloudimg-amd64-vagrant-disk1.box"
  config.vm.network "private_network", ip: "192.168.111.223"
  config.vm.hostname = "sandbox.local"
  config.vm.synced_folder "src/", "/src", nfs: true
  config.vm.provider "virtualbox" do |v|
    mem = `sysctl -n hw.memsize`.to_i / 1024 / 1024 / 2
    cpus = `sysctl -n hw.ncpu`.to_i
    v.customize ["modifyvm", :id, "--memory", mem]
    v.customize ["modifyvm", :id, "--cpus", cpus]
    v.customize ["modifyvm", :id, "--cpuexecutioncap", "50"]
  end
  config.vm.provision "ansible" do |ansible|
    ansible.playbook = "provisioning/playbook.yml"
    ansible.extra_vars = { env: "development" }
    ansible.verbose = "vvvv"
    # ansible.limit = "@/Users/peanuts/playbook.retry"
  end
end
