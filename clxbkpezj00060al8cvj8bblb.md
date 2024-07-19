---
title: "Deploying Vote-App on Azure Kubernetes Service with DevOps and ArgoCD"
seoTitle: "Deploy Vote-App on AKS with DevOps and ArgoCD"
seoDescription: "Leverage Azure DevOps for Continuous Integration (CI) and ArgoCD for Continuous Deployment (CD) to build a robust pipeline that handles everything from code"
datePublished: Wed Jun 12 2024 08:32:49 GMT+0000 (Coordinated Universal Time)
cuid: clxbkpezj00060al8cvj8bblb
slug: vote-app-deploy
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1721422502055/e023f274-bfb4-4cb5-8c3e-600cb68537e8.png
tags: cicd, azure-pipelines, azure-devops, 2articles1week, argocd, aksazure-kubernetes-services

---

In this blog post, I'll guide you through the step-by-step process of setting up a Continuous Integration and Continuous Deployment (CI/CD) pipeline for microservices using Azure DevOps and ArgoCD. This comprehensive guide will help you automate the building, testing, and deployment of your applications, ensuring a seamless and efficient development workflow.

## Introduction

Modern software development demands rapid iterations and seamless deployments. Continuous Integration and Continuous Deployment (CI/CD) practices enable developers to merge code frequently, automate tests, and deploy applications efficiently. In this blog, we'll leverage Azure DevOps for Continuous Integration (CI) and ArgoCD for Continuous Deployment (CD) to build a robust pipeline that handles everything from code commits to deploying applications in Azure Kubernetes cluster(AKS).

%[https://youtu.be/WNlly0IJFhM?si=imSHw_m1R-hn8NIQ] 

## Architecture

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1721422382355/dfd9b898-fd4f-4ec8-9946-aece9ac1cf6e.png align="center")

## Prerequisites

Before we dive in, make sure you have the following:

* An Azure account
    
* An SSH client (e.g., terminal for Mac/Linux, PuTTY for Windows)
    
* Git
    
* Basic knowledge of Kubernetes
    

## Getting Started with Azure

### 1\. Sign Up for an Azure Account

If you don't already have an Azure account, sign up at [Azure](https://azure.microsoft.com/).

### 2\. Sign In to Azure Portal

