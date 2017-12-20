# Evaluatie Enterprise Linux

| :---          | :---                                         |
| i Student     | Hilmi BAYAT                                  |
| Klasgroep     | gent                                         |
| Email         | <mailto:hilmi.bayat.v5278@student.hogent.be> |
| Hoofdopdracht | SME                                          |
| Repo          | <git@github.com:hilmiemrebayat/elnx-sme.git> |

## Hoofdopdracht

W4:

- bezig met LAMP, tandje bijsteken!
- MariaDB net opgezet (fout in mariadb_databases-variabele)
- Nu Wordpress opzetten

W9:

- Bezig met fileserver: Samba af, Vsftpd nog niet
    - Uitleg gegeven over verifiëren filepermissies, FTP-requests met curl
    - admin-wachtwoord juist zetten in de opgave
    - Voeg acl's toe (post_tasks: in site.yml) om laatste permissieproblemen op te lossen

### Eindbeoordeling

O1: <BEOORDELING>

## Troubleshooting

### Eerste troubleshooting-opdracht

- Datalinklaag:
    - Controleer ook op *welke* HO-interface de VM is aangesloten en (op niveau Internetlaag) of de IP instellingen van die interface consistent zijn.
- Internetlaag
    - Controleer ook of je kan pingen van hostsysteem naar de VM!
- Transportlaag:
    - Firewall-instellingen: gebruik enkel `--add-service`, niet ook `--add-port`
- Resultaat:
    - Je schrijft "de server is bereikbaar", maar geeft niet aan hoe je dat kan aantonen. Je zegt dat je geen tekst ziet maar wat wel? Komt er een foutboodschap?

Grondig verslag, houden zo! Het is niet nodig er een doorlopende tekst van te maken. Telegramstijl met enkel de nodige informatie is voldoende.

Beoordeling voor deze opdracht: nog niet bekwaam omdat niet aangetoond is dat de server bereikbaar is vanaf het hostsysteem

### Tweede troubleshooting-opdracht

In je uitvoer van `nslookup` (regel 117-157 in je verslag) komt tot uiting dat je de verkeerde DNS-server ondervraagd hebt!

- Transportlaag:
    - Geen controle op poortnummer/interface (`ss`)
    - Geen controle op firewall-instellingen
- Applicatielaag:
    - Het controleren of de zone-instellingen correct zijn hoort thuis op de applicatielaag ipv de transportlaag

Uit het verslag blijkt wel dat verschillende fouten correct gevonden zijn, maar omdat een aantal controles vergeten zijn, is de service niet bereikbaar voor gebruikers.

Beoordeling voor deze opdracht: nog niet bekwaam

### Derde troubleshooting-opdracht



### Eindbeoordeling

O2: <BEOORDELING>

## Opdracht Actualiteit

### Eindbeoordeling

O3: <BEOORDELING>

## Rapportering

### Laboverslagen

R1: <BEOORDELING>

### Demonstraties

R2: <BEOORDELING>

### Cheat sheet

R3: <BEOORDELING>

