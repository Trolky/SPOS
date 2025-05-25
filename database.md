# MariaDB

```sh
apt install mariadb-server
```

Konfigurační soubory

```
# /etc/mysql/mariadb.conf.d/50-server.cnf
[mysqld]
bind-address = 127.0.0.1
port = 3306
```

Připojení do DB

```sh
mysql
mysql -u user -p
```

Záloha a obnova DB

```sh
mysqldump db1 > db1.sql
cat db1.sql | mysql
```

# PostgreSQL

```sh
apt install postgresql
```

Připojení do databáze: `psql`
- `\conninfo` (zobrazí informace o připojení)
- `\q` (exit)
- `\l` (zobrazí databáze)
- `\c db` (připojí do databáze)
- `\dt` (zobrazí tabulky)
- `\d tabulka` (sloupečky tabulky)

Konfigurační soubory

```
# /etc/postgresql/13/main/postgresql.conf
listen_addresses = 'localhost'
port = 5432
```

```
# /etc/postgresql/13/main/pg_hba.conf
local   all         postgres                         peer
local   all         all                              md5
host    all         all             127.0.0.1/32     md5
host    all         all             ::1/128          md5
```

Připojení do DB

```sh
su postgres
psql
psql -U user -W
```

Vytvoření uživatele a databáze s daným majitelem

```sh
createuser --pwprompt user
createdb -O user database
```
