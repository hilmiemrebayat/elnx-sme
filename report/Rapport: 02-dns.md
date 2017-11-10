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
2. De volgende code toevoegen aan het document vagrant-hosts.yml: 
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
5. Nadat de installatie voltooid is zien we dat de installatie van role "bind" faild. Dit komt omdat we de master server ip adres niet hebben geconfigureerd. Hierdoor gaan we de DNS Server eerst congigureren en daarna laten installeren. Om dit te doen maak je eerst in het map Ansible -> host_vars een bestand "pu001.yml" aan. Dankzij dit bestand kunnen we DNS Server configureren.
6. Eerst maken we een domein aan met de naam "avalon.lan" zoals vermeld op de [volgende document](https://github.com/hilmiemrebayat/elnx-sme/blob/master/doc/02-dns.md). Hievoor voegen we in het bestand "pu001.yml" dat we in stap 5 hebben aangemaakt de volgende code toevoegen:
```Yaml
bind_zone_name: avalon.lan
```
7. Daarna voegen we ook de master server ip adres toe en verbinden we de twee verschillende ip adres zones met elkaar. Dit doe je door de volgende code toe te voegen aan "pu001.yml":
```Yaml
bind_zone_master_server_ip: 192.0.2.10

bind_zone_networks:
  - "192.0.2"
  - "172.16"

```
  Nadat je de bovenstaande code hebt toegevoegd, open je de terminal en doe je `vagrant provision`. Nu zal de dns server voor een deel geconfigureerd worden.

## Test report

## Resources
