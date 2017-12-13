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
2. Pas de vagrant-host.yml aan door onderaan de bestand de volgende code toe te voegen. Hiermee maken we de server op het virtualbox aan met als naam pr001 en ip adres 172.16.0.2/16 (In de opdracht staat 172.16.0.1, maar tijdens de configuratie gaf ansible een waarschuwing om geen .1 te gebruiken, hierdoor heb ik .2 gebruikt):
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
5. Voeg de volgende code toe in de file pr001.yml om de server te laten configureren:
```
#firewall configureren zodat verkeer wordt doorgelaten
rhbase_firewall_allow_services:
  - dhcp

#dhcp package installeren
rhbase_install_packages:
  - dhcp
#DHCP lease time instellen
dhcp_global_default_lease_time: 43200
#IP-adres van DNS Servers meegeven
dhcp_global_domain_name_servers:
  - 192.0.2.10
  - 192.0.2.11
#instellen van de domeinnaam die de client moet gebruiken bij het oplossen van hostnamen
dhcp_global_domain_name: avalon.lan

#IP-adres van de router instellen
dhcp_global_routers: 172.16.255.254

#scope instellen voor dhcp

dhcp_subnets:
  - ip: 172.16.0.0
    netmask: 255.255.0.0
    domain_name_servers: 172.16.255.254
    default_lease_time: 43200
    pools:
      - allow: unknown-clients
        domain_name_servers: 172.16.255.254
        range_begin: 172.16.192.1
        range_end: 172.16.255.253
        default_lease_time: 43200
      - allow: known-clients
        domain_name_servers: 172.16.255.254
        range_begin: 172.16.128.1
        range_end: 172.16.191.254
#automatisch een ip adres toekenen aan client pc met mac adres 08:00:27:81:88:56
dhcp_hosts:
  - name: client1
    mac: '08:00:27:81:88:56'
    ip: 172.16.128.3


```
6. Voer nu de commando 'vagrant up' uit zodat de server geïnstalleerd en geconfigureerd wordt.
7. Nadat de installatie voltooid is, maak een VM aan om de DHCP Server te testen. Zie testplan.

## Test report
### DHCP Server
De DHCP server is goed geconfigureerd aangezien de twee netwerkpoorten ip-adressen krijgen zodat ze het moeten krijgen. Zie afbeelding hieronder:
![Automatisch ip toegekend aan client pc](https://github.com/hilmiemrebayat/elnx-sme/blob/master/report/Afbeeldingen/dhcp.jpeg)
 
## Resources
