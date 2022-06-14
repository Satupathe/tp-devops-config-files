# _*_ mode: ruby _*_

Vagrant.configure("2") do |config|

    config.vm.define "admin" do |subconfig|
        subconfig.vm.box = "centos/7"
        subconfig.vm.hostname = "ansible"
        subconfig.vm.network "private_network", ip: "192.168.60.2"
        subconfig.vm.synced_folder "ansible", "/home/vagrant/ansible", type: "rsync", rsync__exclude: ".git", mount_options: ["dmode=755", "fmode=644"]
        subconfig.vm.provider "virtualbox" do |vb|
            vb.memory = "4096"
        end

        # provision private key for ansible
        subconfig.vm.provision "file", source: "id_rsa", destination: "/home/vagrant/.ssh/"
    end

    (1..1).each do |i|
        config.vm.define "dev#{i}" do |subconfig|
            subconfig.vm.box = "ubuntu/focal64"
            subconfig.vm.hostname = "dev#{i}"
            subconfig.vm.network "private_network", ip: "192.168.60.10#{i}"

            #provision public key for ansible 
            subconfig.vm.provision "shell", inline: $script_inject_pubkey
        end
    end
    config.vm.define "dev-centos" do |subconfig|
        subconfig.vm.box = "centos/7"
    end

end

$script_inject_pubkey =<<-'EOT'
cat /vagrant/id_rsa.pub >> /home/vagrant/.ssh/authorized_keys
EOT