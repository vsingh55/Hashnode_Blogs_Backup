---
title: "5.11 Managing and Configuring Containers with Podman"
datePublished: Sun Sep 24 2023 18:30:00 GMT+0000 (Coordinated Universal Time)
cuid: clsp2rv5p00000ajz2ipa6m74
slug: 511-managing-and-configuring-containers-with-podman
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1708113246169/05aa79ba-15ad-4b29-b417-7a68bdd2b180.png
tags: linux, cloud-computing, devops, 90daysofdevops, shubhamlondhe, trainwithshubham

---

# **Introduction:**

Podman, Skopeo, and Buildah have emerged as lightweight alternatives to Docker, offering improved security, lower resource requirements, and native integration with systemd. This guide provides an overview of Podman and demonstrates how to manage and configure containers using Podman commands.

**Installing Podman:** To install Podman, follow these steps:

```bash
# Install Podman
sudo dnf install podman

# Configure container registries
sudo vi /etc/containers/registries.conf

# Disable Docker CLI emulation message
sudo touch /etc/containers/nodocker
```

**Podman Commands Overview:** Podman offers a rich set of commands for managing containers. Here is an overview of commonly used Podman commands:

| Command | Description |
| --- | --- |
| attach | Attach to a running container |
| build | Build an image using Containerfile instructions |
| commit | Create a new image from a changed container |
| create | Create, but do not start, a container |
| diff | Inspect changes on a container's filesystem |
| exec | Run a process in a running container |
| export | Export a container's filesystem contents as a tar archive |
| help (or h) | Show a list of commands or help for one command |
| history | Show history of a specified image |
| images | List images in local storage |
| import | Import a tarball to create a filesystem image |
| info | Display system information |
| inspect | Display the configuration of a container or image |
| kill | Send a specific signal to one or more running containers |
| load | Load an image from an archive |
| login | Login to a container registry |
| logout | Logout of a container registry |
| logs | Fetch the logs of a container |
| mount | Mount a working container’s root filesystem |
| pause | Pauses all the processes in one or more containers |
| ps | List containers |
| port | List port mappings or a specific mapping for the container |
| pull | Pull an image from a registry |
| push | Push an image to a specified destination |
| restart | Restart one or more containers |
| rm | Remove one or more containers from the host |
| rmi | Removes one or more images from local storage |
| run | Run a command in a new container |
| save | Save an image to an archive |
| search | Search registry for an image |
| start | Start one or more containers |
| stats | Display resource usage statistics for one or more containers |
| stop | Stop one or more containers |
| tag | Add an additional name to a local image |
| top | Display the running processes of a container |
| umount (or unmount) | Unmount a working container’s root filesystem |
| unpause | Unpause the processes in one or more containers |
| version | Display Podman version information |
| wait | Block on one or more containers |

**Understanding Docker Commands:** `docker run` vs `docker start`

Docker is a widely used platform for developing, shipping, and running applications inside containers. Two essential commands in Docker are `docker run` and `docker start`. While both commands are used to manage containers, they serve different purposes. This guide provides an overview of these commands and demonstrates their usage.

**Installing Docker:** To install Docker, follow these steps:

```bash
# Install Docker
sudo dnf install docker

# Configure container registries
sudo vi /etc/containers/registries.conf

# Disable Docker CLI emulation message
sudo touch /etc/containers/nodocker
```

**Using Docker Commands:**

1. **Search and Pull Images:**
    
    * Search for an image:
        
        ```bash
        docker search nginx
        ```
        
    * Pull an image from the registry:
        
        ```bash
        docker pull docker.io/library/nginx
        ```
        
2. **Viewing and Managing Images:**
    
    * View images in the local storage:
        
        ```bash
        docker images
        ```
        
    * Delete an image:
        
        ```bash
        docker rmi nginx
        ```
        
3. **Creating and Managing Containers:**
    
    * Create and start a new container:
        
        ```bash
        docker run -d nginx
        ```
        
    * List running containers:
        
        ```bash
        docker ps
        ```
        
        or
        
        ```bash
        docker container list
        ```
        
    * Stop a container:
        
        ```bash
        docker stop nginx
        ```
        
    * View all containers (including stopped ones):
        
        ```bash
        docker ps --all
        ```
        
    * Delete a container:
        
        ```bash
        docker rm --force nginx
        ```
        
4. **Connecting to Containers:**
    
    * Connect to a container and give it a specific name:
        
        ```bash
        docker run -d -p 8080:80 --name mywebserver nginx
        ```
        

## **Conclusion:**

Podman, Skopeo, and Buildah offer a lightweight and secure alternative to Docker for managing containers. With features such as rootless mode, native systemd integration, and reduced resource requirements, Podman is well-suited for containerized environments. By mastering Podman commands, users can efficiently build, manage, and deploy containers while benefiting from enhanced security and flexibility.

In summary, `docker run` is used to create and start a new container based on an image, while `docker start` is used to start a pre-existing container that has been stopped. By understanding the differences between these commands and their respective use cases, Docker users can effectively manage containers and streamline their development and deployment processes.