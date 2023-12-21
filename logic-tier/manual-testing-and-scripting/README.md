## Logic tier manual testing and scripting

# Manual testing
Similarly to the data tier the manual testing for the logic tier started quite easily, with the commands to update, upgarade, install the dependancies and clone the project code.

I did run into an issue when trying to run the application but with some help from my team we discovered the commands `mvn clean` and `mvn install`, which allowed my app to run when I used the `mvn spring-boot:start` command.

I then moved on to using enviroment variables, which were not hard to create, but it caused issues running my app until my team told me that I needed to put `-E` at the beginning of my start command to pass the enviroment variables to my app.

One thing I found particularly useful when manually testing the logic tier was to install MYSQL so I could manually check the database connection.

# Scripting
When scripting the logic tier it again began easily with the commands to update, upgarade, install the dependancies, cloning the project code and creating the enviroment variables. The only issue I ran into was with the conditional to check the database connection, but with some help with ChatGPT it wasn't to difficult to overcome.