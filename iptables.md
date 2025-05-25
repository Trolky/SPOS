# iptables

```sh
apt install iptables
```

## Základní příkazy

> Pro IPv6 je možné používat `ip6tables`.

Povolení IP či rozsahu

```sh
iptables -A INPUT -s 192.168.128.9/32 -j ACCEPT
```

Povolení vstupního ESTABLISHED nebo RELATED trafficu a localhost trafficu

```sh
iptables -I INPUT -m state --state ESTABLISHED,RELATED -j ACCEPT
iptables -I INPUT -s 127.0.0.0/8 -j ACCEPT
ip6tables -I INPUT -s ::1 -j ACCEPT
```

Zakázaní veškerého vstupního trafficu

```sh
iptables --policy INPUT DROP
```

Vypsání pravidel

```sh
iptables -nvL
```

Povolení portu

```sh
iptables -A INPUT -p tcp --dport 80 -j ACCEPT
```

Odebrání pravidel

```sh
iptables -D INPUT -p tcp --dport 80 -j ACCEPT
```

## Perzistence pravidel

> Pravidla se ve výchozím stavu po restartu smažou.

Instalace balíčku pro obnovení pravidel po startu systému

```sh
apt install iptables-persistent
```

Pravidla uložena v `/etc/iptables/rules.v4`

Znovu načtení pravidel

```sh
service iptables restart
```
