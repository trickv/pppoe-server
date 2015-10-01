$script = <<SCRIPT
set -e

hostnamectl set-hostname pppoe-server
cp -v /vagrant/etc/radvd.conf /etc/radvd.conf
cp -v /vagrant/etc/ppp/* /etc/ppp/

apt-get install pppoe -y

# TODO configure eth1

sed -i -e "s:pppoe-server.*::g" /etc/rc.local

echo "pppoe-server -C rtfm -L 192.168.76.1 -R 192.168.77.10  -O /etc/ppp/pppoe-server-options  -I eth1" >> /etc/rc.local

echo "net.ipv6.conf.default.forwarding=1" > /etc/sysctl.d/ipv6-forwarding.conf
echo "net.ipv6.conf.all.forwarding=1" >> /etc/sysctl.d/ipv6-forwarding.conf
sysctl --system

sed -i -e "s:exit 0::g" /etc/rc.local

apt-get install radvd -y
update-rc.d radvd enable

ip link set dev eth1 up
service radvd start
/etc/rc.local

SCRIPT

Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/vivid64"
  #config.vm.hostname = "pppoe-server"

  # Create a private network, which allows host-only access to the machine
  # using a specific IP.
  # PPPoE server: we later remove this IP, but this makes the HV
  # attach a separate host-only NIC for serving PPPoE.
  config.vm.network :private_network, ip: "192.168.42.67", auto_config: false

  # Share an additional folder to the guest VM. The first argument is
  # the path on the host to the actual folder. The second argument is
  # the path on the guest to mount the folder. And the optional third
  # argument is a set of non-required options.
  # config.vm.synced_folder "../data", "/vagrant_data"

  config.vm.provision "shell", inline: $script
end

# -*- mode: ruby -*-
# vi: set ft=ruby :
