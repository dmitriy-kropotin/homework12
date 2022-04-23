# -*- mode: ruby -*-
# vim: set ft=ruby :

MACHINES = {
  :nginx => {
        :box_name => "oraclelinux/8",
        :box_url => "https://oracle.github.io/vagrant-projects/boxes/oraclelinux/8.json",
        :ip_addr => '192.168.56.150'
  }
}

Vagrant.configure("2") do |config|

  MACHINES.each do |boxname, boxconfig|

      config.vm.define boxname do |box|

          box.vm.box = boxconfig[:box_name]
          box.vm.box_url = boxconfig[:box_url]
          box.vm.host_name = boxname.to_s
          box.vm.synced_folder ".", "/vagrant", disabled: true
          box.vm.network "private_network", ip: boxconfig[:ip_addr]

#          box.vm.provider :virtualbox do |vb|
#            vb.customize ["modifyvm", :id, "--memory", "1024"]            
#          end
          
          box.vm.provision "shell", inline: <<-SHELL
            mkdir -p ~root/.ssh; cp ~vagrant/.ssh/auth* ~root/.ssh
            sed -i '65s/PasswordAuthentication no/PasswordAuthentication yes/g' /etc/ssh/sshd_config
            systemctl restart sshd
            dnf install -y python39
          SHELL

      end
  end
end
