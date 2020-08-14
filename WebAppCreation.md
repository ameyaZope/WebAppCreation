# WebSites and WebApps

## 1 Way to Create a Website/WebApp : LAMP Stack

-------------

### Method 1 : LAMP Stack without WordPreess
In this method you simply need to install the apache2 server onto the hardware that shall be used to host the website. After that make some configuration chages and some html files(as required). The configuration changes and the files that needed to be edited can be found here : 

1. Fast Method(This does not set up the entire LAMP Stack, rather sets up only Apache HTTP Server without backend and persistence support. For back end and persistance support you need to set up Database and backend and this is LAMP stack)https://www.liquidweb.com/kb/configure-apache-virtual-hosts-ubuntu-18-04/

2. Full Fledged LAMP Stack without WordPress: https://www.digitalocean.com/community/tutorials/how-to-install-linux-apache-mysql-php-lamp-stack-ubuntu-18-04

The above full fledged LAMP stack setup can be summarised as follows
1. Install Apache2 using the following commands
```bash
sudo apt update
sudo apt install apache2
``` 

2. Install MySql using the following commands
```bash
sudo apt install mysql-server
sudo mysql_secure_installation
```
	* Note : Answer 'Y' in all the questions asked in mysql_secure_installation command
	* Configure MySql root user to take authentication via mysql_native_password.

3. Install PHP using the following commnads
```bash
sudo apt install php libapache2-mod-php php-mysql
```
	* In most cases, you will want to modify the way that Apache serves files when a directory is requested. Currently, if a user requests a directory from the server, Apache will first look for a file called index.html. We want to tell the web server to prefer PHP files over others, so make Apache look for an index.php file first.
	
	* sudo nano /etc/apache2/mods-enabled/dir.conf ---> the f.php file should be the first after DirectoryIndex

	* After this, restart the Apache web server in order for your changes to be recognized. Do this by typing this:
	```bash
		sudo systemctl restart apache2
	```

4. Set up Virtual Host using the following steps. Each domain must contain your own folder in /var/www/ and its own .conf file in /etc/apache2/sites-available/ :  
	* sudo mkdir /var/www/your_domain
	* sudo chown -R $USER:$USER /var/www/your_domain
	* sudo chmod -R 755 /var/www/your_domain
	* nano /var/www/your_domain/index.html and add below content inside the file
	```html
	<html>
    <head>
        <title>Welcome to Your_domain!</title>
    </head>
    <body>
        <h1>Success!  The your_domain server block is working!</h1>
    </body>
	</html>	
	```
	* sudo nano /etc/apache2/sites-available/your_domain.conf and add the following content into your file
	```conf
	<VirtualHost *:80>
    ServerAdmin webmaster@localhost
    ServerName your_domain
    ServerAlias www.your_domain
    DocumentRoot /var/www/your_domain
    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined
	</VirtualHost>
	```
	* sudo a2ensite your_domain.conf (This command can be used to enable the configuration for your website)
	* sudo a2dissite 000-default.conf (This command is used to disable the configuration for default website)
	* sudo systemctl restart apache2 (This command restart Apache HTTP server to implement the above changes)
5. Done, your lamp stack website is set up. 



Follow the above mentioned guide to set up an apache2 server that serves a website. This server will directly render .html files.

### Method 2 : LAMP Stack with WordPress
WordPress allows one to create beautilful blogs via themes that are either free or paid. There are many free themes on WordPress. Using WordPress, a website/blog can be made very quickl. Much quicker than java+Spring, infact customizing the front end is also easier. And it is also easy to get a more beautiful UI with WordPress because of the use of themes for websites in WordPress. The easiest way to set up a LAMP + WordPress website is mentioned here : https://www.digitalocean.com/community/tutorials/how-to-install-wordpress-with-lamp-on-ubuntu-18-04

To summarize the steps performed in the above link

1. Follow all the steps required to create a website with LAMP stack only with the root diretory as /var/www/wordpress

2. Create MySQL user by the following commands in mysql
```mysql
	CREATE DATABASE wordpress DEFAULT CHARACTER SET utf8 COLLATE utf8_unicode_ci;

	CREATE USER wordpressuser@localhost IDENTIFIED BY '123456';

	GRANT ALL ON wordpress.* TO 'wordpressuser'@'localhost';

	FLUSH PRIVILEGES;

	EXIT;
```

3. Install some php dependencies that might be required
```bash
sudo apt update
sudo apt install php-curl php-gd php-mbstring php-xml php-xmlrpc php-soap php-intl php-zip
```
and then restart apache2 with the following command
```bash
sudo systemctl restart apache2
```

4. Enabling some changes
Type the following in your terminal
```bash
sudo a2enmod rewrite
sudo apache2ctl configtest
sudo systemctl restart apache2
```

5. Downloading WordPress
Paste the following in your terminal
```bash
cd /tmp
curl -O https://wordpress.org/latest.tar.gz
tar xzvf latest.tar.gz
touch /tmp/wordpress/.htaccess
cp /tmp/wordpress/wp-config-sample.php /tmp/wordpress/wp-config.php
mkdir /tmp/wordpress/wp-content/upgrade
sudo cp -a /tmp/wordpress/. /var/www/wordpress
```

