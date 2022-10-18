### Install Packages:
```bash
apt-get install software-properties-common
apt-key adv --recv-keys --keyserver hkp://keyserver.ubuntu.com:80 0xF1656F24C74CD1D8
add-apt-repository -y 'deb [arch=amd64,i386,ppc64el] http://mirrors.coreix.net/mariadb/repo/10.3/ubuntu xenial main'
add-apt-repository -y 'ppa:ondrej/php'
add-apt-repository -y 'ppa:ondrej/nginx'
add-apt-repository -y ppa:certbot/certbot
apt-get update
apt-get -y install gcc curl gzip sqlite3 git tar software-properties-common nginx php8.1-fpm php8.1-xml php8.1-bz2  php8.1-zip php8.1-mysql php8.1-intl php8.1-bcmath php8.1-gd php8.1-curl php8.1-soap php8.1-mbstring python-certbot-nginx composer mariadb-server
#
```
## Optional Package for Image Optimization
These are used by *Laravel Media Library* package. So if you're using that, install these.
```bash
apt-get -y install jpegoptim optipng pngquant gifsicle
```
### Configure PHP
```bash
sed -i 's/upload_max_filesize = 2M/upload_max_filesize = 100M/g' /etc/php/8.1/fpm/php.ini
sed -i 's/max_execution_time = 30/max_execution_time = 600/g' /etc/php/8.1/fpm/php.ini
sed -i 's/max_input_time = 60/max_input_time = 600/g' /etc/php/8.1/fpm/php.ini
sed -i 's/post_max_size = 8M/post_max_size = 120M/g' /etc/php/8.1/fpm/php.ini
sed -i 's/memory_limit = 128M/memory_limit = 512M/g' /etc/php/8.1/fpm/php.ini
service php8.1-fpm restart
#
```
### Clone git repo
```bash
git clone https://github.com/AfzalH/lara-server.git
```

### Create a site: Enter domain
```bash
cd lara-server
echo 'Enter Site Domain [site.com]:' && read site_com
```

...And Change config
```bash
cp nginx/srizon.com /etc/nginx/sites-available/$site_com
sed -i "s/srizon.com/${site_com}/g" /etc/nginx/sites-available/$site_com
mkdir /var/www/$site_com
mkdir /var/www/$site_com/public
touch /var/www/$site_com/public/index.php
ln -s /etc/nginx/sites-available/$site_com /etc/nginx/sites-enabled/$site_com
service nginx reload
```

### Edit to test out
```bash
nano /var/www/$site_com/public/index.php
```

### Clear document root
```bash
rm -rf /var/www/$site_com/public
```
### Move to document root to clone laravel project
```bash
cd /var/www/$site_com
```

Now clone your project. Create database and connect on .env

### Creating database
`replace dbname, username and password before copy-pasting`

```bash
mysql -u root -p
CREATE DATABASE dbname;
GRANT ALL ON dbname.* TO username@localhost IDENTIFIED BY 'password';
exit;
```
### https
```bash
certbot --nginx
```
