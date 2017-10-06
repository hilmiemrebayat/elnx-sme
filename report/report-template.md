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



## Test report

The test report is a transcript of the execution of the test plan, with the actual results. Significant problems you encountered should also be mentioned here, as well as any solutions you found. The test report should clearly prove that you have met the requirements.

## Resources

1. Downloaden van rh-base: https://galaxy.ansible.com/bertvv/rh-base/
