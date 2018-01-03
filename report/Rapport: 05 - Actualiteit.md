# Enterprise Linux Lab Report

- Student name: Hilmi Emre Bayat
- Github repo: https://github.com/hilmiemrebayat/elnx-sme

  1. Installeren van Fail2Ban en de plugin ervan in Wordpress met Ansible

## Test plan
1. Na de installatie surf je via uw eigen pc of via een nieuw aangemaakte VM naar http://192.0.2.50/wordpress 
2. Vul daarna de gevraagde aspecten in (Website naam, gebruikersnaam en wachtwoord)
3. Probeer daarna met de aangemaakte account en wachtwoord in te loggen via de link http://192.0.2.50/wordpress/wp-admin
4. Als dat gelukt is, log uit en probeer een aantal keren verkeerd in te loggen.
5. Open daarna je terminal en geef de commando `vagrant ssh pu004` in.
6. Geef daarna de commando `sudo cat /var/log/messages` in. Je zal een gelijkaardige uitvoer als hieronder moeten zien:
```
Jan  3 11:54:58 localhost wordpress(192.0.2.50)[13697]: Authentication failure for test from 192.0.2.1
Jan  3 11:55:00 localhost wordpress(192.0.2.50)[13697]: Authentication failure for test from 192.0.2.1
Jan  3 11:55:03 localhost wordpress(192.0.2.50)[13697]: Authentication failure for test from 192.0.2.1
Jan  3 11:55:05 localhost wordpress(192.0.2.50)[13697]: Authentication failure for test from 192.0.2.1
Jan  3 11:55:07 localhost systemd-logind: Removed session 6.
Jan  3 11:55:10 localhost wordpress(192.0.2.50)[13697]: Authentication attempt for unknown user greger from 192.0.2.1
Jan  3 11:55:19 localhost wordpress(192.0.2.50)[12375]: Authentication attempt for unknown user egregerg from 192.0.2.1

```
7. Normaal gezien wordt de ip-adres automatisch geblokkeerd na meerdere pogingen. Wij gaan het manueel blokkeren om het te kunnen simuleren.Dit doen we door de commando 'sudo fail2ban-client set wordpress banip 192.0.2.1' in te geven. De ip-adres dat in de commando staat komt uit de log bestand.
8. Probeer nu opnieuw te surfen naar de website in stap 1. Indien dit niet lukt, werkt fail2ban zonder problemen.
9. Als het gelukt is, gaan we de geblokkeerde ip-adres terug verwijderen met de commando `sudo fail2ban-client set wordpress unbanip 192.0.2.1`
## Procedure/Documentation
1. Download en installeer de rol fail2ban met de commando `ansible-galaxy install memiah.fail2ban-wordpress`
2. Open de bestand site.yml in de map ansible. 
3. Voeg de rol `memiah.fail2ban-wordpress` toe. De host pu004 moet er als volgt uit zien:
```
---
- hosts: pu004
  become: true
  roles:
    - bertvv.rh-base
    - bertvv.httpd
    - bertvv.mariadb
    - bertvv.wordpress
    - memiah.fail2ban-wordpress
```
4. Open hierna de bestand pu004.yml in de map Ansible -> host_vars
5. Voeg onderaan de volgende code toe:
```
#De naam van de plugin dat geïnstalleerd wordt
fail2ban_wordress_plugin_name: wp-fail2ban
#De versie van de plugin dat geïnstallerd wordt
fail2ban_wordress_plugin_version: "3.0.0"
#Locatie waarin de plugin geinstalleerd moet worden
fail2ban_wordress_mu_plugins_dir: /usr/share/wordpress/wp-content/plugins
#
fail2ban_wordress_mu_plugins_dir: false
#Fail2ban jail config settings.
fail2ban_wordress_jail_config:
  logpath: /var/log/messages
  maxretry: 3
  bantime: 21600
  findtime: 86400
```
6. Open de terminal en voer de commando `vagrant provision pu004` uit. Zodat fail2ban geinstalleerd en geconfigureerd wordt.
## Test report

## Resources
1. https://galaxy.ansible.com/memiah/fail2ban-wordpress/
2. https://wordpress.org/plugins/wp-fail2ban/
