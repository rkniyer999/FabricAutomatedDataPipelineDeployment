# FabricAutomatedDataPipelineDeployment

![FabricAutomatedDataPipelineDeploymentOverview](images/MigrateFabricDataPipelineOverview.png)
One of the significant use cases we often hear from our customers is the ability to migrate resources across tenants. Supporting tenant migration is crucial for maintaining flexibility, scalability, and resilience in a multi-tenant environment. This capability also allows businesses to reuse and redeploy pipelines across tenants.

For those familiar with Azure Data Factory (ADF), you know that its import and export capabilities have made pipeline migration a breeze, enabling customers and ISVs alike to reuse and redeploy pipelines across tenants.

This repository can be used to automate the Fabric data pipeline deployment within the same tenant and for cross-tenant deployments. To make it simple, I have divided it into a 5-step process.

# STEP 1 Create Workspace,Lakehouse & Data Warehouse in destination Tenant
**Pre-requisites**
1. A Fabric **Destination workspace** is needed where Fabric data pipelines need to be deployed. This can be an **existing workspace or a new workspace**. Get the e.g. Workspace ID - 66a92280-b91b-408e-bdd5-0276ad7d35a1.
1. Within this workspace, create a **Lakehouse** or identify **existing lakehouse** where the data needs to be ingested in **Destination workspace**.
    - Get the Lakehouse ID e.g. a1e8b2fe-7527-40b7-a137-648be36a6602
    - Get the SQL analytics endpoint e.g. x6eps4xrq2xudenlfv6naeo3i4-qarkszq3xghebpovaj3k27jvue.msit-datawarehouse.fabric.microsoft.com
    - Get the name e.g. CMFLHStore
1. **Optional - Only if your pipeline uses Warehouse.** Create a **Warehouse** or identify existing warehouse where the metadata needs to be ingested in **Destination workspace**.
     - Get the Warehouse ID e.g. 4114775b-f303-4e3f-b230-c31c555466d8
     - Get the SQL connection String - x6eps4xrq2xudenlfv6naeo3i4-qarkszq3xghebpovaj3k27jvue.msit-datawarehouse.fabric.microsoft.com
     - Get the Datawarehouse Name  - CMFDWStore

# STEP 2 Identify Data Pipelines to be migrated & save the assets.
1. Identify the data pipelines to be deployed.For all the pipelines that are deployed, Go to the pipeline & select "View JSON code".
![View JSON code](image-4.png)
1. Go to "Copy to clipboard" & "Close".
  ![view JSON code](image-2.png)
  Save the Json & upload the Json in the lakehouse in Destination workspace.

# STEP 3 Identify external connections & create connections in destination tenant
1. Identify External connections/references used in pipelines. External references could be an Azure PostgreSQL DB connection or Azure SQL DB connection or even  Blob Storage/ADLS Gen2 storage in Azure in the data pipeline. You can use "" in scripts to identify the list of extenal connections & their corresponding id in your pipeline. **Note** If your pipeline is simple you can do a find & note the connection id.
![alt text](image-5.png)
1. Create connections in new Tenant for the external references for the source Azure data stores from where you want to ingest data. We have PostgreSQL DB & Blob Storage/ADLS Gen2 storage in Azure. 
Create connections by going to "Settings" -> "Manage connections and gateways" -> "New". Once the new connection is created save the new connection id.
![Manage connections and gateways](image-9.png)

# STEP 4 Update pipeline deployment config files
1. Below figure shows the YAML configuration file for deploying the fabric data pipelines.
![YAML configuration file](image-7.png)

# STEP 5
1. Import the "DeployFabricDataPipelineUtility" notebook.
![image.png](/.attachments/image-e14ba5ea-3a3c-4af4-b844-7406513c3a19.png)
1. Please make sure that lakehouse where we have the Json & YAML files are marked as default.
![image.png](/.attachments/image-7e8d27d9-9076-4e5c-9e51-bbe1951b959b.png)
1. Execute the "DeployFabricDataPipelineUtility" notebook.
--------------------------------------------------------------------


