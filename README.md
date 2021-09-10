# Data Integration for the Florida Lottery Department

<!-- TABLE OF CONTENTS -->
<!-- TABLE OF CONTENTS -->
<details open="open">
  <summary>Table of Contents</summary>
  <ol>
    <li><a href="#about-the-project">About the Project</a></li>
    <li><a href="#built-with">Built With</a></li>
    <li><a href="#getting-started">Getting Started</a></li>
    <li><a href="#azure-logic-apps-workflow">Azure Logic Apps Workflow</a>
        <ul>
          <li><a href="#file-system-trigger">File System Trigger</a></li>
          <li><a href="#branching-logic">Branching Logic</a></li>
          <li><a href="#xml-to-json-conversion">XML to JSON conversion</a></li>
          <li><a href="#loading-data-into-SQL-Server-database">Loading Data into SQL Server Database</a></li>
      </ul>
    </li>
    <li><a href="#azure-data-factory-pipeline">Azure Data Factory Pipeline</a>
        <ul>
          <li><a href="#logic-apps-trigger">Logic Apps Trigger</a></li>
          <li><a href="#utilizing-blob-storage">Utilizing Blob Storage</a></li>
          <li><a href="#copying-data-into-SQL-Server-Database">Copying Data into SQL Server Database</a></li>
          <li><a href="#email-notifications-for-failed-activities">Email Notifications for Failed Activities</a></li>
      </ul>
    </li>
    <li><a href="#further-documentation">Further Documentation</a></li>
    <li><a href="#acknowledgements">Acknowledgements</a></li>
  </ol>
</details>

<!-- ABOUT THE PROJECT -->
## About The Project

With the Florida Lottery as the group’s sponsors, this project aimed to improve the current file ETL processes by supporting the transition from a legacy integration environment to a modern integration platform. In particular, the team analyzed the use of two Microsoft Azure Services - Azure Logic Apps and Azure Data Factory. The proposed solution was a proof-of-concept that each platform could perform the basic file ETL process functionality. This was achieved through research and implementation of a prototype on each platform. After following the cycle of researching, prototyping, and assessing, the team created two final prototypes that each retrieve a file from a local file system, transform the data, and insert the data to an SQL database table.
<br>
<figure>
  <p align="center">
    <kbd>
      <img src="https://github.com/nicholasgonzalez1/Data_Integration_FLD/blob/main/images/about_project.png?raw=true" width="700"/>
    </kbd>
  </p>
</figure>


<!-- BUILT WITH -->
## Built With

The following platforms were selected to build each pipeline:
* Azure Logic Apps
* Azure Data Factory

<!-- AZURE LOGIC APPS -->
## Azure Logic Apps Workflow

Azure Logic Apps utilized built in connectors such as on-premise gateways, liquid templates, and an Azure SQL Server database. Our Logic App prototype creates a file system trigger that is fired once a file is entered into a local storage. From there, we copy the file content and file name, transform the format from XML to JSON, parse the data, and insert the entity into a SQL Server database. 

### File System Trigger

In order to retrieve files that were located on a computer's local directory, a file system connector was configured into the workflow. Everytime a file was added to a specified folder in the local directory, the pipeline would trigger and retrieve the contents of the newly added files. This connector served as a gateway that allowed us to bridge the gap between on-premise devices and the configured Logic Apps workflow.
<br>
<figure>
  <p align="center">
    <kbd>
      <img src="https://github.com/nicholasgonzalez1/Data_Integration_FLD/blob/main/images/file_system_trigger.png?raw=true" width="700"/>
    </kbd>
  </p>
  <p align="center">Logic Apps connector: “When a file is added or modified (properties only)"</p>
</figure>

### Branching Logic

Once the contents are the file were retrieved, the type of file needed to be determined so that it could be further processed by a correct script. For the Logic Apps pipeline, all files being passed through were of XML type. However, the contents of the file were divided by 3 different types: Retailer, Player, and Game. Since actual company was restricted from being given to us, these file types were meant to mimic different classifications of data used at the Florida Lottery. The figure below shows the logic we used to determine the type of file being processed.
<br>
<figure>
  <p align="center">
    <kbd>
      <img src="https://github.com/nicholasgonzalez1/Data_Integration_FLD/blob/main/images/branching_logic.png?raw=true" width="500"/>
    </kbd>
  </p>
  <p align="center">Logic for ensuring XML content is transformed using the right template</p>
</figure>

### XML to JSON Conversion

Once the file type is determined, the XML content is transformed via mapping the content through a Liquid Template. In Azure, we utilized the Liquid connector that has an action of transforming XML content to JSON. Liquid maps were created using Visual Studio Code. These liquid files loop through the content of the XML file and grab the corresponding attributes for each record from the XML file. The figure liquid template file that was used to transform a file of type “player” to JSON.
<br>
<figure>
  <p align="center">
    <kbd>
      <img src="https://github.com/nicholasgonzalez1/Data_Integration_FLD/blob/main/images/xml_to_json.png?raw=true" width="500"/>
    </kbd>
  </p>
  <p align="center">Logic Apps Connector: Transform XML to JSON</p>
</figure>

### Loading Data into SQL Server Database

To load into the database, we used a connection titled “Insert Row (V2)”. This action allows you to select a server, database, and specific table within a SQL server to add entries into via a connection. For each file type, the insert row step inserts entries into each respective database. Be sure to click “Add New Parameters” and match each parsed JSON object to the correct column name.
<br>
<figure>
  <p align="center">
    <kbd>
      <img src="https://github.com/nicholasgonzalez1/Data_Integration_FLD/blob/main/images/la_sql_insertion.png?raw=true" width="500"/>
    </kbd>
  </p>
</figure>

