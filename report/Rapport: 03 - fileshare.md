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

1. Log in op pr011 via terminal met `vagrant ssh pr011` en voer dan de code `sudo /vagrant/test/runbats.sh` uit.


## Procedure/Documentation

1. Pas vagrant-host.yml file aan door de volgende code onderaan toe te voegen. Hiermee wordt de file server met naam pr011,  ip-adres 172.16.0.11 en netmask 255.255.0.0 aangemaakt in het VM:
```Yaml
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
```Yaml
- hosts: pr011
  sudo: true
  roles:
    - bertvv.rh-base
    - bertvv.samba
    - bertvv.vsftpd
```
6. Voer 'vagrant provision' uit met de terminal zodat de rollen geïnstalleerd en geconfigureerd worden op de server.
7. Nadat alle stappen hierboven uitgevoerd zijn, gaan we de samba fileserver configureren. Dit doe je door de volgende code's toe te voegen in het file pr011.yml:
```Yaml
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

#Groepen aanmaken
rhbase_user_groups:
  - management
  - technical
  - sales
  - it

#Groepen configureren
samba_shares:
  - name: management
    comment: ''
    write_list: +management
    valid_users: +management
    group: management
    directory_mode: "0770"
  - name: technical (software development)
    comment: ''
    write_list: +technical
    valid_users: +technical,+management,+sales,+it
    group: technical
  - name: sales
    comment: ''
    write_list: +sales
    valid_users: +sales,+management
    group: sales
  - name: it (system administration)
    comment: ''
    write_list: +it
    valid_users: +it,+management
    group: it
  - name: public
    comment: ''
    public: yes
    write_list: +users
    valid_users: +users
    setype: samba_share_t
    
#Gebruikers aanmaken
rhbase_users:
  - name: hilmiemrebayat
    comment: 'Hilmi Emre Bayat'
    groups:
      - it
      - users
      - wheel
    password: $1$mMxE8dRG$Y9phRhDspuV5..zQivoMy.s
    shell: /bin/bash
  - name: stevenh
    comment: 'Steven Hermans'
    groups:
      - management
      - users
    password: $1$Vy3cB8iE$HVbFHLIOEQQRJ469i1GS/.
  - name: stevenv
    comment: 'Steven Van Daele'
    groups:
      - technical
      - users
    password: $1$xzX1RLnU$KvIIJ4F3OadUqiLRpunG9/
  - name: leend
    comment: 'Leen De Meester'
    groups:
      - technical
      - users
    password: $1$3W7/vvOc$pjmWAHK36Wa36BeqOZV8B0
  - name: svena
    comment: 'Sven Aerens'
    groups:
      - sales
      - users
    password: $1$C0mSTPaQ$k/W6AH7O/nN17Fp58RXs50
  - name: nehirb
    comment: 'Nehir Baayar'
    groups:
      - it
      - users
    password: $1$0NP6tmlz$T5X0xMjrGYwr5Rozr1cAo.
  - name: alexanderd
    comment: 'Alexander De Coninck'
    groups:
      - technical
      - users
    password: $1$tIuyQqOK$LriOgQ3SsrVY3DxcXNKRO.
  - name: krisv
    comment: 'Kris Vanhavrbeke'
    groups:
      - management
      - users
    password: $1$HRUhSVNO$BxeYnERhwZrNg5QPEJQwW0
  - name: benoitp
    comment: 'Benoit Pluquet'
    groups:
      - sales
      - users
    password: $1$67zwncnI$i/OiL4MzItFG0fs/qCEO1/
  - name: anc
    comment: 'An Coppens'
    groups:
      - technical
      - users
    password: $1$f/SZqeA6$4BQsj25md68p6OK1NkPS5/
  - name: elenaa
    comment: 'Elena Andreev'
    groups:
      - management
      - users
    password: $1$1UWg/P4T$9yISTY1x4LiqzbDLOFR8y0
  - name: evyt
    comment: 'Evy Tileman'
    groups:
      - technical
      - users
    password: $1$eXmADiC9$RK92PuP1a9M.RhlQCwwrL/
  - name: christophev
    comment: 'Christophe Van der Meersche'
    groups:
      - it
      - users
    password: $1$2Og0dziu$gyBbo8.b8QS/zgUoybx9C0
    shell: /bin/bash
  - name: stefaanv
    comment: 'Stefaan Vernimmen'
    groups:
      - technical
      - users
    password: $1$sdEl3gnb$CzZrYalXHOr3rEQZ.Lh5C1
    
#workgroup definiëren
samba_workgroup: avalon

#gebruikers met wachtwoord maken voor samba 
samba_users:
  - name: hilmiemrebayat
    password: hilmiemrebayat
  - name: stevenh
    password: stevenh
  - name: stevenv
    password: stevenv
  - name: leend
    password: leend
  - name: svena
    password: svena
  - name: nehirb
    password: nehirb
  - name: alexanderd
    password: alexanderd
  - name: krisv
    password: krisv
  - name: benoitp
    password: benoitp
  - name: anc
    password: anc
  - name: elenaa
    password: elenaa
  - name: evyt
    password: evyt
  - name: christophev
    password: christophev
  - name: stefaanv
    password: stefaanv

```
8. Voer 'vagrant provision' uit via de terminal zodat de samba server geconfigureerd wordt.

## Test report

## Resources
1. https://galaxy.ansible.com/bertvv/samba/
2. https://www.samba.org/samba/docs/man/manpages-3/smb.conf.5.html
3. http://permissions-calculator.org
4. https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/6/html/managing_confined_services/sect-managing_confined_services-configuration_examples-sharing_files_between_services
5. https://galaxy.ansible.com/bertvv/rh-base/
6. https://www.mkpasswd.net/index.php
7. https://galaxy.ansible.com/bertvv/vsftpd/