Once you have an account, sign in to the [Azure Portal](https://portal.azure.com/).

### 3\. Provision Azure Resources

You can either use Terraform scripts to automate the provisioning of resources or manually create the necessary resources on the Azure portal. For simplicity, we’ll outline the manual steps:

* **Create a Linux VM**: This VM will serve as your agent pool for Azure DevOps.
    
* **Create an Azure Container Registry (ACR)**: This will store your Docker images.
    
* **Create an Azure Kubernetes Service (AKS) Cluster**: This will host your microservices.
    

#### **Detailed Steps for Provisioning Resources**

**Approach. 1: Manual creation**

1. **Create a Linux VM**:
    
    * Go to the Azure Portal, click on "Create a resource," and select "Virtual Machine."
        
    * Follow the wizard to set up the VM.
        
2. **Create an Azure Container Registry (ACR)**:
    
    * Navigate to "Create a resource" and select "Container Registry."
        
    * Fill in the required details and create the registry.
        
3. **Create an Azure Kubernetes Service (AKS) Cluster**:
    
    * Go to "Create a resource" and select "Kubernetes Service."
        
    * Follow the wizard to set up the cluster. Note that if you are using a free tier, you may need to choose a different region to avoid usage quota issues.
        

> Note: Make sure all the resources are in the same resource group it will be easy to delete them.

**Approach. 1: Using IaC \[terraform\] creation**

1. **Install Azure CLI**
    
    You can visit the following blog and install Az CLI it will hardly take 5 mins.
    
    %[https://blogs.vijaysingh.cloud/mastering-azure-cli] 
    
2. **Login to Azure**:
    
    Open your terminal and type the following command
    
    ```bash
    az login
    ```
    
    This will open a new browser window for you to sign in to your Azure account. If the CLI can open your default browser, it will do so and load an Azure sign-in page. Otherwise, you need to open a browser page and follow the instructions on the command line to enter an authorization code.
    
3. **Set your subscription** (optional):
    
    If you have multiple Azure subscriptions, and the one you want to use isn’t your default, you can set the subscription you want to use with this command:
    
    ```bash
    az account set --subscription "your-subscription-id"
    ```
    
    Replace `"your-subscription-id"` with your actual subscription ID.
    
4. **Install Terraform**:
    
    If you haven’t installed Terraform, you can download it from the official Terraform website. Unzip the package and move the binary to your PATH.
    
    [Terraform Download](https://developer.hashicorp.com/terraform/install#linux)
    
5. **Provision Azure Resources:**
    
    ```bash
    git clone https://github.com/vsingh55/AzureDevOps-CI-CD.git
    cd AzureDevOps-CI-CD/Terraform
    terraform init
    terraform plan 
    terraform apply
    ```
    

## Setting Up Azure DevOps

### 1\. Create an Azure DevOps Project

* Sign in to [Azure DevOps](https://dev.azure.com/login).
    
* Create a new project by clicking on "New Project."
    
* Give your project a name and description and select the visibility (private or public).
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1718110143168/d91b48ec-d118-45f4-ac84-2a9a3953be1e.png align="center")
    

### 2\. Export the Repository to Azure

* Export the voting-app repository to your azure devops portal because we are going to use microservices of voting-app.
    
* [https://github.com/dockersamples/example-voting-app.git](https://github.com/dockersamples/example-voting-app.git)
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1718110395986/0a2416b9-8f4d-4813-b03c-f45989942a06.png align="center")
    

### 3\. Obtain Personal Access Tokens (PAT)

You'll need two personal access tokens: one for the Azure agent and one for ArgoCD. Follow these steps to generate a PAT:

* Go to your profile in Azure DevOps.
    
* Navigate to "Personal Access Tokens (PAT)."
    
* Generate new tokens with the required scopes.
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1718110554080/274858b9-7c50-4a03-a622-87aabc144759.png align="left")
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1718133236208/f7edd608-fea5-4a53-a2ee-cc496b21332b.png align="center")
    

## Configuring the CI Pipeline

### 1\. Set Up the Agent Pool

#### Adding VM to Agent Pool

1. **Add the Created VM to the Agent Pool**:
    
    * In Azure DevOps, go to "Organization settings" and select "Agent pools."
        
    * Create a new agent pool and register your Linux VM.
        
        ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1718133043338/5fcf01e8-252b-4d59-bbb4-8346370660f0.png align="center")
        
        ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1718133404115/ac17d56f-fceb-4dbb-be9b-7494d051a1ef.png align="center")
        
2. **Install the Azure DevOps Agent**: SSH into your Linux VM and run the following commands:
    
    ```sh
    wget https://vstsagentpackage.azureedge.net/agent/3.239.1/vsts-agent-linux-x64-3.239.1.tar.gz
    sudo apt update
    sudo apt install docker.io  
    mkdir myagent && cd myagent
    tar zxvf vsts-agent-linux-x64-3.239.1.tar.gz
    ./config.sh
    ```
    
    During configuration, provide your Azure DevOps server URL ([`https://dev.azure.com/{your-organization}`](https://dev.azure.com/%7Byour-organization%7D)) and the personal access token.
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1718133618037/70a2152c-ccac-4683-8f0b-6080053f0dc4.png align="center")
    
3. **Start the Agent**:
    
    ```sh
    ./run.sh
    ```
    
    Ensure the agent is running and listed as online in the Azure DevOps portal.
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1718133470434/791d1fb5-e9cf-4cc7-8e23-6b3872965585.png align="center")
    

### 2\. Configure Pipelines

#### Creating and Configuring Pipelines

1. **Create a New Pipeline**:
    
    * Go to the Pipelines section in Azure DevOps.
        
    * Create a new pipeline and select "Azure Repos Git" as the source.
        
        ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1718133787496/462ce424-4c7b-4752-b155-565b0b440326.png align="center")
        
