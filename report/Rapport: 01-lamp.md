# Enterprise Linux Lab Report

- Student name: Hilmi Emre Bayat
- Github repo: https://github.com/hilmiemrebayat/elnx-sme

  1. Lamp server maken door de volgende rollen te downloaden via Ansible en te installeren op de server met "vagrant provision"
    - bertvv.httpd (command om te installeren: $ ansible-galaxy install bertvv.httpd)
    - bertvv.mariadb (command om te installeren: $ ansible-galaxy install bertvv.mariadb)
    - bertvv.wordpress (command om te installeren: $ ansible-galaxy install bertvv.wordpress)
  2. Testplan uitvoeren dat is geschreven door de leerkracht
  3. Testrapport uitschrijven

## Test plan

All machines in the setup can be tested by executing the following command:

```console
$ sudo /vagrant/test/runbats.sh
```

In the test script `test/pu004/lamp.bats`, you may want to change the variables in the test script to the values you have used in your configuration script:

```bash
mariadb_root_password=fogMeHud8
wordpress_database=wp_db
wordpress_user=wp_user
wordpress_password=CorkIgWac
```

## Procedure/Documentation
1. Download de rol bertvv.httpd, bertvv.mariadb en bertvv.wordpress via ansible met de volgende commandos: "ansible-galaxy install bertvv.httpd", "ansible-galaxy install bertvv.mariadb" en "ansible-galaxy install bertvv.wordpress". Hiermee wordt Apache webserver en MariaDB geïnstalleerd op de server.
2. Voeg de volgende code bij roles toe in site.yml om apache en mariadb te installeren op de server: "bertvv.httpd" en "bertvv.mariadb". Site.yml moet als volgt gewijzigd worden:
```
# site.yml
---
- hosts: pu004
  become: true
  roles:
    - bertvv.rh-base
    - bertvv.httpd
    - bertvv.mariadb
    - bertvv.wordpress
```
3. Daarna moet men een map host_vars en daarin een bestand pu004.yml aanmaken in de map ansible. Na het aanmaken voegt men de volgende code toe in pu004.yml (Normaalgezien kunnen we het volgende code ook toevoegen in all.yml maar omdat we meerdere servers gaan installeren en niet alle servers mariadb nodig hebben moeten we het in een andere bestand zetten).
```
# puOO4.yml
#Firewall configureren
rhbase_firewall_allow_services:
 - http
 - https
# aanmaken van een database genaamd wordpressdatabank
mariadb_databases:
  - name: wordpressdatabank
#Aanmeken van een gebruiker voor de databank
mariadb_users:
  - name: hilmiemrebayat
    password: P@ssword
    priv: '*.*:ALL,GRANT'
#root wachtwoord van mariadb wijzigen
mariadb_root_password: P@sswordRoot
#Wordpress configureren
wordpress_database: wordpressdatabank
wordpress_user: hilmiemrebayat
wordpress_password: P@ssword
```
4. Maak nu een certificaat aan voor de server. Dit kan je doen door de stappen op het volgende website te volgen (Je moet de stappen vanaf titel "Create A Certificate (Done Once Per Device)" volgen): https://datacenteroverlords.com/2012/03/01/creating-your-own-ssl-certificate-authority/

5. Na het aanmaken van de server maak een map "certificaten" aan in de map "ansible". Voeg daarin de aangemaakte certificaten toe in de vorige stap.
6. Nadat je de certificaten hebt toegevoegd pas site.yml en pu004.yml aan als volgt:
- pu004.yml
```
# puOO4.yml
#Firewall configureren
rhbase_firewall_allow_services:
    - http
    - https
# aanmaken van een database genaamd wordpressdatabank
mariadb_databases:
  - name: wordpressdatabank
#Aanmeken van een gebruiker voor de databank
mariadb_users:
  - name: hilmiemrebayat
    password: P@ssword
    priv: '*.*:ALL,GRANT'
#root wachtwoord van mariadb wijzigen
mariadb_root_password: P@sswordRoot
#Wordpress configureren
wordpress_database: wordpressdatabank
wordpress_user: hilmiemrebayat
wordpress_password: P@ssword

httpd_SSLCertificateFile: /etc/pki/tls/certs/ca.crt
httpd_SSLCertificateKeyFile: /etc/pki/tls/private/ca.key

```
- site.yml
```
# site.yml
---
- hosts: pu004
  become: true
  roles:
    - bertvv.rh-base
    - bertvv.httpd
    - bertvv.mariadb
    - bertvv.wordpress
  pre_tasks:
    - name: Webserver private key kopieren
      copy:
        src: certificaten/device.key
        dest: /etc/pki/tls/private/ca.key
    - name: Web server certificaat kopieren
      copy:
        src: certificaten/device.crt
        dest: /etc/pki/tls/certs/ca.crt

```
7. Open de terminal, ga naar de map waar de vagrantfile staat met "cd ..." en geef "vagrant provision" in als commando om de installatie uit te voeren.



