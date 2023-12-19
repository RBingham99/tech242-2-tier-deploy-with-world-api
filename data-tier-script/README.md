#!/bin/bash

# Update
sudo apt update -y

# Upgrade
sudo DEBIAN_FRONTEND=noninteractive apt upgrade -y

# Clone repo
sudo git clone https://github.com/RBingham99/tech242-world-app-db.git repo

# Install, start and enable MYSQL
sudo DEBIAN_FRONTEND=noninteractive apt install mysql-server -y
sudo systemctl start mysql
sudo systemctl enable mysql

# Check MYSQL installed
sudo systemctl status mysql

# Edit MYSQL config file
sudo sed -i '0,/bind-address.*/s//bind-address            = 0.0.0.0/' /etc/mysql/mysql.conf.d/mysqld.cnf

<!-- # Access MYSQL
sudo mysql -->

# Run SQL script
sudo mysql < repo/world.sql

# Check script ran correctly
sudo mysql -e 'SHOW DATABASES;'
sudo mysql -e 'USE world; SHOW TABLES;'

# Allow access to root from anywhere
sudo mysql -e "CREATE USER 'root'@'%' IDENTIFIED BY 'root';"
sudo mysql -e "GRANT ALL PRIVILEGES ON *.* TO 'root'@'%';"
sudo mysql -e "GRANT GRANT OPTION ON *.* TO 'root'@'%';"
sudo mysql -e "FLUSH PRIVILEGES;"

<!-- # Exit SQL
exit -->

# Restart MYSQL
sudo systemctl restart mysql