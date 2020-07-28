# Blurp Collaborator

Self hostable clone of Burp Collaborator. [original project](https://github.com/JuxhinDB/OOB-Server)

*Tested on Ubuntu 18.04 and 20.04*

## Features
- DNS Service
- HTTP(S) Request Catcher & File Hosting
- SMTP Service (25,465,587)

## Prerequisites
- Domain name
- Cloud instance

## Namecheap setup
1. Domain list -> Manage (foo.bar) -> Advanced DNS -> Personal DNS Server -> Add Nameserver -> put ns1 and server IP
2. Domain list -> Manage (foo.bar) -> Domain -> Nameservers -> Select Custom DNS -> put ns1.foo.bar, second input "one.one.one.one"
3. Repeat for ns2 to replace one.one.one.one

## Porkbun setup
1. Login
2. Visit https://porkbun.com/account/domains
3. Details (foo.bar)
4. Glue Records -> Manage
5. Input ns1.foo.bar as hostname and server IP
6. Clear all nameserver and replace it with ns1.foo.bar
7. Repeat for ns2

## Godaddy setup ([source](https://youtube.com/watch?v=p8wbebEgtDk))
1. Under domain host names
2. Add -> ns1.foo.bar and server IP
3. Enter My Own Nameservers -> Put ns1.foo.bar -> Add Nameserver
4. Repeat for ns2

## Usage

```
setup DOMAIN_NAME
setup foo.bar --cert --auto-renew --catcher --mail
```

Options :
- --cert            : request certificate from Let's Encrypt
- --auto-renew      : install cronjob to auto renew cert
- --catcher         : install HTTP Request Catcher with nginx
- --mail            : install mail server (make sure port 25,993,587,465 are open)
- --skip-install    : skip install

Note    :
*catcher and mail will not work without certificate*

### Client
`dig 12321931-xxe.foo.bar`

### Server
You can then monitor your Bind9 traffic like so:
```
foo@mycomputer:~$ sudo tail -f /var/log/named/query.log
25-Oct-2018 14:43:28.202 queries: info: client @0x7f24f8001250 195.158.104.28#58760 (12321931-xxe.foo.bar): query: 12321931-xxe.foo.bar IN A -E(0) (127.127.127.127)
25-Oct-2018 14:43:28.297 queries: info: client @0x7f24f8001250 195.158.104.28#58760 (12321931-xxe.foo.bar): query: 12321931-xxe.foo.bar IN A -E(0) (127.127.127.127)
25-Oct-2018 14:43:28.390 queries: info: client @0x7f24f8001250 195.158.104.28#58760 (12321931-xxe.foo.bar): query: 12321931-xxe.foo.bar IN A -E(0) (127.127.127.127)
```

### File Hosting
1. Put file in /var/www/html/
2. Access at file.foo.bar/filename.txt

### Email Settings
Email : tom@foo.bar

|      | Server Hostname | Port |    SSL   | Authentication | Username |
|:----:|:---------------:|:----:|:--------:|:--------------:|:--------:|
| IMAP |     foo.bar     |  993 |  SSL/TLS | Plain / Normal |    tom   |
| SMTP |     foo.bar     |  587 | STARTTLS | Plain / Normal |    tom   |
