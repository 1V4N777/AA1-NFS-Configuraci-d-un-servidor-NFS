# Fase 4 – Exportació i accés a dev_projects

## 1. Objectiu

En aquesta fase s'ha configurat el segon recurs compartit NFS, destinat als desenvolupadors.

S'ha comprovat el muntatge, l'accés, l'escriptura de fitxers i la denegació d'accés a usuaris que no pertanyen al grup corresponent.

## 2. Configuració del servidor

S'ha editat el fitxer `/etc/exports`:

```bash
sudo nano /etc/exports
```

S'ha afegit:

```text
/srv/nfs/dev_projects 192.168.56.0/24(rw,sync,root_squash)
```

La configuració permet l'accés des de la xarxa indicada, amb lectura i escriptura, i manté la protecció `root_squash`.

## 3. Aplicació i verificació

```bash
sudo exportfs -ra
sudo exportfs -v
```

S'ha comprovat que els dos recursos estan exportats:

- `/srv/nfs/admin_tools`
- `/srv/nfs/dev_projects`

## 4. Comprovació des del client

bash
showmount -e 192.168.56.102


Aquest comandament mostra els directoris exportats pel servidor.

S'ha creat el punt de muntatge:

```bash
sudo mkdir -p /mnt/dev_projects
```

I s'ha muntat el recurs:

```bash
sudo mount 192.168.56.102:/srv/nfs/dev_projects /mnt/dev_projects
```

## 5. Configuració del grup al client

S'ha creat un grup local amb el mateix GID que el grup `devs` del servidor:

```bash
sudo groupadd -g 1006 devs_nfs
sudo usermod -aG devs_nfs ivan
```

Això permet que el client utilitzi el mateix identificador de grup que el servidor.

## 6. Prova d'escriptura

```bash
touch /mnt/dev_projects/prueba_dev01.txt
ls -l /mnt/dev_projects
```

S'ha comprovat que el fitxer creat des del client apareix al directori compartit.

Al servidor:

```bash
sudo ls -l /srv/nfs/dev_projects
```

Aquesta comprovació confirma que el fitxer es desa al servidor NFS.

## 7. Prova de denegació d'accés

S'ha comprovat que `admin01`, que pertany al grup `admins`, no pot accedir al directori `dev_projects`.

```bash
sudo -u admin01 ls -la /srv/nfs/dev_projects
```

El resultat és una denegació d'accés perquè l'usuari no pertany al grup autoritzat.

## 8. Captures

Les captures documenten:

- Les exportacions del servidor.
- El muntatge del client.
- Els permisos del directori.
- La prova d'escriptura.
- La comprovació del fitxer al servidor.
- La denegació d'accés a `admin01`.
- La configuració final de les exportacions.

## 9. Conclusió

S'ha configurat correctament el recurs `dev_projects`. Els desenvolupadors poden accedir-hi i crear fitxers, mentre que els usuaris que no pertanyen al grup autoritzat no poden entrar al directori.