
# [Debian 10/11 Buster/Bullseye Setup for amd64/x64_86 Systems - most working on other Systems too (like Raspberry Pi)](https://2020sanoj.github.io/Debian-Setup/)

**Please run everything as root User (run `su` or `sudo su` and than enter the password)** <br/>
**Please run every Command for its own and Please read the Comments!**

# Default:
```sh
# Required for everything on this Site
apt update && apt upgrade -y && apt autoremove -y
apt install wget python-pip-whl make sqlite3 youtube-dl ffmpeg vim sudo redis redis-server cron git curl htop neofetch python3-pip screen apt-transport-https lsb-release ca-certificates software-properties-common gnupg gnupg2 nano unzip zip tar perl libnet-ssleay-perl openssl libauthen-pam-perl libpam-runtime libio-pty-perl apt-show-versions sudo -y
{ crontab -l 2>/dev/null; echo "$(( $RANDOM % 60 )) $(( $RANDOM % 3 + 3 )) * * * apt update && apt upgrade -y && apt autoremove -y" ; } | crontab -
apt update && apt upgrade -y && apt autoremove -y
```

# [List of Content (Only Works on the Github direkt):](https://github.com/2020Sanoj/Debian-Setup/)<br/>
- [Upgrade Debian 10 -> 11](https://github.com/2020Sanoj/Debian-Setup/Debian-Raspian-Upgrade/readme.md)<br/>
- [Default](https://github.com/2020Sanoj/Debian-Setup/#Default)<br/>
- [Java](https://github.com/2020Sanoj/Debian-Setup/#Java)<br/>
- [MongoDB](https://github.com/2020Sanoj/Debian-Setup/#MongoDB)<br/>
- [Ruby and Rails](https://github.com/2020Sanoj/Debian-Setup/#Ruby-and-Rails)<br/>
- [Node.js](https://github.com/2020Sanoj/Debian-Setup/#Nodejs)<br/>
- [PM2](https://github.com/2020Sanoj/Debian-Setup/#PM2)<br/>
- [Snapd](https://github.com/2020Sanoj/Debian-Setup/#Snapd)<br/>
- [Apache and Certbot](https://github.com/2020Sanoj/Debian-Setup/#Apache-and-Certbot)<br/>
- [Apache2 Configs](https://github.com/2020Sanoj/Debian-Setup/#Apache2-Configs)<br/>
- [PHP](https://github.com/2020Sanoj/Debian-Setup/#PHP)<br/>
- [Composer](https://github.com/2020Sanoj/Debian-Setup/#Composer)<br/>
- [MariaDB](https://github.com/2020Sanoj/Debian-Setup/#MariaDB)<br/>
- [PHPMyAdmin](https://github.com/2020Sanoj/Debian-Setup/#PHPMyAdmin)<br/>
- [Docker](https://github.com/2020Sanoj/Debian-Setup/#Docker)<br/>
- [Docker-Compose](https://github.com/2020Sanoj/Debian-Setup/#Docker-Compose)<br/>
- [Docker-Portainer](https://github.com/2020Sanoj/Debian-Setup/#Docker-Portainer)<br/>
- [Wireguard (VPN)](https://github.com/2020Sanoj/Debian-Setup/#Wireguard-VPN)<br/>
- [Webmin](https://github.com/2020Sanoj/Debian-Setup/#Webmin)<br/>
- [Squid (HTTP-Proxy - with Password Authentication)](https://github.com/2020Sanoj/Debian-Setup/#Squid-HTTP-Proxy---with-Password-Authentication)<br/>
- [ProFTPD](https://github.com/2020Sanoj/Debian-Setup/#ProFTPD)<br/>
- [Jenkins](https://github.com/2020Sanoj/Debian-Setup/#Jenkins)<br/>
- [Speedtest](https://github.com/2020Sanoj/Debian-Setup/#Speedtest)<br/>
- [Monitoring Services](https://github.com/2020Sanoj/Debian-Setup/#Monitoring-Services)<br/>

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
snap install certbot-dns-cloudflare certbot-dns-google certbot-dns-hetzner
sudo ln -s /snap/bin/certbot /usr/bin/certbot
{ crontab -l 2>/dev/null; echo "$(( $RANDOM % 60 )) $(( $RANDOM % 3 + 3 )) * * * sudo certbot renew --dry-run" ; } | crontab -
apt install apache2 -y
curl -L -o /etc/apache2/apache2.conf https://dl.san0j.de/setup/apache2.conf
rm /etc/apache2/sites-enabled/000-default.conf
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
Run `curl -L -o /etc/apache2/sites-enabled/DOMAIN.conf https://dl.san0j.de/setup/domains.conf`<br/>
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
Run `curl -L -o /etc/apache2/sites-enabled/SUBDOMAIN.conf https://dl.san0j.de/setup/subdomains.conf`<br/>
`cd /etc/apache2/sites-enabled`
`mv SUBDOMAIN.conf SUBDOMAIN.conf` <- Change Only the last DOMAIN
Replace every `SUBDOMAIN` with your Domain with `nano SUBDOMAIN.conf` <- Change `SUBDOMAIN` in the Command<br/>
Generate before restarting Apache2 a SSL-Certificate with `certbot certonly --apache -d SUBDOMAIN`<br/>
Now restart Apache2 with `service apache2 restart`<br/>

Install Apache2 and Certbot (see https://github.com/2020Sanoj/Debian-Setup#Apache-and-Certbot)
### For IPs:
Replace every `IP` with your IP<br/>
Run `curl -L -o /etc/apache2/sites-enabled/IP.conf https://dl.san0j.de/setup/ip.conf`<br/>
Replace every `IP` with your IP with `nano /etc/apache2/sites-enabled/IP.conf`<br/>
Now restart Apache2 with `service apache2 restart`<br/>


## PHP:
```sh
# Install Apache2 and Certbot (see https://github.com/2020Sanoj/Debian-Setup#Apache-and-Certbot)
apt update && apt upgrade -y && apt autoremove -y
wget -O /etc/apt/trusted.gpg.d/php.gpg https://packages.sury.org/php/apt.gpg
sh -c 'echo "deb https://packages.sury.org/php/ $(lsb_release -sc) main" > /etc/apt/sources.list.d/php.list'
apt update
apt install php8.0 php8.0-cli php8.0-common php8.0-curl php8.0-gd php8.0-intl php8.0-redis php8.0-mbstring php8.0-mysql php8.0-opcache php8.0-readline php8.0-xml php8.0-xsl php8.0-zip php8.0-bz2 libapache2-mod-php8.0 -y
apt install php-zip php-dompdf php-xml php-mbstring php-gd php-curl php-imagick php-intl php-bcmath php-gmp libmagickcore-6.q16-6-extra php-sqlite3 php-apcu -y
apt install php8.0 php8.0-{cli,gd,mysql,pdo,mbstring,tokenizer,bcmath,xml,fpm,curl,zip} -y
apt install libapache2-mod-php php-zip php-dompdf php-xml php-mbstring php-gd php-curl php-imagick libmagickcore-6.q16-6-extra php-intl php-bcmath php-gmp php-cli php-json php-mysql php-zip php-gd  php-mbstring php-curl php-xml php-pear unzip php-apcu redis-server php-redis -y
curl -L -o /etc/php/8.0/apache2/php.ini https://dl.san0j.de/setup/php.ini
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
# Set a Password
y
y
y
y
# Create User with root permissions on MariaDB
mysql -u root -p
# Enter the Password
# Replace "USERNAME" with an Username don not use root and "PASSWORD" with a Password
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
curl -L -o /var/www/pma/config.inc.php https://dl.san0j.de/setup/config.inc.php.txt
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
# Only on x86_64 / amd64 Platforms
apt update && apt upgrade -y && apt autoremove -y
apt-get remove docker docker-engine docker.io containerd runc -y
curl -fsSL https://download.docker.com/linux/debian/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
echo "deb [arch=amd64 signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/debian $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
apt update
apt-get install docker-ce docker-ce-cli containerd.io -y
systemctl start docker
systemctl enable docker
apt update && apt upgrade -y && apt autoremove -y
```

## Docker-Compose
```sh
# Only on x86_64 / amd64 Platforms
# Install Docker (see https://github.com/2020Sanoj/Debian-Setup#Docker)
apt update && apt upgrade -y && apt autoremove -y
curl -L "https://github.com/docker/compose/releases/latest/download/docker-compose-linux-x86_64" -o /usr/local/bin/docker-compose
chmod +x /usr/local/bin/docker-compose
{ crontab -l 2>/dev/null; echo "$(( $RANDOM % 60 )) $(( $RANDOM % 3 + 3 )) * * * curl -L "https://github.com/docker/compose/releases/latest/download/docker-compose-linux-x86_64" -o /usr/local/bin/docker-compose" ; } | crontab -
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
curl -L -o /etc/squid/squid.conf https://dl.san0j.de/setup/squid.conf
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
curl -L -o /etc/proftpd/proftpd.conf https://dl.san0j.de/setup/proftpd10.conf
# Debian 11 Bullseye
curl -L -o /etc/proftpd/proftpd.conf https://dl.san0j.de/setup/proftpd11.conf

service proftpd restart
apt update && apt upgrade -y && apt autoremove -y
```

## Jenkins:
```sh
# Install Java11 (see https://github.com/2020Sanoj/Debian-Setup#Java)
apt update && apt upgrade -y && apt autoremove -y
wget -q -O - https://pkg.jenkins.io/debian/jenkins.io.key | sudo apt-key add -
# For Weekly Updates
add-apt-repository 'deb https://pkg.jenkins.io/debian binary/'
# Or for LTS
add-apt-repository 'deb https://pkg.jenkins.io/debian-stable binary/'
sudo apt-get update
sudo apt-get install jenkins
apt update && apt upgrade -y && apt autoremove -y

# Install Apache2 and Certbot (see https://github.com/2020Sanoj/Debian-Setup#Apache-and-Certbot)
# Set your DNS Records
# Create Jenkins Apache2 Config
curl -L -o /etc/apache2/sites-enabled/ci.conf https://dl.san0j.de/setup/ci.conf
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
# Install Node.js (see https://github.com/2020Sanoj/Debian-Setup#Nodejs)
npm install -g gtop
# Use:
gtop

# vtop:
# Install Node.js (see https://github.com/2020Sanoj/Debian-Setup#Nodejs)
npm install -g vtop
# Use:
vtop

# duf (for disks):
# Install Snapd (see https://github.com/2020Sanoj/Debian-Setup#Snapd)
snap install duf-utility
# Use:
duf

# htop:
apt install htop -y
# Use:
htop
```
