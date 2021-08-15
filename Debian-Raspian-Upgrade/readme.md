# Debian 10 Buster to Debian 11 Upgrade for amd64/x64_86 Systems - most working on other Systems too (like Raspberry Pi)

**Please run everything as root User (run `su` or `sudo su` and than enter your root password)**<br/>
**Please run every Command for its own**<br/>
**Only Working with Debian Packages and external Packages of this Manual**<br/>
**Please make a Backup/Snapshot of your Server!**<br/>
**See Pictures in also in this Folder**<br/>

## Update System:

```sh
apt update && apt upgrade -y && apt autoremove -y
apt install python-pip-whl gcc-8-base sqlite3 vim sudo redis redis-server cron git curl htop neofetch python3-pip screen apt-transport-https lsb-release ca-certificates software-properties-common gnupg gnupg2 nano unzip zip tar perl libnet-ssleay-perl openssl libauthen-pam-perl libpam-runtime libio-pty-perl apt-show-versions -y
{ crontab -l 2>/dev/null; echo "$(( $RANDOM % 60 )) $(( $RANDOM % 3 + 3 )) * * * apt update && apt upgrade -y && apt autoremove -y" ; } | crontab -
apt update && apt upgrade -y && apt autoremove -y

## Edit APT Sources

```sh
cd /etc/apt
# Replace every Buster with Bullseye (Not with MongoDB Sources current) - see Pictures
# Or remove everything from Debian and paste the Lines below
nano sources.list
```

### New Debian-Sources
```sh
deb http://deb.debian.org/debian/ bullseye main
deb-src http://deb.debian.org/debian/ bullseye main
deb http://security.debian.org/debian-security bullseye-security/updates main
deb-src http://security.debian.org/debian-security bullseye-security/updates main
deb http://deb.debian.org/debian/ bullseye-updates main
deb-src http://deb.debian.org/debian/ bullseye-updates main
deb http://deb.debian.org/debian bullseye-backports main
```

### New Raspberry Pi OS (Raspbian) Sources
```sh
deb http://raspbian.raspberrypi.org/raspbian/ bullseye main contrib non-free rpi
deb-src http://raspbian.raspberrypi.org/raspbian/ bullseye main contrib non-free rpi
```

### Other Sources not in the Main File
```sh
cd sources.list.d/
# Replace every Buster with Bullseye in all Files in this Directory (use "nano FILE-NAME")
apt update
apt full-upgrade -y
```

Press Q if you see the Changelogs<br/>
If you getting asked for auto restarts of services Select Yes with Tab to select ant Space to Enter it<br/>
If you getting asked what should happen with an Configfile select keep local Version<br/>

```sh
reboot
apt update && apt upgrade -y && apt autoremove -y
# Check if Upgrade was successful
cat /etc/os-release
```

## Proftpd
**Only If you running Proftpd please run now:**
```sh
curl -L -o /etc/proftpd/proftpd.conf https://dl.san0j.de/setup/proftpd11.conf
apt update && apt upgrade -y && apt autoremove -y
apt install proftpd -y
apt update && apt upgrade -y && apt autoremove -y
service proftpd restart
```

## Recommended
**Remove Python2**
Reinstall every pip Package you need with pip3<br/>
Change in every Start of a Python File `python` with `python3` in the start command<br/>
Make sure all your python files work with Python 3.9!<br/>
```sh
apt update
apt remove python2
apt update && apt upgrade -y && apt autoremove -y
```
