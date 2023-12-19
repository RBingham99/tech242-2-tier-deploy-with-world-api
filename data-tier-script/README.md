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
SHOW DATABASES;
SHOW TABLES;

# Allow access to root from anywhere
CREATE USER 'root'@'%' IDENTIFIED BY 'root';
GRANT ALL PRIVILEGES ON *.* TO 'root'@'%';
GRANT GRANT OPTION ON *.* TO 'root'@'%';
FLUSH PRIVILEGES;

# Exit SQL
exit

# Restart MYSQL
sudo systemctl restart mysql