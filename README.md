
# [Debian 10/11 Buster/Bullseye Setup](./README.md#List-of-Content)

**Please run everything as root User (run `su` or `sudo su` and than enter the password)** <br/>
**Please run every Command for its own and Please read the Comments!**

# [List of Content:](./README.md#List-of-Content)<br/>
- [Upgrade Debian 10 -> 11](./Debian-Raspian-Upgrade/readme.md)<br/>
- [Default](./README.md#Default)<br/>
- [Java](./README.md#Java)<br/>
- [MongoDB](./README.md#MongoDB)<br/>
- [Ruby and Rails](./README.md#Ruby-and-Rails)<br/>
- [mono](./README.md#mono)
- [Node.js](./README.md#Nodejs)<br/>
- [Snapd](./README.md#Snapd)<br/>
- [Apache and Certbot](./README.md#Apache-and-Certbot)<br/>
- [Apache2 Configs](./README.md#Apache2-Configs)<br/>
- [PHP](./README.md#PHP)<br/>
- [Composer](./README.md#Composer)<br/>
- [MariaDB](./README.md#MariaDB)<br/>
- [PHPMyAdmin](./README.md#PHPMyAdmin)<br/>
- [docker & docker compose:](./README.md#docker--docker-compose)<br/>
- [docker-compose (compose-switch)](./README.md#docker-compose-compose-switch)<br/>
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
- [Check Open Ports](./README.md#Port-Check)<br/>
- [ZRAM](./README.md#ZRAM)<br/>
- [Sysbench](./README.md#Sysbench)<br/>
- [BIOS-Version](./README.md#BIOS-Version)<br/>

# Default:
```sh
# Required for everything on this Site
apt update -y && apt upgrade -y --allow-downgrades && apt dist-upgrade -y --allow-downgrades && apt autoremove --purge -y && apt autoclean -y && apt clean -y
apt install -y wget maven ftp python-is-python3 dirmngr dmidecode apache2-utils tmux jq python-pip-whl net-tools make sqlite3 youtube-dl ffmpeg vim sudo redis redis-server cron git curl htop neofetch python3-pip screen apt-transport-https lsb-release ca-certificates software-properties-common gnupg gnupg2 gnupg1 nano unzip zip tar perl libnet-ssleay-perl openssl libauthen-pam-perl libpam-runtime libio-pty-perl apt-show-versions mariadb-client
{ crontab -l 2>/dev/null; echo "$(( $RANDOM % 60 )) $(( $RANDOM % 3 + 3 )) * * * apt update -y && apt upgrade -y --allow-downgrades && apt dist-upgrade -y --allow-downgrades && apt autoremove --purge -y && apt autoclean -y && apt clean -y" ; } | crontab -
journalctl --vacuum-time=1d
apt update -y && apt upgrade -y --allow-downgrades && apt dist-upgrade -y --allow-downgrades && apt autoremove --purge -y && apt autoclean -y && apt clean -y
```

## Java:
```sh
apt update -y && apt upgrade -y --allow-downgrades && apt dist-upgrade -y --allow-downgrades && apt autoremove --purge -y && apt autoclean -y && apt clean -y
wget -q -O - https://apt.corretto.aws/corretto.key | apt-key add - 
add-apt-repository 'deb https://apt.corretto.aws stable main'
apt update -y && apt upgrade -y --allow-downgrades && apt dist-upgrade -y --allow-downgrades && apt autoremove --purge -y && apt autoclean -y && apt clean -y
# For Java 19
apt install -y java-19-amazon-corretto-jdk
# For Java 18
apt install -y java-18-amazon-corretto-jdk
# For Java 17
apt install -y java-17-amazon-corretto-jdk
# For Java 16
apt install -y java-16-amazon-corretto-jdk
# For Java 15
apt install -y java-15-amazon-corretto-jdk
# For Java 11
apt install -y java-11-amazon-corretto-jdk
# For Java 8
apt install -y java-1.8.0-amazon-corretto-jdk
apt update -y && apt upgrade -y --allow-downgrades && apt dist-upgrade -y --allow-downgrades && apt autoremove --purge -y && apt autoclean -y && apt clean -y
```

## MongoDB:
```sh
# Not working on every server, no plan why
apt update -y && apt upgrade -y --allow-downgrades && apt dist-upgrade -y --allow-downgrades && apt autoremove --purge -y && apt autoclean -y && apt clean -y
apt remove mongodb
wget -qO - https://www.mongodb.org/static/pgp/server-5.0.asc | apt-key add -
echo "deb http://repo.mongodb.org/apt/debian buster/mongodb-org/5.0 main" | tee /etc/apt/sources.list.d/mongodb-org-5.0.list
apt update -y && apt upgrade -y --allow-downgrades && apt dist-upgrade -y --allow-downgrades && apt autoremove --purge -y && apt autoclean -y && apt clean -y
apt install -y mongodb-org
systemctl daemon-reload
systemctl start mongod
systemctl enable mongod
apt update -y && apt upgrade -y --allow-downgrades && apt dist-upgrade -y --allow-downgrades && apt autoremove --purge -y && apt autoclean -y && apt clean -y
```

## Ruby and Rails
```sh
apt update -y && apt upgrade -y --allow-downgrades && apt dist-upgrade -y --allow-downgrades && apt autoremove --purge -y && apt autoclean -y && apt clean -y
gpg2 --recv-keys 409B6B1796C275462A1703113804BB82D39DC0E3 7D2BAF1CF37B13E2069D6956105BD0E739499BDB
curl -sSL https://get.rvm.io | bash -s stable --rails
source /usr/local/rvm/scripts/rvm
apt update -y && apt upgrade -y --allow-downgrades && apt dist-upgrade -y --allow-downgrades && apt autoremove --purge -y && apt autoclean -y && apt clean -y
```

## mono
```sh
apt update -y && apt upgrade -y --allow-downgrades && apt dist-upgrade -y --allow-downgrades && apt autoremove --purge -y && apt autoclean -y && apt clean -y
apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 3FA7E0328081BFF6A14DA29AA6A19B38D3D831EF
echo "deb https://download.mono-project.com/repo/debian stable-buster main" | tee /etc/apt/sources.list.d/mono-official-stable.list
apt update -y && apt upgrade -y --allow-downgrades && apt dist-upgrade -y --allow-downgrades && apt autoremove --purge -y && apt autoclean -y && apt clean -y
apt install -y mono-complete
apt update -y && apt upgrade -y --allow-downgrades && apt dist-upgrade -y --allow-downgrades && apt autoremove --purge -y && apt autoclean -y && apt clean -y
```

## Node.js:
```sh
apt update -y && apt upgrade -y --allow-downgrades && apt dist-upgrade -y --allow-downgrades && apt autoremove --purge -y && apt autoclean -y && apt clean -y
# For Node.js 18.x
curl -fsSL https://deb.nodesource.com/setup_18.x | bash -
# For Node.js 17.x
curl -fsSL https://deb.nodesource.com/setup_17.x | bash -
# For Node.js 16.x
curl -fsSL https://deb.nodesource.com/setup_16.x | bash -
# For Node.js 14.x
curl -fsSL https://deb.nodesource.com/setup_14.x | bash -
apt update -y && apt upgrade -y --allow-downgrades && apt dist-upgrade -y --allow-downgrades && apt autoremove --purge -y && apt autoclean -y && apt clean -y
apt install -y nodejs
npm config set fund false --global
npm i npm pm2 yarn cross-env -g
{ crontab -l 2>/dev/null; echo "$(( $RANDOM % 60 )) $(( $RANDOM % 3 + 3 )) * * * npm i npm pm2 yarn cross-env -g" ; } | crontab -
apt update -y && apt upgrade -y --allow-downgrades && apt dist-upgrade -y --allow-downgrades && apt autoremove --purge -y && apt autoclean -y && apt clean -y
```

## Snapd
```sh
apt install -y snapd
snap install core; snap refresh core
```

## Apache and Certbot:
```sh
# Install Snapd (see https://github.com/SanCraftDev/Debian-Setup#Snapd)
apt update -y && apt upgrade -y --allow-downgrades && apt dist-upgrade -y --allow-downgrades && apt autoremove --purge -y && apt autoclean -y && apt clean -y
apt remove certbot
snap install --classic certbot
snap set certbot trust-plugin-with-root=ok
snap install certbot-dns-cloudflare
ln -s /snap/bin/certbot /usr/bin/certbot
{ crontab -l 2>/dev/null; echo "$(( $RANDOM % 60 )) $(( $RANDOM % 3 + 3 )) * * * certbot renew --dry-run" ; } | crontab -
apt install -y apache2 libapache2-mod-php
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
apt update -y && apt upgrade -y --allow-downgrades && apt dist-upgrade -y --allow-downgrades && apt autoremove --purge -y && apt autoclean -y && apt clean -y
# To generate an Certificate use "certbot certonly --apache -d DOMAIN" (replace DOMAIN with the Domain or Subdomain)
# SSL dont work with IPs
```

## Apache2 Configs:
First make sure your domain or subdomain is located to your server in its dns setting
### For Domains:

Generate a SSL-Certificate with `certbot certonly --apache -d DOMAIN`<br/>
Replace every `DOMAIN` with your Domain in the next commands<br/>
Generate a SSL-Certificate with `certbot certonly --apache -d DOMAIN`<br/>
Run `curl -L -o /etc/apache2/sites-enabled/DOMAIN.conf https://raw.githubusercontent.com/SanCraftDev/Debian-Setup/main/configs/domains.conf`<br/>
Replace every `DOMAIN` with your Domain with `nano /etc/apache2/sites-enabled/DOMAIN.conf` <- Change `DOMAIN` in the Command and set a folder path under "DocumentRoot", all files in this folder will be aviable on your IP in the web<br/>
Now create this folder with `mkdir /var/www/FOLDER`
Now restart Apache2 with `service apache2 restart`<br/>

### For Subdomains:

Generate a SSL-Certificate with `certbot certonly --apache -d SUBDOMAIN`<br/>
Replace every `DOMAIN` with your Domain in the next commands<br/>
Generate a SSL-Certificate with `certbot certonly --apache -d SUBDOMAIN`<br/>
Run `curl -L -o /etc/apache2/sites-enabled/SUBDOMAIN.conf https://raw.githubusercontent.com/SanCraftDev/Debian-Setup/main/configs/subdomains.conf`<br/>
Replace every `DOMAIN` with your Domain with `nano /etc/apache2/sites-enabled/SUBDOMAIN.conf` <- Change `SUBDOMAIN` in the Command and set a folder path under "DocumentRoot", all files in this folder will be aviable on your IP in the web<br/>
Now create this folder with `mkdir /var/www/FOLDER`
Now restart Apache2 with `service apache2 restart`<br/>

### For IPs:
Replace every `IP` with your IP with your IP in the next commands<br/>
Run `curl -L -o /etc/apache2/sites-enabled/IP.conf https://raw.githubusercontent.com/SanCraftDev/Debian-Setup/main/configs/ip.conf`<br/>
Replace every `IP` with your IP with `nano /etc/apache2/sites-enabled/IP.conf` and set a folder path under "DocumentRoot", all files in this folder will be aviable on your IP in the web<br/>
Now create this folder with `mkdir /var/www/FOLDER`
Save your edit with ctrl + x, then press y and now save it with enter
Now restart Apache2 with `service apache2 restart`<br/>


## PHP:
```sh
# Install Apache2 and Certbot (see https://github.com/SanCraftDev/Debian-Setup#Apache-and-Certbot)
apt update -y && apt upgrade -y --allow-downgrades && apt dist-upgrade -y --allow-downgrades && apt autoremove --purge -y && apt autoclean -y && apt clean -y
wget -O /etc/apt/trusted.gpg.d/php.gpg https://packages.sury.org/php/apt.gpg
sh -c 'echo "deb https://packages.sury.org/php/ $(lsb_release -sc) main" > /etc/apt/sources.list.d/php.list'
apt update -y && apt upgrade -y --allow-downgrades && apt dist-upgrade -y --allow-downgrades && apt autoremove --purge -y && apt autoclean -y && apt clean -y
apt install -y zip unzip redis-server libapache2-mod-php8.1 libmagickcore-6.q16-6-extra php-dompdf php-pear php8.1 php8.1-{cli,gd,mysql,pdo,mbstring,tokenizer,bcmath,xml,fpm,curl,zip,common,intl,redis,opcache,readline,xsl,bz2,imagick,bcmath,gmp,sqlite3,apcu}
curl -L -o /etc/php/8.1/apache2/php.ini https://raw.githubusercontent.com/SanCraftDev/Debian-Setup/main/configs/php.ini
a2enmod proxy_fcgi setenvif
a2enconf php8.1-fpm
cp /etc/php/8.1/apache2/php.ini /etc/php/8.1/cli/php.ini
cp /etc/php/8.1/apache2/php.ini /etc/php/8.1/fpm/php.ini
a2dismod php*
a2enmod php8.1
service php8.1-fpm restart
service apache2 restart
apt update -y && apt upgrade -y --allow-downgrades && apt dist-upgrade -y --allow-downgrades && apt autoremove --purge -y && apt autoclean -y && apt clean -y
```

## Composer:
```sh
apt update -y && apt upgrade -y --allow-downgrades && apt dist-upgrade -y --allow-downgrades && apt autoremove --purge -y && apt autoclean -y && apt clean -y
curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer
{ crontab -l 2>/dev/null; echo "$(( $RANDOM % 60 )) $(( $RANDOM % 3 + 3 )) * * * curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer" ; } | crontab -
apt update -y && apt upgrade -y --allow-downgrades && apt dist-upgrade -y --allow-downgrades && apt autoremove --purge -y && apt autoclean -y && apt clean -y
```

## MariaDB:
```sh
apt update -y && apt upgrade -y --allow-downgrades && apt dist-upgrade -y --allow-downgrades && apt autoremove --purge -y && apt autoclean -y && apt clean -y
apt install -y mariadb-server
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
apt update -y && apt upgrade -y --allow-downgrades && apt dist-upgrade -y --allow-downgrades && apt autoremove --purge -y && apt autoclean -y && apt clean -y
```

## PHPMyAdmin:
```sh
apt update -y && apt upgrade -y --allow-downgrades && apt dist-upgrade -y --allow-downgrades && apt autoremove --purge -y && apt autoclean -y && apt clean -y
curl -L https://www.phpmyadmin.net/downloads/phpMyAdmin-latest-all-languages.zip -o phpmyadmin.zip
unzip phpmyadmin.zip
rm phpmyadmin.zip
mv phpMyAdmin-*-all-languages pma
mv pma /var/www
curl -L -o /var/www/pma/config.inc.php https://raw.githubusercontent.com/SanCraftDev/Debian-Setup/main/configs/config.inc.php.txt
chown -R www-data:www-data /var/www
apt update -y && apt upgrade -y --allow-downgrades && apt dist-upgrade -y --allow-downgrades && apt autoremove --purge -y && apt autoclean -y && apt clean -y

# Install Apache2 and Certbot (see https://github.com/SanCraftDev/Debian-Setup#Apache-and-Certbot)
# Install PHP (see https://github.com/SanCraftDev/Debian-Setup#PHP)

# Add "Alias /pma /var/www/pma" in your Apache Configfile of the Domain/IP you want (in /etc/apache2/sites-enabled)
# Now you can open in the Web your IP/Domain and add after that /pma

# Or create an Subdomain an - set "/var/www/pma" as Directory (see https://github.com/SanCraftDev/Debian-Setup#Apache2-Configs)
# Now you can open in the Web your Subdomain

service apache restart
```

## docker & docker compose:
```sh
apt update -y && apt upgrade -y --allow-downgrades && apt dist-upgrade -y --allow-downgrades && apt autoremove --purge -y && apt autoclean -y && apt clean -y
apt remove docker docker-engine docker.io containerd runc docker-compose -y
curl -fsSL https://download.docker.com/linux/debian/gpg | gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/debian $(lsb_release -cs) stable" | tee /etc/apt/sources.list.d/docker.list > /dev/null
apt update -y && apt upgrade -y --allow-downgrades && apt dist-upgrade -y --allow-downgrades && apt autoremove --purge -y && apt autoclean -y && apt clean -y
apt install -y docker-ce docker-ce-cli containerd.io docker-compose-plugin
systemctl enable --now docker
apt update -y && apt upgrade -y --allow-downgrades && apt dist-upgrade -y --allow-downgrades && apt autoremove --purge -y && apt autoclean -y && apt clean -y
```

## docker-compose (compose-switch)
```sh
# Install Docker (see [https://github.com/SanCraftDev/Debian-Setup#Docker](https://github.com/SanCraftDev/Debian-Setup/blob/main/README.md#docker--docker-compose))

## for the x86_64 / amd64 platform
curl -sL https://github.com/SanCraftDev/compose-compose-switch/releases/latest/download/docker-compose-linux-amd64 -o /usr/local/bin/docker-compose
{ crontab -l 2>/dev/null; echo "0 * * * * curl -sL https://github.com/SanCraftDev/compose-compose-switch/releases/latest/download/docker-compose-linux-amd64 -o /usr/local/bin/docker-compose" ; } | crontab -
chmod +x /usr/local/bin/docker-compose

## for the arm64 / aarch64 / armv8 platform
curl -sL https://github.com/SanCraftDev/compose-compose-switch/releases/latest/download/docker-compose-linux-arm64 -o /usr/local/bin/docker-compose
{ crontab -l 2>/dev/null; echo "0 * * * * curl -sL https://github.com/SanCraftDev/compose-compose-switch/releases/latest/download/docker-compose-linux-arm64 -o /usr/local/bin/docker-compose" ; } | crontab -
chmod +x /usr/local/bin/docker-compose

## for the arm / armhf / armv7 platform
curl -sL https://github.com/SanCraftDev/compose-compose-switch/releases/latest/download/docker-compose-linux-arm -o /usr/local/bin/docker-compose
{ crontab -l 2>/dev/null; echo "0 * * * * curl -sL https://github.com/SanCraftDev/compose-compose-switch/releases/latest/download/docker-compose-linux-arm -o /usr/local/bin/docker-compose" ; } | crontab -
chmod +x /usr/local/bin/docker-compose

apt update -y && apt upgrade -y --allow-downgrades && apt dist-upgrade -y --allow-downgrades && apt autoremove --purge -y && apt autoclean -y && apt clean -y
```

## Docker-Portainer
```sh
# Install Docker-Compose and Docker (see https://github.com/SanCraftDev/Debian-Setup/blob/main/README.md#docker--docker-compose)
apt update -y && apt upgrade -y --allow-downgrades && apt dist-upgrade -y --allow-downgrades && apt autoremove --purge -y && apt autoclean -y && apt clean -y
docker volume create portainer
docker run -d -p 9000:9000 --name=portainer --restart=always -v /var/run/docker.sock:/var/run/docker.sock -v portainer:/data portainer/portainer
apt update -y && apt upgrade -y --allow-downgrades && apt dist-upgrade -y --allow-downgrades && apt autoremove --purge -y && apt autoclean -y && apt clean -y
# Open http://IP:9000 with your Browser
```

## Wireguard (VPN):
```sh
apt update -y && apt upgrade -y --allow-downgrades && apt dist-upgrade -y --allow-downgrades && apt autoremove --purge -y && apt autoclean -y && apt clean -y
wget git.io/wireguard -O /root/wireguard-install.sh && bash /root/wireguard-install.sh
# Select your IP address - Press Enter
# Set a Random Number under 1000 - Press Enter
# Give your first VPN Client an Name - Press Enter
# I recomend to use AdGuard - 6 - Press Enter
# Press Enter
wget git.io/wireguard -O /root/wireguard-install.sh
chmod 700 /root/wireguard-install.sh
apt update -y && apt upgrade -y --allow-downgrades && apt dist-upgrade -y --allow-downgrades && apt autoremove --purge -y && apt autoclean -y && apt clean -y
# Download the Clients from here: https://www.wireguard.com/install/ and load the generated Config
# To create a New Wireguard User or remove one use:
/root/wireguard-install.sh
# Do the same as before
# Client Configs are saved in "/root"
```

## Webmin:
```sh
apt update -y && apt upgrade -y --allow-downgrades && apt dist-upgrade -y --allow-downgrades && apt autoremove --purge -y && apt autoclean -y && apt clean -y
add-apt-repository 'deb https://download.webmin.com/download/repository sarge contrib'
wget https://download.webmin.com/jcameron-key.asc
apt-key add jcameron-key.asc 
rm jcameron-key.asc
apt update -y && apt upgrade -y --allow-downgrades && apt dist-upgrade -y --allow-downgrades && apt autoremove --purge -y && apt autoclean -y && apt clean -y
apt install -y webmin
apt update -y && apt upgrade -y --allow-downgrades && apt dist-upgrade -y --allow-downgrades && apt autoremove --purge -y && apt autoclean -y && apt clean -y
```

## Squid (HTTP-Proxy - with Password Authentication):
```sh
apt update -y && apt upgrade -y --allow-downgrades && apt dist-upgrade -y --allow-downgrades && apt autoremove --purge -y && apt autoclean -y && apt clean -y
apt install -y squid
curl -L -o /etc/squid/squid.conf https://raw.githubusercontent.com/SanCraftDev/Debian-Setup/main/configs/squid.conf
# Create User (Replace "USERNAME" with a Username)
htpasswd -c /etc/squid/passwords USERNAME
# Enter a new Password
service squid restart
# Port is 8449
# The restart take a moment
apt update -y && apt upgrade -y --allow-downgrades && apt dist-upgrade -y --allow-downgrades && apt autoremove --purge -y && apt autoclean -y && apt clean -y
```

## ProFTPD:
```sh
apt update -y && apt upgrade -y --allow-downgrades && apt dist-upgrade -y --allow-downgrades && apt autoremove --purge -y && apt autoclean -y && apt clean -y
apt install -y proftpd
addgroup ftpuser
# Replace USERNAME with an Username and replace PATH with the Folder Path
adduser USERNAME --shell /bin/false --home /PATH --ingroup ftpuser
# Debian 10 Buster
curl -L -o /etc/proftpd/proftpd.conf https://raw.githubusercontent.com/SanCraftDev/Debian-Setup/main/configs/proftpd10.conf
# Debian 11 Bullseye
curl -L -o /etc/proftpd/proftpd.conf https://raw.githubusercontent.com/SanCraftDev/Debian-Setup/main/configs/proftpd11.conf

service proftpd restart
apt update -y && apt upgrade -y --allow-downgrades && apt dist-upgrade -y --allow-downgrades && apt autoremove --purge -y && apt autoclean -y && apt clean -y
```

## Jenkins:
```sh
# Install Java11 (see https://github.com/SanCraftDev/Debian-Setup#Java)
apt update -y && apt upgrade -y --allow-downgrades && apt dist-upgrade -y --allow-downgrades && apt autoremove --purge -y && apt autoclean -y && apt clean -y
wget -q -O - https://pkg.jenkins.io/debian/jenkins.io.key | apt-key add -
# For Weekly Updates
add-apt-repository 'deb https://pkg.jenkins.io/debian binary/'
# Or for LTS
add-apt-repository 'deb https://pkg.jenkins.io/debian-stable binary/'
apt update -y && apt upgrade -y --allow-downgrades && apt dist-upgrade -y --allow-downgrades && apt autoremove --purge -y && apt autoclean -y && apt clean -y
apt install -y jenkins
apt update -y && apt upgrade -y --allow-downgrades && apt dist-upgrade -y --allow-downgrades && apt autoremove --purge -y && apt autoclean -y && apt clean -y

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
apt update -y && apt upgrade -y --allow-downgrades && apt dist-upgrade -y --allow-downgrades && apt autoremove --purge -y && apt autoclean -y && apt clean -y
curl -s https://install.speedtest.net/app/cli/install.deb.sh | bash
apt install -y speedtest
apt update -y && apt upgrade -y --allow-downgrades && apt dist-upgrade -y --allow-downgrades && apt autoremove --purge -y && apt autoclean -y && apt clean -y
```

## Monitoring Services:
```sh
# gtop (recomended):
# Install Node.js (see https://github.com/SanCraftDev/Debian-Setup#Nodejs)
npm install -g gtop
{ crontab -l 2>/dev/null; echo "$(( $RANDOM % 60 )) $(( $RANDOM % 3 + 3 )) * * * npm i gtop -g" ; } | crontab -
# Use:
gtop

# vtop:
# Install Node.js (see https://github.com/SanCraftDev/Debian-Setup#Nodejs)
npm install -g vtop
{ crontab -l 2>/dev/null; echo "$(( $RANDOM % 60 )) $(( $RANDOM % 3 + 3 )) * * * npm i vtop -g" ; } | crontab -
# Use:
vtop

# duf (for disks):
# Install Snapd (see https://github.com/SanCraftDev/Debian-Setup#Snapd)
snap install duf-utility
# Use:
duf
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
# Replace "master" with the name of the branche (Default: master or main)
git reset --hard origin/master
```

## Port-Check
```sh
netstat -tulpn | grep LISTEN
```

## ZRAM
```sh
apt update -y && apt upgrade -y --allow-downgrades && apt dist-upgrade -y --allow-downgrades && apt autoremove --purge -y && apt autoclean -y && apt clean -y
apt install -y zram-tools
nano /etc/default/zramswap
# Now replace `#SIZE=256` with `SIZE=1024` you can set a value over 1024 if you want to use more than 1024 MB of ZRAM / Swap
service zramswap restart
apt update -y && apt upgrade -y --allow-downgrades && apt dist-upgrade -y --allow-downgrades && apt autoremove --purge -y && apt autoclean -y && apt clean -y
```

## Sysbench
```sh
# On Debian 11
apt update -y && apt upgrade -y --allow-downgrades && apt dist-upgrade -y --allow-downgrades && apt autoremove --purge -y && apt autoclean -y && apt clean -y
apt install -y sysbench
sysbench run cpu
apt update -y && apt upgrade -y --allow-downgrades && apt dist-upgrade -y --allow-downgrades && apt autoremove --purge -y && apt autoclean -y && apt clean -y
```

## BIOS-Version
```sh
apt update -y && apt upgrade -y --allow-downgrades && apt dist-upgrade -y --allow-downgrades && apt autoremove --purge -y && apt autoclean -y && apt clean -y
dmidecode -s bios-version
apt update -y && apt upgrade -y --allow-downgrades && apt dist-upgrade -y --allow-downgrades && apt autoremove --purge -y && apt autoclean -y && apt clean -y
```
