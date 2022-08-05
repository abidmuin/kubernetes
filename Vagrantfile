Vagrant.configure("2") do |config|
  servers=[
      {
        :hostname => "kmaster",
        :box => "centos/7",
        :ip => "10.101.73.176",
        :ssh_port => '2200'
      },
      {
        :hostname => "kworker",
        :box => "centos/7",
        :ip => "10.101.73.177",
        :ssh_port => '2201'
      }
    ]
  servers.each do |machine|
      config.vm.define machine[:hostname] do |node|
          node.vm.box = machine[:box]
          node.vm.hostname = machine[:hostname]
          node.vm.network :private_network, ip: machine[:ip]
          node.vm.network "forwarded_port", guest: 22, host: machine[:ssh_port], id: "ssh"
          # node.vm.synced_folder "shared_folder", "/home/vagrant/shared_folder"
          # This will upgrade kernel while setting up the vagrant vm but it causes ssh issue
          # ssh issue fix pwsh => $env:VAGRANT_PREFER_SYSTEM_BIN="0", cmd => set VAGRANT_PREFER_SYSTEM_BIN=0
          node.vbguest.installer_options = { allow_kernel_upgrade: true }
          node.vm.provider :virtualbox do |vb|
              vb.name = machine[:hostname]
              vb.customize ["modifyvm", :id, "--memory", 2048]
              vb.customize ["modifyvm", :id, "--cpus", 2]
          end
      end
  end
end
