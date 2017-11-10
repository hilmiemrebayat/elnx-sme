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
 
## Test report

## Resources
