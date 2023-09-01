# Redeemer_HTB

IP: `10.129.40.82`


# Nmap Scan

```bash
nmap {IP}
```

But it doesn't showed any open ports, so i defined to scan all port with `-p-` flag

```bash
nmap -p- {IP} -T5
```

-T5 command is speed up things because scanning all ports will take time.

Output - 

PORT      STATE    SERVICE
6379/tcp  open     redis
57570/tcp filtered unknown


Redis is a  In-memory Database And *redis-cli* is used to interact with redis sever.

So I downloaded redis

```bash
apt install redis
```

After that

```bash
redis-cli --help
```

Found -h was used to define host.
```bash
redis-cli -h {IP}
```

After i was in, I didn't knew what to so I just typed help, we learned this from previous machine.

There was Command *INFO* to print all the statistics.


```bash
INFO
INFO server
```

redis-cli 5.0.7, is the version of redis that target machine is using.

IMP Command Redis command - 
INFO
SELECT
KEYS 
GET

SO First select database 0, then type `KEYS *` to print all the keys, there will one key named flag, then enter command `GET flag`

Congrats we pawned one more machine.
