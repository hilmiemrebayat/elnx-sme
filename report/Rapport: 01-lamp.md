# Enterprise Linux Lab Report

- Student name: Hilmi Emre Bayat
- Github repo: https://github.com/hilmiemrebayat/elnx-sme

  1. Volgende rol downloaden via Ansible en installeren op de server met de "vagrant provision"
    - bertvv.httpd (command om te installeren: $ ansible-galaxy install bertvv.httpd)
  2. Testplan uitvoeren dat is geschreven door de leerkracht
  3. Testrapport uitschrijven

## Test plan

In the test script `test/pu004/lamp.bats`, you may want to change the variables in the test script to the values you have used in your configuration script:

```bash
mariadb_root_password=fogMeHud8
wordpress_database=wp_db
wordpress_user=wp_user
wordpress_password=CorkIgWac
```

## Procedure/Documentation
1. Download de rol bertvv.httpd via ansible met de volgende commando: "ansible-galaxy install bertvv.httpd". Hiermee wordt Apache webserver geïnstalleerd op de server.
2. Voeg de volgende code bij roles toe in site.yml om apache te installeren op de server: "bertvv.httpd". Site.yml moet als volgt te zien zijn:
```
# site.yml
---
- hosts: pu004
  become: true
  roles:
    - bertvv.rh-base
    - bertvv.httpd
```
3. Open de terminal, ga naar de map waar de vagrantfile staat met "cd ..." en geef "vagrant provision" in als commando om de installatie uit te voeren.




6. Nadat de installatie klaar is, voer de volgende commando uit om te testen of alles juist is geïnstalleerd en geconfigureerd: "sudo /vagrant/test/runbats.sh".


## Test report

## Resources

1. Downloaden van rh-base: https://galaxy.ansible.com/bertvv/httpd/
2. httpd role configuratie: https://github.com/bertvv/ansible-role-httpd
