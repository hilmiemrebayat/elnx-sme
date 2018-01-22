# Uitvoeren van demo presentatie

## Installeren van alle hosts
1. cd /Users/hilmiemrebayat/Documents/GitHub/elnx-sme
2. vagrant up

## Demostratie van pu004 (Lamp)
1. vagrant ssh pu004
2. sudo /vagrant/test/runbats.sh

## Demostratie van pu001 (DNS)
1. vagrant ssh pu001
2. sudo /vagrant/test/runbats.sh
3. dig @192.0.2.11 ns1.avalon.lan +short
4. dig @192.0.2.11 avalon.lan www.avalon.lan +short

## Demostratie van pu002 (Slave DNS)
1. vagrant ssh pu002
2. sudo /vagrant/test/runbats.sh
3. dig @192.0.2.10 ns1.avalon.lan +short
4. dig @192.0.2.10 avalon.lan www.avalon.lan +short

## Demostratie van pr011 (FileServer)
1. vagrant ssh pr011
2. sudo /vagrant/test/runbats.sh

## Demostratie van pr001 (DHCP en router)
Tijdens het testen van de DHCP server en router gaan we ook de LAMP, DNS en FileServer visueel testen.
### DHCP
1. Start de VM "Centos" in VirtualBox
2. Selecteer de taal Nederlands en klik op "try ubuntu"
3. Ga daarna naar de netwerk instellingen en bekijk of de twee interfaces automatisch een ip-adres hebben gekregen. 
- Eén interface moet een ip-adres binnen het range 172.16.192.1- 172.16.255.253 krijgen;
- En de andere binnen het range 172.16.128.1 - 172.16.191.254
### LAMP, DNS en router
1. Open Firefox en surf naar www.avalon.lan en naar www.google.be
2. Open Firefox en surf naar www.avalon.lan/wordpress
3. Vul de gevraagde gegevens in en log in op wordpress (www.avalon.lan/wordpress/wp-admin)
4. Surf via uw eigen pc naar http://192.0.2.50/wordpress/

### FileServer
1. Open Firefox en surf naar ftp://172.16.0.11
2. Log in met de volgende gegevens:


## Demostratie van Fail2Ban voor Wordpress
1. Surf via je pc naar http://192.0.2.50/wordpress/
2. Probeer in te loggen met verkeerde inloggegevens en daarna met de juiste.
3. Ga naar de plugin tabblad en activeer de plugin
4. Terminal:
- vagrant ssh pu004
- sudo cat /var/log/messages
- sudo fail2ban-client set wordpress banip <ip-adres dat staat in de logboek>
5. Surf via je pc opniew naar http://192.0.2.50/wordpress/
6. Terminal:
- sudo fail2ban-client set wordpress unbanip 192.0.2.1
7. Surf via je pc opniew naar http://192.0.2.50/wordpress/