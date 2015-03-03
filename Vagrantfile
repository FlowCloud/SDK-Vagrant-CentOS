# -*- mode: ruby -*-
# vi: set ft=ruby :

$script = <<SCRIPT
echo '##################################'
echo '###      Updating system      ####'
echo '##################################'
sudo yum update -y
echo '##################################'
echo '### Installing required Repos ####'
echo '##################################'
sudo rpm --quiet --nosignature -U http://download.fedoraproject.org/pub/epel/7/x86_64/e/epel-release-7-5.noarch.rpm
sudo wget -q http://rpm.flowworld.com/flowsdk.repo -O /etc/yum.repos.d/flowsdk.repo
echo '##################################'
echo '### Installing required tools ####'
echo '##################################'
sudo yum install httpd gcc libflowcoredocs libflowcorehowtos libflowcore-devel libflowmessaging-devel libflowmessagingdocs libflowcorehowtos libflowmessaginghowtos libflowcore-python libflowmessaging-python libflowcorepythondocs libflowmessagingpythondocs -y --nogpgcheck
(sudo cat - > /etc/httpd/conf.d/flowsdk.conf) << END
Alias /flowsdk /opt/flowsdk/docs
<Directory "/opt/flowsdk/docs">
Options Indexes
AllowOverride All
Require all granted
</Directory>
END
sudo systemctl start httpd.service

SCRIPT


$post_msg = <<MSG
#################################

Flow-SDK environment is ready for use !!

To login into the vm:
   vagrant ssh 
  or use putty to connect to 
  host : localhost  Port: 2222 
Change to build the C examples go to directory 
  $ cd /opt/flowsdk/howtos/c/

Choose which howtos you want to build 
  FlowCore or FlowMessaging
  Ex:  $ cd FlowCore/Debug

To build the howtos type  
  $ sudo make

For online docs please go to: 

http://localhost:8080/flowsdk

Please visit http://flowcloud.io for more information

#################################
MSG

# Vagrantfile API/syntax version. Don't touch unless you know what you're doing!
VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config| 
 config.vm.box = "chef/centos-7.0"
 config.vm.provider "virtualbox" do |v|
  v.gui = false # use true to debug
 end
 config.vm.hostname = "FlowCloud"
 config.vm.provision "shell", inline: $script
 config.vm.network "forwarded_port", guest: 80, host: 8080
 config.vm.post_up_message = $post_msg
end