6. Configuring WordPress directory
Paste the folowing in your terminal
```bash
sudo chown -R www-data:www-data /var/www/wordpress
sudo find /var/www/wordpress/ -type d -exec chmod 750 {} \;
sudo find /var/www/wordpress/ -type f -exec chmod 640 {} \;

```

7. Set up WordPress configuration file
```bash
curl -s https://api.wordpress.org/secret-key/1.1/salt/
```
Copy the output of the above into the file 
```bash
sudo nano /var/www/wordpress/wp-config.php
```
Change the DB_NAME,  DB_USER, DB_PASSWORD fields in the above file as required

8.  Completing the installation
Go to localhost where the wordpress website is hosted and 

## 3 Ways to Create WebApps using Spring

----------

### Method 1 : Spring MVC
One main advantage of this method is that we get to isntall our own tomcat (servlet container). So we can easily acccess the logs and debug anything that is required.

To create a spring MVC web app use this link : https://medium.com/@yuntianhe/create-a-web-project-with-maven-spring-mvc-b859503f74d7

-----------

### Method 2 : Spring Boot(Spring MVC + Spring Configuration + Embedded Tomcat server)
One main advantage of this method is that the jar file that shall be created for deployment of the webapp shall also have an embedded tomcat serevr. This would mean that any server which can execute java can act as a server for this WebApp. 

Also this method can be used to create production ready web app.

To create a spring boot web app do the following 

1. Go to Spring Initializr (https://start.spring.io/)

2. Add dependency spring-web (you can add other dependencies as required but this one is bare minimum for creating a Spring Boot web app)

3. Fill in the rest of the details on the page(like group, artifact, name, description, java version, maven or gradle etc)

4. Hit the "Generate" button on that page once all details are filled. This will download a basic web app that can be run using spring mvc and embedded tomcat server inside the jar packaging.

5. Open IntelliJ and start coding.

--------

### Method 3 : Spring CLI
This is a very basic method, just for a hello world program, this should not be used for creating production ready web app.

1. Create a file named "hello.java" with the following content

	```java
	@RestController
	class WebApplication {
    	@RequestMapping("/")
    	public String home() {
        	return "Hello World!"
    	}
	}
	```


2. Then type the following onto your terminal.
```bash
spring run hello.groovy -- --server.port=9000	
```

3. More info regarding the above can be found here : https://docs.spring.io/spring-boot/docs/current/reference/html/spring-boot-cli.html#cli-using-the-cli


## Tomcat Server
### Installing tomcat server
Type the following commands in your terminal

```bash
sudo apt-get install tomcat9
sudo apt-get install tomcat9-admin tomcat9-docs
tomcat9-exmples
```

To know about the CATALINA_BASE and CATALINA_HOME variable and more tomcat related varialbes and some logs , type the following command in the terminal

```console
journalctl -u tomcat9
```

To view the catalina.out log file of the tomcat server, type the following command on your terminal
```bash
journalctl -u tomcat9 -f
```

## Spring MVC(standalone, i.e. Without Spring Boot)
TO use freemarker for renering dynamic content in a spring mvc application follow the below steps
1. Add the following dependency in your pom.xml file 

## Spring Boot with Freemarker Front end

To add and use freemarker inside a spring boot application follow the below steps

1.Add the following dependency in your pom.xml file

```xml
<dependency>
	<groupId>org.springframework.boot</groupId>
	<artifactId>spring-boot-starter-freemarker</artifactId>
</dependency>
```

2. Then add the following two lines in the "application.properties" file of your spring-boot project

```properties
spring.freemarker.template-loader-path: /WEB-INF/views
spring.freemarker.suffix: .ftl
``` 

3. Also make sure that the directory structure contains the following

```
src---> main---> webapp--->WEB-INF--->views--->*.ftl(all ftl files must be placed here)
```

## Spring Boot Application with JSP front end

---------------------------------------

To create a Spring boot application with JSP as front end, follow the below steps

1. Create a spring application from https://start.spring.io/ . Add the Spring Web, Spring Boot DevTools and Lombok dependency

2. Add the following lines to your spring "application.properties" file
```
spring.mvc.view.prefix: /WEB-INF/jsp/
spring.mvc.view.suffix: .jsp

logging.level.web=debug
logging.level.root=debug
```

3. Create the following directory for the views
```
src---> main---> webapp--->WEB-INF--->jsp--->*.jsp
```
	Note : All the .jsp files must be saved where the \*.jsp file is shown above.

4. Add the following two dependencies to your pom.xml file
```xml
<dependency>
	<groupId>org.springframework.boot</groupId>
	<artifactId>spring-boot-starter-tomcat</artifactId>
	<scope>provided</scope>
</dependency>
<dependency>
	<groupId>org.apache.tomcat.embed</groupId>
	<artifactId>tomcat-embed-jasper</artifactId>
	<scope>provided</scope>
</dependency>
```
	Note: If you dont add the above two dependencies, when you open the home page you will get "Whitelabel Error Page" with 404 Not found error. Inside the server logs you can see that "	WARN :  WEB-INF or META-INF" error 
