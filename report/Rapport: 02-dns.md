# Enterprise Linux Lab Report

- Student name: Hilmi Emre Bayat
- Github repo: https://github.com/hilmiemrebayat/elnx-sme

  1. DNS en slave dns server aanmaken
  2. Testplan uitvoeren dat is geschreven door de leerkracht
  3. Testrapport uitschrijven

## Test plan
Both servers can be validated with the test scripts. Execute `sudo /vagrant/test/runbats.sh` to run them. Remark that these tests run locally on the VMs. Ensure that the DNS service is also available from your host system!

Om de test uit te voeren moet je dus de volgende commando uitvoeren: `sudo /vagrant/test/runbats.sh`

## Procedure/Documentation
### DNS Server
1. Downloaden en installeren van bertvv.bind met de volgende commando: `ansible-galaxy install bertvv.bind`. Dankzij dit role is het mogelijk om een DNS server op te zetten.
2. Voeg de volgende code toe aan de document vagrant-hosts.yml: 
```Yaml
- name: pu001
  ip: 192.0.2.10
```
   Dit code komt onder pu004. Dankzij dit code laten we vagrant een server aanmaken met de naam pu001 en ip adres 192.0.2.10.
   
3. Voeg de volgende code toe aan site.yml, zodat de host aangemaakt en de roles "rh-base" (zorgt ervoor dat de basis installatie uitgevoerd wordt op de server) en "bind" (configureerd de server als DNS server) geïnstalleerd worden op de server pu001.
```Yaml
- hosts: pu001
  sudo: true
  roles:
    - bertvv.rh-base
    - bertvv.bind
```
4. Open de terminal, ga met 'cd <pad van bestand(en)>' naar de map waar je de vagrant file terug kan vinden en voer de commando `vagrant up` en daarna `vagrant provision` uit zodat de server geïnstalleerd wordt.
5. Nadat de installatie voltooid is zien we dat de installatie van role "bind" niet lukt. Dit komt omdat we de minimale vereisten van voor de dns server niet hebben meegegeven. Hierdoor gaan we de DNS Server eerst congigureren en daarna laten installeren. Om dit te doen maak je eerst in het map Ansible -> host_vars een bestand genaamd "pu001.yml" aan. Dankzij dit bestand kunnen we DNS Server configureren.
6. Eerst gaan we de minimale vereisten configureren. De minimale veriesten om de server werkend te krijgen staat op dit [document](https://galaxy.ansible.com/bertvv/bind/). Voeg de volgende code toe in het bestand "pu001.yml" dat we in stap 5 hebben aangemaakt om de DNS server te configureren:
```Yaml
#Hier wordt de domein naam geconfigureerd
bind_zone_name: avalon.lan

#Hier wordt de ip-adres van de DNS server geconfigureerd
bind_zone_master_server_ip: 192.0.2.10

#Hier worden de  netwerken dat in de domein zitten geconfigureerd. In ons geval zijn er twee verschillende domeinen, namelijk 192.0.2.0 en 172.16.0.0
bind_zone_networks:
  - "192.0.2"
  - "172.16"
  
#Hier worden de namen van de dns servers in het domein geconfigureerd. In ons domein hebben we twee dns servers met de namen pu001 (DNS server) en pu002 (slave dns server)
bind_zone_name_servers:
  - pu001
  - pu002
  
#Hier worden alle servers geconfigureerd binnen het domein, zodat de dns server een lookup en reserve zone kan creeëren voor het vertalen van de IP-Adres naar naam en omgekeerd.
bind_zone_hosts:
  - name: r001
    ip: 192.0.2.254
    aliases:
      - gw
  - name: pu001
    ip: 192.0.2.10
    aliases:
      - ns1
  - name: pu002
    ip: 192.0.2.11
    aliases:
      - ns2
  - name: pu003
    ip: 192.0.2.20
    aliases:
      - mail
  - name: pu004
    ip: 192.0.2.50
    aliases:
      - www
  - name: pr001
    ip: 172.16.0.2
    aliases:
      - dhcp
  - name: pr002
    ip: 172.16.0.3
    aliases:
      - directory
  - name: pr010
    ip: 172.16.0.10
    aliases:
      - inside
  - name: pr011
    ip: 172.16.0.11
    aliases:
      - files
      
#Hier geef je mee, naar welke interfaces er naar geluisterd moet worden. In ons geval moet er naar alle interfaces geluisterd worden.
bind_listen_ipv4:
  - any
  
#Hier geef je mee welke servers toegang hebben tot de DNS server. In ons geval hebben alle servers toegang.
bind_allow_query:
  - any
```
7. Nadat we de basis configuratie hebben uitgevoerd moeten we de mx record point voor de mail server configureren. Dit doe je door de volgende code toe te voegen aan "pu001.yml":
```Yaml
bind_zone_mail_servers:
  - name: pu003
    preference: 10
```
8. Als laatst gaan we de firewall configureren. Dit doen we gemakkelijk door de volgende code toe te voegen aan "pu001.yml"
```Yaml
rhbase_firewall_allow_services:
  - dns
```
9. Indien je alle bovenstaande stappen hebt uitgevoerd, open je de terminal en voer je de comando `vagrant provision` uit. Nu zal alles geïnstalleerd en geconfigureerd worden.
10. Nadat de installatie voltooid is, voer je de commando `vagrant ssh pu001` uit. Dankzij dit commando log je in, in het dns server. Na het inloggen voer je de commando `sudo /vagrant/test/runbats.sh` uit om de test van de server te starten. Indien alle testen slagen, is alles goed geconfigureerd en werkt de dns server zonder problemen.

### Slave DNS Server

1. Voeg de volgende code toe aan de document vagrant-hosts.yml: 
```Yaml
- name: pu002
  ip: 192.0.2.11
```
   Dit code komt onder pu004. Dankzij dit code laten we vagrant een server aanmaken met de naam pu002 en ip adres 192.0.2.11.
   
3. Voeg de volgende code toe aan site.yml, zodat de host aangemaakt en de roles "rh-base" (zorgt ervoor dat de basis installatie uitgevoerd wordt op de server) en "bind" (configureerd de server als DNS server) geïnstalleerd worden op de server pu002.
```Yaml
- hosts: pu002
  sudo: true
  roles:
    - bertvv.rh-base
    - bertvv.bind
```
4. Maak een bestand in het map Ansible -> host_vars genaamd "pu002.yml" aan. Dankzij dit bestand kunnen we de Slave DNS Server configureren.
5. Omdat we een slave DNS server gaan maken moeten we niet alle minimale vereisten van een dns server configreren. Het enige wat eigenlijk weg valt is de "bind_zone_hosts". Om de slave dns server te configureren moeten we de volgende code toevoegen in het bestand "pu001.yml":
```Yaml
#Hier wordt de domein naam geconfigureerd
bind_zone_name: avalon.lan

#Hier wordt de ip-adres van de DNS server geconfigureerd
bind_zone_master_server_ip: 192.0.2.10

#Hier worden de  netwerken dat in de domein zitten geconfigureerd. In ons geval zijn er twee verschillende domeinen, namelijk 192.0.2.0 en 172.16.0.0
bind_zone_networks:
  - "192.0.2"
  - "172.16"
  
#Hier worden de namen van de dns servers in het domein geconfigureerd. In ons domein hebben we twee dns servers met de namen pu001 (DNS server) en pu002 (slave dns server)
bind_zone_name_servers:
  - pu001
  - pu002
 
#Hier geef je mee, naar welke interfaces er naar geluisterd moet worden. In ons geval moet er naar alle interfaces geluisterd worden.
bind_listen_ipv4:
  - any
  
#Hier geef je mee welke servers toegang hebben tot de DNS server. In ons geval hebben alle servers toegang.
bind_allow_query:
  - any
```
6. Nadat de basis configuratie van de slave dns server geconfigureerd is gaan we de firewall configureren. Dit doen we gemakkelijk door de volgende code toe te voegen aan "pu002.yml"
```Yaml
rhbase_firewall_allow_services:
  - dns
```
7. Indien je alle bovenstaande stappen hebt uitgevoerd, open je de terminal en voer je de comando `vagrant up` en daarna `vagrant provision` uit. Nu zal alles geïnstalleerd en geconfigureerd worden.
8. Nadat de installatie voltooid is, voer je de commando `vagrant ssh pu002` uit. Dankzij dit commando log je in, in het dns server. Na het inloggen voer je de commando `sudo /vagrant/test/runbats.sh` uit om de test van de server te starten. Indien alle testen slagen, is alles goed geconfigureerd en werkt de dns server zonder problemen.
## Test report
### DNS Server

Na het uitvoeren van de test, krijg ik als uitvoer: 
```
Running test /vagrant/test/common.bats
 ✓ EPEL repository should be available
 ✓ Bash-completion should have been installed
 ✓ bind-utils should have been installed
 ✓ Git should have been installed
 ✓ Nano should have been installed
 ✓ Tree should have been installed
 ✓ Vim-enhanced should have been installed
 ✓ Wget should have been installed
 ✓ Admin user hilmi should exist
 ✓ Custom /etc/motd should have been installed

10 tests, 0 failures
Running test /vagrant/test/pu001/masterdns.bats
 ✓ The `dig` command should be installed
 ✓ The main config file should be syntactically correct
 ✓ The forward zone file should be syntactically correct
 ✓ The reverse zone files should be syntactically correct
 ✓ The service should be running
 ✓ Forward lookups public servers
 ✓ Forward lookups private servers
 ✓ Reverse lookups public servers
 ✓ Reverse lookups private servers
 ✓ Alias lookups public servers
 ✓ Alias lookups private servers
 ✓ NS record lookup
 ✓ Mail server lookup

13 tests, 0 failures
```
We kunnen dus concluderen dat alles goed geconfigureerd is en goed werkt.

### Slave DNS Server
Na het uitvoeren van de test, krijg ik als uitvoer: 

```
Running test /vagrant/test/common.bats
 ✓ EPEL repository should be available
 ✓ Bash-completion should have been installed
 ✓ bind-utils should have been installed
 ✓ Git should have been installed
 ✓ Nano should have been installed
 ✓ Tree should have been installed
 ✓ Vim-enhanced should have been installed
 ✓ Wget should have been installed
 ✓ Admin user hilmi should exist
 ✓ Custom /etc/motd should have been installed

10 tests, 0 failures
Running test /vagrant/test/pu002/slavedns.bats
 ✓ The `dig` command should be installed
 ✓ The main config file should be syntactically correct
 ✓ The server should be set up as a slave
 ✓ The server should forward requests to the master server
 ✓ There should not be a forward zone file
 ✓ The service should be running
 ✓ Forward lookups public servers
 ✓ Forward lookups private servers
 ✓ Reverse lookups public servers
 ✓ Reverse lookups private servers
 ✓ Alias lookups public servers
 ✓ Alias lookups private servers
 ✓ NS record lookup
 ✓ Mail server lookup

14 tests, 0 failures
```
We kunnen dus concluderen dat alles goed geconfigureerd is en goed werkt.

## Resources
1. https://github.com/hilmiemrebayat/elnx-sme/blob/master/doc/02-dns.md
2. https://galaxy.ansible.com/bertvv/bind/
3. https://github.com/hilmiemrebayat/elnx-sme/blob/master/README.md
4. https://galaxy.ansible.com/bertvv/rh-base/
