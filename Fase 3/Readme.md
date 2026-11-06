Fase 3 – Exportació NFS i root_squash

1. Objectiu

En aquesta fase s'ha configurat l'exportació d'un directori del servidor Ubuntu Server perquè pugui ser accessible des de la xarxa.

També s'ha comprovat el funcionament de root_squash i no_root_squash.

2. Configuració de l'exportació

S'ha editat el fitxer:

sudo nano /etc/exports

S'ha configurat el directori admin_tools:

/srv/nfs/admin_tools 192.168.56.0/24(rw,sync,root_squash)

Els paràmetres utilitzats són:

192.168.56.0/24: xarxa autoritzada.

rw: permet lectura i escriptura.

sync: confirma les operacions després d'escriure les dades.

root_squash: transforma les peticions del root remot en un usuari sense privilegis de root.

3. Aplicació de la configuració

sudo exportfs -ra
sudo exportfs -v

exportfs -ra aplica les exportacions configurades.

exportfs -v permet comprovar les carpetes exportades i els seus paràmetres.

4. Configuració del client

Al client Zorin OS s'ha instal·lat el suport NFS:

sudo apt install nfs-common

S'ha creat el punt de muntatge:

sudo mkdir -p /mnt/admin_tools

I s'ha muntat el recurs:

sudo mount 192.168.56.102:/srv/nfs/admin_tools /mnt/admin_tools

La comprovació s'ha realitzat amb:

mount | grep admin_tools

5. Prova de root_squash

S'ha comprovat que un usuari root del client no pot actuar com a root real del servidor.

La configuració root_squash evita que els privilegis de root del client es traslladin directament al servidor.

6. Comparació amb no_root_squash

S'ha modificat temporalment l'exportació per utilitzar:

/srv/nfs/admin_tools 192.168.56.0/24(rw,sync,no_root_squash)

Després d'aplicar la configuració, s'ha comprovat que les operacions de root del client es realitzaven amb UID 0 al servidor.

Aquesta configuració és menys segura perquè permet conservar els privilegis de root del client.

Finalment, s'ha restaurat root_squash.

7. Captures

Les captures documenten:

El muntatge del recurs NFS.

La denegació d'accés amb root_squash.

La comparació amb no_root_squash.

8. Conclusió

S'ha comprovat que NFS permet compartir directoris entre màquines i que root_squash és una mesura de seguretat important per evitar que el root del client tingui privilegis de root al servidor.