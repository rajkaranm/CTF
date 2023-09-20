
# Scaning With Nmap

```bash
nmap -sC 10.129.210.50
```

`-sC` for running defualt scripts.

Result

```
PORT   STATE SERVICE
21/tcp open  ftp
| ftp-anon: Anonymous FTP login allowed (FTP code 230)
| -rw-r--r--    1 ftp      ftp            33 Jun 08  2021 allowed.userlist
|_-rw-r--r--    1 ftp      ftp            62 Apr 20  2021 allowed.userlist.passwd
| ftp-syst:
|   STAT:
| FTP server status:
|      Connected to ::ffff:10.10.16.70
|      Logged in as ftp
|      TYPE: ASCII
|      No session bandwidth limit
|      Session timeout in seconds is 300
|      Control connection is plain text
|      Data connections will be plain text
|      At session startup, client count was 2
|      vsFTPd 3.0.3 - secure, fast, stable
|_End of status
80/tcp open  http
|_http-title: Smash - Bootstrap Business Template
```

version of fpt is `svFTPd 3.0.3`.

So let's try to login as anonymous in fpt as our nmap script say Anonymous FPT login allowed with code `230`

```bash
fpt {IP_ADDRESS}
```

Enter `anonymous` as username and you are in. and we can see two file we do `ls`  allowrd users and there passwords

let's do another nmap scan to get version of apache.

```bash
nmap -sV {IP_ADDRESS}
```

`-sV` for getting version of services

output

```
PORT   STATE SERVICE VERSION
21/tcp open  ftp     vsftpd 3.0.3
80/tcp open  http    Apache httpd 2.4.41 ((Ubuntu))
Service Info: OS: Unix
```

## Gobuster

let explore gobuster

```
gobuster -h
```

we got flag -h, we can use it to get help so.

```
gobuster dir -h
```

we got -x we can specify the extension of the file.

```
gobuster -w common.txt -u 10.129.210.50 -x php, html
```

you will get to that website has a `/login.php`

open it and you will get login page, now use allowd users and password list you got from ftp server.

We can use hydra but it has only 4 password so just it one by one and you will get the flag.