2. **Add YAML Configuration**: Use the provided YAML files for each microservice. These files define the steps to build and push Docker images to the ACR. For example, the `voting-service.yml` file might look like this:
    
    ```yaml
    # Docker
    # Build and push an image to Azure Container Registry
    # https://docs.microsoft.com/azure/devops/pipelines/languages/docker
    
    trigger:
      paths:
        include: 
          - vote/*
    
    resources:
    - repo: self
    
    variables:
      # Container registry service connection established during pipeline creation
      dockerRegistryServiceConnection: 'this Will be automatically generated'
      imageRepository: 'voteapp'
      containerRegistry: 'vijayazurecicd.azurecr.io'
      dockerfilePath: '$(Build.SourcesDirectory)/vote/Dockerfile'
      tag: '$(Build.BuildId)'
    
    pool:
     name: 'azureagent'
    
    stages:
    - stage: Build
      displayName: Build stage
      jobs:
      - job: Build
        displayName: Build
        steps:
        - task: Docker@2
          displayName: Build an image to Azure container registry
          inputs:
            containerRegistry: '$(dockerRegistryServiceConnection)'
            repository: '$(imageRepository)'
            command: 'build'
            Dockerfile: 'vote/Dockerfile'
            tags: '$(tag)'
    
    - stage: Push
      displayName: Push stage
      jobs:
      - job: Push
        displayName: Push
        steps:
        - task: Docker@2
          displayName: Push an image to Azure container registry
          inputs:
            containerRegistry: '$(dockerRegistryServiceConnection)'
            repository: '$(imageRepository)'
            command: 'push'
            tags: '$(tag)'
    ```
    
3. **Run the Pipeline**: Trigger the pipeline to ensure everything is set up correctly. The pipeline should build the Docker image and push it to the ACR.
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1718134194294/ac698541-6325-4f46-aadd-53e5e7521507.png align="center")
    

<div data-node-type="callout">
<div data-node-type="callout-emoji">💡</div>
<div data-node-type="callout-text">For all the services please visit Pipelines folder in <a target="_blank" rel="noopener noreferrer nofollow" href="https://github.com/vsingh55/AzureDevOps-CI-CD.git" style="pointer-events: none">Git Repo</a>.</div>
</div>

### 3\. Continuous Integration with Azure DevOps

Azure DevOps will handle the building and pushing of Docker images to the Azure Container Registry. Every time a change is pushed to the repository, the pipeline will automatically run, ensuring that the latest version of the code is built and ready for deployment.

## Setting up Continuous Delivery:

### Azure Kubernetes Services (AKS)

If you have provisioned resources using terraform then skip the step and run the cmd to connect from terminal. It is for those who wants to do it manually.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1718135062432/41f97d8e-cfa8-4019-8fd1-bc85c340f5f0.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1718135095715/3d891ab2-13ac-41a6-acfa-56ed8d4d4407.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1718135114486/17b8a04c-df0f-460f-af0d-1ca75c634eb9.png align="center")

Run cmd on terminal

```bash
az aks get-credentials --resource-group <resource-group-name> --name <K8's cluster name> --overwrite-existing
```

## Deploying with ArgoCD

### 1\. Install ArgoCD

#### Installing ArgoCD on Kubernetes Cluster

1. **Create a Namespace**:
    
    ```sh
    kubectl create namespace argocd
    ```
    
2. **Install ArgoCD**:
    
    ```sh
    kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
    kubectl get pods -n argocd
    ```
    
    Ensure all ArgoCD pods are running.
    
3. **Access ArgoCD**:
    
    ```sh
    kubectl get svc -n argocd
    kubectl edit svc argocd-server -n argocd
    ```
    
    Change the `type` from `ClusterIP` to `NodePort` to expose the ArgoCD server.
    

### 2\. Configure ArgoCD

#### Configuring ArgoCD for Continuous Deployment

1. **Retrieve Initial Admin Password**:
    
    ```sh
    kubectl get secrets -n argocd
    kubectl edit secret argocd-initial-admin-secret -n argocd
    ```
    
    Within the file find password and copy it.
    
    ```sh
    echo <password> | base64 --decode
    ```
    
    This will give you the password for the ArgoCD admin user.
    
