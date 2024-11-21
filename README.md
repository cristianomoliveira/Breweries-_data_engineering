# BEES Data Engineering – reweries Case
 
## Table of Contents 
 
1. [Introduction](#introduction) 
2. [Creating a Resource Group](#creating-a-resource-group)
3. [Creating a Storage Account](#creating-a-storage-account) 
4. [Get and Send Files](#get-and-send-files) 
5. [Creating a Data Factory](#creating-a-data-factory) 
6. [Linking Storage Accounts to Data Factory](#linking-storage-accounts-to-data-factory) 
7. [Creating Data Sets ](#creating-data-sets )
8. [Copy With Data Factory ](#copy-with-data-factory)
9. [Notes](#notes) 
10. [Further Information](#further-information) 
 
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

## Creating a Data Lake from a Storage Account 
 
1. In the Azure Portal, create a new Storage. 

2. In the Azure Portal, click "Create a resource" or use the "+" button in the top left corner. 
 
3. In the search bar, type "Storage account" and select this option from the results. 
 
4. Click "Create" on the Storage Account overview page. 
 
5. In the "Basics" tab, fill in the following information: 
   - Select the Subscription you want to use. 
   - Choose the Resource Group you created in the previous section. 
   - Enter a unique name for your Storage Account. Example "datalake-..."
   - Choose the region. Example: "(US) East US 2". 
   - Select the performance type (Standard). 
   - Choose the storage account type (usually StorageV2). 
   - Select the desired redundancy level (LRS). 

6. In the "Advanced" tab, fill in the following information: 
 - Enable hierarchical namespace. 
 
7. Click "Review + Create" at the bottom of the page. 
 
8. In the left-hand menu, under "Data Lake Storage", click on "Containers". 
 
9. Click the "+ Container" button at the top of the page. 
 
10. Enter a name for your new container, for example "data-project-bees". This will be the root of your Data Lake file system. 
 
11. Click "Create". 
 
12. Your Data Lake is now ready to use. You can upload files, create directories, and manage access control. Now create the directories named: Bronze, Silver and Gold.

## Creating a Data Factory 
 
1. In the Azure Portal, click "Create a resource" or use the "+" button in the top left corner. 
 
2. In the search bar, type "Data Factory" and select it from the results. 
 
3. Click "Create" on the Data Factory overview page. 
 
4. In the "Basics" tab, fill in the following information: 
   - Select the Subscription you want to use. 
   - Choose the Resource Group you created earlier. 
   - Enter a name for your Data Factory. Example "service-adf-project-bees"
   - Choose the region where you want the Data Factory to be created. Example: "(US) East US 2". 
   - Select the version (V2 is recommended). 
 
5. Click "Next: Git configuration >" and configure Git repository if needed, or skip this step. 
 
6. Click "Review + Create". 
 
7. Review all settings on the summary page. 
 
8. If everything is correct, click "Create". 
 
9. Wait while Azure deploys your Data Factory. This may take a few minutes. 
 
## Linking Storage Accounts to Data Factory 
 
1. Once your Data Factory is created, go to its resource page. 
 
2. In the left-hand menu, under "Author & Monitor", click on "Open Azure Data Factory Studio". 
 
3. In the Data Factory Studio, click on "Manage" in the left sidebar. 
 
4. Under "Connections", click on "Linked services". 
 
5. Click on "+ New" to create a new linked service. 
 
6. Choose "Azure Blob Storage" from the list of data stores. 
 
7. Fill in the details for the first Storage Account: 
   - Name: Give a name to your linked service. For example "azure-blob-storage-project-bees"
   - Account selection method: Choose "From Azure subscription". 
   - Azure subscription: Select your subscription. 
   - Storage account name: Choose the first Storage Account you created. 
 
8. Click "Test connection" to ensure it works, then click "Create". 
 
9. Repeat steps 5-8 for the second Storage Account, choosing "Azure Data Lake Storage Gen2" from the list of data stores. 
 
10. You now have both Storage Accounts linked to your Data Factory and can use them in your data pipelines. 
 
## Creating Data Sets 
 
1. In the Azure Data Factory Studio, click on "Author" in the left sidebar. 
 
2. Under the "Factory Resources" pane, click the "+" button and select "Dataset". 
 
3. In the "New dataset" window, choose the data store type. For Azure Storage Account, you can select "Azure Blob Storage". 

 
4. Configure the dataset: 
   - Name: Give your dataset a descriptive name. Example: "ds-src-project-bees" 
   - Linked service: Select the linked service you created before for Blob Storage. 
   - File path: Specify the container and path to your data. Select the file "breweries.csv". 
   - File format: Choose the appropriate format (DelimitedText). 
 
5. Click "OK" to create the dataset. 
 
6. Click "Publish All" at the top of the page to save your changes. 
 
7. Repeat steps 2-6 to create additional dataset as needed, , one for the destination, using "Azure Data Lake Storage Gen 2" for data store type,  in Linked service: Select the linked service you created before for Data Lake. In File path: Specify the container and path to save your data. Select the directory "Bronze". 

## Copy With Data Factory

1. In the Azure Data Factory Studio, click on "Author" in the left sidebar. 
2. Under the "Factory Resources" pane, click the "+" button and select "Pipeline". 
3. Give your pipeline a descriptive name. Example: "data-ingestion-project-bees" . 
4. In the pipeline canvas, drag and drop a "Copy Data" activity from the "Move & Transform" section. 
 
5. In the "Source" tab of the Copy Data activity: 
   - Source dataset: Select your Blob Storage dataset. 
 
6. In the "Sink" tab: 
   - Sink dataset: Select your Data Lake dataset. 
   - File Extension: .csv

8. Click "Validate" to validate your pipeline.  
9. Click "Publish All" to save your pipeline. 

 
## Setting Up Trigger for Automated Copy 
 
1. In your pipeline canvas, click "Add trigger" and then "New/Edit". 
 
2. In the "Add triggers" window, click "New". 
 
3. Configure your trigger: 
   - Name: Give your trigger a name. 
   - Type: Select "Event". 
   - Azure subscription: Choose your subscription. 
   - Storage account name: Select your Blob Storage account. 
   - Container name: Choose the container with your source data. 
 
4. Under "Blob path begins with", enter the path where changes should trigger the pipeline. 
 
5. Under "Blob path ends with", enter a file extension if applicable (e.g., `.csv`). 
 
6. Choose the events that should trigger the pipeline (e.g., "Blob created"). 
 
7. Click "OK" to create the trigger. 
 
8. Click "Publish All" to save your changes. 
 

## Notes 
 
- Enabling hierarchical namespace (Data Lake Storage Gen2) must be done during Storage Account creation. It cannot be changed later. 
- Data Lake Storage Gen2 is built on top of Azure Blob Storage and provides file system semantics, file-level security, and scale. 
- You can adjust additional settings in the other tabs (Networking, Data protection, Advanced) before creating the Storage Account, if necessary. 
- It's a best practice to organize your Azure resources into Resource Groups for easier management and organization. 
- Linked services in Data Factory define the connection information to data stores and compute resources. 
- Data Factory allows you to create data pipelines to move and transform data between different data stores. 
- Datasets in Azure Data Factory represent data structures within data stores, pointing to or referencing the data you want to use in your activities as inputs and outputs. 
- When creating datasets, ensure that the file paths and formats accurately reflect your data structure in the Storage Accounts. 
- Event-based triggers in Azure Data Factory allow for near real-time data processing. 
- Ensure that the service principal for your Data Factory has the necessary permissions on both the Blob Storage and Data Lake. 
- Consider implementing error handling and logging in your pipeline for production scenarios. 




## Further Information 
 
For more information, refer to the official Azure documentation: 
- [Create a Resource Group](https://docs.microsoft.com/en-us/azure/azure-resource-manager/management/manage-resource-groups-portal#create-resource-groups) 
- [Create a Storage Account](https://docs.microsoft.com/en-us/azure/storage/common/storage-account-create) 
- [Create a Data Lake Storage Gen2 account](https://docs.microsoft.com/en-us/azure/storage/blobs/create-data-lake-storage-account) 
- [Use Data Lake Storage Gen2](https://docs.microsoft.com/en-us/azure/storage/blobs/data-lake-storage-introduction) 
- [Create a Data Factory](https://docs.microsoft.com/en-us/azure/data-factory/quickstart-create-data-factory-portal) 
- [Create linked services in Azure Data Factory](https://docs.microsoft.com/en-us/azure/data-factory/quickstart-create-data-factory-portal#create-a-linked-service) 
- [Create datasets in Azure Data Factory](https://docs.microsoft.com/en-us/azure/data-factory/concepts-datasets-linked-services) 
- [Copy activity in Azure Data Factory](https://docs.microsoft.com/en-us/azure/data-factory/copy-activity-overview) 
- [Trigger pipelines on storage events](https://docs.microsoft.com/en-us/azure/data-factory/how-to-create-event-trigger) 





