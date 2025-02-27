---
title: "Streamlining File Uploads to Azure Blob Storage with LTC-CloudUploader_CLI"
seoTitle: "Simplify Azure Uploads with LTC-CloudUploader_CLI"
seoDescription: "Simplify Azure Blob Storage uploads with LTC-CloudUploader_CLI, an easy Bash script. Enhance file management!"
datePublished: Sun Dec 31 2023 08:03:24 GMT+0000 (Coordinated Universal Time)
cuid: clqt7fvxv000508lb4tjo25t0
slug: ltc-clouduploadercli
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1721418884220/78be714b-2c6f-4ac7-be59-f3de938812c9.png
tags: cloud, azure, bash, cloud-computing, scripting

---

# **Introduction**

In today's fast-paced digital world, efficient file management is crucial for seamless operations. If you're working with Azure Blob Storage and looking for a hassle-free way to upload files from your local system, look no further. In this blog post, we'll introduce you to the LTC-CloudUploader\_CLI, a Bash script designed to simplify and expedite the process of uploading files to Azure Blob Storage.

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
    

## Script explanation:

### 1\. Usage Information

```bash
function display_usage {
    echo "Usage: $0 <file_path> [options]"
    echo "Options:"
    echo "  -c, --container   Target container in the cloud"
    echo "  -h, --help        Display this help message"
}
```

This function `display_usage` is responsible for showing usage information when the user runs the script without proper arguments or with the `-h` or `--help` option. It simply echoes out a formatted message explaining how to use the script.

### 2\. Reading User Input

```bash
function read_input {
    read -p "$1: " INPUT
    echo "$INPUT"
}
```

The `read_input` function prompts the user to input information. It takes an argument as the prompt message and then uses `read` command to read input from the user. The input is stored in a variable called `INPUT`, which is then echoed back.

### 3\. Command-line Argument Parsing

```bash
while [[ $# -gt 0 ]]; do
    case $1 in
        -c|--container)
            TARGET_CONTAINER=$2
            shift
            shift
            ;;
        -h|--help)
            display_usage
            exit 0
            ;;
        *)
            FILENAME=$1
            shift
            ;;
    esac
done
```

**Iterating Over Command-line Arguments**

The script uses a `while` loop to iterate over each command-line argument provided by the user. The condition `[[ $# -gt 0 ]]` checks whether there are still arguments remaining to process (`$#` represents the number of arguments).

**Case Statement**

Within the loop, a `case` statement is used to evaluate the value of each argument (`$1`). The script examines the argument and takes different actions based on its value.

**Handling Different Argument Cases**

* **\-c|--container:** If the argument is `-c` or `--container`, the script assumes that the next argument contains the name of the target container in the cloud. It assigns the value of the next argument (`$2`) to the `TARGET_CONTAINER` variable. Then it shifts the arguments twice (`shift` command) to move to the next set of arguments.
    
* **\-h|--help:** If the argument is `-h` or `--help`, the script calls the `display_usage` function to show the usage information. Then it exits the script with a status code of `0`, indicating successful completion.
    
* **Filename:** If the argument doesn't match any of the above cases, the script assumes it's the filename. It assigns the value of the argument to the `FILENAME` variable and shifts the arguments once to move to the next argument.
    

### 4\. File Existence Check

```bash
if [ ! -f "$FILENAME" ]; then
    echo "Error: File not found - $FILENAME"
    exit 1
fi
```

This part checks if the file specified by the user exists. If the file doesn't exist, it prints an error message and exits the script with a non-zero exit code.

### 5\. Azure Blob Storage Details Input

```bash
echo "Please provide Azure Blob Storage details:"

ACCOUNT_NAME=$(read_input "Azure Storage Account Name")
ACCOUNT_KEY=$(read_input "Azure Storage Account Key")
TARGET_CONTAINER=$(read_input "Target Container Name")
```

Here, the script prompts the user to provide Azure Blob Storage details, specifically the storage account name, account key, and the target container name. It uses the `read_input` function we discussed earlier to get this information from the user.

### 6\. File Upload to Azure Blob Storage

```bash
echo "Uploading $FILENAME to $CLOUD_PROVIDER..."

az storage blob upload --account-name "$ACCOUNT_NAME" --account-key "$ACCOUNT_KEY" --container-name "$TARGET_CONTAINER" --name "$(basename "$FILENAME")" --type block --content-type "application/octet-stream" --file "$FILENAME"
```

Finally, the script uploads the specified file to Azure Blob Storage using the Azure CLI (`az`) command. It uses the provided Azure storage account name, account key, target container name, and the file name.

## **How To Use**

Follow these simple steps to get started with LTC-CloudUploader\_CLI:

### **1\. Clone the Repository**

```bash
sudo dnf install -y git
git clone https://github.com/vsingh55/LTC-CloudUploader_CLI.git
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

### **Example**

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