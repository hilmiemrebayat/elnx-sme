# Enterprise Linux Lab Report

- Student name: Hilmi Emre Bayat
- Github repo: https://github.com/hilmiemrebayat/elnx-sme

  1. Opzetten van een DHCP Server
  2. Opzetten van een router met VyOS

## Test plan
In this assignment, the goal is to complete the network we've been building with a DHCP server for workstations and a router. When a workstation is attached to the network, it should receive correct network settings from the DHCP server (IP address, default gateway, DNS) and be able to use the services on the local network (specifically the webserver and fileserver) and to access the Internet through the router.
### DHCP 
1. Create a new VirtualBox VM manually, give it two host-only network interfaces, both attached to the VirtualBox host-only network with IP 172.16.0.0/16.
2. Write down the MAC address of one of the two interfaces (or set it manually).
3. Boot the VM with a LiveCD ISO (e.g. Fedora, but Ubuntu, Kali, etc. should also be fine).
4. Ensure both interfaces will get an IP address assigned by DHCP
5. Check the IP addresses: one should be the reserved IP address, the other should come from the pool of dynamic addresses.
6. Finally, this VM should be able to view the "company website" by surfing to http://www.avalon.lan in a web browser and be able to access the fileserver both through SMB and FTP.


## Procedure/Documentation

## Test report
 
## Resources
