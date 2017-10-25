# Enterprise Linux Lab Report

- Student name: Hilmi Emre Bayat
- Github repo: https://github.com/hilmiemrebayat/elnx-sme

  1. Volgende rol downloaden via Ansible en installeren op de server met de "vagrant provision"
    - bertvv.httpd (command om te installeren: $ ansible-galaxy install bertvv.httpd)
    - bertvv.mariadb (command om te installeren: $ ansible-galaxy install bertvv.mariadb)
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
1. 
## Test report

## Resources