8. Wijzig de bestand lamp.bats in de map test/pu004/ als volgt:
```
#! /usr/bin/env bats
#
# Author: Bert Van Vreckem <bert.vanvreckem@gmail.com>
#
# Acceptance test script for a LAMP stack with Wordpress

#{{{ Helper Functions


#}}}
#{{{ Variables
sut=192.0.2.50
mariadb_root_password=P@sswordRoot
wordpress_database=wordpressdatabank
wordpress_user=hilmiemrebayat
wordpress_password=P@ssword
#}}}

# Test cases

@test 'The necessary packages should be installed' {
  rpm -q httpd
  rpm -q MariaDB-server
  rpm -q wordpress
  rpm -q php
  rpm -q php-mysql
}

@test 'The Apache service should be running' {
  systemctl status httpd.service
}

@test 'The Apache service should be started at boot' {
  systemctl is-enabled httpd.service
}

@test 'The MariaDB service should be running' {
  systemctl status mariadb.service
}

@test 'The MariaDB service should be started at boot' {
  systemctl is-enabled mariadb.service
}

@test 'The SELinux status should be ‘enforcing’' {
  [ -n "$(sestatus) | grep 'enforcing'" ]
}

@test 'Web traffic should pass through the firewall' {
  firewall-cmd --list-all | grep 'services.*http\b'
  firewall-cmd --list-all | grep 'services.*https\b'
}

@test 'Mariadb should have a database for Wordpress' {
  mysql -uroot -p${mariadb_root_password} --execute 'show tables' ${wordpress_database}
}

@test 'The MariaDB user should have "write access" to the database' {
  mysql -u${wordpress_user} -p${wordpress_password} \
    --execute 'CREATE TABLE a (id int); DROP TABLE a;' \
    ${wordpress_database}
}

@test 'The website should be accessible through HTTP' {
  # First check whether port 80 is open
  [ -n "$(ss -tln | grep '\b80\b')" ]

  # Fetch the main page (/) with Curl/
  #  - If the site is not up, curl will exit with an error and the test will fail
  #  - If the site is up, but the index page cannot be found, ${result} will be nonempty
  run curl --silent "http://${sut}/"
  [ -z "$(echo ${output} | grep 404)" ]
}

@test 'The website should be accessible through HTTPS' {
  # We're just checking if port 443 is open here. If HTTP runs and serves pages, HTTPS should as well
  # We check the open TCP server ports with ss and check if port 443 is listed.
  [ -n "$(ss -tln | grep '\b443\b')" ]
}

@test 'The certificate should not be the default one' {
  # Fetch the server certificate
  run openssl s_client -showcerts -connect ${sut}:443 < /dev/null

  [ "0" -eq "${status}" ]

  # The default certificate for Apache has "SomeOrganization" as the organization
  # So grepping it in the output of the openssl command should return an empty string
  # if a self-signed certificate was installed by the administrator.
  [ -z "$(echo ${output} | grep SomeOrganization)" ]
}

@test "The Wordpress install page should be visible under http://${sut}/wordpress/" {
  [ -n "$(curl --silent --location http://${sut}/wordpress/ | grep '<title>WordPress')" ]
}

@test 'MariaDB should not have a test database' {
  run mysql -uroot -p${mariadb_root_password} --execute 'show tables' test
  [ "0" -ne "${status}" ]
}

@test 'MariaDB should not have anonymous users' {
  result=$(mysql -uroot -p${mariadb_root_password} --execute "select * from user where user='';" mysql)
  [ -z "${result}" ]
}

```
9. Nadat de installatie klaar is, voer de volgende commando uit om te testen of alles juist is geïnstalleerd en geconfigureerd: "sudo /vagrant/test/runbats.sh".


## Test report
Na het uitvoeren van de commando "sudo /vagrant/test/runbats.sh" komt er als uitvoer het volgende uit:
```
[vagrant@pu004 ~]$ sudo /vagrant/test/runbats.sh
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
Running test /vagrant/test/pu004/lamp.bats
 ✓ The necessary packages should be installed
 ✓ The Apache service should be running
 ✓ The Apache service should be started at boot
 ✓ The MariaDB service should be running
 ✓ The MariaDB service should be started at boot
 ✓ The SELinux status should be ‘enforcing’
 ✓ Web traffic should pass through the firewall
 ✓ Mariadb should have a database for Wordpress
 ✓ The MariaDB user should have "write access" to the database
 ✓ The website should be accessible through HTTP
 ✓ The website should be accessible through HTTPS
 ✓ The certificate should not be the default one
 ✓ The Wordpress install page should be visible under http://192.0.2.50/wordpress/
 ✓ MariaDB should not have a test database
 ✓ MariaDB should not have anonymous users

15 tests, 0 failures
```
Hieruit kunnen we besluiten dat de test succesvol is uitgevoerd en alles goed is geconfigureerd.
## Resources

1. Downloaden van httpd: https://galaxy.ansible.com/bertvv/httpd/
2. Downloaden van mariadb: https://galaxy.ansible.com/bertvv/mariadb/
3. Downloaden van wordpress: https://galaxy.ansible.com/bertvv/wordpress/
4. httpd role configuratie: https://github.com/bertvv/ansible-role-httpd
5. mariadb role configuratie: https://github.com/bertvv/ansible-role-mariadb en https://github.com/nickjj/ansible-mariadb
6. Wordpress role configuratie: https://github.com/bertvv/ansible-role-wordpress
7. Aanmaken certificaat: https://datacenteroverlords.com/2012/03/01/creating-your-own-ssl-certificate-authority/
