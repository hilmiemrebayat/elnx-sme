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
   
3. Voeg de volgende code toe aan site.yml, zodat de host aangemaakt en de roles "rh-base" en "bind" geïnstalleerd worden op de server pu001.
```Yaml
- hosts: pu001
  sudo: true
  roles:
    - bertvv.rh-base
    - bertvv.bind
```
4. 
## Test report

## Resources
