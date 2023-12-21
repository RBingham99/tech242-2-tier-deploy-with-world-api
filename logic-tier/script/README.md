```
#!/bin/bash

# Update
echo "Update..."
sudo apt update -y
echo "Done!"
echo ""

# Upgrade
echo "Upgrade..."
sudo DEBIAN_FRONTEND=noninteractive apt upgrade -y
echo "Done!"
echo ""

# Install maven
echo "Install maven..."
sudo DEBIAN_FRONTEND=noninteractive apt install maven -y
echo "Done!"
echo ""

# Check maven is installed
echo "Check maven is installed..."
mvn -version
echo "Done!"
echo ""

# install JDK (java) 17
echo "Install java..."
sudo DEBIAN_FRONTEND=noninteractive apt install openjdk-17-jdk -y
echo "Done!"
echo ""

# check JDK is installed
echo "Check java is installed..."
java -version
echo "Done!"
echo ""
# Install, start and enable MYSQL
echo "Install MYSQL..."
sudo DEBIAN_FRONTEND=noninteractive apt install mysql-server -y
echo "Done!"
echo ""

echo "Start MYSQL..."
sudo systemctl start mysql
echo "Done!"
echo ""

echo "Enable MYSQL..."
sudo systemctl enable mysql
echo "Done!"
echo ""

# Check MYSQL installed
echo "Check MYSQL..."
sudo systemctl status mysql
echo "Done!"
echo ""

# Copy the app code to this vm
echo "Clone app from github..."
sudo git clone https://github.com/RBingham99/tech242-world-app.git repo
echo "Done!"
echo ""

# Install apache
echo "Install apache..."
sudo DEBIAN_FRONTEND=noninteractive apt install apache2 -y
echo "Done!"
echo ""

# Start apache
echo "Starting apache..."
sudo systemctl start apache2
echo "Done!"
echo ""

# Enable apache
echo "Enabling Apache..."
sudo systemctl enable apache2
echo "Done!"
echo ""

# Check Apache
echo "Check apache is installed..."
sudo systemctl status apache2
echo "Done!"
echo ""

# Install apache modules
echo "Installing apache modules..."
sudo a2enmod proxy
sudo a2enmod proxy_http
echo "Done!"
echo ""

# Open and edit config file
echo "Editing apache config..."
VHOST_CONF="/etc/apache2/sites-available/000-default.conf"
cat <<EOF | sudo tee "$VHOST_CONF" > /dev/null
<VirtualHost *:80>
    ServerAdmin webmaster@localhost
    DocumentRoot /var/www/html

    ProxyPreserveHost On
    ProxyPass / http://localhost:5000/
    ProxyPassReverse / http://localhost:5000/

    ErrorLog \${APACHE_LOG_DIR}/error.log
    CustomLog \${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
EOF
echo "Done!"
echo ""

# Restart apache
echo "Restarting apache..."
sudo systemctl reload apache2
echo "Done!"
echo ""

# Create enviroment variables
export DB_HOST=jdbc:mysql://172.31.38.134:3306/world
export DB_USER=root
export DB_PASS=root
export DB_IP=172.31.38.134
export MYSQL_PWD=root

# Conditional to check can connect to database
echo "Checking database connection..."
if mysql -u"$DB_USER" -h"$DB_IP" -e "use world"; then
    echo "Done!"
    echo ""

    echo "navigating into repo..."
    cd /repo/WorldProject
    echo "Done!"
    echo ""

    echo "Run application..."
    sudo -E mvn package spring-boot:start
    echo "Done!"
    echo ""
else
    echo "Error: Unable to connect to MySQL database."
fi
```