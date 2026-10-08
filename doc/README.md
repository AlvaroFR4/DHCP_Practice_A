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

## Checkpoint 2 - DHCP Service Configuration



The main DHCP server configuration file was configured:



/etc/dhcp/dhcpd.conf



The configuration specifies:



\- Default lease time: 1 day.

\- Maximum lease time: 8 days.

\- Domain: alvaro.test.

\- DNS servers: 10.0.0.2 and 4.4.4.4.

\- Network: 192.168.57.0/24.

\- DHCP address range: 192.168.57.20 - 192.168.57.50.



To verify that the file’s syntax was correct, the following command was used:



sudo dhcpd -t



The DHCP service was then restarted:



sudo systemctl restart isc-dhcp-server



The service was verified to be active with:



systemctl status isc-dhcp-server



The service appeared as:



Active: active (running)



Finally, the open UDP ports were checked using:



sudo ss -lun



UDP port 67 appeared in the output:



0.0.0.0:67



This confirms that the DHCP server is active and listening for client requests.

## Checkpoint 3 - Testing the DHCP Client c1



The virtual machine `c1` was created and configured as a DHCP client within the internal network `intnet`.



To check the IP address obtained, the following command was used:



ip -br a



The client received the address:



192.168.57.20/24



This address falls within the range configured on the DHCP server:



192.168.57.20 - 192.168.57.50



Next, the messages exchanged between the client and the DHCP server were reviewed using:



sudo cat /var/log/syslog | grep dhcpd



The following messages were verified:



DHCPDISCOVER

DHCPOFFER

DHCPREQUEST

DHCPACK



Finally, the server’s lease file was reviewed:



sudo cat /var/lib/dhcp/dhcpd.leases



This file records the assignment of the address 192.168.57.20 to client c1.

## Checkpoint 4 - DHCP Reservation for Printer



A virtual machine named `printer` was configured within the internal network `intnet`.



Its network interface was assigned a known MAC address:



08:00:27:A1:B2:C3



A reservation was created on the DHCP server so that this MAC address always receives the following IP address:



192.168.57.111



The configuration was verified on `printer` using:



ip -br a



The internal network interface successfully received:



192.168.57.111/24



This confirms that the MAC-based DHCP reservation is working correctly.

