## AMI

Creating an AMI for the logic tier was fairly easy but had a few more steps than creating an AMI for data tier.

1) First I created an AMI from the instance that was working with user data, just like I did with the data tier.
2) Then I created a new instance off of that AMI.
3) But in the user data of the new instance I did need to add a few things, unlike I did with the data tier.
   What I added was:
   ```
   #!/bin/bash

    export DB_HOST=jdbc:mysql://IP_OF_DATABASE:3306/world
    export DB_USER=root
    export DB_PASS=root
    export DB_IP=IP_OF_DATABASE
    export MYSQL_PWD=root

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
   But where it says 'IP_OF_DATABASE' I entered the IP of the database I want to connect to, I did this so I can use this AMI to connect to the database even if it is running on a differant instance with a differant IP. After the export lines I added my conditional to check the database connection and the commands to run the app if it could connect to the database or to output an error if it cannot.