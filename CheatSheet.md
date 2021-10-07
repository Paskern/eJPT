# Cheat Sheet - eJPT

This is a cheat sheet created as an exam preparation for the eLearnSecurity's eJPT. 

## Enumeration

```
$ fping -a -g <IP RANGE> 2>/dev/null | tee hosts.txt
$ nmap -A -iL hosts.txt
$ nmap -A <IP ADDR>
```
