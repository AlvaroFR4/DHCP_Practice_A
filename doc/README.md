\# DHCP Practice A



\## Checkpoint 1 - Linux Server Configuration



\### Network configuration



The server network interfaces were checked with:



ip -br a



The internal network interface is eth2 and has the following IP address:



192.168.57.10/24



\### DHCP installation



The DHCP service was installed using:



sudo apt update

sudo apt install isc-dhcp-server -y



The DHCP service was configured to listen on the internal interface:



INTERFACESv4="eth2"



\### Configuration backup



Before modifying the DHCP configuration file, a backup was created with:



sudo cp /etc/dhcp/dhcpd.conf /etc/dhcp/dhcpd.conf.bak

