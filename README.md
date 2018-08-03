# Wordpress-alpine-php Docker Image
This is a WordPress Docker image which can run on both [Azure Web App on Linux](https://docs.microsoft.com/en-us/azure/app-service-web/app-service-linux-intro) and your Docker engines's host.

## Components
This docker image currently contains the following components:

1. WordPress
2. Alpine (3.7)
3. PHP (7.1.7)
4. Apache/Httpd (2.4.33)
5. MariaDB ( 10.1.32/if using Local Database )
6. Phpmyadmin ( 4.8.0/if using Local Database )

## How to configure to use Azure Database for MySQL with web app
1. Create a Web App for Containers
2. Browse your site
3. Complete WordPress install and Enter the Credentials for Azure database for MySQL

## How to configure GIT Repo and Branch
1. Create a Web App for Containers
2. Add new App Settings

Name | Default Value
---- | -------------
GIT_REPO | https://github.com/tsferr/wp-linux
GIT_BRANCH | master

4. Browse your site

>Note: GIT directory: /home/site/wwwroot.
>
>Note: When you deploy it first time, Sometimes need to check wp-config.php. RM it and re-config DB information is necessary.
>
>Note: Before restart web app, need to store your changes by "git push", it will be pulled again after restart.
>

## How to configure to use Local Database with web app
1. Create a Web App for Containers
2. Update App Setting ```WEBSITES_ENABLE_APP_SERVICE_STORAGE``` = true (If you like to keep you DB after restart.)
3. Add new App Settings

Name | Default Value
---- | -------------
DATABASE_TYPE | local
DATABASE_USERNAME | wordpress
DATABASE_PASSWORD | some-string
>Note: We create a database "azurelocaldb" when using local mysql . Hence use this name when setting up the app

4. Browse your site
5. Complete WordPress install

>Note: Do not use the app setting DATABASE_TYPE=local if using Azure database for MySQL

## How to turn on Xdebug
1. By default Xdebug is turned off as turning it on impacts performance.
2. Connect by SSH.
3. Go to ```/etc/php7/conf.d```,  Update ```xdebug.ini``` as wish, don't modify the path of below line.
```zend_extension=/usr/lib/php7/modules/xdebug.so```
4. Save ```xdebug.ini```, Restart apache by below cmd:
```/usr/sbin/httpd -k restart```
5. Xdebug is turned on.

## Updating WordPress version , themes , files

If ```WEBSITES_ENABLE_APP_SERVICE_STORAGE``` = false  ( which is the default setting ) , we recommend you DO NOT update the WordPress core version , themes or files from WordPress admin dashboard.

Choose either one option to updated your files :

There is a tradeoff between file server stability and file persistence . Since we are using local storage for better stability for the web app , you will not get file persistence.  In this case , we recommend to follow these steps to update WordPress Core  or a theme or a Plugins version :
1.	Fork the repo https://github.com/azureappserviceoss/wordpress-azure
2.	Clone your repo locally and make sure to use ONLY linux-appservice branch
3.	Download the latest version of WordPress , plugin or theme being used locally
4.	Commit the latest version bits into local folder of your cloned repo
5.	Push your changes to the your forked repo
6.	Login to Azure portal and select your web app
7.	Click on Application Settings -> App Settings and change GIT_REPO to use your repository from step #1. If you have changed the branch name , you can continue to use linux-apservice . If you wish to use a different branch , update GIT_BRANCH setting as well.

## Connect WordPress to Redis
1. Log-in to WordPress admin. In the left navigation, select **Plugins**, and then select **Add New**.
2. Search **Redis Object Cache**, Click **Install**, wait, then click **Activate**.
3. In the left navigation, select **Plugins**, and then select **Installed Plugins**.
4. In the plugins page, find **Redis Object Cache** and click **Settings**.
5. Click the **Enable Object Cache** button.
6. WordPress connects to the Redis server. The connection status appears on the same page.
7. [More infomation about **Redis Object Cache**](https://wordpress.org/plugins/redis-cache)

## Limitations
- Some unexpected issues may happen after you scale out your site to multiple instances, if you deploy a WordPress site on Azure with this docker image and use the MariaDB built in this docker image as the database.
- The phpMyAdmin built in this docker image is available only when you use the MariaDB built in this docker image as the database.
- Please Include  App Setting ```WEBSITES_ENABLE_APP_SERVICE_STORAGE``` = true  when use built in MariaDB since we need files to be persisted.


# How to Contribute
If you have feedback please create an issue but **do not send Pull requests** to these images since any changes to the images needs to tested before it is pushed to production.
