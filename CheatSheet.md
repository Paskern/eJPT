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

Displaying existing routes:
```
$ route
$ ip route
```

Adding new routes:
```
# route add -net <NETWORK IP AND MASK> gw <GATEWAY IP> tap0
# ip route add <NETWORK IP AND MASK> via <GATEWAY IP> dev tap0
```

Adding new default gateway:
```
# route add default gw <GATEWAY IP>
# ip route add default via <GATEWAY IP>
```

Removing routes:
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
Using netcat:
```
$ nc <IP ADDR> <PORT>
```
Using openssl (for HTTPS)
```
$ openssl s_client -connect <IP ADDR>:<PORT>
```

### Directory Enumeration
Using dirb:
```
$ dirb <IP ADDR>:<PORT>
$ dirb <IP ADDR>:<PORT> -u <USERNAME>:<PASSWORD>
$ dirb <IP ADDR>:<PORT> /usr/share/wordlists/dirb/common.txt
```

Dirbuster has a GUI and is faster than dirb. Use the wordlist:
```
/usr/share/wordlists/dirb/common.txt
```

Using gobuster:
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

