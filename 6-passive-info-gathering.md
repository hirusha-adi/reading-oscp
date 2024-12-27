# Passive Information Gathering

- take notes of each step

### basic lookup

- get employee details
- names
- phone numbers, emails, etc...
- possibly for targetted attacks too

- check the whois records
- gives info about who is hosting the ip address

```bash
whois megacorpone.com
```

- a reverse whois lookup is what we cann when we whois the ip

```bash
whois 38.100.193.70
```

- find the IP the domain reports to

```bash
nslookup megacorpone.com
```

### google hacking

- aka google dorks
- returns indexed/crawled, public stuff

- limit results only to one site:

```
site:domainame.com
```

- include only the mentioned file type

```
filetype:php
```

- dont show other file types

```
-filetype:html
```

- to find simple file servers hosted online
- check for "index of" in the title
- and content "parent directory" in the body of the page

```
intitle:“index of” “parent directory”
```

- go to GHDB (google hacking database) to find a lot of resources like these

- like use google, we can also use other sites like: netcraft.com
  - here, we can also look for sub domains
  - use `*.megacorpone.com`, like in bash

### recon-ng

- run `recon-ng`
- this is like metasploit

```bash
marketplace search github

# we can see info of modules
marketplace info recon/domains-hosts/google_site_web

# install the module
marketplace install recon/domains-hosts/google_site_web

# load module and start using it
modules load recon/domains-hosts/google_site_web

# see Information and set values
info

# set values / options
options set SOURCE megacorpone.com

# start module
run

# go back
back

# see results
show
```

### gitleaks

- check for git repos for client secrets / api keys / access tokens
- for git repos, use [gitleaks](https://github.com/gitleaks/gitleaks)

- either `git clone` and run `gitleaks -v`
- or

```bash
gitleaks -v -r=https://github.com/user/repo.git
```

### shodan

- https://www.shodan.io/
- crawls for servers and iot devices
- lists the ones that are insecure / misconfigured
- will show running services and vulns in them, if any
- make an account and log in

- to search for domain

```bash
hostname:megacorpone.com:
```

### other cool scanners

- security headers
  - https://securityheaders.com/
  - http headers
  - can get a rough idea of how hardened the server actually is
- ssl server tests
  - https://www.ssllabs.com/ssltest/
  - to identify some SSL/TLS related vulnerabilities, such as [Poodle](https://en.wikipedia.org/wiki/POODLE) or [Heartbleed](https://en.wikipedia.org/wiki/Heartbleed).
- search for pastes
  - eg: https://pastebin.com
- user info gathering
  - of employees
  - check previous data leaks
  - bruteforce with password lists?
- stack overflow
  - to see questions asked by employees
  - to be able to guess how stuff works
- social searcher
  - https://www.social-searcher.com
  - limited searchching for free users
  - to find reviews and stuff
- Twofi
  - need logged in user's valid API key to use
  - twitter OSINT
- OSINT framework
  - https://osintframework.com/
  - has all sorts of tools we can use on many services
- Maltego Community Edition
  - helps both do osint / recon
  - and visualize everything

### email harvesting

- search for email addresses of a domain in google
- `-b google` to specify the data source
- what comes after `-d` is the domain

```bash
theharvester -d megacorpone.com -b google
```
