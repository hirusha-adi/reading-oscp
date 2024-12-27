# Basic Linux Stuff

- making folders

```bash
mkdir -p test/{recon,exploit,report}

# will make folders:
test/exploit
test/recon
test/report
```

- locating files (`locate`)

```bash
sudo updatedb
locate filename
```

- finding files (`find`)

```bash
sudo find / -name sbd*
# will search for sbd* in / dir
```

- which process uses which network ports?

```
sudo ss -antlp | grep sshd
```

- list all systemd unit files

```
systemctl list-unit-files
```

- search packages

```
apt-cache search package-name
```

- view package information

```
apt show package-name
```

- remove package

```
sudo apt remove --purge package-name
```

- installing `.deb` files

```
sudo apt install ./filename.deb -y
sudo dpkg -i ./filename.deb
```
