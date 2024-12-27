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

### oss

- check for github repos for client secrets / api keys / access tokens
