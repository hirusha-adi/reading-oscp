# Command Line Fun

- some useful environment variables

```bash
$USER # username
$PWD # current working directory
$HOME # user's home folder
```

- set and access an env var

```bash
export b=10.11.1.220
echo $b # will show 10.11.1.220
```

- current shell's PID

```bash
echo "$$"
```

- see distro information

```bash
cat /etc/lsb-release

DISTRIB_ID=Kali
DISTRIB_RELEASE=kali-rolling
DISTRIB_CODENAME=kali-rolling
DISTRIB_DESCRIPTION="Kali GNU/Linux Rolling"
```

### stdout to file

- to new file, replace if file exists

```bash
echo "test" > redirection_test.txt
```

- append to file, make if already exists

```bash
echo "test" >> redirection_test.txt
```

### stdin from file

- count words, use `-m` to count no. of characters, `-l` to see number of lines.

```bash
wc -m < redirection_test.txt
```

### piping

- detailed input redirection examples

```bash
kali@kali:~$ cat error.txt
ls: cannot access '/test': No such file or directory

kali@kali:~$ cat error.txt | wc -m
53

kali@kali:~$ cat error.txt | wc -m > count.txt

kali@kali:~$ cat count.txt
53
```

### text manipulation

- `sed`, stream editor: 

```bash
echo "I need to try hard" | sed 's/hard/harder/'

# I need to try harder
```

- `cut`, like `.split()` and selecting index. `-f` is field number, `-d` for feild delimiter

```bash
echo "I hack binaries,web apps,mobile apps, and just about anything else" | cut -f 2 -d ","

```

- `cut`, look for contents in a file

```bash
cut -d ":" -f 1 /etc/passwd
```

- `awk` for data extracttion from text

```bash
echo "hello::there::friend" | awk -F "::" '{print $1, $3}'

# hello friend
```

- advanced example

```bash
head access.log

# 201.21.152.44 - - [25/Apr/2013:14:05:35 -0700] "GET /favicon.ico HTTP/1.1" 404 89 "-" "Mozilla/5.0 (Windows NT 6.2; WOW64) AppleWebKit/537.31 (KHTML, like Gecko) Chrome/26.0.1410.64 Safari/537.31" "random-site.com"
# 70.194.129.34 - - [25/Apr/2013:14:10:48 -0700] "GET /include/jquery.jshowoff.min.js HTTP/1.1" 200 2553 "http://www.random-site.com/" "Mozilla/5.0 (Linux; U; Android 4.1.2; en-us; SCH-I535 Build/JZO54K) AppleWebKit/534.30 (KHTML, like Gecko) Version/4.0 Mobile Safari/534.30" "www.random-site.com"

cat access.log | cut -d " " -f 1 | sort -u

# 201.21.152.44
# 208.115.113.91
# 208.54.80.244

cat access.log | cut -d " " -f 1 | sort | uniq -c | sort -urn

# 1038 208.68.234.99
#   59 208.115.113.91
#   22 208.54.80.244

cat access.log | grep '208.68.234.99' | grep '/admin ' | sort -u

# 208.68.234.99 - - [22/Apr/2013:07:51:20 -0500] "GET //admin HTTP/1.1" 401 742 "-" "Teh Forest Lobster"
```

- compare text files

```bash
comm scan-a.txt scan-b.txt

# or

# -u: unified format
# -c: context format
diff -u scan-a.txt scan-b.txt
```

- see end of file (live updating)

```bash
tail -f file.txt
```

### processes

- list all processes, `-e` to select all processes, `-f` to display full format listing

```bash
ps -ef 
```

- select process by name, use `-C` 

```bash
ps -fC leafpad
```

- see logged in users

```bash
w
```

- run a command every X seconds. the command below will run the command: `w` each 5 seconds

```bash
watch -n 5 w
```

### download

- download files

```bash
wget -O file.txt https://url.com

curl -o file.txt https://url.com

axel -a -n 20 -o file.txt https://url.com
# -a: see progress
# -n: no. of multiple connections used
# -o: output
```

### bash

- remove duplicates from bash history

```bash
export HISTCONTROL=ignoredups
```

- ignore commands from history

```bash
export HISTIGNORE="&:ls:[bf]g:exit:history"
```

- change history format, to see the time the command was run:

```bash
export HISTTIMEFORMAT='%F %T '
```

- aliases

```bash
alias lsa='ls -la'

unalias lsa
```

- edit `~/.bashrc`, with `nano` or `vi` or `neovim` or whatever the editor

```
HISTSIZE=1000
HISTFILESIZE=2000
```
