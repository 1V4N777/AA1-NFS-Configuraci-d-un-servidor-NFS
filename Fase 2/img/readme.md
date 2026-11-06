Fase 2 – Usuaris, grups i permisos

1. Objectiu

En aquesta fase s'ha preparat l'estructura d'usuaris, grups i directoris del servidor Ubuntu Server per controlar l'accés als recursos compartits mitjançant NFS.

2. Creació dels grups

S'han creat dos grups per separar els permisos:

devs: grup destinat als desenvolupadors.

admins: grup destinat als administradors.

sudo groupadd devs
sudo groupadd admins

Els grups permeten assignar permisos a diferents tipus d'usuaris sense haver de configurar cada usuari individualment.

3. Creació dels usuaris

S'han creat els usuaris dev01 i admin01.

sudo useradd -m -s /bin/bash dev01
sudo useradd -m -s /bin/bash admin01

Després s'han incorporat als grups corresponents:

sudo usermod -aG devs dev01
sudo usermod -aG admins admin01

La finalitat és separar els permisos dels desenvolupadors i dels administradors.

4. Creació dels directoris

S'han creat els directoris compartits:

sudo mkdir -p /srv/nfs/dev_projects
sudo mkdir -p /srv/nfs/admin_tools

Cada directori està destinat a un grup diferent.

5. Assignació de propietaris i permisos

sudo chown root:devs /srv/nfs/dev_projects
sudo chown root:admins /srv/nfs/admin_tools

S'han aplicat permisos 2770:

sudo chmod 2770 /srv/nfs/dev_projects
sudo chmod 2770 /srv/nfs/admin_tools

El permís especial 2 activa el bit setgid. Això permet que els fitxers creats dins del directori heretin el grup del directori.

Els permisos 770 permeten accés complet al propietari i al grup, però no als altres usuaris.

6. Comprovació

ls -ld /srv/nfs/dev_projects
ls -ld /srv/nfs/admin_tools

S'ha comprovat que els directoris pertanyen a root i als grups corresponents.

7. Captures

Les captures de pantalla d'aquesta fase documenten la creació dels grups, els usuaris, els directoris i els permisos.

8. Conclusió

La configuració permet separar els recursos dels desenvolupadors i dels administradors. Aquesta estructura facilita la gestió dels permisos i prepara el servidor per compartir els directoris mitjançant NFS.