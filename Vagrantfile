# -*- mode: ruby -*-
# vi: set ft=ruby :


#[fred@chene vagrant]$ vagrant plugin install vagrant-disksize
#Installing the 'vagrant-disksize' plugin. This can take a few minutes...
#Fetching vagrant-disksize-0.1.3.gem
#Installed the plugin 'vagrant-disksize (0.1.3)'!
#[fred@chene vagrant]$


VAGRANTFILE_API_VERSION = "2"

cluster = {
	"k8smaster0" => { :ip => "192.168.1.100", :cpus => 2, :mem => 3072 },
	#"k8smaster0" => { :ip => "192.168.1.101", :cpus => 2, :mem => 3072 },
  	"k8sworker0" => { :ip => "192.168.1.102", :cpus => 2, :mem => 3072 },
  	"k8sworker1" => { :ip => "192.168.1.103", :cpus => 2, :mem => 3072 },
  	"k8sworker2" => { :ip => "192.168.1.104", :cpus => 2, :mem => 3072 }
}
 
Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  cluster.each_with_index do |(hostname, info), index|

    config.vm.define hostname do |cfg|
      cfg.vm.provider :virtualbox do |vb, override|
        #config.vm.box = "ubuntu/trusty64"
        #config.vm.box = "ubuntu/xenial64"
	config.vm.box = "generic/centos8"
      	#config.vm.box = "geerlingguy/centos7"
	override.vm.network :public_network, ip: "#{info[:ip]}", bridge: "enp3s0"
        override.vm.hostname = hostname
        vb.name = hostname
        vb.customize ["modifyvm", :id, "--memory", info[:mem], "--cpus", info[:cpus], "--hwvirtex", "on"]
      end #provider
      #config.disksize.size = "50GB"
      config.vm.provision "file", source: "id_rsa", destination: "/home/vagrant/.ssh/id_rsa"
      public_key = File.read("id_rsa.pub")
      $script = <<-SCRIPT
      echo "Seqense provisioning..."
      set -e -u
      echo 'Copying ansible-vm public SSH Keys to the VM'
      mkdir -p /home/vagrant/.ssh
      echo '#{public_key}' >> /home/vagrant/.ssh/authorized_keys
      echo 'Host 192.168.*.*' >> /home/vagrant/.ssh/config
      echo 'StrictHostKeyChecking no' >> /home/vagrant/.ssh/config
      echo 'UserKnownHostsFile /dev/null' >> /home/vagrant/.ssh/config
      echo 'enable vagrant PasswordAuthentication'
      sed -i 's/PasswordAuthentication no/PasswordAuthentication yes/g' /etc/ssh/sshd_config
      echo 'UserKnownHostsFile /dev/null' >> /home/vagrant/.ssh/config
      echo 'Disable selinux'
      sed -i 's/^SELINUX=.*/SELINUX=disabled/g' /etc/selinux/config
      echo 'set access rights'
      chmod -R 700 /home/vagrant/.ssh/authorized_keys
      chmod 700 /home/vagrant/.ssh
      chmod -R 700 /home/vagrant/.ssh/config
      echo "Disable firewalld"
      systemctl disable firewalld
      echo "Restart sshd"
      sudo systemctl restart sshd
      #echo "Update system"
      #sudo yum update -y
      SCRIPT
      config.vm.provision "shell", inline: $script
   end
 end
end
