---
title: "Azure ARM Templetes"
datePublished: Sun Feb 11 2024 02:30:06 GMT+0000 (Coordinated Universal Time)
cuid: clsgw11hh000009jq6vuwak67
slug: azure-arm-templetes
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1707584806650/ede6dc4b-a489-46f5-8582-d18432df6a0c.png
tags: azure, devops, azure-devops, 90daysofdevops, trainwithshubham, 10weeksofcloudops

---

# Introduction

Azure ARM (Azure Resource Manager) templates are a powerful tool for managing Azure resources. They are JSON files that describe the resources and properties to be deployed to Azure. This blog post will discuss Azure ARM templates, the ARM extension in Visual Studio Code (VS Code), and how to write your first ARM template for creating a storage account with a detailed explanation of each term.

## Azure ARM Templates

ARM templates provide a declarative way to define the deployment of Azure resources. They are written in JSON and contain a series of "resources" blocks, each describing a resource to be created. One of the main advantages of ARM templates is that they allow for consistent deployments. For example, if you need to deploy multiple resources with the same configuration, you can create an ARM template and use it to deploy those resources consistently. This eliminates the risk of manual errors and ensures that all resources adhere to the same configuration standards. Additionally, ARM templates support advanced functions like conditional deployment and looping, providing a comprehensive and flexible tool for resource management in Azure.

## ARM Extension in VS Code

VS Code, a popular code editor, provides an extension for working with ARM templates. The ARM extension offers IntelliSense support, which helps with auto-completion and provides quick information about the syntax and parameters. It also includes a schema validation feature that checks your ARM templates against the Azure deployment schema. This can catch common mistakes before you deploy your template. The extension also supports snippets, which are small reusable pieces of code, aiding in quicker and more efficient ARM template development.

## First ARM Template for Storage Account Creation

Creating a storage account using an ARM template involves defining the type, name, location, and properties of the storage account in the JSON file. Here is an example:

```json
{
    "$schema": "<https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#>",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "storageName": {
            "type": "string",
            "metadata": {
                "description": "Name of the storage account"
            }
        },
        "location": {
            "type": "string",
            "metadata": {
                "description": "Location of the storage account"
            }
        }
    },
    "variables": {
        "storageSkuName": "Standard_LRS",
        "storageKind": "StorageV2"
    },
    "resources": [
        {
            "type": "Microsoft.Storage/storageAccounts",
            "apiVersion": "2019-06-01",
            "name": "[parameters('storageName')]",
            "location": "[parameters('location')]",
            "sku": {
                "name": "[variables('storageSkuName')]"
            },
            "kind": "[variables('storageKind')]",
            "properties": {}
        }
    ]
}
```

In this template, the `$schema` and `contentVersion` are metadata properties that specify the schema and version of the template. The `resources` array contains the resources to be deployed. Each resource has a `type` (the type of Azure resource to be created), `apiVersion` (the version of the API to use), `name`, `location`, `sku` (defines the performance tier and replication scenario), and `kind` (the type of the storage account). The `properties` object allows you to set additional properties for the resource.

The Azure Resource Management extension in VS Code primarily utilizes the Azure deployment schema for validating ARM templates. This schema checks the structure and syntax of the templates against the standard set by Azure for resource deployment.

## Different modes of Schema

Azure Resource Manager (ARM) provides different modes of deployment for managing resources - Incremental (ARM!), Complete (ARM!P), Validate (ARM!S), What-if (ARM!T), and Management Groups (ARM!MG).

* **Incremental (ARM!):** This is the default deployment mode for ARM. In this mode, resource deployment is incremental. Resources that are specified in the ARM template are added or updated in the resource group, but resources not specified in the template are not deleted.
    
* **Complete (ARM!P):** In this mode, all resources in the resource group that are not specified in the ARM template are deleted during deployment. This mode is useful when you want to ensure that only the resources specified in the template exist in the resource group.
    
* **Validate (ARM!S):** This deployment mode is used for validating the template. It does not create or update any resources. It is used to check the template for errors before actual deployment.
    
* **What-if (ARM!T):** This mode is used to predict the changes that would be made by the deployment without actually making those changes. It is useful for reviewing changes before deploying the template.
    
* **Management Groups (ARM!MG)**: This is not a deployment mode but a way to manage your Azure resources at scale. Management groups provide a level of scope above subscriptions. They allow you to manage access, policies, and compliance across multiple subscriptions.
    

Each of these modes serves a unique purpose and are chosen based on the requirements of the deployment task. For instance, if you wish to ensure that the resources in your resource group exactly match your ARM template, you would choose the 'Complete' mode. If you want to check for errors in your template before deployment, the 'Validate' mode would be suitable. The 'What-if' mode allows you to foresee the potential outcome of your deployment without actually implementing it, providing an opportunity to rectify any undesired actions. 'Management Groups', although not a deployment mode, offer an efficient way to manage resources at a larger scale, especially useful for larger organizations managing multiple subscriptions.

### Conclusion

Azure ARM templates are a potent tool for managing Azure resources consistently and efficiently. The ARM extension in VS Code further simplifies this process by providing useful features like auto-completion, schema validation, and code snippets. By understanding the structure and syntax of ARM templates, you can start creating your own templates for various Azure resources, such as storage accounts, and leverage the power of Infrastructure as Code in your Azure deployments.

<div data-node-type="callout">
<div data-node-type="callout-emoji">💡</div>
<div data-node-type="callout-text">You may visit for more ARM templates:</div>
</div>

> [https://github.com/krvsc/Azure-Resource-Manager-templetes.git](https://github.com/krvsc/Azure-Resource-Manager-templetes.git)