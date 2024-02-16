---
title: "5.10 Configuring a Database Server (MariaDB)"
datePublished: Tue Sep 19 2023 18:30:00 GMT+0000 (Coordinated Universal Time)
cuid: clsp2igs5000009jx2fwq2qx7
slug: 510-configuring-a-database-server-mariadb
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1708113043979/654f2db5-3c60-48b0-980f-3c2a9c60fb0c.png
tags: linux, devops, mariadb, linux-for-beginners, 90daysofdevops, shubhamlondhe, trainwithshubham

---

# **Introduction:**

A database server is a critical component of many applications, providing the storage and retrieval of data. MariaDB is a popular open-source database server based on MySQL technology. This guide offers step-by-step instructions for configuring and securing a MariaDB database server.

**Installing and Starting MariaDB:** To install and start MariaDB, follow these steps:

```bash
# Install MariaDB
sudo yum install mariadb-server

# Start the MariaDB service
sudo systemctl start mariadb.service

# Enable the MariaDB service to start at boot
sudo systemctl enable mariadb.service

# Allow MariaDB through the firewall
sudo firewall-cmd --add-service=mysql
sudo firewall-cmd --add-service=mysql --permanent
sudo firewall-cmd --runtime-to-permanent
```

**Securing MariaDB:** Securing MariaDB involves setting a root password and removing potential security vulnerabilities. Use the `mysql_secure_installation` command to initiate this process:

```bash
# Run the secure installation script
sudo mysql_secure_installation
```

This interactive script guides you through various security measures, such as setting a root password, removing anonymous users, and disallowing remote root logins.

**Configuring MariaDB:** Customizing MariaDB settings can be done through configuration files. The main configuration file is typically located at `/etc/my.cnf`, while specific server settings can be adjusted in `/etc/my.cnf.d/mariadb-server.cnf`.

```bash
# Open the main configuration file
sudo vi /etc/my.cnf

# Open the server-specific configuration file
sudo vi /etc/my.cnf.d/mariadb-server.cnf
```

Within these files, you can specify parameters such as the data directory, socket location, error log file, and PID file. Additionally, you can limit MariaDB to listen on specific IP addresses by setting the `bind-address` parameter.

## **Conclusion:**

Configuring a MariaDB database server involves installing the software, securing it against potential threats, and customizing its settings to suit your requirements. By following the steps outlined in this guide, you can set up a reliable and secure database server to support your applications and data storage needs. MariaDB remains a popular choice for both small-scale and enterprise-level database deployments due to its performance, reliability, and open-source nature.