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

## Test report

## Resources
1. https://github.com/hilmiemrebayat/elnx-sme/blob/master/doc/02-dns.md
2. https://galaxy.ansible.com/bertvv/bind/
3. https://github.com/hilmiemrebayat/elnx-sme/blob/master/README.md
4. https://galaxy.ansible.com/bertvv/rh-base/
