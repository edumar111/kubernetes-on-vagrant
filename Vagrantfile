Vagrant.configure("2") do |config|

  config.vm.define "node1" do |node1|
      node1.vm.box = "centos/7"
      node1.vm.hostname = "node1"
      node1.vm.provision "shell", path: "init.sh"
      node1.vm.network "public_network", ip: "192.168.1.40",  bridge: "en1: Wi-Fi (AirPort)", auto_config: false
      # default router
      node1.vm.provision "shell",
          run: "always",
          inline: "route add default gw 192.168.1.1"
      # delete default gw on eth0
      node1.vm.provision "shell",
          run: "always",
          inline: "eval `route -n | awk '{ if ($8 ==\"eth0\" && $2 != \"0.0.0.0\") print \"route del default gw \" $2; }'`"
      node1.vm.provision "shell", path: "install.sh"
      node1.vm.provider :virtualbox do |vb1|
          vb1.customize ["modifyvm", :id, "--memory", 1024]
       end

  end
  config.vm.define "node2" do |node2|
      node2.vm.box = "centos/7"
      node2.vm.hostname = "node2"
      node2.vm.provision "shell", path: "init.sh"
      node2.vm.network "public_network", ip: "192.168.1.41",  bridge: "en1: Wi-Fi (AirPort)", auto_config: false
        # default router
      node2.vm.provision "shell",
        run: "always",
        inline: "route add default gw 192.168.1.1"
      # delete default gw on eth0
      node2.vm.provision "shell",
        run: "always",
        inline: "eval `route -n | awk '{ if ($8 ==\"eth0\" && $2 != \"0.0.0.0\") print \"route del default gw \" $2; }'`"
      node2.vm.provision "shell", path: "install.sh"
      node2.vm.provider :virtualbox do |vb2|
          vb2.customize ["modifyvm", :id, "--memory", 1024]
      end
  end
  config.vm.define "manager" do |node3|
      node3.vm.box = "centos/7"
      node3.vm.hostname = "manager"
      node3.vm.provision "shell", path: "init.sh"
      node3.vm.network "public_network", ip: "192.168.1.42",  bridge: "en1: Wi-Fi (AirPort)", auto_config: false
       # default router
      node3.vm.provision "shell",
        run: "always",
        inline: "route add default gw 192.168.1.1"
      # delete default gw on eth0
      node3.vm.provision "shell",
        run: "always",
        inline: "eval `route -n | awk '{ if ($8 ==\"eth0\" && $2 != \"0.0.0.0\") print \"route del default gw \" $2; }'`"
      node3.vm.provision "shell", path: "install.sh"
      node3.vm.provider :virtualbox do |vb3|
          vb3.customize ["modifyvm", :id, "--memory", 2048]
      end
  end
end
