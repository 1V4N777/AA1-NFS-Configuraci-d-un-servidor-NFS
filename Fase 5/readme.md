# Fase 5 – Muntatge automàtic dels recursos NFS

## 1. Objectiu

En aquesta fase s'ha configurat el muntatge automàtic dels recursos NFS al client Zorin OS.

L'objectiu és que els directoris compartits estiguin disponibles després d'iniciar el sistema, sense haver d'executar manualment el comandament `mount`.

## 2. Comprovació inicial

S'han comprovat els recursos NFS muntats:

```bash
mount | grep nfs
```

Aquest comandament mostra els sistemes de fitxers NFS actius.

## 3. Còpia de seguretat

Abans de modificar la configuració, s'ha creat una còpia de seguretat:

```bash
sudo cp /etc/fstab /etc/fstab.backup
```

Això permet recuperar el fitxer original si es produeix algun error.

## 4. Configuració de fstab

S'ha editat el fitxer:

```bash
sudo nano /etc/fstab
```

S'han afegit les següents línies:

```text
192.168.56.102:/srv/nfs/admin_tools /mnt/admin_tools nfs defaults,_netdev 0 0
192.168.56.102:/srv/nfs/dev_projects /mnt/dev_projects nfs defaults,_netdev 0 0
```

Els paràmetres utilitzats són:

- La IP del servidor i la ruta del recurs.
- El punt de muntatge local.
- El tipus de sistema de fitxers `nfs`.
- `defaults`: utilitza les opcions de muntatge predeterminades.
- `_netdev`: indica que el recurs necessita la xarxa.
- `0 0`: desactiva el bolcat i la comprovació automàtica tradicional del sistema de fitxers.

## 5. Prova de la configuració

```bash
sudo mount -a
```

Aquest comandament intenta muntar tots els recursos definits a `/etc/fstab`.

Després s'ha comprovat:

```bash
mount | grep nfs
```

S'ha verificat que els dos recursos apareixen muntats.

## 6. Comprovació després del reinici

S'ha reiniciat el client Zorin OS amb el servidor Ubuntu encès.

Després de l'inici, s'ha executat:

```bash
mount | grep nfs
```

S'ha comprovat que els dos recursos apareixen muntats sense executar manualment `sudo mount -a`.

## 7. Comprovació dels permisos

S'ha comprovat que l'usuari `ivan` pertany als grups locals corresponents:

- `devs_nfs`, GID 1006.
- `admins_nfs`, GID 1007.

Per provar l'accés s'han utilitzat:

```bash
sg devs_nfs -c 'ls -la /mnt/dev_projects'
sg admins_nfs -c 'ls -la /mnt/admin_tools'
```

Aquests comandaments permeten executar les proves utilitzant cada grup com a grup principal.

S'ha comprovat que els dos directoris es poden consultar correctament.

## 8. Captures

Les captures documenten:

- Els muntatges inicials.
- La configuració del fitxer `/etc/fstab`.
- La prova amb `sudo mount -a`.
- Els muntatges després del reinici.
- La comprovació final d'accés als dos recursos.

## 9. Conclusió

S'ha configurat el muntatge automàtic dels recursos NFS mitjançant `/etc/fstab`.

La configuració permet que els recursos compartits estiguin disponibles després de reiniciar el client, sempre que el servidor i la xarxa estiguin accessibles.