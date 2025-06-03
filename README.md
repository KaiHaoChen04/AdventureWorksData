# AdventureWorksData

## Introduction
This project highlights the process of creating and maintaining an ETL Pipeline using Azure resources like Data Factory, Data lake, Databricks, Synapse; Tools like Apache sparks, Power BI, Clusters, etc...

Primary Language used was Python.
### Dataset
Primary data set used for this project is adventure works that contains a relatively large amount of data with tables such as:
 - Sales
 - Customers
 - Products
 - Sub Products
 - Territories
 - Calendar\

Link: https://www.kaggle.com/datasets/ukveteran/adventure-works

### Azure
Resource Group:\
-> adb_aw_project(Azure Databricks Service)\
-> awdatafactorykai(Data factory (V2))\
-> awstoragedatalakekai(Data Lake storage account)\
-> synapsestoragekai(Synapse Analytic)\
-> synapse-awproject-kai(Synapse)\
<img width="1036" alt="Screenshot 2025-06-03 at 5 16 25 PM" src="https://github.com/user-attachments/assets/8725af62-9dc7-4105-b38b-9e7b8235ab30" />

### Data Extraction (Ingestion)
Used Copy Data and for each iteration functions in Data Factory to effectively extract data from HTTP(Git) in CSV format onto bronze container in Data Lake using a well-formed pipeline.
Utilized parameters within the pipeline to pass dynamic values such as file names and paths, ensuring flexibility and scalability.

<img width="512" alt="Screenshot 2025-06-03 at 5 18 27 PM" src="https://github.com/user-attachments/assets/54253adc-ebc9-4ea4-9617-47916071d813" />
<img width="500" alt="Screenshot 2025-06-03 at 5 20 04 PM" src="https://github.com/user-attachments/assets/87adec38-1e90-44a8-ad7f-1d1e81b00539" />
<img width="500" alt="Screenshot 2025-06-03 at 5 20 32 PM" src="https://github.com/user-attachments/assets/7e5e4c69-650a-4142-bd5a-8719a6c91310" />


### Data Transformation 
Apache Spark was used to read CSV format data residing in bronze container thus transforming and aggregating data using PySpark;Operations include GroupBy, Filtering, WithColumn and visually displaying data.
Finally pushing the data onto Silver container in parquet format powered by DataBrick Cluster's Virtual Machine.

(Establish data lake and data brick connection)

from pyspark.sql.functions import *\
from pyspark.sql.types import *

service_credential = dbutils.secrets.get(scope="<secret-scope>",key="<service-credential-key>")

spark.conf.set("fs.azure.account.auth.type.<storage-account>.dfs.core.windows.net", "OAuth")
spark.conf.set("fs.azure.account.oauth.provider.type.<storage-account>.dfs.core.windows.net", "org.apache.hadoop.fs.azurebfs.oauth2.ClientCredsTokenProvider")
spark.conf.set("fs.azure.account.oauth2.client.id.<storage-account>.dfs.core.windows.net", "<application-id>")
spark.conf.set("fs.azure.account.oauth2.client.secret.<storage-account>.dfs.core.windows.net", service_credential)
spark.conf.set("fs.azure.account.oauth2.client.endpoint.<storage-account>.dfs.core.windows.net", "https://login.microsoftonline.com/<directory-id>/oauth2/token")
<img width="914" alt="Screenshot 2025-06-03 at 5 30 19 PM" src="https://github.com/user-attachments/assets/1204b1cf-25bb-4f4e-9d90-3e4a53590468" />
<img width="969" alt="Screenshot 2025-06-03 at 5 30 54 PM" src="https://github.com/user-attachments/assets/3918560a-36f2-4233-8b99-ff12783a87e1" />

(Sample Visual)
<img width="500" alt="Screenshot 2025-06-03 at 5 25 06 PM" src="https://github.com/user-attachments/assets/42dd5a3c-8a24-4aa4-99c2-a6af41d37e05" />


### Synapse Analytics
Ran SQL-Based Queries that allowed me to create schemas in gold containers with external tables aka Serverless Database. At last, this database was exported to PowerBI where datas are displayed 
visually in the essence of graphs and tables.

<img width="400" alt="Screenshot 2025-06-03 at 5 28 02 PM" src="https://github.com/user-attachments/assets/e61a9a21-b279-4a71-a5a5-95caf8960509" />
