# 2021 Tokyo Olympic - Azure Data Engineering Project

![image](https://github.com/masadamsahid/tokyo-olympic-azure-data-engineering-project/assets/62916459/a66499e0-4d8d-4fd4-8d0b-06cb12a8424f)

This is a simple project about data engineering in Azure using the [Kaggle 2021 Olympics in Tokyo](https://www.kaggle.com/datasets/arjunprasadsarkhel/2021-olympics-in-tokyo "2021 Olympics in Tokyo") by [Arjun Prasad Sarkhel](https://www.kaggle.com/arjunprasadsarkhel "Arjun Prasad Sarkhel").

The technologies used in this project are:
- Azure Data Factory
- Azure Data Lake Storage Gen 2
- Azure Databricks
- Azure Synapse Analytics
- Power BI

## Pipeline Overview

![Pipeline](https://github.com/masadamsahid/tokyo-olympic-azure-data-engineering-project/assets/62916459/d83be7ab-2e4e-4e86-860c-1ec6aa8b8d42)

## Details

  1. Download the datasets from Kaggle and store them in a Github repo for better accessibility. Like [here](./data "raw data")
      ![image](https://github.com/masadamsahid/tokyo-olympic-azure-data-engineering-project/assets/62916459/8e3273a9-c8d2-476f-9c9e-6a6bd85559aa)

  2. Provision a Data Factory resources.
  3. Provision a Storage account to create a ADLS Gen 2. Make sure to check ✅ the `Enable hierarchical namespace` option in the `Advanced` tab.
    ![image](https://github.com/masadamsahid/tokyo-olympic-azure-data-engineering-project/assets/62916459/63bf913c-0897-4d87-9002-e61fe9532882)
  4. Create a new container and create add new two directories `raw-data` and `transformed-data`.
    ![image](https://github.com/masadamsahid/tokyo-olympic-azure-data-engineering-project/assets/62916459/f0f73c4a-c677-42ac-b9b4-3d2ee75fa499)
  5. Back to Azure Data Factory. Then, start ingesting the athletes dataset using the "Copy data" activity.
       - Specify the dataset source using the `HTTP` as the data source and `CSV` as the format.
         ![image](https://github.com/masadamsahid/tokyo-olympic-azure-data-engineering-project/assets/62916459/9ff7ed8e-5df3-44b8-9af5-fb53f7686cc8)
       - Check the data using the `Preview 👓` button. Make sure the data source is correct.
         ![image](https://github.com/masadamsahid/tokyo-olympic-azure-data-engineering-project/assets/62916459/d1277cfd-2320-4ac3-9eb4-1afa7cb5d1c0)
       - Next, specify the dataset sink using `Azure Data Lake Storage Gen2` as the data store and `CSV` as the format. And make sure to set the data store's file path to `{YOUR_DATALAKE_CONTAINER_NAME}/raw-data/athletes.csv`. Check `First row as header` options and select `None` for import schema. Click continue.
         ![image](https://github.com/masadamsahid/tokyo-olympic-azure-data-engineering-project/assets/62916459/17f78233-2273-47b7-a0b4-e99310530bcc)
       - Then do a validation by clicking the `Validate all` and make sure there is no error.
       - Do the same steps for each other datasets (Coaches, Entries Gender, Medals, and Teams). And makes sure each pipeline activity is connected. Then, publish all and run.
       - Check the copied raw datasets in the `raw-data` directory.
         ![image](https://github.com/masadamsahid/tokyo-olympic-azure-data-engineering-project/assets/62916459/8fde554d-5169-45c1-a02b-1c473fbb2d94)

  6. Then back to Azure Portal and go to App Registrations.
       - Then create new registration
         ![image](https://github.com/masadamsahid/tokyo-olympic-azure-data-engineering-project/assets/62916459/b3719f51-3710-4015-8490-60ad700662f2)
       - On the created app registration overview copy the `Application (client) ID` and `Directory (tenant) ID`.
         ![image](https://github.com/masadamsahid/tokyo-olympic-azure-data-engineering-project/assets/62916459/672fe3ab-2729-4e8e-8366-5e043e59ef33)
       - Then search in the app registration sidebar menu and go to `Certificates & secrets` and create a client secret. Copy the client secret `Value`.
         ![image](https://github.com/masadamsahid/tokyo-olympic-azure-data-engineering-project/assets/62916459/18c3a8ec-a7d9-46ba-bfb9-c7c787297b48)
       - Save those three IDs and secret in a file on your local machine.
  7. Provision a new Azure Databricks resource.
  8. In the Databricks, create a compute cluster first.
    ![image](https://github.com/masadamsahid/tokyo-olympic-azure-data-engineering-project/assets/62916459/338fb559-bbd1-493a-bc7a-7324370aa2a2)

  9. Create a new notebook. And do a transformation as in the `.ipynb` Python notebook. Use the saved `Application (client) ID`, `Directory (tenant) ID`, and `Client secret value` to replace the filesystem mount configuration.
     ![image](https://github.com/masadamsahid/tokyo-olympic-azure-data-engineering-project/assets/62916459/59483a91-c2bd-4bd2-bf17-cfc45c984616)
     ![image](https://github.com/masadamsahid/tokyo-olympic-azure-data-engineering-project/assets/62916459/9ece2b43-3b88-4c36-8596-143230d9e391)
     The notebook is available [here](./Tokyo%20Olympic%20Transformation.ipynb "Databrick transformation notebook").

  10. Go to the `transformed-data` directory in the data lake and check the transformed data in each directory.
    ![image](https://github.com/masadamsahid/tokyo-olympic-azure-data-engineering-project/assets/62916459/4f6f14fb-dcac-4ec8-a8dc-9ec87ee584ac)
    ![image](https://github.com/masadamsahid/tokyo-olympic-azure-data-engineering-project/assets/62916459/65866e4e-dbb1-4280-bc1e-1e1bd1aca399)

  11. Provision a Synapse Analytics Resource. And go to it after the deployment.
  12. In the Data menu, create a new resource by clicking the `➕` button and select the `Lake database`, then name the database.
  13. Click the created database. And then create a new table by clicking `➕ Table` dropdown and select `From data lake`. Name and link each table corresponds with each dataset (athletes, coaches, entriesgender, etc.)
    ![image](https://github.com/masadamsahid/tokyo-olympic-azure-data-engineering-project/assets/62916459/edb24844-a21f-41d7-bf36-e0ad9b0bfc31)
    ![image](https://github.com/masadamsahid/tokyo-olympic-azure-data-engineering-project/assets/62916459/c577c516-77c5-4ff4-b58b-d47c6222f0e1)

  14. [Optional] Perform a basic SQL analytic like bellow:
      ```sql
      -- count the number of athletes from each country:
      SELECT Country, COUNT(*) AS TotaAthletes
      FROM athletes
      GROUP BY Country
      ORDER BY TotaAthletes DESC;
      
      -- count the total medals wpm by each country:
      SELECT
          Team_Country,
          SUM(Gold) Total_Gold,
          SUM(Silver) Total_Silver,
          SUM(Bronze) Total_Bronze
      FROM medals
      GROUP BY Team_Country
      ORDER BY
       Total_Gold DESC,
       Total_Silver DESC,
       Total_Bronze DESC;
      
      --  Calculate the percentage number of entries by gender for each discipline:
      SELECT
          Discipline,
          Male,
          Female,
          Total,
          (CAST(Male AS FLOAT) / CAST(Total AS FLOAT)) Male_Percentage,
          (CAST(Female AS FLOAT) / CAST(Total AS FLOAT)) Female_Percentage
      FROM entriesgender;
      ```
      ![image](https://github.com/masadamsahid/tokyo-olympic-azure-data-engineering-project/assets/62916459/4962864f-ec6b-46ee-a287-59812a976087)
      ![image](https://github.com/masadamsahid/tokyo-olympic-azure-data-engineering-project/assets/62916459/5c464a53-0778-45e6-998a-ea470ab0c278)
      ![image](https://github.com/masadamsahid/tokyo-olympic-azure-data-engineering-project/assets/62916459/888c8396-6a8f-48a8-8af2-797bbb4420ce)


  15. Click Publish
  16. Open a Power BI Desktop and import the data from Synapse Analytics SQL database using a  (Obtain the Serverless SQL endpoint in the Synapse Analytics Properties in the Azure Portal)
      ![image](https://github.com/masadamsahid/tokyo-olympic-azure-data-engineering-project/assets/62916459/03971118-196e-4391-9bc0-4c3aca5dc994)
      We use `Import` instead of `Direct queries` to save the synapse cost. By importing means we copy/download the database to our Power BI project so we don't need to query on the synapse SQL server while building the dashboard.

  17. Manage the data relationship
      ![image](https://github.com/masadamsahid/tokyo-olympic-azure-data-engineering-project/assets/62916459/6e11adbb-b2d2-4271-9261-02d3e2dc58d8)

  18. Create a visualization dashboard
      ![image](https://github.com/masadamsahid/tokyo-olympic-azure-data-engineering-project/assets/62916459/8cf0ef4a-d4ff-4ef9-bf2d-4c2460982059)

      
