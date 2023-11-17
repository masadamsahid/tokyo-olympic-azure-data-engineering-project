# 2021 Tokyo Olympic - Azure Data Engineering Project

This is a simple project about data engineering in Azure using the [Kaggle 2021 Olympics in Tokyo](https://www.kaggle.com/datasets/arjunprasadsarkhel/2021-olympics-in-tokyo "2021 Olympics in Tokyo") by [Arjun Prasad Sarkhel
](https://www.kaggle.com/arjunprasadsarkhel "Arjun Prasad Sarkhel
").

The technologies used in this project are:
- Azure Data Factory
- Azure Data Lake Storage Gen 2
- Azure Databricks
- Azure Synapse Analytics
- Power BI

## Pipeline Overview

![Pipeline](https://github.com/masadamsahid/tokyo-olympic-azure-data-engineering-project/assets/62916459/d83be7ab-2e4e-4e86-860c-1ec6aa8b8d42)

## Details

  1. Download the datasets from Kaggle and store them in a Github repo for better accessibility.
  2. Provision a Data Factory resources.
  3. Provision a Storage account to create a ADLS Gen 2. Make sure to check ✅ the `Enable hierarchical namespace` option in the `Advanced` tab.
  4. Create a new container and create two directories `raw-data` and `transformed-data`.
  6. Back to Azure Data Factory. Then, start ingesting the athletes dataset using the "Copy data" activity.
       - Specify the dataset source using the `HTTP` as the data source and `CSV` as the format.
       - Check the data using the `Preview 👓` button. Make sure the data source is correct.
       - Specify the dataset sink using `Azure Data Lake Storage Gen2` as the data store and `CSV` as the format. And make sure to set the data store's file path to `{YOUR_DATALAKE_CONTAINER_NAME}/raw-data/athletes.csv`. Check `First row as header` options and select `None` for import schema. Click continue.
       - Then do a validation by clicking the `Validate all` and make sure there is no error.
       - Do the same steps for each other datasets (Coaches, Entries Gender, Medals, and Teams). And makes sure each pipeline activity is connected. Then, publish all and run.
       - Check the copied raw datasets in the `raw-data` directory.
  7. Then back to Azure Portal and go to App Registrations.
       - Then create New Registraion
       - On the created app registration overview copy the `Application (client) ID` and `Directory (tenant) ID`.
       - Then search in the app registration sidebar menu and go to `Certificates & secrets` and create a client secret. Copy the client secret `Value`.
       - Save those three IDs and secret in a file on your local machine.
  8. Provision a new Azure Databricks resource.
  9. In the Databricks, create a compute cluster first.
  10. Create a new notebook. And do a transformation as in the `.ipynb` Python notebook. Use the saved `Application (client) ID`, `Directory (tenant) ID`, and `Client secret value` to replace the filesystem mount configuration.
  11. Go to the `transformed-data` directory in the data lake and check the transformed data in each directory.
  12. Provision an Synapse Analytics Resource. And go to it after the deployment.
  13. In the Data menu, create a new resource by clicking the `➕` button and select the `Lake database`, then name the database.
  14. Click the created database. And then create a new table by clicking `➕ Table` dropdown and select `From data lake`. Name and link each table corresponds with each dataset (athletes, coaches, entriesgender, etc.)
  15. [Optional] Perform a basic SQL analytic like bellow:
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
  17. Click Publish
  18. Open a Power BI Desktop and import the data from Synapse Analytics SQL database using a  (Obtain the Serverless SQL endpoint in the Synapse Analytics Properties in the Azure Portal)
  19. Manage the data relationship
  21. Create a visualization dashboard
      
