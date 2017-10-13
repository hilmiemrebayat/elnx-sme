# Enterprise Linux Lab Report

- Student name: Hilmi Emre Bayat
- Github repo: https://github.com/hilmiemrebayat/elnx-sme

  1. Code ophalen uit GitHub en server installeren op VirtualBox met "vagrant up"
  2. Volgende rol downloaden via Ansible en installeren op de server met de "vagrant provision"
    - bertvv.rh-base (command om te installeren: $ ansible-galaxy install bertvv.rh-base)
  3. Testplan uitvoeren dat is geschreven door de leerkracht
  4. Testrapport uitschrijven

## Test plan


1. On the host system, go to the local working directory of the project repository
2. Execute `vagrant status`
    - There should be one VM, `pu004` with status `not created`. If the VM does exist, destroy it first with `vagrant destroy -f pu004`
3. Execute `vagrant up pu004`
    - The command should run without errors (exit status 0)
4. Log in on the server with `vagrant ssh pu004` and run the acceptance tests. They should succeed

    ```
    [vagrant@pu004 test]$ sudo /vagrant/test/runbats.sh
    Running test /vagrant/test/common.bats
    ✓ EPEL repository should be available
    ✓ Bash-completion should have been installed
    ✓ bind-utils should have been installed
    ✓ Git should have been installed
    ✓ Nano should have been installed
    ✓ Tree should have been installed
    ✓ Vim-enhanced should have been installed
    ✓ Wget should have been installed
    ✓ Admin user bert should exist
    ✓ Custom /etc/motd should be installed

    10 tests, 0 failures
    ```

    Any tests for the LAMP stack may fail, but this is not part of the current assignment.

5. Log off from the server and ssh to the VM as described below. You should **not** get a password prompt.

    ```
    $ ssh bert@192.0.2.50
    Welcome to pu004.localdomain.
    enp0s3     : 10.0.2.15         fe80::a00:27ff:fe5c:6428/64
    enp0s8     : 192.0.2.50        fe80::a00:27ff:fecd:aeed/64
    [bert@pu004 ~]$
    ```
 

## Procedure/Documentation
1. Download de rol rh-base via ansible met de volgende commando: "ansible-galaxy install bertvv.rh-base". Hiermee wordt de basis installatie van een server uitgevoerd.
2. Open de bestand "site.yml" en voeg de box toe door de volgende commando toe te voegen onder roles: bertvv.rh-base. Site.yml moet als volgt te zien zijn:
```
# site.yml
---
- hosts: pu004
  become: true
  roles:
    - bertvv.rh-base
```
3. Open daarna de bestand all.yml en voeg de packages die je wilt installeren, firewalls die je wilt configureren en de gebruikers toe. In mijn geval ziet de bestand er zo uit:
```
# group_vars/all.yml
# Variables visible to all nodes
---
rhbase_motd: true
rhbase_repositories:
 - epel-release
rhbase_install_packages:
 - bash-completion
 - bind-utils
 - vim-enhanced
 - git
 - nano
 - tree
 - wget
rhbase_firewall_allow_services:
 - http
 - https
rhbase_admin_user: hilmi
rhbase_admin_ssh_key: ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQC2bRpvOGm2Rvvta4avhCjYUqEn09UFEK/WGNbq/yr6edO7cQGlbGw/0+/kT/lGWOA6xx2HPOqfse7QntLtgO1fkJR2EvYn66gJ27e7uwgBs60ElYfMuOEox1KeuDq8RlkDWLapwatxzSs4pj8N+KJd+kG0Zxl5NDGfJc1Ntz92EK5aQ0arrLJ4pgw/YpwfQ9fazfX20MOZpvJ0zRwuMoENv46Byo66XGOwNul9XA3tGj2yTDyVfcQYzzh+S4sBvF0zMoJJ1YEd3heSvLPpJHcJqfwZ0SBLvfP4nC9JtAUin3g3iNPFcUdEr2O6hrbIf2CooFXvCyW/eR5SsUexMorn hilmiemrebayat@MacBook-Pro-van-Hilmi.local
rhbase_users:
  - name: hilmi
    comment: 'Hilmi Emre Bayat'
    groups:
      - users
      - wheel
```
4. Nadat je de bovenstaande bestanden hebt gewijzigd, open de bestand comands.bats en wijzig de code op lijn 4 naar je eigen gebruikersnaam dat je hebt aangemaakt in de vorige stap ("admin_user=hilmi").
5. Nu is de configuratie af. Open de terminal, ga naar de map waar de vagrantfile staat met cd ..., type vagrant up in en druk op enter. Indien je vagrant up al hebt uitgevoerd, geef dan direct vagrant provision in om de laatste wijzigingen uit te laten voeren.
6. Nadat de installatie klaar is, voer de volgende commando uit om te testen of alles juist is geïnstalleerd en geconfigureerd: "sudo /vagrant/test/runbats.sh".


## Test report

Na het uitvoeren van de testplan krijg ik als uitvoer de volgende:
1. Uitvoer na het uitvoeren van de volgende commando: sudo /vagrant/test/runbats.sh
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
```
2. Uitvoer na het inloggen met ssh, er wordt geen wachtwoord gevraagd:
```
DEPRECATION: The 'sudo' option for the Ansible provisioner is deprecated.
Please use the 'become' option instead.
The 'sudo' option will be removed in a future release of Vagrant.

Last login: Fri Oct  6 09:52:38 2017 from 192.0.2.1
Welcome to pu004..
enp0s3     : 10.0.2.15         fe80::f43c:c038:d3c2:fd73/64  
enp0s8     : 192.0.2.50        fe80::a00:27ff:fe07:160f/64  
```
De test is dus succesvol en alles is goed geïnstalleerd en geconfigureerd.
## Resources

1. Downloaden van rh-base: https://galaxy.ansible.com/bertvv/rh-base/
2. rh-base role configuratie: https://github.com/bertvv/ansible-role-rh-base/blob/master/README.md
