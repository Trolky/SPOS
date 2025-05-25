# E-maily

Odesílání e-mailu přes SMTP pomocí Telnetu

```
telnet localhost 25
HELO localhost
MAIL FROM: pepa@localhost
RCPT TO: root@localhost
DATA
Subject: Testovaci
Ahoj svete
.
QUIT
```

Testovací přípojení protokolem IMAP

```
telnet localhost 143
a login user1 heslo
a list "" "*"
a status inbox (messages)
a examine inbox
a fetch 1 body[]
a logout
```

Testovací přípojení protokolem POP3

```
telnet localhost 110
USER user1
PASS heslo
STAT
LIST
RETR 1
QUIT
```

# Mutt

> Klient pro čtení e-mailů v terminálu

```sh
apt install mutt
```

```sh
mutt -f ~root/Maildir
mutt -f /var/spool/mail/root
```

# Postfix

> Program pro příjem a odesílání e-mailů

```sh
apt install postfix
```

Přijímání e-mailů do maildiru

```
# /etc/postfix/main.cf
home_mailbox = Maildir/
mailbox_command =
```

Virtuální domény a e-maily a mapování na unix uživatele

```
# /etc/postfix/main.cf
virtual_alias_domains_map = hash:/etc/postfix/virtual_domains
virtual_alias_maps = hash:/etc/postfix/virtual
```

```
# /etc/postfix/virtual_domains
domena1.local	OK
domena2.local	OK
```

```
# /etc/postfix/virtual
info@domena1.local	root
info@domena2.local	root
```

```sh
postmap /etc/postfix/virtual_domains
postmap /etc/postfix/virtual
```

# Dovecot IMAP a POP3

Porty
- **110** POP3
- **995** POP3 + SSL
- **143** IMAP
- **993** IMAP + SSL

Instalace POP3

```sh
apt install dovecot-pop3d
```

Instalace IMAP

```sh
apt install dovecot-imapd
```

Nastavení lokace e-mailů

```
# /etc/dovecot/conf.d/10-mail.conf
mail_location = maildir:~/Maildir
```

Nastavení SSL

```sh
openssl req -new -x509 -nodes -out "/etc/ssl/certs/dovecot.pem" -keyout "/etc/ssl/private/dovecot.pem"
```

```
# /etc/dovecot/conf.d/10-ssl.conf
ssl = yes
ssl_cert = </etc/ssl/certs/dovecot.pem
ssl_key = </etc/ssl/private/dovecot.key
```
