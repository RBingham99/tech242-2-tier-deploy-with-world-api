## Data tier manual testing and scripting

# Manual testing
The start of manual testing for the data tier was fairly straigh forward, update, upgrade, install dependancies and clone the repo.

It became a little harder when I needed to reconfigure and use MYSQL, fortunately ChatGPT told me which file I needed to edit, which I did using the `nano` command, and it also helped me with the SQL commands I needed to set up the database and change the permissions.

# Scripting
When it came to scripting the data tier it again started fairly easily however when it came to creating the database and changing the permissions it became a bit harder as the script would stop running when I logged into the MYSQL interface, however with a little researching and asking ChatGPT I discovered that there are commands that you can use in bash that will run in sql like `sudo mysql < repo/world.sql` to create and seed the database rather than logging into MYSQL and then running `source repo/world.sql`, so it didn't engage my terminal. A similar command I found was `sudo mysql -e "SQL COMMAND"`, the `-e` runs the SQL command in the SQL interface without engaging the terminal with the MYSQL interface.