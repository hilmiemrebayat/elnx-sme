# Enterprise Linux Lab Report

- Student name: Hilmi Emre Bayat
- Github repo: https://github.com/hilmiemrebayat/elnx-sme

  1. Samba FileServer aanmaken
  2. FTP Server aanmaken
  2. Testplan uitvoeren dat is geschreven door de leerkracht
  3. Testrapport uitschrijven

## Test plan

For testing purposes, the users have a password that is identical to their name. Adapt the variables at the beginning of the script, if necessary (particularly admin_user and admin_password).

Currently, most test cases are skipped, because failing tests will probably timeout which takes a lot of time. Remove the lines with the command skip at the beginning of a test case to execute it.


## Procedure/Documentation

1. Pas vagrant-host.yml file aan door de volgende code onderaan toe te voegen. Hiermee wordt de file server met naam pr011,  ip-adres 172.16.0.11 en netmask 255.255.0.0 aangemaakt in het VM:
```
- name: pr011
ip: 172.16.0.11
netmask: 255.255.0.0
```
2. Open daarna de terminal, ga met `cd` naar de map waarin de vagrantfile zit en doe `vagrant up` zodat de server wordt aangemaakt.
3. Maak een file pr011.yml aan in de map host_vars zodat de server configureerbaar wordt met ansible.
4. Installeer de rollen bertvv.samba en bertvv.vsftpd met de volgende code via de terminal, zodat we de server kunnen configureren als file en ftp server
```
$ ansible-galaxy install bertvv.samba
$ ansible-galaxy install bertvv.vsftpd
```
5. Pas de file site.yml aan en voeg de volgende code onderaan toe zodat de basis configuratie van de server gebeurt en de rollen geïnstalleerd worden(bertvv.rh-base, bertvv.samba, bertvv.vsftpd)
```
- hosts: pr011
sudo: true
roles:
- bertvv.rh-base
- bertvv.samba
- bertvv.vsftpd
```
6. Voer 'vagrant provision' uit met de terminal zodat de rollen geïnstalleerd en geconfigureerd worden op de server.
7. Nadat alle stappen hierboven uitgevoerd zijn, gaan we de samba fileserver configureren. Dit doe je door de volgende code's toe te voegen in het file pr011.yml:
```
#Configuratie van de firewall
rhbase_firewall_allow_services:
- samba
- ftp
#Printer delen uitschakelen
samba_load_printers:
- false

#Bios naam van de server configureren
samba_netbios_name: files

#Configureren van gebruikersmap voor elk gebruikers
samba_load_homes: true



```

## Test report

## Resources
1. https://github.com/hilmiemrebayat/elnx-sme/blob/master/doc/02-dns.md
2. https://galaxy.ansible.com/bertvv/bind/
3. https://github.com/hilmiemrebayat/elnx-sme/blob/master/README.md
4. https://galaxy.ansible.com/bertvv/rh-base/
