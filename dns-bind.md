# DNS

> Překlad domén na IP adresy a naopak + další záznamy

Nástroj `host`

```sh
host domena.cz
host -t NS domena.cz
host -t MX domena.cz localhost
```

Nástroj `dig`

```sh
dig domena.cz
dig AAAA domena.cz
dig SOA domena.cz @localhost
```

Další nástroje

```sh
nslookup
whois
```

Nastavení DNS serverů a záznamů
- `/etc/resolv.conf`
- `/etc/hosts`

# bind9

```sh
apt install bind9
```

Restartování

```sh
service named restart
```

Konfigurační soubory
- `/etc/bind/named.conf.options`
- `/etc/bind/named.conf.local`

Nová zóna `domena.local`

```
# /etc/bind/named.conf.local
zone "domena.local." in {
	type master;
	file "/etc/bind/db.domena.local";
};
```

Poslech na lokálních adresách

```
# /etc/bind/named.conf.options
options {
	listen-on {127.0.0.1;};
	listen-on-v6 {::1;};
};
```
