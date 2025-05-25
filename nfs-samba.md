# NFS

```sh
apt install nfs-kernel-server nfs-common
```

Konfigurační soubor
- `/etc/exports`
- [ukázka](conf/nfs.conf)

Reload nastavení

```sh
exportfs -ra
```

Připojení NFS sdílení

```sh
mount -t nfs 10.0.0.6:/mnt/share1 /mnt/remote
```

Odpojení NFS sdílení

```sh
umount -l /mnt/remote
```

# Samba

Instalace

```sh
apt install samba smbclient cifs-utils
```

Konfigurační soubor
- `/etc/samba/smb.conf`
- [ukázka](conf/smb.conf)

Vytvoření uživatele s domovským adresářem

```sh
useradd user1 -m
```

Nastavení hesla uživatele

```sh
smbpasswd -a user1
```

Přehled samba uživatelů

```sh
pdbedit -w -L
```

Přidání skupiny a jejího uživatele

```sh
addgroup --gid 8888 uzivatele
usermod -G uzivatele user1
```

Zkouška připojení

```sh
smbclient //10.0.0.6/share1 -U user1
smbclient //10.0.0.6/share1 -U guest%
```

Připojení přes fstab

```sh
//10.0.0.6/anonymous /mnt/samba cifs username=user1,password=heslo,iocharset=utf8,nofail 0 0
```
