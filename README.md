# BEES Data Engineering – reweries Case
 
## Table of Contents 
 
1. [Introduction](#introduction) 
2. [Creating a Resource Group](#creating-a-resource-group)
3. [Creating a Storage Account](#creating-a-storage-account) 
4. [Get and Send Files](#get-and-send-files) 
5. [Notes](#notes) 
6. [Further Information](#further-information) 
 
## Introduction 
 
This guide provides a step-by-step process for setup  the Azure Portal web interface,  consuming data from an API,  transforming and persisting it
into a data lake following the medallion architecture with three layers: raw data, curated data
partitioned by location, and an analytical aggregated layer. 

###  Architecture


<img src="https://github.com/cristianomoliveira/Breweries-_data_engineering/blob/main/src/architecture.png" alt="Architecture">

## Creating a Resource Group 
 
1. Log in to the [Azure Portal](https://portal.azure.com). 
 
2. In the left-hand menu, click on "Resource groups". 
 
3. Click the "+ Create" button at the top of the page. 
 
4. Fill in the following information: 
   - Select the Subscription you want to use. 
   - Enter a name for your new Resource Group. 
   - Choose the Region where you want the Resource Group to be located. Example: "(US) East US 2"
 
5. Click "Review + Create". 
 
6. Review the settings and click "Create". 
 
7. Wait for the deployment to complete. This usually takes a few seconds. 
 
## Creating a Storage Account 
 
1. In the Azure Portal, click "Create a resource" or use the "+" button in the top left corner. 
 
2. In the search bar, type "Storage account" and select this option from the results. 
 
3. Click "Create" on the Storage Account overview page. 
 
4. In the "Basics" tab, fill in the following information: 
   - Select the Subscription you want to use. 
   - Choose the Resource Group you created in the previous section. 
   - Enter a unique name for your Storage Account. 
   - Choose the region. Example: "(US) East US 2". 
   - Select the performance type (Standard). 
   - Choose the storage account type (usually StorageV2). 
   - Select the desired redundancy level (LRS). 
 
5. Click "Review + Create" at the bottom of the page. 
 
6. Review all settings on the summary page. 
 
7. If everything is correct, click "Create". 
 
8. Wait while Azure deploys your Storage Account. This usually takes a few seconds. 
 
9. When the deployment is complete, you will see a notification. Click "Go to resource" to access your new Storage Account. 

10. Click "Blob Service"

11. Click "Create Container"
- Enter the name "data"


## Get and Send Files 
 
- You need execute the notebook called "send_file.ipynb". 
- Before the execution, you need fill some variables, for example "account_url", and execute pip install requirements.txt .
 
## Notes 
 
- You can adjust additional settings in the other tabs (Networking, Data protection, Advanced) before creating the account, if necessary. 
- These settings can be modified later, but some may require recreating the account. 
 
## Further Information 
 
For more information, refer to the [official Azure documentation](https://docs.microsoft.com/en-us/azure/storage/common/storage-account-create). 
