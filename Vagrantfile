# -*- mode: ruby -*-
# vi: set ft=ruby ts=2 sw=2 tw=0 et :

role = File.basename(File.expand_path(File.dirname(__FILE__)))

$packages = <<EOF
echo "Installing extra packages"
yum update -y
yum install vim git rsync wget telnet bind-utils traceroute net-tools -y
setenforce 0
sed -i s/SELINUX=enforcing/SELINUX=disabled/g /etc/selinux/config
EOF

$hostnames = <<EOF
echo "Setting up /etc/hosts"
echo -e "172.16.3.10\tansible\n172.16.3.11\thaproxy1\n172.16.3.12\thaproxy2\n" >> /etc/hosts
EOF

boxes = [
  {
    :name => "ansible",
    :box => "geerlingguy/centos7",
    :ip => '172.16.3.10',
    :ram => "512"
   },
  {
    :name => "haproxy1",
    :box => "geerlingguy/centos7",
    :ip => '172.16.3.11',
    :ram => "512"
  },
  {
    :name => "haproxy2",
    :box => "geerlingguy/centos7",
    :ip => '172.16.3.12',
    :ram => "512"
  },
]

Vagrant.configure("2") do |config|
  boxes.each do |box|
    config.vm.define box[:name] do |vms|
      vms.vm.box = box[:box]
      vms.vm.hostname = "#{box[:name]}"

      vms.vm.provider "virtualbox" do |v|
        v.customize ["modifyvm", :id, "--memory", box[:ram]]
      end

      vms.vm.network :private_network, ip: box[:ip]

      vms.vm.provision "shell", inline: $packages
      vms.vm.provision "shell", inline: $hostnames

    end
  end
end
