---
title: "5.9 Configuring HTTP Server Log Files and Restricting Access"
datePublished: Sun Sep 17 2023 18:30:00 GMT+0000 (Coordinated Universal Time)
cuid: clsp2c4pc000008jyguxo7ogc
slug: 59-configuring-http-server-log-files-and-restricting-access
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1708112538815/59eda12a-e29d-4b11-9cc2-bf1718b4e2b6.png
tags: linux, cloud-computing, linux-for-beginners, 90daysofdevops, trainwithshubham, lfcs

---

# **Introduction:**

Configuring HTTP server log files and restricting access to specific web pages are essential tasks for managing an Apache HTTP Server effectively. This guide provides step-by-step instructions to configure HTTP server log files and restrict access to specific directories or web pages.

### **Configuring HTTP Server Log Files:**

By default, Apache HTTP Server maintains two log files: the error log and the access log. The following steps detail how to configure these log files in the main configuration file (`httpd.conf`).

1. **Open** `httpd.conf` File: Edit the main configuration file of Apache HTTP Server.
    
    ```bash
    sudo vi /etc/httpd/conf/httpd.conf
    ```
    
2. **Error Log Configuration:** Specify the location of the error log file and control the logging level.
    
    ```apacheconf
    ErrorLog "logs/error_log"
    LogLevel warn
    ```
    
3. **Access Log Configuration:** The access log records details about website visitors, accessed pages, and web browsers used. The default configuration already logs access information.
    

```bash
sudo vi /etc/httpd/conf/httpd.conf
#Location of error log file : logs/error_log
ErrorLog "logs/error_log"

#Control number of messages logged to error log
#Possible values: debug, info, notice, warn, error, crit, alert, emerg.
LogLevel warn #basically says only log events that are warnings, or worse
```

### **Restricting Access to a Web Page:**

You can control access to specific directories or web pages using directives in the main configuration file (`httpd.conf`).

1. **Open** `httpd.conf` File: Navigate to the main configuration file of Apache HTTP Server.
    
    ```bash
    sudo vi /etc/httpd/conf/httpd.conf
    ```
    
2. **Define Document Root and Access Options:** Set the document root and define access options for the directory.
    
    ```apacheconf
    DocumentRoot "/var/www/html/"
    <Directory "/var/www/html/">
        Options Indexes FollowSymLinks
        AllowOverride None
        Require all granted
    </Directory>
    ```
    
3. **Restrict Access to Specific Directory:** Use `<Directory>` directive to restrict access to a specific directory.
    
    ```apacheconf
    <Directory "/var/www/html/admin">
        Require all denied
    </Directory>
    ```
    
4. **Additional Access Restrictions:** You can further restrict access by IP address or allow specific IP addresses only.
    
    ```apacheconf
    <Directory "/var/www/html/admin">
        Require ip 192.168.1.79 203.0.1.113
    </Directory>
    ```
    
5. **Prevent Access to** `.ht*` Files: Prevent web clients from accessing `.htaccess` and `.htpasswd` files.
    
    ```apacheconf
    <Files ".ht*">
        Require all denied
    </Files>
    ```
    

```bash
sudo vi /etc/httpd/conf/httpd.conf
#Directory to serve documents: DocumentRoot
DocumentRoot "/var/www/html/"
#how the default config file restricts or allows access to this location
# Possible values for the options directive are "None", "All", or any combination of:
#Indexes Includes FollowSymLinks SymLinksifOwnerMatch ExeccGI MultiViews
# Note that "Multiviews " must be named *explicitly* --options All
# doesn't give it to you.
Options Indexes FollowSymLinks

# Allow override controls what directives may be placed in .htaccess files.
# It can be "All", "None", or any combination of the keywords : Options FileInfo AuthConfig Limit 
AllowOverride None

#Allow or Disallow acces to a directory
Require all granted
#To Disable access to everyone in Admin Directory
<Directory "/var/www/html/admin">
	Require all denied #everyone is denied access
	#OR
	Require ip 192.168.1.79 203.0.1.113 #Allow access to only these 2 IP's
</Directory>

#The following lines prevent .htaccess and .htpas swd files from being viewed by Web clients.
<Files ".ht*">
		Require all denied
</Files>
```

## **Conclusion:**

Configuring HTTP server log files and restricting access to specific web pages are important tasks in managing an Apache HTTP Server securely and efficiently. By following the steps outlined in this guide, you can ensure proper logging of server events and control access to sensitive areas of your website. Apache HTTP Server remains a reliable and versatile choice for hosting web content on the internet.