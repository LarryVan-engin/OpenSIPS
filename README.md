# OpenSIPS
sudo su
apt update
apt install wget gnupg2

#Them Opensips cho repository cua ubuntu
curl https://apt.opensips.org/opensips-org.gpg -o /usr/share/keyrings/opensips-org.gpg
echo "deb [signed-by=/usr/share/keyrings/opensips-org.gpg] https://apt.opensips.org focal 3.5-releases" >/etc/apt/sources.list.d/opensips.list

curl https://apt.opensips.org/opensips-org.gpg -o /usr/share/keyrings/opensips-org.gpg
echo "deb [signed-by=/usr/share/keyrings/opensips-org.gpg] https://apt.opensips.org focal cli-nightly" >/etc/apt/sources.list.d/opensips-cli.list 

# Cai dat opensips
apt update
apt install opensips opensips-mysql-module opensips-cli



# Cai dat mysqlserver
apt install mysql-server
sudo systemctl status MySQL

# Tao database
sudo mysql -u root -p
UNINSTALL COMPONENT 'file://component_validate_password';
Exit

# Lenh tao cac bang
sudo opensips-cli -x database create

# Bat tu dong khoi dong opensips
sudo systemctl enable opensips
sudo systemctl start opensips

# Cai dat opensips control panel
sudo apt install -y apache2 php php-mysql php-cli php-xml php-curl git unzip

sudo nano /etc/apache2/sites-available/opensips-cp.conf

# Dan doan nay vao
<VirtualHost *:80>
    DocumentRoot /var/www/html/opensips-cp
    <Directory /var/www/html/opensips-cp>
        Options Indexes FollowSymLinks
        AllowOverride All
        Require all granted
    </Directory>
</VirtualHost>

# Tai opensips-cp GUI ve va cap quyen
cd /var/www/html
sudo git clone https://github.com/OpenSIPS/opensips-cp.git
sudo chown -R www-data: www-data opensips-cp
mysql -Dopensips -p < config/db_schema.mysql
cp config/tools/system/smonitor/opensips_stats_cron /etc/cron.d/
sudo a2ensite opensips-cp
sudo systemctl restart apache2
