
# [Debian 10/11 Buster/Bullseye Setup](./README.md#List-of-Content)

**Please run everything as root User (run `su` or `sudo su` and than enter the password)** <br/>
**Please run every Command for its own and Please read the Comments!**

# Default:
```sh
# Required for everything on this Site
apt update && apt upgrade -y && apt autoremove -y
apt install wget ftp apache2-utils tmux jq python-pip-whl make sqlite3 youtube-dl ffmpeg vim sudo redis redis-server cron git curl htop neofetch python3-pip screen apt-transport-https lsb-release ca-certificates software-properties-common gnupg gnupg2 nano unzip zip tar perl libnet-ssleay-perl openssl libauthen-pam-perl libpam-runtime libio-pty-perl apt-show-versions sudo -y
{ crontab -l 2>/dev/null; echo "$(( $RANDOM % 60 )) $(( $RANDOM % 3 + 3 )) * * * apt update && apt upgrade -y && apt autoremove -y" ; } | crontab -
apt update && apt upgrade -y && apt autoremove -y
```

# [List of Content:](./README.md#List-of-Content)<br/>
- [Upgrade Debian 10 -> 11](./Debian-Raspian-Upgrade/readme.md)<br/>
- [Default](./README.md#Default)<br/>
- [Java](./README.md#Java)<br/>
- [MongoDB](./README.md#MongoDB)<br/>
- [Ruby and Rails](./README.md#Ruby-and-Rails)<br/>
- [Node.js](./README.md#Nodejs)<br/>
- [PM2](./README.md#PM2)<br/>
- [Snapd](./README.md#Snapd)<br/>
- [Apache and Certbot](./README.md#Apache-and-Certbot)<br/>
- [Apache2 Configs](./README.md#Apache2-Configs)<br/>
- [PHP](./README.md#PHP)<br/>
- [Composer](./README.md#Composer)<br/>
- [MariaDB](./README.md#MariaDB)<br/>
- [PHPMyAdmin](./README.md#PHPMyAdmin)<br/>
- [Docker](./README.md#Docker)<br/>
- [Docker-Compose](./README.md#Docker-Compose)<br/>
- [Docker-Portainer](./README.md#Docker-Portainer)<br/>
- [Wireguard (VPN)](./README.md#Wireguard-VPN)<br/>
- [Webmin](./README.md#Webmin)<br/>
- [Squid (HTTP-Proxy - with Password Authentication)](./README.md#Squid-HTTP-Proxy---with-Password-Authentication)<br/>
- [ProFTPD](./README.md#ProFTPD)<br/>
- [Jenkins](./README.md#Jenkins)<br/>
- [Speedtest](./README.md#Speedtest)<br/>
- [Monitoring Services](./README.md#Monitoring-Services)<br/>
- [SSH-Tarpit](./README.md#SSH-Tarpit)<br/>
- [Force-Update to latest git version of something](./README.md#Force-Update-to-latest-git-version-of-something)<br/>

## Java:
```sh
apt update && apt upgrade -y && apt autoremove -y
wget -O- https://apt.corretto.aws/corretto.key | sudo apt-key add - 
add-apt-repository 'deb https://apt.corretto.aws stable main'
# For Java 17
apt-get update; sudo apt-get install -y java-17-amazon-corretto-jdk maven
# For Java 16
apt-get update; sudo apt-get install -y java-16-amazon-corretto-jdk maven
# For Java 11
apt-get update; sudo apt-get install -y java-11-amazon-corretto-jdk maven
apt update && apt upgrade -y && apt autoremove -y
```

## MongoDB:
```sh
# Not working on every server, no plan why
apt update && apt upgrade -y && apt autoremove -y
apt remove mongodb
wget -qO - https://www.mongodb.org/static/pgp/server-5.0.asc | sudo apt-key add -
echo "deb http://repo.mongodb.org/apt/debian buster/mongodb-org/5.0 main" | sudo tee /etc/apt/sources.list.d/mongodb-org-5.0.list
apt update
sudo apt-get install -y mongodb-org
sudo systemctl daemon-reload
sudo systemctl start mongod
sudo systemctl enable mongod
apt update && apt upgrade -y && apt autoremove -y
```

## Ruby and Rails
```sh
apt update && apt upgrade -y && apt autoremove -y
gpg2 --recv-keys 409B6B1796C275462A1703113804BB82D39DC0E3 7D2BAF1CF37B13E2069D6956105BD0E739499BDB
curl -sSL https://get.rvm.io | bash -s stable --rails
source /usr/local/rvm/scripts/rvm
apt update && apt upgrade -y && apt autoremove -y
```

## Node.js:
```sh
apt update && apt upgrade -y && apt autoremove -y
# For Node.js 16.x
curl -fsSL https://deb.nodesource.com/setup_16.x | bash -
# For Node.js 14.x
curl -fsSL https://deb.nodesource.com/setup_14.x | bash -
apt update
apt-get install nodejs -y
npm config set fund false --global
npm i npm -g
{ crontab -l 2>/dev/null; echo "$(( $RANDOM % 60 )) $(( $RANDOM % 3 + 3 )) * * * sudo npm i npm -g" ; } | crontab -
apt update && apt upgrade -y && apt autoremove -y
```

## PM2:
```sh
# Install Node.js (see https://github.com/2020Sanoj/Debian-Setup#Nodejs)
apt update && apt upgrade -y && apt autoremove -y
npm install pm2 -g
{ crontab -l 2>/dev/null; echo "$(( $RANDOM % 60 )) $(( $RANDOM % 3 + 3 )) * * * sudo npm i pm2 -g" ; } | crontab -
apt update && apt upgrade -y && apt autoremove -y
```

## Snapd
```sh
apt install snapd -y
snap install core
snap install core; sudo snap refresh core
```

## Apache and Certbot:
```sh
# Install Snapd (see https://github.com/2020Sanoj/Debian-Setup#Snapd)
apt update && apt upgrade -y && apt autoremove -y
apt-get remove certbot
sudo snap install --classic certbot
snap set certbot trust-plugin-with-root=ok
snap install certbot-dns-cloudflare
sudo ln -s /snap/bin/certbot /usr/bin/certbot
{ crontab -l 2>/dev/null; echo "$(( $RANDOM % 60 )) $(( $RANDOM % 3 + 3 )) * * * sudo certbot renew --dry-run" ; } | crontab -
apt install apache2 libapache2-mod-php -y
curl -L -o /etc/apache2/apache2.conf https://raw.githubusercontent.com/SanCraftDev/Debian-Setup/main/configs/apache2.conf
a2enmod rewrite
a2enmod headers
a2enmod env
a2enmod dir
a2enmod mime
a2enmod proxy
a2enmod proxy_http
a2enmod proxy_wstunnel
a2enmod headers
a2enmod ssl
service apache2 restart
apt update && apt upgrade -y && apt autoremove -y
# To generate an Certificate use "certbot certonly --apache -d DOMAIN" (replace DOMAIN with the Domain or Subdomain)
# SSL dont work with IPs
```

## Apache2 Configs:

### For Domains:

Install Apache2 and Certbot (see https://github.com/2020Sanoj/Debian-Setup#Apache-and-Certbot)
Set your DNS Records
Replace every `DOMAIN` with your Domain<br/>
Run `curl -L -o /etc/apache2/sites-enabled/DOMAIN.conf https://raw.githubusercontent.com/SanCraftDev/Debian-Setup/main/configs/domains.conf`<br/>
`cd /etc/apache2/sites-enabled`
`mv DOMAIN.conf DOMAIN.conf` <- Change Only the last DOMAIN
Replace every `DOMAIN` with your Domain with `nano DOMAIN.conf` <- Change `DOMAIN` in the Command<br/>
Generate before restarting Apache2 a SSL-Certificate with `certbot certonly --apache -d DOMAIN`<br/>
And `certbot certonly --apache -d www.DOMAIN`<br/>
Now restart Apache2 with `service apache2 restart`<br/>

### For Subdomains:

Install Apache2 and Certbot (see https://github.com/2020Sanoj/Debian-Setup#Apache-and-Certbot)
Set your DNS RecordsSet your DNS Records
Replace every `SUBDOMAIN` with your Subdomain<br/>
Run `curl -L -o /etc/apache2/sites-enabled/SUBDOMAIN.conf https://raw.githubusercontent.com/SanCraftDev/Debian-Setup/main/configs/subdomains.conf`<br/>
`cd /etc/apache2/sites-enabled`
`mv SUBDOMAIN.conf SUBDOMAIN.conf` <- Change Only the last DOMAIN
Replace every `SUBDOMAIN` with your Domain with `nano SUBDOMAIN.conf` <- Change `SUBDOMAIN` in the Command<br/>
Generate before restarting Apache2 a SSL-Certificate with `certbot certonly --apache -d SUBDOMAIN`<br/>
Now restart Apache2 with `service apache2 restart`<br/>

Install Apache2 and Certbot (see https://github.com/2020Sanoj/Debian-Setup#Apache-and-Certbot)
### For IPs:
Replace every `IP` with your IP<br/>
Replace every `IP` with your IP with `nano /etc/apache2/sites-enabled/IP.conf`<br/>
Now restart Apache2 with `service apache2 restart`<br/>


## PHP:
```sh
# Install Apache2 and Certbot (see https://github.com/2020Sanoj/Debian-Setup#Apache-and-Certbot)
apt update && apt upgrade -y && apt autoremove -y
wget -O /etc/apt/trusted.gpg.d/php.gpg https://packages.sury.org/php/apt.gpg
sh -c 'echo "deb https://packages.sury.org/php/ $(lsb_release -sc) main" > /etc/apt/sources.list.d/php.list'
apt update
apt install libapache2-mod-php libapache2-mod-php8.0 php8.0 php8.0-cli php8.0-common php8.0-curl php8.0-gd php8.0-intl php8.0-redis php8.0-mbstring php8.0-mysql php8.0-opcache php8.0-readline php8.0-xml php8.0-xsl php8.0-zip php8.0-bz2 libapache2-mod-php8.0 -y
apt install php-zip php-dompdf php-xml php-mbstring php-gd php-curl php-imagick php-intl php-bcmath php-gmp libmagickcore-6.q16-6-extra php-sqlite3 php-apcu -y
apt install php8.0 php8.0-{cli,gd,mysql,pdo,mbstring,tokenizer,bcmath,xml,fpm,curl,zip} -y
apt install libapache2-mod-php php-zip php-dompdf php-xml php-mbstring php-gd php-curl php-imagick libmagickcore-6.q16-6-extra php-intl php-bcmath php-gmp php-cli php-json php-mysql php-zip php-gd  php-mbstring php-curl php-xml php-pear unzip php-apcu redis-server php-redis -y
curl -L -o /etc/php/8.0/apache2/php.ini https://raw.githubusercontent.com/SanCraftDev/Debian-Setup/main/configs/php.ini
apt update && apt upgrade -y && apt autoremove -y
```

## Composer:
```sh
apt update && apt upgrade -y && apt autoremove -y
curl -sS https://getcomposer.org/installer | sudo php -- --install-dir=/usr/local/bin --filename=composer
{ crontab -l 2>/dev/null; echo "$(( $RANDOM % 60 )) $(( $RANDOM % 3 + 3 )) * * * curl -sS https://getcomposer.org/installer | sudo php -- --install-dir=/usr/local/bin --filename=composer" ; } | crontab -
apt update && apt upgrade -y && apt autoremove -y
```

## MariaDB:
```sh
apt update && apt upgrade -y && apt autoremove -y
apt install mariadb-server mariadb-client -y
mysql_secure_installation
# Press Enter
y
y
# Set a Password
y
y
y
y
# Create User with root permissions on MariaDB
mysql -u root -p
# Enter the Password
# Replace "USERNAME" with an Username do not use root and "PASSWORD" with a Password
CREATE USER 'USERNAME'@'localhost' IDENTIFIED BY 'PASSWORD';
GRANT ALL PRIVILEGES ON *.* TO 'Username'@'localhost' WITH GRANT OPTION;
FLUSH PRIVILEGES;
exit;
apt update && apt upgrade -y && apt autoremove -y
```

## PHPMyAdmin:
```sh
apt update && apt upgrade -y && apt autoremove -y
curl -L https://www.phpmyadmin.net/downloads/phpMyAdmin-latest-all-languages.zip -o phpmyadmin.zip
unzip phpmyadmin.zip
rm phpmyadmin.zip
mv phpMyAdmin-*-all-languages pma
mv pma /var/www
curl -L -o /var/www/pma/config.inc.php https://raw.githubusercontent.com/SanCraftDev/Debian-Setup/main/configs/config.inc.php.txt
chown -R www-data:www-data /var/www
apt update && apt upgrade -y && apt autoremove -y

# Install Apache2 and Certbot (see https://github.com/2020Sanoj/Debian-Setup#Apache-and-Certbot)
# Install PHP (see https://github.com/2020Sanoj/Debian-Setup#PHP)

# Add "Alias /pma /var/www/pma" in your Apache Configfile of the Domain/IP you want (in /etc/apache2/sites-enabled)
# Now you can open in the Web your IP/Domain and add after that /pma

# Or create an Subdomain an - set "/var/www/pma" as Directory (see https://github.com/2020Sanoj/Debian-Setup#Apache2-Configs)
# Now you can open in the Web your Subdomain

service apache restart
```

## Docker:
```sh
apt update && apt upgrade -y && apt autoremove -y
apt-get remove docker docker-engine docker.io containerd runc docker-compose -y
curl -fsSL https://download.docker.com/linux/debian/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/debian $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
apt update
apt-get install docker-ce docker-ce-cli containerd.io -y
systemctl start docker
systemctl enable docker
apt update && apt upgrade -y && apt autoremove -y
```

## Docker-Compose
```sh
# Install Docker (see https://github.com/2020Sanoj/Debian-Setup#Docker)
apt update && apt upgrade -y && apt autoremove -y
mkdir /usr/local/lib/docker/
mkdir /usr/local/lib/docker/cli-plugins

# For x86_64 / amd64 Platforms - Please use instead of "docker-compose" "docker compose" in a command
curl -L https://github.com/docker/compose/releases/latest/download/docker-compose-linux-x86_64 > /usr/local/lib/docker/cli-plugins/docker-compose
{ crontab -l 2>/dev/null; echo "$(( $RANDOM % 60 )) $(( $RANDOM % 3 + 3 )) * * * curl -L https://github.com/docker/compose/releases/latest/download/docker-compose-linux-x86_64 > /usr/local/lib/docker/cli-plugins/docker-compose" ; } | crontab -
chmod +x /usr/local/lib/docker/cli-plugins/docker-compose
curl -fL https://github.com/docker/compose-switch/releases/latest/download/docker-compose-linux-amd64 -o /usr/local/bin/docker-compose
{ crontab -l 2>/dev/null; echo "$(( $RANDOM % 60 )) $(( $RANDOM % 3 + 3 )) * * * curl -fL https://github.com/docker/compose-switch/releases/latest/download/docker-compose-linux-amd64 -o /usr/local/bin/docker-compose" ; } | crontab -
chmod +x /usr/local/bin/docker-compose

# For Raspbery Pi - You must use instead of "docker-compose" "docker compose" in a command
curl -L https://github.com/docker/compose/releases/latest/download/docker-compose-linux-armv7 > /usr/local/lib/docker/cli-plugins/docker-compose
{ crontab -l 2>/dev/null; echo "$(( $RANDOM % 60 )) $(( $RANDOM % 3 + 3 )) * * * curl -L https://github.com/docker/compose/releases/latest/download/docker-compose-linux-armv7 > /usr/local/lib/docker/cli-plugins/docker-compose" ; } | crontab -
chmod +x /usr/local/lib/docker/cli-plugins/docker-compose

# For 64-Bit Raspbery Pi - Please use instead of "docker-compose" "docker compose" in a command
curl -L https://github.com/docker/compose/releases/latest/download/docker-compose-linux-arm64 > /usr/local/lib/docker/cli-plugins/docker-compose
{ crontab -l 2>/dev/null; echo "$(( $RANDOM % 60 )) $(( $RANDOM % 3 + 3 )) * * * curl -L https://github.com/docker/compose/releases/latest/download/docker-compose-linux-arm64 > /usr/local/lib/docker/cli-plugins/docker-compose" ; } | crontab -
chmod +x /usr/local/lib/docker/cli-plugins/docker-compose
curl -fL https://github.com/docker/compose-switch/releases/latest/download/docker-compose-linux-arm64 -o /usr/local/bin/docker-compose
{ crontab -l 2>/dev/null; echo "$(( $RANDOM % 60 )) $(( $RANDOM % 3 + 3 )) * * * curl -fL https://github.com/docker/compose-switch/releases/latest/download/docker-compose-linux-arm64 -o /usr/local/bin/docker-compose" ; } | crontab -
chmod +x /usr/local/bin/docker-compose

apt update && apt upgrade -y && apt autoremove -y
```

## Docker-Portainer
```sh
# Install Docker-Compose and Docker (see https://github.com/2020Sanoj/Debian-Setup#Docker and https://github.com/2020Sanoj/Debian-Setup#Docker)
apt update && apt upgrade -y && apt autoremove -y
docker volume create portainer_data
docker run -d -p 8000:8000 -p 9000:9000 --name=portainer --restart=always -v /var/run/docker.sock:/var/run/docker.sock -v portainer_data:/data portainer/portainer
apt update && apt upgrade -y && apt autoremove -y
# Open http://IP:9000 with your Browser
```

## Wireguard (VPN):
```sh
apt update && apt upgrade -y && apt autoremove -y
wget git.io/wireguard -O wireguard-install.sh && bash wireguard-install.sh
# Select your IP address - Press Enter
# Set a Random Number under 1000 - Press Enter
# Give your first VPN Client an Name - Press Enter
# I recomend to use AdGuard - 6 - Press Enter
# Press Enter
wget git.io/wireguard -O wireguard-install.sh
chmod 700 /root/wireguard-install.sh
apt update && apt upgrade -y && apt autoremove -y
# Download the Clients from here: https://www.wireguard.com/install/ and load the generated Config
# To create a New Wireguard User or remove one use:
/root/wireguard-install.sh
# Do the same as before
# Client Configs are saved in "/root"
```

## Webmin:
```sh
apt update && apt upgrade -y && apt autoremove -y
add-apt-repository 'deb https://download.webmin.com/download/repository sarge contrib'
wget https://download.webmin.com/jcameron-key.asc
apt-key add jcameron-key.asc 
rm jcameron-key.asc
apt update
apt install webmin -y
apt update && apt upgrade -y && apt autoremove -y
```

## Squid (HTTP-Proxy - with Password Authentication):
```sh
apt update && apt upgrade -y && apt autoremove -y
apt install squid squid3 -y
curl -L -o /etc/squid/squid.conf https://raw.githubusercontent.com/SanCraftDev/Debian-Setup/main/configs/squid.conf
# Create User (Replace "USERNAME" with a Username)
htpasswd -c /etc/squid/passwords USERNAME
# Enter a new Password
service squid restart
# Port is 8449
# The restart take a moment
apt update && apt upgrade -y && apt autoremove -y
```

## ProFTPD:
```sh
apt update && apt upgrade -y && apt autoremove -y
apt install proftpd -y
addgroup ftpuser
# Replace USERNAME with an Username and replace PATH with the Folder Path
adduser USERNAME --shell /bin/false --home /PATH --ingroup ftpuser
# Debian 10 Buster
curl -L -o /etc/proftpd/proftpd.conf https://raw.githubusercontent.com/SanCraftDev/Debian-Setup/main/configs/proftpd10.conf
# Debian 11 Bullseye
curl -L -o /etc/proftpd/proftpd.conf https://raw.githubusercontent.com/SanCraftDev/Debian-Setup/main/configs/proftpd11.conf

service proftpd restart
apt update && apt upgrade -y && apt autoremove -y
```

## Jenkins:
```sh
# Install Java11 (see https://github.com/SanCraftDev/Debian-Setup#Java)
apt update && apt upgrade -y && apt autoremove -y
wget -q -O - https://pkg.jenkins.io/debian/jenkins.io.key | sudo apt-key add -
# For Weekly Updates
add-apt-repository 'deb https://pkg.jenkins.io/debian binary/'
# Or for LTS
add-apt-repository 'deb https://pkg.jenkins.io/debian-stable binary/'
sudo apt-get update
sudo apt-get install jenkins
apt update && apt upgrade -y && apt autoremove -y

# Install Apache2 and Certbot (see https://github.com/SanCraftDev/Debian-Setup#Apache-and-Certbot)
# Set your DNS Records
# Create Jenkins Apache2 Config
curl -L -o /etc/apache2/sites-enabled/ci.conf https://raw.githubusercontent.com/SanCraftDev/Debian-Setup/main/configs/ci.conf
# Replace SUBDOMAIN with your Subdomain you want to use
nano /etc/apache2/sites-enabled/ci.conf
certbot certonly --apache -d SUBDOMAIN
service apache2 restart
# Now open in the Web your Subdomain

# Or add this to your Apache Config of your IP/Domain:
        ProxyPass         /jenkins  http://localhost:8080/jenkins nocanon
        ProxyPassReverse  /jenkins  http://localhost:8080/jenkins
        ProxyRequests     Off
        AllowEncodedSlashes NoDecode
    <Proxy http://localhost:8080/jenkins*>
    Order deny,allow
    Allow from all
    </Proxy>
service apache2 restart
# Now open in the Web your IP/Domain and add after that /jenkins
```

## Speedtest:
```sh
apt update && apt upgrade -y && apt autoremove -y
curl -s https://install.speedtest.net/app/cli/install.deb.sh | sudo bash
sudo apt-get install speedtest -y
apt update && apt upgrade -y && apt autoremove -y
```

## Monitoring Services:
```sh
# gtop (recomended):
# Install Node.js (see https://github.com/SanCraftDev/Debian-Setup#Nodejs)
npm install -g gtop
{ crontab -l 2>/dev/null; echo "$(( $RANDOM % 60 )) $(( $RANDOM % 3 + 3 )) * * * sudo npm i gtop -g" ; } | crontab -
# Use:
gtop

# vtop:
# Install Node.js (see https://github.com/SanCraftDev/Debian-Setup#Nodejs)
npm install -g vtop
{ crontab -l 2>/dev/null; echo "$(( $RANDOM % 60 )) $(( $RANDOM % 3 + 3 )) * * * sudo npm i vtop -g" ; } | crontab -
# Use:
vtop

# duf (for disks):
# Install Snapd (see https://github.com/SanCraftDev/Debian-Setup#Snapd)
snap install duf-utility
# Use:
duf

# htop:
apt install htop -y
# Use:
htop
```

## SSH-Tarpit
```sh
# Make shure your ssh port is NOT 22
nano /etc/ssh/sshd_config
# Now Install the Tarpit
pip3 install ssh-tarpit
cd /root
curl -L -o /root/tarpit.sh https://raw.githubusercontent.com/SanCraftDev/Debian-Setup/main/configs/tarpit.sh.txt
chmod 700 ./tarpit.sh
# Run on Reboot
{ crontab -l 2>/dev/null; echo "@reboot sleep 10 && cd /root && ./tarpit.sh" ; } | crontab -
# Run now
./tarpit.sh
# Your Tarpit log is in /root/tarpit.log saved

# For analyzation i recommend https://github.com/RPiList/TarpitAn
```
## Force-Update to latest git version of something
```sh
# Local Changes may get lost
git fetch origin
git reset --hard origin/master
```
