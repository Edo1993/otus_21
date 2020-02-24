# -*- mode: ruby -*-
# vim: set ft=ruby :

MACHINES = {
  :inetRouter => {
        :box_name => "centos/7",
        #:public => {:ip => '10.10.10.1', :adapter => 1},
        :net => [
                   {ip: '192.168.255.1', adapter: 2, netmask: "255.255.255.240", virtualbox__intnet: "router-net"}
               ]
  },
  :inetrouter2 => {
        :box_name => "centos/7",
        :net => [
                  {ip: '192.168.255.3', adapter: 2, netmask: "255.255.255.240", virtualbox__intnet: "router-net"},
                ]
            },

  :centralRouter => {
        :box_name => "centos/7",
        :net => [
                   {ip: '192.168.255.2', adapter: 2, netmask: "255.255.255.240", virtualbox__intnet: "router-net"},
                   {ip: '192.168.0.1', adapter: 3, netmask: "255.255.255.240", virtualbox__intnet: "dir-net"},
                ]
  },
  
  :centralServer => {
        :box_name => "centos/7",
        :net => [
                   {ip: '192.168.0.2', adapter: 2, netmask: "255.255.255.240", virtualbox__intnet: "dir-net"}
                ]
  },
}

Vagrant.configure("2") do |config|

  MACHINES.each do |boxname, boxconfig|
      
    config.vm.define boxname do |box|

        box.vm.box = boxconfig[:box_name]
        box.vm.host_name = boxname.to_s

        config.vm.provider "virtualbox" do |v|
          v.memory = 256
        end

        boxconfig[:net].each do |ipconf|
          box.vm.network "private_network", ipconf
        end
        
        if boxconfig.key?(:public)
          box.vm.network "public_network", boxconfig[:public]
        end

        box.vm.provision "shell", inline: <<-SHELL
          mkdir -p ~root/.ssh
                cp ~vagrant/.ssh/auth* ~root/.ssh
        SHELL
        
        case boxname.to_s
          when "inetRouter"
            box.vm.provision "shell", run: "always", inline: <<-SHELL
              yum install traceroute -y
              echo 'net.ipv4.conf.all.forwarding=1'  >> /etc/sysctl.conf
              iptables -t nat -A POSTROUTING ! -d 192.168.0.0/16 -o eth0 -j MASQUERADE
              iptables-restore < /vagrant/iptables_inetrouter.rules
              service iptables save
              ip route add 192.168.0.0/16 via 192.168.255.2
              sed -i 's/PasswordAuthentication no/PasswordAuthentication yes/g' /etc/ssh/sshd_config
              service sshd restart
              SHELL

        when "inetrouter2"
            box.vm.network 'forwarded_port', guest: 8080, host: 8080, host_ip: '127.0.0.1'
            box.vm.provision "shell", run: "always", inline: <<-SHELL
              echo '1'
              echo 'net.ipv4.conf.all.forwarding=1'  >> /etc/sysctl.conf
              echo '2'
              echo 'net.ipv4.ip_forward = 1' >> /etc/sysctl.conf
              echo '3'
              sysctl -p /etc/sysctl.conf
              echo '4'
              iptables -t nat -A POSTROUTING ! -d 192.168.0.0/16 -o eth0 -j MASQUERADE
              echo '5'
              ip route add 192.168.0.0/16 via 192.168.255.2
              echo '6'
              echo 'GATEWAY=192.168.255.1' >> /etc/sysconfig/network-scripts/ifcfg-eth1
              echo '7'
              echo 'DEFROUTE=no' >> /etc/sysconfig/network-scripts/ifcfg-eth0
              echo '8'
              iptables-restore < /vagrant/iptables_inetrouter2.rules
              echo '9'
              service iptables save
              echo '10'
            SHELL

          when "centralRouter"
            box.vm.provision "shell", run: "always", inline: <<-SHELL
              yum install traceroute nmap -y
              echo 'net.ipv4.conf.all.forwarding=1' >> /etc/sysctl.conf
              echo 'net.ipv4.ip_forward = 1' >> /etc/sysctl.conf
              sysctl -p /etc/sysctl.conf
              echo "DEFROUTE=no" >> /etc/sysconfig/network-scripts/ifcfg-eth0 
              echo "GATEWAY=192.168.255.1" >> /etc/sysconfig/network-scripts/ifcfg-eth1
              systemctl restart network
              chmod +x /vagrant/knock.sh
              cp /vagrant/knock.sh ~/
              systemctl stop firewalld
              setenforce 0
              SHELL

          when "centralServer"          
            box.vm.provision "shell", run: "always", inline: <<-SHELL
              yum install epel-release -y 
              yum install traceroute nginx -y
              echo "DEFROUTE=no" >> /etc/sysconfig/network-scripts/ifcfg-eth0 
              echo "GATEWAY=192.168.0.1" >> /etc/sysconfig/network-scripts/ifcfg-eth1
              systemctl restart network
              systemctl stop firewalld
              setenforce 0
              systemctl enable nginx
              systemctl start nginx
              echo 'net.ipv4.conf.all.forwarding=1'  >> /etc/sysctl.conf
              echo 'net.ipv4.ip_forward=1' >> /etc/sysctl.conf
              echo "GATEWAY=192.168.255.1" >> /etc/sysconfig/network-scripts/ifcfg-eth1
              systemctl restart network
              SHELL
          
        end

      end

  end
  
end
