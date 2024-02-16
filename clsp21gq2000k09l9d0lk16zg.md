---
title: "5.8 Setting Up and Configuring Apache HTTP Server"
datePublished: Sun Sep 17 2023 18:30:00 GMT+0000 (Coordinated Universal Time)
cuid: clsp21gq2000k09l9d0lk16zg
slug: 58-setting-up-and-configuring-apache-http-server
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1708112318782/d92dc6fc-d185-46d6-aafc-8480e3281c86.png
tags: linux, devops, linux-for-beginners, 90daysofdevops, shubhamlondhe, trainwithshubham

---

# **Introduction:**

Apache HTTP Server, commonly referred to as httpd, is a powerful and widely-used web server that serves web content over the HTTP protocol. This guide provides step-by-step instructions to configure an HTTP server using Apache, including basic installation, configuration, and setting up virtual hosts.

**Installing Apache HTTP Server:** Begin by installing Apache HTTP Server using the package manager (`dnf` in this case).

```bash
sudo dnf install httpd
```

**Configuring Firewall:** Open necessary ports in the firewall to allow HTTP and HTTPS traffic.

```bash
sudo firewall-cmd --add-service=http
sudo firewall-cmd --add-service=https
sudo firewall-cmd --runtime-to-permanent
```

**Starting and Enabling Apache:** Start the Apache service and enable it to start automatically at boot time.

```bash
sudo systemctl start httpd
sudo systemctl enable httpd
```

**Editing** `httpd.conf`: Edit the main Apache configuration file to modify essential settings.

```bash
sudo vi /etc/httpd/conf/httpd.conf
```

1. **Changing Default Port:** Modify the default port on which Apache listens for incoming connections.
    
    ```apacheconf
    Listen 80
    ```
    
2. **Server Configuration:** Configure server-related settings such as `ServerAdmin` and `ServerName`.
    
    ```apacheconf
    ServerAdmin root@localhost
    ServerName www.example.com:80
    ```
    
3. **Setting DocumentRoot:** Specify the directory from which Apache serves web content.
    
    ```apacheconf
    DocumentRoot "/var/www/html"
    ```
    

**Virtual Hosts Configuration:** Apache allows hosting multiple websites on a single server through virtual hosts. Follow these steps to set up virtual hosts:

1. **Creating Document Roots:** Create separate directories for each domain to host.
    
    ```bash
    mkdir /var/www/store/
    mkdir /var/www/blog/
    ```
    
2. **Creating HTML Files:** Create a simple HTML file for each domain.
    
    ```bash
    echo "vHost store.example.com" > /var/www/store/index.html
    echo "vHost blog.example.com" > /var/www/blog/index.html
    ```
    
3. **Configuring Virtual Hosts:** Create a new configuration file for virtual hosts and define virtual host settings.
    
    ```apacheconf
    sudo vi /etc/httpd/conf.d/two-websites.conf
    ```
    
    ```apacheconf
    <VirtualHost *:80>
        DocumentRoot "/var/www/store/"
        ServerName store.example.com
    </VirtualHost>
    
    <VirtualHost *:80>
        DocumentRoot "/var/www/blog/"
        ServerName blog.example.com
    </VirtualHost>
    ```
    
4. **Testing Configuration:** Ensure the syntax of the configuration files is correct.
    
    ```bash
    apachectl configtest
    ```
    

## **Conclusion:**

By following the steps outlined in this guide, you can set up and configure an Apache HTTP server to serve web content efficiently. Additionally, configuring virtual hosts allows hosting multiple websites on a single server, providing flexibility and scalability for web hosting needs. Apache HTTP Server remains a reliable and robust choice for serving web content on the internet.