2. **Access ArgoCD UI**: In your browser, navigate to `http://<node-external_ip>:<nodeport>`. Use `admin` as the username and the decoded password to log in.
    
3. **Allow inbound rule for ArgoCD:** Add inbound rule for argoCD nodeport in AKS networking settings.
    
4. **Connect to Azure Git Repo**:
    
    * In the ArgoCD UI, go to Settings and connect your Azure Git repository.
        
    * Use the repository URL format:
        
        ```sh
        https://<personal_access_token>@dev.azure.com/<organization_name>/<project_name>/_git/<project_name>
        ```
        
        ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1718176946014/a92ec5ac-bf7a-4599-99da-d2894ca2e62c.png align="center")
        
5. **Create an Application in ArgoCD**:
    
    * In ArgoCD, create a new application.
        
    * Fill in the details such as application name, project, sync policy, repository URL, and path.
        
        ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1718177003576/9a3d6331-bf16-4391-a726-f9c8d8899e45.png align="center")
        
6. **Automate Image Updates**:
    
    * Write a [script](https://github.com/vsingh55/AzureDevOps-CI-CD/tree/ced362db6db3ac30c5e29e560f18a2c9522ff472/scripts) to update Kubernetes manifests with the new image name from the ACR.
        
    * Create a folder in your Azure repo for the scripts and integrate them into your pipelines lets call it update stage.
        
7. **Enable AKS to Pull Images from ACR**:
    
    ```sh
    kubectl create secret docker-registry <secret-name> \
      --namespace <namespace> \
      --docker-server=<container-registry-name>.azurecr.io \
      --docker-username=<service-principal-ID> \
      --docker-password=<service-principal-password>
    ```
    
    Get the service principal ID and password from the Azure portal:
    
    * Go to the Azure portal, navigate to your ACR, and enable admin access.
        
    * Copy the service principal ID and password.
        

### Verifying the Deployment

1. **Verify ArgoCD Sync**:
    
    * Make changes to the application code and push to the repository.
        
    * ArgoCD will detect the changes and sync the application automatically.
        
        ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1718364249179/6c759c97-856a-489f-93d3-baa648e78be7.png align="center")
        
2. **Access Deployed Applications**:
    
    * Get the external IP and node ports for your applications:
        
        ```bash
        kubectl get nodes -o wide //To know node external ip 
        kubectl get svc -n <namespace>   //
        ```
        
    * Access the applications using the external IP and ports.
        
    * Add Inbound rule for htttp port.
        
        ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1718177555911/94184fa4-b0e5-4d44-a073-c365acc26add.png align="center")
        
        ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1718177567833/71e348d6-91ab-49c8-bb86-ee4b731d6630.png align="center")
        
    * Here are some screenshots before and after the implementing CI/CD
        
        ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1718364388601/8e5bb8c9-3684-4cbf-b13c-b56e6cb7d485.png align="center")
        
        ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1718364402213/35129aeb-1d8e-47ce-8867-061692165d59.png align="center")
        
        ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1718364419977/ee7b170c-82ec-4361-8067-c9be4fdce451.png align="center")
        
        ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1718364435327/29e53101-0e26-41c1-a596-b27aef3f33b4.png align="center")
        

## Conclusion

Congratulations! You've successfully set up a CI/CD pipeline using Azure DevOps, AKS and ArgoCD. This pipeline automates the building, testing, and deployment of your microservices, ensuring a seamless and efficient workflow. By leveraging these powerful tools, you can focus on writing code and delivering features while the pipeline takes care of the rest.

### Acknowledgements

Thanks to [Azure](https://portal.azure.com/#home), [Azure DevOps](https://azure.microsoft.com/en-us/services/devops/) , [ArgoCD](https://argoproj.github.io/argo-cd/) for providing the platform and tools to build this CI/CD pipeline.

<div data-node-type="callout">
<div data-node-type="callout-emoji">💡</div>
<div data-node-type="callout-text">Feel free to comment if you have any questions or suggestions!</div>
</div>