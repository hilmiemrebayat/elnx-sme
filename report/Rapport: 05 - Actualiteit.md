# Enterprise Linux Lab Report

- Student name: Hilmi Emre Bayat
- Github repo: https://github.com/hilmiemrebayat/elnx-sme

  1. Installeren van Fail2Ban en de plugin ervan in Wordpress met Ansible

## Test plan
1. Na de installatie surf je via uw eigen pc of via een nieuw aangemaakte VM naar http://192.0.2.50/wordpress 
2. Vul daarna de gevraagde aspecten in (Website naam, gebruikersnaam en wachtwoord)
3. Probeer daarna met de aangemaakte account en wachtwoord in te loggen.
4. Als dat gelukt is, log uit en probeer een aantal keren verkeerd in te loggen.
5. Open daarna je terminal en geef de commando `vagrant ssh pu004` in.
6. Geef daarna de commando `sudo cat /var/log/messages` in. Je zal een geleijkaardige uitvoer als hieronder meoten zien:
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
### DHCP Server


## Test report

## Resources
1. https://galaxy.ansible.com/bertvv/dhcp/
2. https://wiki.vyos.net/wiki/User_Guide
3. https://wiki.vyos.net/wiki/Basic_setup
4. https://www.youtube.com/watch?v=mX_KIrSb6mE
5. https://www.youtube.com/watch?v=S1bJpsS27Qk
6. https://www.youtube.com/watch?v=bfQWY3XY3SI
