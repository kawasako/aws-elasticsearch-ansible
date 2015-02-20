# -*- mode: ruby -*-
# vi: set ft=ruby :

# Vagrantfile API/syntax version. Don't touch unless you know what you're doing!
VAGRANTFILE_API_VERSION = "2"
Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  config.vm.define "es1" do |elasticsearch|
    elasticsearch.vm.box = "dummy"
    elasticsearch.vm.provider :aws do |aws, override|
      aws.access_key_id = ""
      aws.secret_access_key = ""
      aws.keypair_name = "develop"

      aws.ami = "ami-35072834"
      aws.region = "ap-northeast-1"
      aws.security_groups = ['unionQuery']
      aws.iam_instance_profile_name = 'unionQuery'
      aws.instance_type = "t2.micro"
      aws.user_data = "#!/bin/sh\nsed -i 's/^.*requiretty/# Defaults requiretty/' /etc/sudoers\n"
      aws.block_device_mapping = [
        {
          'DeviceName' => "/dev/xvda",
          'VirtualName' => "es1",
          'Ebs.VolumeSize' => 16,
          'Ebs.DeleteOnTermination' => true,
          'Ebs.VolumeType' => 'standard',
        }
      ]

      override.ssh.username = "ec2-user"
      override.ssh.private_key_path = "~/.ssh/develop.pem"
    end
    elasticsearch.vm.synced_folder ".", "/vagrant", disabled: true
  end

  config.vm.define "es2" do |elasticsearch|
    elasticsearch.vm.box = "dummy"
    elasticsearch.vm.provider :aws do |aws, override|
      aws.access_key_id = ""
      aws.secret_access_key = ""
      aws.keypair_name = "develop"

      aws.ami = "ami-35072834"
      aws.region = "ap-northeast-1"
      aws.security_groups = ['unionQuery']
      aws.iam_instance_profile_name = 'unionQuery'
      aws.instance_type = "t2.micro"
      aws.user_data = "#!/bin/sh\nsed -i 's/^.*requiretty/# Defaults requiretty/' /etc/sudoers\n"
      aws.block_device_mapping = [
        {
          'DeviceName' => "/dev/xvda",
          'VirtualName' => "es2",
          'Ebs.VolumeSize' => 16,
          'Ebs.DeleteOnTermination' => true,
          'Ebs.VolumeType' => 'standard',
        }
      ]

      override.ssh.username = "ec2-user"
      override.ssh.private_key_path = "~/.ssh/develop.pem"
    end
    elasticsearch.vm.synced_folder ".", "/vagrant", disabled: true
  end

  config.vm.define "webapp" do |webapp|
    webapp.vm.box = "dummy"
    webapp.vm.provider :aws do |aws, override|
      aws.access_key_id = ""
      aws.secret_access_key = ""
      aws.keypair_name = "develop"

      aws.ami = "ami-35072834"
      aws.region = "ap-northeast-1"
      aws.security_groups = ['unionQuery']
      aws.iam_instance_profile_name = 'unionQuery'
      aws.instance_type = "t2.micro"
      aws.user_data = "#!/bin/sh\nsed -i 's/^.*requiretty/# Defaults requiretty/' /etc/sudoers\n"

      override.ssh.username = "ec2-user"
      override.ssh.private_key_path = "~/.ssh/develop.pem"
    end
    webapp.vm.synced_folder ".", "/vagrant", disabled: true
  end

  config.vm.provision "ansible" do |ansible|
    ansible.playbook = "site.yml"
    ansible.groups = {
      "elasticsearch" => ["es1", "es2"],
      "webapp" => ["webapp"]
    }
  end
end
