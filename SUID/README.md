# Binaries That AutoElevate

## General

`SUID`: Set User ID is a type of permission that allows users to execute a file with the permissions of a specified user. Those files which have suid permissions run with higher privileges.  Assume we are accessing the target system as a non-root user and we found suid bit enabled binaries, then those file/program/command can run with root privileges.

+ SGID (chmod 2000) - run as the group, not the user who started it.

+ SUID (chmod 4000) - run as the owner, not the user who started it.

## Set SUID

```
$ chmod 4755 /bin/bash
```

```
$ chmod u+s /bin/bash
```

## Scanning

Find file with enabled permission

```
find / -perm -g=s -type f 2>/dev/null    # SGID
find / -perm -u=s -type f 2>/dev/null    # SUID
find / -perm -g=s -o -perm -u=s -type f 2>/dev/null    # SGID or SUID
```

## Example

```
find / -perm -u=s -type f 2>/dev/null
<snip>
/usr/bin/nmap
```

Refer [GTFOBins](https://gtfobins.github.io) to find step to exploit common binaries.

## Reference

[https://www.hackingarticles.in/linux-privilege-escalation-using-suid-binaries](https://www.hackingarticles.in/linux-privilege-escalation-using-suid-binaries)