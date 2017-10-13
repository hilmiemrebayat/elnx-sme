# Cheat sheets and checklists

- Student name: Hilmi Emre Bayat
- Github repo: https://github.com/hilmiemrebayat/elnx-sme/

## Basic commands

| Task                | Command |
| :---                | :---    |
| Query IP-adress(es) | `ip a`  |
| Status van vagrant opvragen | `vagrant status`  |
| Het verwijderen en ongedaan maken van de server | `vagrant destroy -f naam_van_server`  |
| Installeren van server | `vagrant up`  |
| Inloggen op de server via terminal of cmd | `vagrant ssh naam_server`  |
| Naar een gewenste map gaan | `cd ...`  |
| Mappen en bestanden bekijken in de directory | `ls`  |


## Git workflow

Simple workflow for a personal project without other contributors:

| Task                                         | Command                   |
| :---                                         | :---                      |
| Current project status                       | `git status`              |
| Select files to be committed                 | `git add FILE...`         |
| Commit changes to local repository           | `git commit -m 'MESSAGE'` |
| Push local changes to remote repository      | `git push`                |
| Pull changes from remote repository to local | `git pull`                |

## Checklist network configuration

1. Is the IP-adress correct? `ip a`
2. Is the router/default gateway correct? `ip r -n`
3. Is a DNS-server available? `cat /etc/resolv.conf`

