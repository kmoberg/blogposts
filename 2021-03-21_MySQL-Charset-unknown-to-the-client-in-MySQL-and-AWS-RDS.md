Struggling with a strange MySQL error when connecting to MySQL Version 8?

```PDO::__construct(): Server sent charset (255) unknown to the client. Please, report to the developers```

Turns out this error is related to how MySQL sets the default charset to _utf8mb4_, which isn't known to a lot of the worlds programming languages such as PHP < 7.3 (?), C++/MySQL connector etc. which can be a bit frustrating to fix, having to be forced to upgrade the application.++

There is, however a simple fix! If you're hosting your own MySQL database, you only need to change the MySQL Config file (/etc/my.cnf) to the following:

```[client]
default-character-set=utf8

[mysql]
default-character-set=utf8


[mysqld]
collation-server = utf8_unicode_ci
character-set-server = utf8
```

Once that is done, restart `mysqld` and you should be good to go, however, if you're using AWS RDS, you do not have direct access to the MySQL config file. Fret not! There is a way to fix this too! You need to configure some DB Parameter Groups which will allow you to add the parameters we need. This can be done either from the console, AWS CLI or RDS API. Reference the AWS Documentation for more info.

Login to your AWS Console, navigate to AWS RDS and find your databases.

![Screen-Shot-2020-08-18-at-23.42.56-edited.f16987a33c6544d68b5e3f0bc785166f](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/x9smc3coh85926vf721d.png)
 

Start by navigating to the `Parameter Groups` menu on the left and click to `Create parameter group`. 

Give the group a new name and description, and make sure that the group family is mysql8.0. Create the group, then navigate back to the group's page and click the newly created group. 

Click `Edit` parameters in the top right to start modifying the parameters. In the filters box, search for "CHAR". You should set the following:

- Character_set_client = "utf8"
- Character_set_connection = "utf8"
- Character_set_database = "utf8"
- Character_set_server = "utf8"

![Screen-Shot-2020-08-18-at-23.47.04-edited.482575f4d17142f39666c3619a68ed75](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/z515m75lh8iau0jwe6rm.png)

Finish by saving the changes. Now navigate back to your database, and click the "Modify" button to make changes to the configuration of the RDS instance. 

Scroll down to locate the `Database options` then locate the `DB parameter group` and set it to the group you just created. Scroll down to save.

![Screen-Shot-2020-08-18-at-23.48.34-edited.7221cdebe8f94863b250164e6fa52dc3](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/5nqkmzghw7acx6vmy9rx.png) 

## Important!
 When applying the changes, you WILL be asked when to apply the changes. This can either be done in the next maintenance window, which could be far in the future, or you can set it to happen immediately. **If you set it to immediately the MySQL instance _will become unavailable_ wile it applies the changes!** This can take from a few minutes up to 10-15 minutes.

In addition to this you **must** do a **restart** of the RDS instance to ensure that the database has been updated with the changes. **This will also make the MySQL instance unavailable while it is restarting!**

However, once you have restarted the instance, you should now be able to connect to the instance using older programming languages once again!
