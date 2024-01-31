---
title: "Project: Streamlining File Uploads to Azure Blob Storage with LTC-CloudUploader_CLI"
datePublished: Sun Dec 31 2023 08:03:24 GMT+0000 (Coordinated Universal Time)
cuid: clqt7fvxv000508lb4tjo25t0
slug: project-streamlining-file-uploads-to-azure-blob-storage-with-ltc-clouduploadercli
cover: https://cdn.hashnode.com/res/hashnode/image/stock/unsplash/0qvBNep1Y04/upload/49e549a281c8ea1a94a7e1c575a475eb.jpeg
tags: cloud, azure, bash, cloud-computing, scripting

---

## **Introduction**

In today's fast-paced digital world, efficient file management is crucial for seamless operations. If you're working with Azure Blob Storage and looking for a hassle-free way to upload files from your local system, look no further. In this blog post, we'll introduce you to the LTC-CloudUploader\_CLI, a Bash script designed to simplify and expedite the process of uploading files to Azure Blob Storage.

## **Overview**

The LTC-CloudUploader\_CLI is a versatile Bash script that leverages the Azure Storage REST API, offering a straightforward and effective solution for uploading files. Whether you're dealing with large datasets or just a few critical files, this script streamlines the upload process, making it accessible for both beginners and experienced users.

## **Prerequisites**

Before diving into the LTC-CloudUploader\_CLI, make sure you have the following prerequisites in place:

1. **Azure Account:** Sign up for an Azure account or use an existing one.
    
2. **Azure Storage Account:** Create a storage account via the Azure portal.
    
3. **Azure Storage Explorer (Optional):** Utilize the Azure Storage Explorer for enhanced management and exploration of your Azure Storage resources.
    

## **Setup**

### **Azure Storage Account Setup**

1. Navigate to the Azure portal.
    
2. Create a new Storage Account or use an existing one.
    

### **Configure Azure Storage Account Credentials**

When running the script, you'll be prompted for the following credentials:

1. Azure Storage Account Name
    
2. Azure Storage Account Key
    
3. Target Container Name
    

## **How To Use**

Follow these simple steps to get started with LTC-CloudUploader\_CLI:

### **1\. Clone the Repository**

```bash
sudo dnf install -y git
git clone https://github.com/krvsc/LTC-CloudUploader_CLI.git
```

### **2\. Run the Command**

```bash
az login
```

### **3\. Run the Script**

Make the script executable:

```bash
chmod +x CloudUploader.sh
```

### **4\. Run the Script with File Path**

Specify the local file path and the desired Azure Blob Storage destination path:

```bash
bash CloudUploader.sh /path/to/local/file.*
```

## **Example**

As an illustration, let's upload a file named `sample.txt` from the local system to the Azure Blob Storage container:

```bash
bash CloudUploader.sh ~/Documents/sample.txt
```

This command will efficiently upload the specified file, streamlining the process and enhancing your overall file management experience.

## **Conclusion**

In conclusion, LTC-CloudUploader\_CLI provides a powerful yet user-friendly solution for uploading files to Azure Blob Storage. By following the simple setup and usage steps outlined in this blog, you can enhance your file management workflow and ensure a seamless interaction with Azure Storage. Whether you're a seasoned developer or just getting started, this Bash script is a valuable addition to your toolkit. Try it out and optimize your file upload process today!  

%[https://github.com/krvsc/LTC-CloudUploader_CLI.git] 

<div data-node-type="callout">
<div data-node-type="callout-emoji">💡</div>
<div data-node-type="callout-text">Feel free to contribute to this project by opening issues or submitting pull requests.</div>
</div>

> I would greatly appreciate your contribution to enhance this script for multi-cloud and multifile uploading.