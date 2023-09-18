---
Created: Invalid date
URL: https://bobcares.com/blog/docker-wordpress-multisite/
---
_Wondering how to create Docker wordpress multisite? We can help you._

At Bobcares, we offer solutions for every query, big and small, as a part of our [Server Management Service](https://bobcares.com/server-management/).

Let’s take a look at how our [Support Team](https://bobcares.com/server-management/) assist with this query.

## How to create Docker wordpress multisite?

Today, let us see the steps followed by our [Support techs](https://bobcares.com/server-management/):

### **Create a Dockerfile**

Firstly, create a Dockerfile for our custom WP image.

Make sure to use the default name “Dockerfile” so docker-compose knows what to do.

```
FROM wordpress:latestCOPY wp-config-sample.php /usr/local/bin/COPY wordpress-entrypoint.sh /usr/local/bin/ENTRYPOINT ["wordpress-entrypoint.sh"]CMD ["apache2-foreground"]
```

The first line says we are inheriting everything from the “wordpress:latest” image.

The next line puts the wp-config-sample.php file that we will modify in a safe place until the entrypoint script runs.

If we tried to put it in /var/www/html it would get written over when WordPress is downloaded into that directory.

The second COPY line puts our custom entrypoint script into the container.

The ENTRYPOINT line runs our custom entrypoint script, which is just the default docker-entrypoint.sh script with a minor change we will make.

And finally, the CMD line runs our Apache server.

### **Modify wp-config-sample.php**

Add the following just above “/* That’s all, stop editing! Happy blogging. */” to a clean copy of wp-config-sample.php:

```
define('WP_ALLOW_MULTISITE', true);
```

### **Create the Entrypoint Script**

The default entrypoint script is located in the same directory as the Dockerfile on GitHub.

Then, copy the contents of this file into a new one called wordpress-entrypoint.sh and add the following after line 52:

```
# Overwrite wp-config-sample.php with the multisite versionmv -f /usr/local/bin/wp-config-sample.php /var/www/html/wp-config-sample.php
```

### **Create and run docker-compose.yml**

```
version: '3.1'services:  wordpress:    build: .    image: wordpress-multisite    environment:      WORDPRESS_TABLE_PREFIX: wp_      WORDPRESS_DB_PASSWORD: mysecurepwd      WORDPRESS_DEBUG: 'true'    ports:      - '80:80'    volumes:      - wordpress:/var/www/html      - ./themes:/var/www/html/wp-content/themes      - ./.htaccess:/var/www/html/.htaccess    depends_on:      - mysql  mysql:    image: mariadb    environment:      MYSQL_ROOT_PASSWORD: mysecurepwd    ports:      - '3306:3306'    volumes:      - db-data:/var/lib/mysql  volumes:    wordpress:    db-data:
```

Setting up the volumes isn’t entirely necessary, but it’s nice to have them named something sensible like “wordpress” and “db-data” rather than a random string when you run a “docker volume ls” command like we will do later.

Also, we created a mounted volume for our themes directory since this is likely what we’ll modifying in a development environment.

After saving your compose file, run:

```
docker-compose up -d
```

### **Install WordPress**

Go to http://localhost in a browser and you should see the familiar WordPress installation wizard.

If something goes wrong you can troubleshoot by viewing your wp-config.php file in the running wordpress container by entering: docker exec -it [your folder name]_wordpress_1 /bin/bash

This should take you to a shell prompt in your container and you can view your file by running: cat wp-config.php.

The first thing I would look for is to see if WP_DEBUG is set to ‘true’ instead of the default ‘false’ value.

Enter your language, username, password, etc.

Log in after the installation is complete and enable Multisite by navigating to Tools > Network Setup.

After clicking “Install”, WordPress will give you some instructions on how to enable the network by changing wp-config.php and .htaccess.

This is what we will do next.

### **Modify wp-config-sample.php**

Add the lines from the admin screen to wp-config-sample.php above the line reading /* That’s all, stop editing! Happy blogging.

They should look something like this:

```
define('MULTISITE', true);define('SUBDOMAIN_INSTALL', false);define('DOMAIN_CURRENT_SITE', 'localhost');define('PATH_CURRENT_SITE', '/');define('SITE_ID_CURRENT_SITE', 1);define('BLOG_ID_CURRENT_SITE', 1);
```

### **Create an .htaccess file**

Copy the text from the admin into a file named .htaccess It should look like this:

```
RewriteEngine OnRewriteBase /RewriteRule ^index\.php$ - [L]# add a trailing slash to /wp-adminRewriteRule ^([_0-9a-zA-Z-]+/)?wp-admin$ $1wp-admin/ [R=301,L]RewriteCond %{REQUEST_FILENAME} -f [OR]RewriteCond %{REQUEST_FILENAME} -dRewriteRule ^ - [L]RewriteRule ^([_0-9a-zA-Z-]+/)?(wp-(content|admin|includes).*) $2 [L]RewriteRule ^([_0-9a-zA-Z-]+/)?(.*\.php)$ $2 [L]RewriteRule . index.php [L]
```

### **Modify docker-compose.yml**

Under the wordpress volumes section add the following line which will override the default .htaccess file with the new one.

```
- ./.htaccess:/var/www/html/.htaccess
```

### **Rebuild the WordPress Image**

Now we have to clean up our docker environment by stopping the processes:

```
docker-compose down
```

Removing our wordpress-multisite image:

```
docker image rm wordpress-multisite
```

Removing our wordpress volume: (you can get your volume’s name by running docker volume ls)

```
docker volume rm [volume name]
```

Now that everything has been cleaned up, we can re-run:

```
docker-compose up -d
```

This will rebuild our WordPress image and volume with the updated wp-config.php file and our new .htaccess file.

### **Bask in the Glory of WordPress Multisite using Docker**

When you log back into your WordPress Admin, hover over “My Sites” in the top-left.

If all has gone well, Network Admin will be the first link you see.

**[Looking for a solution to another query?** [**We are just a click away**](https://bobcares.com/microsoft-sql-server-support/)**.]**

## **Conclusion**

To sum up, our skilled [Support Engineers](https://bobcares.com/microsoft-sql-server-support/) at Bobcares demonstrated how to create Docker wordpress multisite

### Submit a Comment

Managing a server is time consuming. Whether you are an expert or a newbie, that is time you could use to focus on your product or service. Leave your server management to us, and use that time to focus on the growth and success of your business.