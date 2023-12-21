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

# Clone repo
echo "Clone repo..."
sudo git clone https://github.com/RBingham99/tech242-world-app-db.git repo
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

# Edit MYSQL config file
echo "Edit MYSQL config file..."
sudo sed -i '0,/bind-address.*/s//bind-address            = 0.0.0.0/' /etc/mysql/mysql.conf.d/mysqld.cnf
echo "Done!"
echo ""

# Run SQL script
echo "Seed MYSQL database..."
sudo mysql < repo/world.sql
echo "Done!"
echo ""

# Check script ran correctly
echo "Check MYSQL script ran correctly..."
sudo mysql -e 'SHOW DATABASES;'
sudo mysql -e 'USE world; SHOW TABLES;'
echo "Done!"
echo ""

# Delete root user in exists
sudo mysql -e "REVOKE ALL PRIVILEGES ON *.* FROM 'root'@'%'; FLUSH PRIVILEGES;"
sudo mysql -e "DROP USER IF EXISTS 'root'@'%';"

# Allow access to root from anywhere
echo "Create MYSQL user..."
sudo mysql -e "CREATE USER 'root'@'%' IDENTIFIED BY 'root';"
sudo mysql -e "GRANT ALL PRIVILEGES ON *.* TO 'root'@'%';"
sudo mysql -e "GRANT GRANT OPTION ON *.* TO 'root'@'%';"
sudo mysql -e "FLUSH PRIVILEGES;"
echo "Done!"
echo ""

# Restart MYSQL
echo "Restart MYSQL..."
sudo systemctl restart mysql
echo "Done!"
echo ""
```