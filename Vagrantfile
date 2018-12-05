# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  # boxes at https://vagrantcloud.com/search.
  
  config.vm.box_check_update = false
  

  config.vm.define "jenkins", primary:true do  |jenkins|
        jenkins.vm.box = "centos/7"
		jenkins.vm.hostname = 'node4'
		jenkins.vm.network "forwarded_port", guest: 8080, host: 8080
		jenkins.vm.synced_folder "./ansible", "/opt/ansible", type: "smb"
		jenkins.vm.provider "virtualbox" do |vb|
		    vb.customize ['modifyvm', :id, '--nictype1', '82540EM']
			vb.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
			vb.name = "jenkis"
			vb.gui = false
			vb.memory = "2048"
		end
	jenkins.vm.provision "shell", inline: <<-SHELL
	yum --enablerepo=base clean metadata
	yum -y --nogpgcheck update
	yum -y --nogpgcheck install  git vim mc curl wget epel-release
	yum -y --nogpgcheck install ansible
	yum -y --nogpgcheck install java-1.8.0-openjdk
	cd /opt/ansible && git clone https://github.com/lean-delivery/ansible-role-jenkins.git
	cp /vagrant/ansible/playbook.yml /opt/ansible
	cd /opt/ansible && ansible-playbook ./playbook.yml
	SHELL
  end
  
  
end
