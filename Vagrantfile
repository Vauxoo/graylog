# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.

Vagrant.configure(2) do |config|

  config.vm.box = "ubuntu/trusty64"
  config.vm.hostname = "graylog"
  api_port = 12900

  config.vm.provider "virtualbox" do |vb, override|
    vb.memory = "2048"

    # Graylog2 json endpoint
    override.vm.network "forwarded_port", guest: 12900, host: api_port
    # Nginx: To test it as in prod evironment
    override.vm.network "forwarded_port", guest: 8080, host: 8080
    # Graylog2 web interface: To access direct to the web interface
    override.vm.network "forwarded_port", guest: 9000, host: 9000
    # Graylog2 GELF port
    override.vm.network "forwarded_port", guest: 12201, host: 12201
    override.vm.provision "ansible" do |ansible|
      ansible.playbook = "provision.yml"
      ansible.extra_vars = {
        api_port: api_port,
        provider: "virtualbox"
      }
    end
  end

  config.vm.provider :aws do |aws, override|
    aws.keypair_name = "infraestructure"

    aws.ami = "ami-fce3c696"
    aws.region = "us-east-1"
    aws.instance_type = "t2.large"
    aws.access_key_id = ENV['ACCESS_KEY_ID']
    aws.secret_access_key = ENV['SECRET_ACCESS_KEY']
    aws.security_groups = ['sg-483ae233']
    aws.subnet_id = 'subnet-f1df62db'
    aws.block_device_mapping = [{ 'DeviceName' => '/dev/sda1', 'Ebs.VolumeSize' => 100 }]

    override.vm.box = "dummy"
    override.ssh.username = "ubuntu"
    override.ssh.private_key_path = "/home/truiz/.ssh/infraestructure.pem"

    override.vm.provision "ansible" do |ansible|
      ansible.playbook = "provision.yml"
      ansible.extra_vars = {
        nginx_port: 80,
        server_name: "log.vauxoo.com",
        provider: "amazon",
      }
    end
   end

end
