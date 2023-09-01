# FAWN HTB 

IP: `10.129.211.58`

--- 

### Nmap Scan

```bash
nmap -sV {IP}
```

-sV Flag to get version of the sevices that are running

Found out FPT server was runing on port 21

so I tried logging in using FTP command in linux

```bash
ftp {IP}
```

I tried guessing the username and password but it didn't worked, So I click on the Hint button, And it said that what username we use to login in FTP server without creating a account.

I Googled it and found it was *anonymous*. 

So I entered this anonymous and left password blank and entered. And I was logged in VOILA! 

```bash
ls
```

There was flag.txt, I downloaded with get command in ftp.


# Learnings

#### Nmap
- -sV flag in nmap to get version of services running

#### What is username that is used over FTP when you want to log in without having an account? 
- anonymous

#### FPT
- get command to download a file in ftp sever
- 230 response code for succesful login in ftp server
