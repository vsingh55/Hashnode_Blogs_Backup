---
title: "Deploying 3-Tier Architecture on AKS with Terraform, Jenkins"
seoTitle: "3-Tier AKS Architecture: Terraform, Jenkins Guide"
seoDescription: "Deploy a 3-Tier architecture on AKS using Terraform and Jenkins. Follow this step-by-step guide for efficient deployment and CI/CD setup"
datePublished: Sat Aug 03 2024 03:30:26 GMT+0000 (Coordinated Universal Time)
cuid: clzdksukr000x09lf40hy4di8
slug: 3-tier-architecture-deployment-across-multiple-environments
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1722629790042/60d13480-e4a8-448d-921c-790f8eb6dc21.png
ogImage: https://cdn.hashnode.com/res/hashnode/image/upload/v1722629976747/7f038ad6-2bc1-4f1d-a364-d6eeedd24106.png
tags: docker, azure, sonarqube, automation, devops, terraform, jenkins, cicd-cjy1vtdk2005kjjs17n8couc3, 2articles1week, terraform-workspace, trivy, aksazure-kubernetes-services, terraform-module, powertocloud, 3-tier

---

# **Introduction to Architecture**:

YelpCamp is a 3-tier web application specifically designed for campground reviews. It boasts a variety of features such as Campground Listings, User Reviews, Photo Sharing, and User Accounts. The primary aim of YelpCamp is to assist outdoor enthusiasts in discovering the best camping spots and sharing their experiences with a community of like-minded individuals. The comprehensive features of YelpCamp make it an excellent platform for leveraging cloud and DevOps skills.

## **Objectives of the Project**:

* Provisioning Infrastructure using IaC \[**Terraform\]**.
    
* Deployment of 3-Tier App in multiple environments: Local, Dev, Prod.
    
* Containerizing applications with **Docker**.
    
* Conducting static code analysis using **SonarQube** and vulnerability scanning using **Trivy**.
    
* Deploying applications to multiple environments for different purpose like;
    
    * **local** for testing purpose, **Container Deployment** for development & **Azure Kubernetes Service (AKS)** for production.
        
* Setting up robust CI/CD pipelines using **Jenkins**.
    

### Architecture

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1722595229302/3502312e-c5f4-4ec7-8cef-40ed3b3fe756.png align="center")

## About Infrastructure & Deployment Process:

**Project Structure:** Project is organized into distinct directories, each serving a specific purpose:

```bash
╰─$ tree -L 4
.
├── src     // Contains sources code + Dockerfile + Manifests + .ENV
├── JenkinsPipeline-Dev  
├── JenkinsPipeline-Prod
└── Terraform  //terraform modular approach
    ├── modules
    │   ├── aks
    │   │   ├── main.tf
    │   │   ├── output.tf
    │   │   └── variables.tf
    │   ├── bastion
    │   │   ├── main.tf
    │   │   ├── output.tf
    │   │   └── variables.tf
    │   ├── compute
    │   │   ├── main.tf
    │   │   ├── output.tf
    │   │   └── variables.tf
    │   ├── keyvault
    │   │   ├── main.tf
    │   │   ├── output.tf
    │   │   └── variables.tf
    │   ├── network
    │   │   ├── main.tf
    │   │   ├── output.tf
    │   │   └── variables.tf
    │   ├── pip
    │   │   ├── main.tf
    │   │   ├── output.tf
    │   │   └── varibales.tf
    │   ├── resourcegroup
    │   │   ├── main.tf
    │   │   ├── output.tf
    │   │   └── variables.tf
    │   └── ServicePrincipal
    │       ├── main.tf
    │       ├── output.tf
    │       └── variables.tf
    ├── main.tf
    ├── variables.tf
    ├── local.auto.tfvars 
    ├── dev.auto.tfvars
    ├── prod.auto.tfvars
    └── output.tf
├── scripts  // Contains scripts to automate setup
└── README.md
```

