# Active Information Gathering

## DNS Enumeration

- hierarcihal structure with top-level root zone
- watch the vide of powercirt to understand a summarized version of it
- first DNS server is called the DNS recursor
- DNS recursor contacts DNS servers in the DNS root zone
- root server sends address of DNS server correcponding to the TLD
- then request the TLD
- lookup types
  - forward lookup (domain -> ip (default))
  - backward lookup (ip -> domain (can optionally be enabled))
- dns cache to store local copies
- dns server owners can change default cache time by editing TTL  
- types of dns records
  - NS - Nameserver records contain the name of the authoritative servers hosting the DNS records for a domain.
  - A - Also known as a host record, the “a record” contains the IP address of a hostname (such as www.megacorpone.com).
  - MX - Mail Exchange records contain the names of the servers responsible for handling email for the domain. A domain can contain multiple MX records.
  - PTR - Pointer Records are used in reverse lookup zones and are used to find the records
associated with an IP address.
  - CNAME - Canonical Name Records are used to create aliases for other host records.
  - TXT - Text records can contain any arbitrary data and can be used for various purposes, such as domain ownership verification. 
- are there public / private records? like txt, the ones given by email providers like mxmail 

- find ip of server / like A record from domain

```bash
host www.megacorpone.com
```

- go through mx records

```bash
host -t mx megacorpone.com
```

- get the txt records, to learn more
- sometimes, some email related stuff is in here (often times they are private)

```bash
host -t txt megacorpone.com
```

- if nothing exists, it will give the error: `NXDOMAIN`
- meaning no public DNS records were found

```bash
host idontexist.megacorpone.com
# Host idontexist.megacorpone.com not found: 3(NXDOMAIN)
```

- see if multiple subdomains / hostnames are up.
- sort of like brute force
- (but its slow, there are other fuzzing tools for thing)
- (is this fuzzing? cuz in fuzzing, you check for a url thats up, but with this, we check if domain names are set)


```bash
for ip in $(cat list.txt); do host $ip.megacorpone.com; done

# www.megacorpone.com has address 38.100.193.76
# Host ftp.megacorpone.com not found: 3(NXDOMAIN)
# mail.megacorpone.com has address 38.100.193.84

# where cat.txt looks something like
# www
# ftp
# mail
```

- SecLists Project
- theres a set of pre-made wordlists
- you might want to install them manually (pre-installed in kali)
  - `sudo apt install seclists`
- you can use these for fuzzing
- they will be available at:

```
ls /usr/share/seclists
```

- reverse lookup, domain from ip
- for range of IP addresses
- in this example

```bash
for ip in $(seq 50 100); do host 38.100.193.$ip; done | grep -v "not found"
```

### DNS zone transfers

- basically a database replication between related DNS servers
- where the zone file is copied from a master DNS server to a slave server
- dns servers should be configured properly
  - separate the internal DNS namespace and external DNS namespace
  - into separate, unrelated zones
  - otherwise, we can get a full map of network infrastructure


- how to do a zone transfer

```bash
host -l "<domain name>" "<dns server address>"
```

- (example) when rejected

```bash
host -l megacorpone.com ns1.megacorpone.com

# Using domain server:
# Name: ns1.megacorpone.com
# Address: 38.100.193.70#53
# Aliases:
# Host megacorpone.com not found: 5(REFUSED)
# ; Transfer failed.
```

- obtain dns servers for given domain name

```
host -t ns megacorpone.com | cut -d " " -f 4
```

- bash dns zone transfer script: `dns-axfr.sh`

```bash
#!/bin/bash

# Simple Zone Transfer Bash Script
# $1 is the first argument given after the bash script
# Check if argument was given, if not, print usage
if [ -z "$1" ]; then
  echo "[*] Simple Zone transfer script"
  echo "[*] Usage : $0 <domain name> "
  exit 0
fi

# if argument was given, identify the DNS servers for the domain

for server in $(host -t ns $1 | cut -d " " -f4); do
  # For each of these servers, attempt a zone transfer
  host -l $1 $server |grep "has address"
done
```

- running the DNS zone transfer Bash script

```bash
bash ./dns-axfr.sh megacorpone.com
# admin.megacorpone.com has address 38.100.193.83
# beta.megacorpone.com has address 38.100.193.88
# fs1.megacorpone.com has address 38.100.193.82
# intranet.megacorpone.com has address 38.100.193.87
# mail.megacorpone.com has address 38.100.193.84
# mail2.megacorpone.com has address 38.100.193.73
```

- using `dnsrecon` to perform a zone transfer
- `-d` means domain
- `-t` means enumeration type
  - `axfr` means a DNS zone transfer

```bash
dnsrecon -d megacorpone.com -t axfr

# [*] Testing NS Servers for Zone Transfer
# [*] Checking for Zone Transfer for megacorpone.com name servers
# [*] Resolving SOA Record
# [+] SOA ns1.megacorpone.com 38.100.193.70
# [*] Resolving NS Records
# [*] NS Servers found:
# [*] NS ns1.megacorpone.com 38.100.193.70
# [*] NS ns2.megacorpone.com 38.100.193.80
# [*] NS ns3.megacorpone.com 38.100.193.90
# [*] Removing any duplicate NS server IP Addresses...
# [*]
# [*] Trying NS server 38.100.193.80
# [+] 38.100.193.80 Has port 53 TCP Open
# [+] Zone Transfer was successful!!
# [*] NS ns1.megacorpone.com 38.100.193.70
```  

- bruteforcing host names with `dnsrecon`
- `-d` for domain
- `-D` to specify file for bruteforce list
- `-t` type is `brt` meaning bruteforce

```bash
dnsrecon -d megacorpone.com -D ~/list.txt -t brt

# [*] Performing host and subdomain brute force against megacorpone.com
# [*] A router.megacorpone.com 38.100.193.71
# [*] A www.megacorpone.com 38.100.193.76
# [*] A mail.megacorpone.com 38.100.193.84
# [+] 3 Records Found
``` 

- another tool is `dnsenum`
- which is the easiest

```bash
dnsenum example.com
```

## Port Scanning
