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
1. Open terminal en installeer de rol bertvv.dhcp met de commando `ansible-galaxy install bertvv.dhcp`
2. Pas de vagrant-host.yml aan door onderaan de bestand de volgende code toe te voegen. Hiermee maken we de server op het virtualbox aan met als naam pr001 en ip adres 172.16.0.2/16:
```
- name: pr001
  ip: 172.16.0.2
  netmask: 255.255.0.0
```
3. Open de file site.yml in de map ansible en voeg onderaan de volgende code toe. Hiermee voegen we de rollen rh-base en dhcp toe en kunnen we configureren:
```
- hosts: pr001
  sudo: true
  roles:
    - bertvv.rh-base
    - bertvv.dhcp
```
4. Ga naar de map ansible -> host_vars en maak een file met als naam pr001.yml aan zodat we de server kunnen configureren.
5. Voeg de volgende code toe in de file pr001.yml om de server te kunnen configureren:

## Test report
 
## Resources
