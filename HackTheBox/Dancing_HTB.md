# Dancing HTB Machine

IP: `10.129.167.121`

#### Nmap Scan

ouput -

PORT    STATE SERVICE       VERSION                                                                     
135/tcp open  msrpc         Microsoft Windows RPC
139/tcp open  netbios-ssn   Microsoft Windows netbios-ssn
445/tcp open  microsoft-ds? 

Service Info: OS: Windows; CPE: cpe:/o:microsoft:windows


#### Understood That 

445 smbclient is running, so we use smbclient of samba to connect

`smbclient -L <IP>`

output -
Sharename       Type      Comment
---------       ----      -------
  ADMIN$          Disk      Remote Admin                          
  C$              Disk      Default share                       
  IPC$            IPC       Remote IPC                 
  WorkShares      Disk                                    

SMB1 disabled -- no workgroup available

#### Then - 
learned about smb client

```bash
smbclient \\\\{Target_IP}\\{All the sharename}
```

WorkShare worked and was able to access without password.

Output -

```bash
smb \> `help`
```

There was all command, ls and get where two main command.

I typed ls and there was two directory

`
Amy.J
James.P
`

```bash
cd James.P && ls
```

output -
`
.
..
flag.txt
`

```bash
get flag.txt
```



# Learnings from solving this CTF

What does SMB Stands for?
- Server Message Block and use port `445`
- smbclient can be used to connect or it.
- smbget can be used to download file

There are multiple ways to transfer a file between two hosts (computers) on the same network. One ofthese protocols is studied in this example, and that is SMB (Server Message Block). 

This communication protocol provides shared access to files, printers, and serial ports between endpoints on a network. We mostly see SMB services running on Windows machines

