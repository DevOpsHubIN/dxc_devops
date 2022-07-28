# Ansible Master script

$setup_server = <<SCRIPT

# Install java, epel and utilities

yum install -y java epel-release net-tools wget vim

# Enable root login in ssh configuration

sed -i "s/^PermitRootLogin no/PermitRootLogin yes/g" /etc/ssh/sshd_config
sed -i "s/^PasswordAuthentication no/PasswordAuthentication yes/g" /etc/ssh/sshd_config
service sshd restart

# Install git and ansible

yum install -y git ansible

wget -O /etc/yum.repos.d/jenkins.repo https://pkg.jenkins.io/redhat-stable/jenkins.repo

rpm --import rpm --import https://pkg.jenkins.io/redhat-stable/jenkins.io.key

yum install -y jenkins

systemctl start jenkins

systemctl enable jenkins

SCRIPT

# Client script

$setup_client = <<SCRIPT

# Install network tools

yum install -y java epel-release net-tools wget

# Enable root login in ssh configuration

sed -i "s/^PermitRootLogin no/PermitRootLogin yes/g" /etc/ssh/sshd_config
sed -i "s/^PasswordAuthentication no/PasswordAuthentication yes/g" /etc/ssh/sshd_config
service sshd restart

# Install tomcat

#yum instll -y tomcat

#yum install -y tomcat-webapps tomcat-admin-webapps tomcat-docs-webapp

#systemctl start tomcat

#systemctl enable tomcat

# edit /usr/share/tomcat/conf/tomcat-users.xml
# uncoment the roles and user lines and add password

SCRIPT

$setup_docker = <<SCRIPT

# Install network tools

yum install -y java epel-release net-tools wget vim

# Enable root login in ssh configuration

sed -i "s/^PermitRootLogin no/PermitRootLogin yes/g" /etc/ssh/sshd_config
sed -i "s/^PasswordAuthentication no/PasswordAuthentication yes/g" /etc/ssh/sshd_config
service sshd restart

# Install docker

useradd docker
usermod -aG docker vagrant

yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo

yum install -y docker-ce docker-ce-cli containerd.io

systemctl start docker
systemctl enable docker

SCRIPT



Vagrant.configure("2") do |config|
  config.vm.define "master" do |subconfig|
    subconfig.vm.box = "centos/7"
	subconfig.vm.hostname = "ansible-master"
#    subconfig.vm.network "public_network"
	subconfig.vm.network "private_network", ip: "192.168.2.20"
    subconfig.vm.provision "shell", inline:$setup_server
	
	subconfig.vm.provider "virtualbox" do |vb|
		vb.memory = "2048"
	end
    
  end

  config.vm.define "node1" do |subconfig|
    subconfig.vm.box = "centos/7"
	subconfig.vm.hostname = "node1"
	subconfig.vm.network "private_network", ip: "192.168.2.40"
	subconfig.vm.provision "shell", inline:$setup_docker
	
	subconfig.vm.provider "virtualbox" do |vb|
		vb.memory = "1024"
	end
  end

end

