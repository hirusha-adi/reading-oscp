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
- they will be available at:

```
ls /usr/share/seclists
```




