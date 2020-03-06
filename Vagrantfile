DOCKER_VERSION = '5:19.03.7~3-0~ubuntu-bionic'
MK8S_CHANNEL = '1.17/stable'

CPUS = '4'
MEMORY = '8192'

#------------------ No need to change anything below this line -----------
$dockerscript = <<-SCRIPT
echo I am provisioning docker...
sudo apt-get update
sudo apt-get install -y apt-transport-https ca-certificates curl gnupg-agent software-properties-common
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
sudo add-apt-repository \
   "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
   $(lsb_release -cs) \
   stable"
sudo apt-get update
sudo apt-get install -y docker-ce=#{DOCKER_VERSION} docker-ce-cli=#{DOCKER_VERSION} containerd.io
sudo usermod -aG docker $USER
SCRIPT

$microk8sscript = <<SCRIPT
#!/bin/bash

#Install and configure microk8s
sudo snap install microk8s --classic --channel=#{MK8S_CHANNEL}

sudo usermod -a -G microk8s $USER

## Configure vagrant clients dir
printf "alias kubectl='microk8s.kubectl'\n" >> /home/vagrant/.bashrc
printf "source /etc/bash_completion\n" >> /home/vagrant/.bashrc
printf "source <(kubectl completion bash)\n" >> /home/vagrant/.bashrc

# extras
sudo apt-get -y install bash-completion
SCRIPT

Vagrant.configure("2") do |config|
  config.vm.box = "bento/ubuntu-18.04"

  config.vm.provider "virtualbox" do |vb|
    vb.customize ["modifyvm", :id, "--cpus", "#{CPUS}"] # set number of vcpus
    vb.customize ["modifyvm", :id, "--memory", "#{MEMORY}"] # set amount of memory allocated vm memory
  end

  config.vm.provision "shell", inline: $dockerscript, privileged: false
  config.vm.provision "shell", inline: $microk8sscript, privileged: false

end