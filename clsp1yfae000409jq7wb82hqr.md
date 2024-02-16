---
title: "Enhancing Security with Squid Proxy Server: Restricting Access"
datePublished: Fri Sep 15 2023 18:30:00 GMT+0000 (Coordinated Universal Time)
cuid: clsp1yfae000409jq7wb82hqr
slug: enhancing-security-with-squid-proxy-server-restricting-access
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1708112154170/fc94e186-c31f-462a-9037-d0bfd038d9d0.png
tags: linux, devops, linux-for-beginners, 90daysofdevops, shubhamlondhe, trainwithshubham, lfcs

---

# **Introduction:**

Squid is a versatile proxy server that enhances network performance by caching content and optimizing web page delivery. However, to ensure security and control access to sensitive resources, it's essential to configure access restrictions effectively. This guide outlines the process of restricting access to the Squid HTTP proxy server, focusing on enforcing access control rules to enhance security.

**Installing and Configuring Squid:** Before configuring access restrictions, ensure Squid is installed and running on your system. Follow these steps to install and configure Squid:

1. **Installation:** Use your package manager (`dnf` in this case) to install Squid.
    
    ```bash
    sudo dnf install squid
    ```
    
2. **Starting and Enabling Squid:** Start the Squid service and enable it to start automatically at boot time.
    
    ```bash
    sudo systemctl start squid
    sudo systemctl enable squid
    ```
    
3. **Firewall Configuration:** Allow access to the Squid service through the firewall.
    
    ```bash
    sudo firewall-cmd --add-service=squid
    sudo firewall-cmd --add-service=squid --permanent
    ```
    

**Access Control Configuration:** Access control in Squid is managed through ACLs (Access Control Lists) defined in the `squid.conf` configuration file. Follow these steps to restrict access to specific domains:

1. **Editing** `squid.conf`: Open the Squid configuration file for editing.
    
    ```bash
    sudo vi /etc/squid/squid.conf
    ```
    
2. **Defining ACLs:** Define ACLs to specify the access control rules. In this example, we'll restrict access to the YouTube domain.
    
    ```bash
    acl youtube dstdomain .youtube.com
    ```
    
3. **Denying Access:** Use `http_access deny` to deny access to the defined ACLs.
    
    ```bash
    http_access deny youtube
    ```
    
    Alternatively, you can use `http_access allow` to explicitly allow access to specific ACLs and deny access to all others.
    
    ```bash
    http_access allow localnet !youtube
    ```
    
4. **Applying Changes:** After configuring access control rules, reload the Squid service to apply the changes.
    
    ```bash
    sudo systemctl reload squid.service
    ```
    

```bash
sudo dnf install squid
sudo systemctl start squid
sudo systemctl enable squid
sudo firewall-cmd --add-service=squid
sudo firewall-cmd --add-service=squid --permanent

#Adding access rules
#By default, the /etc/squid/squid.conf file contains the http_access allow localnet rule 
#that allows using the proxy from all IP ranges specified in localnet ACLs.
sudo vi /etc/squid/squid.conf
#acl:access control list
#localnet : name of access control list


#After we add all the things we need in an access control list,
#we'll also need to specifically allow or deny access to each ACL
#to actually enforce the rules


#To deny access to a particular domain (exampe: Youtube)
#dstdomain: destination source type domain
#To denay youtube and all its subdomains use (.youtube.com) and 
#to deny only Youtube and not its subdomains (youtube.com)
acl youtube dstdomain .youtube.com
http_access deny youtube

#or use this
http_access allow localnet !youtube #(this will trigger both rules together)


#to apply changes
sudo systemctl reload squid.service
```

## **Conclusion:**

By configuring access restrictions in Squid, administrators can effectively control access to web resources and enhance security by preventing unauthorized access to sensitive domains. By following the steps outlined in this guide, organizations can leverage Squid's caching capabilities while maintaining strict control over access policies, thereby ensuring a secure and optimized web browsing experience for users.