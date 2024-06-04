# Automate-Installation-with-Kickstart-file
Guide line to automate installation new machine using kickstart file

The Kickstart feature of Red Hat Enterprise Linux automates system installations. You can use
Kickstart text files to configure disk partitioning, network interfaces, package selection, and
customize the install. The Anaconda installer uses Kickstart files to perform a complete installation
without user interaction.

Our Demo is a guided steps to customize and automate RHEL server installation
## From working machine do the following
## Configuring DHCP : 

1- 

    cp /usr/share/doc/dhcp-server/dhcp.conf.example /etc/dhcp/dhcpd.conf

    vi /etc/dhcp/dhcpd.conf

then add the following configuration to create pool of IPs that will be distributed :
    
    subnet 192.168.190.1 netmask 255.255.255.0{
              range 192.168.190.10 192.168.190.40
    }

2-  Add new network adapter as Hostonly then assigned an ip address of the same range provided by dhcp
     
    $nmcli con add "Wired connection 1" ipv4.addresses  192.168.190.5/24

## Configuring Apache

1- 

    dnf install httpd
    systemctl enable --now httpd
    firewall-cmd --permanent --add-port=80/tcp
Copy the local anaconda file to /var/www/html/kickstart to be used in the newly installed machine 

    cp /root/anaconda-ks.cfg /var/www/html/kickstart
    chmod o+r /var/www/html/kickstart

## From newly created machine do the next:

- Boot the machine
- Click any key to Interrupt boot process then to the last of the line add : inst.ks=http:192.168.190.5/kickstart




