## Proxy

```bash
$ ssh -f -N -D 1080 user@server

$ ss -lntp | grep 1080

LISTEN 0      128         127.0.0.1:1080       0.0.0.0:*    users:(("ssh",pid=83057,fd=5))
```

```bash 
$ sudo apt install proxychains4
```

```
/etc/proxychains4.conf

[ProxyList]
# socks4        127.0.0.1 9050
socks5 127.0.0.1 1080
```
`
```bash
$ proxychains4 tldr -u
```

```bash
$ ps aux | grep "1080"

maestro  83057  0.0  0.0  12516  5600 ?   Ss   22:11   0:00 ssh -f -N -D 1080 user@server

$ kill pid
```