* **src/**: Contains source code, Dockerfile, manifests, and environment configuration files.
    
* **JenkinsPipeline-Dev/**: Jenkins pipeline configuration for the development environment.
    
* **JenkinsPipeline-Prod/**: Jenkins pipeline configuration for the production environment.
    
* **Terraform/**: Infrastructure as Code configuration using a modular approach.
    
* **scripts/**: Automation scripts for setup and maintenance.
    
* [**README.md**](http://README.md): Documentation for the project.
    

#### Terraform Modules

I adopted a modular approach to organize Terraform code, making it reusable and easier to manage. The modules directory contains submodules for various components such as resource groups, service principals, networks, compute instances, and Azure Kubernetes Service (AKS).

#### Main Infrastructure Components

1. **Resource Group**: The foundational component where all other resources are grouped together. It is provisioned for each environment (local, dev, prod).
    
2. **Service Principal**: An Azure Active Directory application used for managing access to Azure resources. It is crucial for automating tasks and maintaining security.
    
3. **Key Vault**: Securely stores secrets, keys, and certificates. We store SSH keys and service principal credentials here for secure access.
    
4. **Network**: Defines the virtual network, subnets and NSG rules for isolating and managing resources efficiently.
    
5. **Compute Instances**: Virtual machines (VMs) provisioned with specific configurations for running Jenkins and SonarQube in the development environment. In production, additional VMSS support the application workload and AKS nodes.
    
6. **Azure Kubernetes Service (AKS)**: Manages our containerized applications with features like scaling, updates. It’s used in the production environment for deploying scalable applications.
    

#### Environment-Specific Configuration

* **Local Environment**: One VM to mimic the development setup for testing and debugging.
    
* **Development Environment**: Two VMs - one for Jenkins (CI/CD tool) and another for SonarQube (code quality analysis).
    
* **Production Environment**: Two VMs for application workload and an AKS cluster for managing containerized applications.
    

#### Infrastructure Deployment Workflow

1. **Provision Resource Group**: Each environment starts with a resource group to logically group and manage resources.
    
2. **Create Service Principal**: A service principal is created and granted necessary permissions to manage resources within the resource group.
    
3. **Setup Key Vault**: Securely store sensitive information like client IDs and secrets used by the service principal.
    
4. **Configure Network**: Define virtual networks and subnets to ensure proper isolation and communication between resources.
    
5. **Deploy Compute Instances**: Provision VMs with specific configurations (like size, OS, SSH keys) to run Jenkins, SonarQube, and other application components.
    
6. **Setup AKS**: In the production environment, deploy an AKS cluster to manage containerized applications, providing features like scaling and self-healing.
    

#### Terraform Workspaces

Terraform workspaces allow us to manage multiple environments within the same configuration. By using workspaces, we can separate the state files and configurations for local, development, and production environments.

* **Local Workspace**: For testing and debugging on a single VM.
    
* **Development Workspace**: For deploying Jenkins and SonarQube on separate VMs.
    
* **Production Workspace**: For deploying the application workload and AKS cluster.
    

#### Executing the Code

1. **Install Terraform**: Ensure Terraform is installed on your machine. You can download it from [the official website](https://developer.hashicorp.com/terraform/tutorials/aws-get-started/install-cli).
    
2. **Clone the repository:**
    
    %[https://github.com/vsingh55/3-tier-Architecture-Deployment-across-Multiple-Environments] 
    
    ```bash
    git clone https://github.com/vsingh55/3-tier-Architecture-Deployment-across-Multiple-Environments.git
    ```
    
3. **Initialize Terraform**: Authenticate and Navigate to the Terraform directory and before initializing the configuration fill appropriate value in .auto.tfvars files.
    
    ```bash
    az login
    cd Terraform
    terraform init
    ```
    
4. **Select the Workspace**: Create and Select the appropriate workspace for your environment (local, dev, prod).
    
    ```bash
    terraform workspace select local   # For local environment
    terraform workspace select dev     # For development environment
    terraform workspace select prod    # For production environment
    ```
    
    ```bash
    # Some usefull commands to use terraform workspace
    $ terraform workspace          
    Usage: terraform [global options] workspace
    
      new, list, show, select and delete Terraform workspaces.
    
    Subcommands:
        delete    Delete a workspace
        list      List Workspaces
        new       Create a new workspace
        select    Select a workspace
        show      Show the name of the current workspace
    ```
    
5. **Review and Apply Configuration**: Plan and apply the Terraform configuration to provision the infrastructure.
    
    <div data-node-type="callout">
    <div data-node-type="callout-emoji">💡</div>
    <div data-node-type="callout-text">If you are using workspace method then ignore -var-file option.</div>
    </div>
    
    ```bash
    terraform plan -var-file=local.auto.tfvars  # For local environment
    terraform plan -var-file=dev.auto.tfvars    # For development environment
    terraform plan -var-file=prod.auto.tfvars   # For production environment
    
    terraform apply -var-file=local.auto.tfvars  # For local environment
    terraform apply -var-file=dev.auto.tfvars    # For development environment
    terraform apply -var-file=prod.auto.tfvars   # For production environment
    ```
    
    The `plan` command allows you to see the changes that will be made, while the `apply` command provisions the resources.
    

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1721127789817/069d2c14-dd19-49d3-86ee-55f3aa2c4355.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1722612565368/76e17586-0023-498e-9431-0b69bd3557bb.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1722612638776/97421ef3-3b23-4daf-b2af-9027815a1240.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1722612574542/503c6be3-a7ad-415d-bde6-865c107edfdf.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1722612629409/442f257d-7a3f-450c-af3e-a928f91c862f.png align="center")

## CI/CD Pipeline Steps:

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1722612672781/4338fba3-0072-4301-9e3e-3e50384c3403.png align="center")

1. **Install Dependencies**: Ensure all necessary dependencies are installed.
    
2. **Run Tests**: Execute unit and integration tests.
    
3. **Code Analysis**: Used tools like SonarQube for static code analysis.
    
4. **File System Scan**: Performed security scans using tools like Trivy.
    
5. **Build Docker Image**: Created a Docker image of the application.
    
6. **Scan Docker Image**: Ensuring the image is secure.
    
7. **Push Image to Repository**: Pushed the Docker image to a Docker Hub registry.
    
8. **Deploy Application**: Deploy the Docker image to the target environment ( AKS, container).
    

## Step-by-Step Deployment Process:

Before begin the deployment process, it's essential to understand the components and tools you will be using. One critical decision is the type of database (DB) to deploy. You can choose between a container-based DB or a cloud-based DB. Here are the considerations:

* **Container-based DB**: Requires manual setup of deployments, services, and volume management. This option offers more control but requires more effort for configuration and maintenance.
    
* **Cloud-based DB**: Provides flexibility and ease of management but may be more expensive depending on usage. For this project, we will use a cloud-based DB (**MongoDB**) for its scalability and convenience.
    

In addition to the database, you'll need to configure several environment variables:

* [**Cloudinary**](https://cloudinary.com/users/login): For storing images in the database.
    
* [Mapbox Token](https://account.mapbox.com/auth/signup/?route-to=%22https%3A%2F%2Faccount.mapbox.com%2Faccess-tokens%2F%22): For linking campground locations on the map.
    

**Prerequisite**:

* Cloud Provider Account \[Azure\]
    
* Knowledge of tools like Trivy, SonarQube, Docker, GIt, Jenkins used in project & IaC (Terraform)
    
* <mark>Getting Cloudinary variables:</mark>
    
    * **Sign Up for Cloudinary Account:**
        
        * Go to the Cloudinary [website](https://www.cloudinary.com/) and sign up for a new account.
            
    * **AccessDashboard:**
        
        * Once you have signed up and logged in to your Cloudinary account, you will be taken to the dashboard.
            
    * **Find Your Credentials:**
        
        * In the Cloudinary dashboard, navigate to the "Dashboard Settings" section.
            
        * You will find your CLOUDINARY\_CLOUD\_NAME, CLOUDINARY\_KEY, and CLOUDINARY\_SECRET there. These are unique for your account and should be kept secure.
            
    * **Use Credentials in Your Application:**
        
        * Now that you have obtained these credentials, you will use later them in your application to connect to Cloudinary for image and video management.
            
    * <mark>Getting Mapbox token:</mark>
        
        * Signup and login to your [Mapbox account.](https://account.mapbox.com/auth/signup/?route-to=%22https%3A%2F%2Faccount.mapbox.com%2Faccess-tokens%2F%22)
            
        * Navigate to create [acces token](https://account.mapbox.com/access-tokens/create).
            
        * Fill out token name and check all **Secret scopes** as you are practising it not using for corporate project.
            
        * Click create and copy and save it.
            
    * <mark>Getting DB_URL:</mark>
        
        * **Sign up for MongoDB Atlas:**
            
            * Go to the [MongoDB Atlas](https://www.mongodb.com/cloud/atlas/register?utm_source=google&utm_campaign=search_gs_pl_evergreen_atlas_general_prosp-brand_gic-null_apac-in_ps-all_desktop_eng_lead&utm_term=google%20mongodb&utm_medium=cpc_paid_search&utm_ad=p&utm_ad_campaign_id=6501677905&adgroup=84316982521&cq_cmp=6501677905&gad_source=1&gclid=CjwKCAjw7NmzBhBLEiwAxrHQ-X-CNQ5fHm5qbtD9T67pn_8Rxnma_C8rvfe2AbSABzhWxZJ5yy5TaBoCyWgQAvD_BwE).
                
            * You can sign up using your Google account, or you can fill in the required information to create a new account.
                
        * Fill the details as shown in figure:
            

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1719056909178/fd174c7e-d6af-4084-8ad0-92529600367d.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1719056969214/ca8a0dc6-5081-4493-bf18-974e0adb65b1.png align="center")

> You can choose your own cloud provider but keep in mind choose your nearest region.

Click Create Deployment and follow instructions.

* Now copy Username & Password on notepad and save it.
    
* First click on **Create Database User** then **Choose a connection method.**
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1719058419919/f0e820b8-51b3-446c-b1b5-dbf4a251f71d.png align="center")
    
* Select Driver and choose Node.js (as we are using App written in Node.js).
    
* Copy **DB\_URL** and **save it.**
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1719057513258/37cdb22a-f373-44dc-86ca-81aaae875f72.png align="center")
    

### Local Deployment:

Local environment deployment plays a crucial role in the DevOps process for several reasons:

* Local deployment enables rapid development and testing by allowing developers to quickly build, test, and debug code without remote servers, speeding up the development cycle.
    
* It ensures consistency and reliability by mimicking the production environment, reducing environment-specific bugs.
    
* Additionally, it is resource-efficient and cost-effective, as it eliminates the need for expensive cloud resources, leveraging local machine resources instead.
    

Let's proceed with deployment steps:  
**Step.1:** As we have already discussed about infrastructure provisioning.

**Step.2:** Put the environment variables into .env file present in /src directory.

**Step.3:** SSH to provisioned VM and follow the instruction:

1. run \[ ls /opt/ \] & make sure git cloned
    
2. if git cloned then run:
    
    ```bash
    cd /opt/3-tier-Architecture-Deployment-across-Multiple-Environments/
    ```
    
3. now you need to run following cmd to install npm:
    
    ```bash
    export NVM_DIR="/opt/nodejs/.nvm" && source "$NVM_DIR/nvm.sh"
    ```
    
4. Verify is npm installed by running `npm-v`
    
5. run `cd src`
    
6. run `npm start`
    
7. There should be database connected message on terminal thats it.
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1719102338971/a42dde3f-852f-4b02-85ae-8cd2009f3f0c.png align="center")
    
8. Access Application at [http://VM\_PublicIP:3000/](http://VM_PublicIP:3000/) relpace VM\_PublicIP with actual PIP provisioned on your Azure Portal.
    

Now Register and login to Yelpcamp app and create new campgrounds and verify it on database portal in database section.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1719102384562/45e471a0-116f-4c97-acc6-705eb642fd32.png align="center")

> Congratulations🎉 you have deployed app in local enironment.🙌

---

### Deploying Dev Environment:

A development environment (Dev Env) is a crucial setup for software development teams. It provides a controlled **space where developers can build, test, and refine applications before they are deployed to production**. Setting up a reliable Dev Env ensures that developers can work efficiently and collaborate effectively.

**Overview of Deployment Process**

Here’s what we will cover:

1. **Infrastructure Provisioning:** We have already discussed the way to provision infra in Infrastructure section.
    
2. **Access & Configuring Jenkins and SonarQube**: Setting up Jenkins and SonarQube portals for continuous integration and code quality analysis is a tedious task, so I have pinned down all the detailed steps in a separate blog as mentioned below:
    
    %[https://blogs.vijaysingh.cloud/unlocking-jenkins] 
    
3. **Creating a Jenkins Pipeline**: Writing and explaining a Jenkins Pipeline script step by step.
    
4. **Running the Pipeline**: Executing the pipeline and accessing the deployed application.
    
5. **Troubleshooting**: Tips for debugging common issues during the deployment process.
    

### Step-by-Step Detailed Deployment Process

**3\. Creating a Jenkins Pipeline**

Jenkins Pipeline script for deploying a Node.js application:

**Create a New Job**: Go to Jenkins Dashboard -&gt; New Item. Create a pipeline job named `Deploy-Trio-Dev`. I have decided to keep only two pipelines as history.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1719912857845/953c77fc-e565-401f-9609-30fd6bab274d.png align="center")

**Configure the Pipeline**:

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1719913057326/8c614d39-f506-4180-a942-1140efef75bf.png align="center")

<mark>Pipeline:</mark>

```bash
pipeline {
    agent any

    tools {
        nodejs 'node22'
    }

    environment {
        SCANNER_HOME = tool 'sonar-scanner'
    }

    stages {
        stage('Git Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/vsingh55/3-tier-Architecture-Deployment-across-Multiple-Environments.git'
            }
        }
        
        stage('Install Package Dependencies') {
            steps {
                dir('src') {                 
                    sh 'npm install'
                }
            }
        }
        
        stage('Unit Test') {
            steps {
                dir('src') {
                    sh 'npm test'
                }
            }
        }
        
        stage('Trivy FS Scan') {
            steps {
                dir('src') {
                    sh 'trivy fs --format table -o fs-report.html .'
                }
            }
        }
        
        stage('SonarQube') {
            steps {
                dir('src') {
                    withSonarQubeEnv("sonar") {
                        sh "\$SCANNER_HOME/bin/sonar-scanner -Dsonar.projectKey=Campground -Dsonar.projectName=Campground"
                    }
                }
            }
        }
        
        stage('Docker Build & Tag') {
            steps {
                script {
                    dir('src') {
                        withDockerRegistry(credentialsId: 'docker-crd', toolName: 'docker') {
                            sh "docker build -t vsingh55/camp:latest ."
                        }
                    }
                } 
            }
        }
        
        stage('Trivy Image Scan') {
            steps {
                sh 'trivy image --format table -o image-report.html vsingh55/camp:latest'
            }
        }
        
        stage('Docker Push Image') {
            steps {
                script {
                    withDockerRegistry(credentialsId: 'docker-crd', toolName: 'docker') {
                        sh "docker push vsingh55/camp:latest"
                    }
                }
            }
        }
        
        stage('Docker Deploy To DEV Env') {
            steps {
                script {
                    withDockerRegistry(credentialsId: 'docker-crd', toolName: 'docker') {
                        sh "docker run -d -p 3000:3000 vsingh55/camp:latest"
                    }
                }
            }
        }
    }
}
```

<div data-node-type="callout">
<div data-node-type="callout-emoji">💡</div>
<div data-node-type="callout-text">Since I have stored all the files src folder as shown in project structure, dir('src') { included in pipeline.</div>
</div>

* You can use pipeline syntax option provided in jenkins during writing pipeline. Example is shown below to generate script for docker.
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1719914278999/0f88c130-795a-4e8c-8447-620ddda656a5.png align="center")
    

**4\. Running the Pipeline**

* **Triggering the Pipeline**: Commit changes to your application's Git repository to trigger the Jenkins Pipeline.
    
* **Monitoring Pipeline Execution**: Watch the pipeline stages execute in Jenkins dashboard.
    
* **Accessing Deployed Application**: Once deployment succeeds, access your application via `http://<Jenkins-VM-Public-IP>:3000`
    

**5\. Troubleshooting Tips**

* **Pipeline Failures**: Check Jenkins console output for detailed error messages.
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1722618324521/df6b5aad-60bb-457b-a559-b8d29e79eb2e.png align="center")
    
* **Infrastructure Issues**: Verify resource configurations in Azure Portal or Terraform scripts.
    
* **Permission Issues**: Ensure you’ve provided adds the user “jenkins” to the “docker” group.
    
    ```bash
    sudo usermod -aG docker jenkins
    ```
    

<mark>Scenario</mark>: your pipeline is running successfully, and the Docker image is also operational, but you're unable to access the application's full features due to misconfigured environment variables. After making corrections and pushing the changes to Git, rerunning the pipeline results in an error because port 3000 is occupied by the previously running image. To resolve this, you must first stop the current image before rerunning the new image on port 3000. To automate this process, I have added two stages in the code: one to check if port 3000 is free before the deployment stage, and another to confirm that the new image is running after deployment.

```bash
stage('Checking & Stop Running Containers on Port 3000') {
    steps {
        script {
            def runningContainers = sh(script: "docker ps -q --filter 'publish=3000'", returnStdout: true).trim()
            if (runningContainers) {
                echo "Stopping running containers on port 3000: ${runningContainers}"
                sh "docker stop ${runningContainers}"
            } else {
                echo "No running containers found on port 3000"
            }
        }
   }
}      

stage('Docker Deploy To DEV Env') {
// already provided in pipeline script
}

stage('Verify Deployment') {
    steps {
        script {
            try {
                def containerId = sh(script: 'docker ps -q --filter ancestor=vsingh55/camp:latest', returnStdout: true).trim()
                if (containerId) {
                echo "Container ID: ${containerId}"
                sh "docker logs ${containerId}"
                } else {
                    error "No running container found for image vsingh55/camp:latest"
                }
            } catch (Exception e) {
                echo "Error during verification: ${e.getMessage()}"
                sh 'docker ps -a'
                error "Verification stage failed"
            }
        }
    }    
}
```

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1719924277257/02a6c30c-5f71-45be-a4e8-4c2869212883.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1719924301434/6d67b713-0bcc-4eb4-9aca-6bc0cc525dbd.png align="center")

Create new campgrounds, sign up, and log in with new users.

<div data-node-type="callout">
<div data-node-type="callout-emoji">💡</div>
<div data-node-type="callout-text">Congratulations🎉🎉🎉 successfully deployed app on Dev Env✌🏻✌🏻</div>
</div>

### Deploying Prod Environment:

In the production environment, the application is deployed on Azure Kubernetes Service (AKS) rather than using container deployment as in the development environment.

Deploying a Docker image stored in Docker Hub to an Azure Kubernetes Service (AKS) cluster using a Jenkins pipeline involves several steps, including setting up the AKS cluster, configuring Jenkins, and creating the Jenkins pipeline. Here are the detailed instructions:

**Step 1: Set up variables:**

Encode and put the values of environment variables using following cmd:

```bash
$ echo 'enter the env variable' | base64 
```

Put the values of environment variables in /src/Manifests/dss.yml file.

```bash
# Put all the values these are generated in local deployment. 
data:
  CLOUDINARY_CLOUD_NAME: 
  CLOUDINARY_KEY: 
  CLOUDINARY_SECRET: 
  MAPBOX_TOKEN: 
  DB_URL: 
  SECRET: 
```

> You can also use --literal flag with kube CLI instead of converting and putting these values individually.

**Step 2: Create an AKS Cluster**:

* Already provisioned infra using Terraform.
    
* **Configure kubectl**:
    
    * Install Azure CLI and kubectl if not already installed.
        
    * Connect to your AKS cluster:
        
        ```sh
        $ az aks get-credentials --resource-group rg-Deploy-Trio-prod --name AKS-cluster-Deploy-Trio-australiaeast-prod --overwrite-existing
        $ kubectl create namespace webapps
        ```
        

**Step 3: Configure Jenkins to Interact with AKS**

Follow the same steps that have been suggested in Dev Deployment process.

**Step 4: Create the Jenkins Pipeline**

1. **Create Jenkins Pipeline Job**:
    
    * Create a new Jenkins pipeline job.
        
    * Use the following pipeline as a reference.
        
2. **Pipeline Script**:
    
    ```bash
    pipeline {
        agent any
    
        tools {
            nodejs 'node21'
        }
    
        environment {
            SCANNER_HOME = tool 'sonar-scanner'
        }
    
        stages {
            stage('Git Checkout') {
                steps {
                    git branch: 'main', url: 'https://github.com/vsingh55/3-tier-Architecture-Deployment-across-Multiple-Environments.git'
                }
            }
    
            stage('Install Package Dependencies') {
                steps {
                    dir('src') {
                        sh 'npm install'
                    }
                }
            }
    
            stage('Unit Test') {
                steps {
                    dir('src') {
                        sh 'npm test'
                    }
                }
            }
    
            stage('Trivy FS Scan') {
                steps {
                    dir('src') {
                        sh 'trivy fs --format table -o fs-report.html .'
                    }
                }
            }
    
            stage('SonarQube') {
                steps {
                    dir('src') {
                        withSonarQubeEnv("sonar") {
                            sh "\$SCANNER_HOME/bin/sonar-scanner -Dsonar.projectKey=Campground -Dsonar.projectName=Campground"
                        }
                    }
                }
            }
    
            stage('Docker Build & Tag') {
                steps {
                    script {
                        dir('src') {
                            withDockerRegistry(credentialsId: 'docker-crd', toolName: 'docker') {
                                sh "docker build -t vsingh55/campprod:latest ."
                            }
                        }
                    } 
                }
            }
    
            stage('Trivy Image Scan') {
                steps {
                    sh 'trivy image --format table -o image-report.html vsingh55/campprod:latest'
                }
            }
    
            stage('Docker Push Image') {
                steps {
                    script {
                        withDockerRegistry(credentialsId: 'docker-crd', toolName: 'docker') {
                            sh "docker push vsingh55/campprod:latest"
                        }
                    }
                }
            }
    
            stage('Deploy to AKS Cluster') {
                steps {
                    dir('src') {
                        withCredentials([file(credentialsId: 'k8-secret', variable: 'KUBECONFIG')]) {
                            sh "kubectl apply -f Manifests/dss.yml -n webapps"
                            sh "kubectl apply -f Manifests/svc.yml -n webapps"
                            sleep 60
                        }
                    }
                }
            }
        }
    }
    ```
    

**Step 4: Run the Jenkins Pipeline**

1. **Trigger the Pipeline**:
    
    * Trigger the pipeline manually or configure it to run on Git commits.
        
    * Monitor the pipeline stages in Jenkins to ensure each step completes successfully.
        
2. **Monitor Deployment**:
    
    * After the deployment stage completes, monitor your AKS cluster to ensure the application is running.
        
    * You can use the following commands to check the status:
        
        ```sh
        kubectl get pods -n webapps
        kubectl get svc -n webapps
        ```
        

By following these steps, you can deploy a Docker image from Docker Hub to an AKS cluster using a Jenkins pipeline.

### Conclusion:

Deploying a 3-tier architecture on Azure Kubernetes Service (AKS) using Terraform and Jenkins provides a robust and scalable solution for managing web applications like YelpCamp. By leveraging Infrastructure as Code (IaC) with Terraform, we ensure consistent and repeatable deployments across multiple environments. The integration of Docker for containerization, SonarQube for static code analysis, and Trivy for vulnerability scanning enhances the security and quality of the application. The CI/CD pipelines set up with Jenkins automate the deployment process, ensuring efficient and reliable updates to the application. This comprehensive approach not only streamlines the development and deployment process but also ensures that the application is secure, scalable, and maintainable.

**References:**

Refer to the following blogs to gain a better understanding of Terraform modules and Jenkins configurations:

* **Terraform Module:**
    

%[https://blogs.vijaysingh.cloud/modular-terraform] 

%[https://blogs.vijaysingh.cloud/automate-aks] 

* **Configuring Jenkins:**
    

%[https://blogs.vijaysingh.cloud/unlocking-jenkins]