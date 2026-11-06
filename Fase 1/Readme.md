Fase 1 – Instal·lació i preparació del servidor NFS

1. Objectiu

L'objectiu d'aquesta fase és preparar l'entorn de treball per implementar un servidor NFS (Network File System) amb Ubuntu Server i un client Zorin OS virtualitzats amb VirtualBox.

NFS permet compartir directoris entre diferents equips a través de la xarxa, de manera que els clients poden accedir als fitxers emmagatzemats al servidor.
2. Entorn de treball

S'han utilitzat dues màquines virtuals:

Servidor: Ubuntu Server.

Client: Zorin OS.

Virtualització: Oracle VirtualBox.

Xarxa de comunicació: xarxa interna de tipus Host-Only.

Les adreces IP utilitzades són:

Equip

Adreça IP

Ubuntu Server

192.168.56.102

Zorin OS

192.168.56.104

La xarxa utilitzada és 192.168.56.0/24.

3. Preparació del servidor

S'ha preparat Ubuntu Server per funcionar com a servidor de recursos compartits.

Per comprovar la configuració de xarxa s'utilitza:

ip a

Aquest comandament mostra les interfícies de xarxa i les adreces IP assignades.

4. Actualització del sistema

Abans d'instal·lar el servei, s'actualitzen els índexs dels paquets:

sudo apt update

Aquest comandament consulta els repositoris configurats i actualitza la informació dels paquets disponibles.

5. Instal·lació del servei NFS

Al servidor Ubuntu s'instal·la el paquet corresponent:

sudo apt install nfs-kernel-server

Aquest paquet proporciona el servei necessari per exportar directoris i compartir-los amb altres equips de la xarxa.

6. Comprovació del servei

Per comprovar que el servei està funcionant:

sudo systemctl status nfs-kernel-server

Aquesta ordre mostra l'estat del servei. L'estat active (exited) o un estat actiu equivalent pot aparèixer segons la configuració del sistema i de les unitats de servei.

7. Preparació de l'estructura de directoris

S'utilitza /srv/nfs com a ubicació per als recursos que es compartiran posteriorment.

Els directoris específics i els seus permisos es configuren en les fases següents.

8. Captures de pantalla

Les captures d'aquesta fase han de mostrar les comprovacions de la preparació del servidor:

Configuració de xarxa i adreça IP del servidor.

Instal·lació del servei NFS, si es va documentar.

Estat del servei NFS.

Les imatges s'han de desar dins de la carpeta images/.

9. Conclusió

En aquesta fase s'ha preparat l'entorn virtualitzat i el servidor Ubuntu per poder implementar el servei NFS.

La instal·lació i comprovació del servei constitueixen la base per continuar amb la creació d'usuaris, grups, directoris compartits i configuracions de permisos de les fases següents.