<!-- AZURE DATA FACTORY -->
## Azure Data Factory Pipeline
The basic structure of a project in Azure Data Factory involves securing connections to all sources and applications that the data must move between, followed by consolidating the data in a centralized storage container. From this storage area, the data can easily be transformed and analyzed using Azure’s various tools. For this project’s Azure Data Factory prototype, Azure Blob Storage is used as the centralized storage container, and Data Factory tools provide a dynamic movement of the data from Blob Storage to our SQL Database.

### Logic Apps Trigger
Due Azure Data Factory being limited to event based triggers, we determined that the best way to trigger the Data Factory pipeline would be through a file system trigger implemented in Azure Logic Apps. This was the same exact trigger used previously in the Logic Apps workflow; however, after we retrieve the contents of the newly added file, we then call upon the Data Factory pipeline using the connector displayed below. 
<br>
<figure>
  <p align="center">
    <kbd>
      <img src="https://github.com/nicholasgonzalez1/Data_Integration_FLD/blob/main/images/call_pipeline.png?raw=true" width="675"/>
    </kbd>
  </p>
  <p align="center">Logic Apps Connector: Create a pipeline run</p>
</figure>
<br>
As seen in the figure, two arguments are being passed into this activity - blobName and blobType. These parameters are used later in the Data Factory pipeline to locate the file within the Blob Storage (centralized storage container), which is further explained in the next section. 

### Utilizing Blob Storage

Blob storage on an online Azure storage account was chosen as the primary storage solution due to its optimal capabilities when storing unstructured data files. A single container was created which consisted of three separate folders, each defining the different data entities, as seen below. "Creating a blob" refers to the upload of a new file to the storage container.
<br>
<figure>
  <p align="center">
    <kbd>
      <img src="https://github.com/nicholasgonzalez1/Data_Integration_FLD/blob/main/images/blob_storage.png?raw=true" width="475"/>
    </kbd>
  </p>
  <p align="center">Azure Blob Storage: locations for different data file contents</p>
</figure>
While still in the Logic Apps portion of this workflow, information is recorded about the data file being processed and a new blob is created within the Azure storage account. The folder path within the container is dynamically specified so that file can be placed in its appropriate entity’s folder. The other fields, blob name and blob content, are also dynamically inputted using outputs from previous logic app actions. This is the last step before the Azure data factory pipeline is called.
<br><br>
<figure>
  <p align="center">
    <kbd>
      <img src="https://github.com/nicholasgonzalez1/Data_Integration_FLD/blob/main/images/create_blob.png?raw=true" width="700"/>
    </kbd>
  </p>
  <p align="center">Logic Apps Connector: Create blob</p>
</figure>
<br>
To reiterate, the purpose for utilizing blob storage is because we cannot simply pass the contents of a data file into the Data Factory environment; instead, we must upload the data files to a centralized storage account such as a blob storage so that we can reference it there later while in the Data Factory pipeline.

### Copying Data into SQL Server Database

The key benefit of using Data Factory was observed through it’s selection of activities to transform and move the data. In the pipeline, moving the data from the blob storage to the database table was done in a single step called a Copy Data activity. Within the Copy Data activity, we needed to specify exactly which file to transfer from the blob storage. This is done by referencing the parameters blobName and blobType that were passed in from the Logic Apps workflow portion; with these parameters, we're able to pinpoint the exact location where we stored the processed data file. 
<br><br>
<figure>
  <p align="center">
    <kbd>
      <img src="https://github.com/nicholasgonzalez1/Data_Integration_FLD/blob/main/images/copy_data.png?raw=true" width="600"/>
    </kbd>
  </p>
  <p align="center">Data Factory activity: Copy Data</p>
</figure>
<br>

### Email Notifications for Failed Activities

Another functionality that was added to the Data Factory pipeline was the ability to send email notifications to  administrators when particular errors occur. The pipeline included error alerts to be sent after attempting to transfer the file contents to the SQL database. When an activity fails, an HTTP POST is made thus triggering a separate Azure Logic Apps workflow, as seen below. Information about the particular failure in the data factory pipeline is listed in the body of the POST method, which is formatted as a JSON array.
<br>
<figure>
  <p align="center">
    <kbd>
      <img src="https://github.com/nicholasgonzalez1/Data_Integration_FLD/blob/main/images/data_factory_error.png?raw=true" width="600"/>
    </kbd>
  </p>
  <p align="center">Data Factory activity: HTTP Post describing failed activity</p>
</figure>
When the HTTP POST method is received, a simple two-step workflow in Logic Apps is triggered. The initial trigger action processes the HTTP body with a previously generated JSON schema. Afterwards, an email is sent to a specified address containing the information passed in from the HTTP POST method, where the email’s body is now formatted appropriately using a predefined template.
<br><br>
<figure>
  <p align="center">
    <kbd>
      <img src="https://github.com/nicholasgonzalez1/Data_Integration_FLD/blob/main/images/la_error_workflow.png?raw=true" width="600"/>
    </kbd>
  </p>
  <p align="center">Logic Apps Connector: Create blob</p>
</figure>
<br>

## Further Documentation

This readme file only highlights the main features of the Azure Data Factory pipeline and Azure Logic Apps workflow. Please refer to our project's [documentation](https://github.com/nicholasgonzalez1/Data_Integration_FLD/blob/main/Data_Integration_Documentation.pdf) for an extensive overview of how each workflow was constructed. This documentation contains detailed steps necessary to build these pipelines from scratch.

<!-- ACKNOWLEDGEMENTS -->
## Acknowledgements
Thank you to my other team members:
* [Jessica Silang](https://www.linkedin.com/in/jessica-silang/)
* [Natalie Danner](https://www.linkedin.com/in/natalie-danner-166a97165/)
