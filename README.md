# Data Integration for the Florida Lottery Department

<!-- TABLE OF CONTENTS -->
<!-- TABLE OF CONTENTS -->
<details open="open">
  <summary>Table of Contents</summary>
  <ol>
    <li><a href="#about-the-project">About the Project</a></li>
    <li><a href="#built-with">Built With</a></li>
    <li><a href="#getting-started">Getting Started</a></li>
    <li><a href="#azure-logic-apps-pipeline">Azure Logic Apps Pipeline</a>
        <ul>
          <li><a href="#file-system-trigger">File System Trigger</a></li>
          <li><a href="#branching-logic">Branching Logic</a></li>
          <li><a href="#xml-to-json-conversion">XML to JSON conversion</a></li>
          <li><a href="#loading-entries-into-SQL-Server-database">Loading Data into SQL Server Database</a></li>
      </ul>
    </li>
    <li><a href="#azure-data-factory">Azure Data Factory</a>
        <ul>
          <li><a href="#logic-apps-trigger">Logic Apps Trigger</a></li>
          <li><a href="#blob-storage">Blob Storage</a></li>
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

With the Florida Lottery as the group’s sponsors, this project aimed to improve the current file ETL processes by supporting the transition from a legacy integration platform to a modern integration platform. In particular, the team analyzed the use of two Microsoft Azure Services - Azure Logic Apps and Azure Data Factory. The proposed solution is a proof-of-concept that each platform could perform the basic file ETL process functionality. This was achieved through research and implementation of a prototype on each platform. After following the cycle of researching, prototyping, and assessing, the team created two final prototypes that each retrieve a file from a local file system, transform the data, and insert the data to an SQL database table.
<br>
<kbd>
  <figure>
    <p align="center">
      <img src="https://github.com/nicholasgonzalez1/Data_Integration_FLD/blob/main/images/about_project.png?raw=true" width="500"/>
    </p>
  </figure>
</kbd>

<!-- BUILT WITH -->
## Built With

The following platforms were selected to build each pipeline:
* Azure Logic Apps
* Azure Data Factory

<!-- AZURE LOGIC APPS -->
## Azure Logic Apps Pipeline

<!-- FILE SYSTEM TRIGGER -->
### File System Trigger

In order to retrieve files that were located on a computer's local directory, a file system connector was configured into the workflow. Everytime a file was added to a specified folder in the local directory, the pipeline would trigger and retrieve the contents of the newly added files. This connector served as a gateway that allowed us to bridge the gap between on-premise devices and the configured Logic Apps workflow.
<br>
<kbd>
  <figure>
    <p align="center">
      <img src="https://github.com/nicholasgonzalez1/Data_Integration_FLD/blob/main/images/file_system_trigger.png?raw=true" width="700"/>
    </p>
    <p align="center">Logic Apps connector: “When a file is added or modified (properties only)"</p>
  </figure>
</kbd>

<!-- BRANCHING LOGIC -->
### Branching Logic

Once the contents are the file were retrieved, the type of file needed to be determined so that it could be further processed by a correct script. For the Logic Apps pipeline, all files being passed through were of XML type. However, the contents of the file were divided by 3 different types: Retailer, Player, and Game. Since actual company was restricted from being given to us, these file types were meant to mimic different classifications of data used at the Florida Lottery. The figure below shows the logic we used to determine the type of file being processed.
<br>
<kbd>
  <figure>
    <p align="center">
      <img src="https://github.com/nicholasgonzalez1/Data_Integration_FLD/blob/main/images/branching_logic.png?raw=true" width="500"/>
    </p>
    <p align="center">Logic for ensuring XML content is transformed using the right template</p>
  </figure>
</kbd>

<!-- XML TO JSON CONVERSION -->
### XML to JSON Conversion

Once the file type is determined, the XML content is transformed via mapping the content through a Liquid Template. In Azure, we utilized the Liquid connector that has an action of transforming XML content to JSON. Liquid maps were created using Visual Studio Code. These liquid files loop through the content of the XML file and grab the corresponding attributes for each record from the XML file. The figure liquid template file that was used to transform a file of type “player” to JSON.
<br>
<kbd>
  <figure>
    <p align="center">
      <img src="https://github.com/nicholasgonzalez1/Data_Integration_FLD/blob/main/images/xml_to_json.png?raw=true" width="500"/>
    </p>
    <p align="center">Logic Apps Connector: Transform XML to JSON</p>
  </figure>
</kbd>

<!-- LOADING DATA INTO SQL SERVER DATABASE -->
### Loading Data into SQL Server Database

To load into the database, we used a connection titled “Insert Row (V2)”. This action allows you to select a server, database, and specific table within a SQL server to add entries into via a connection. For each file type, the insert row step inserts entries into each respective database. Be sure to click “Add New Parameters” and match each parsed JSON object to the correct column name.
<br>
<kbd>
  <figure>
    <p align="center">
      <img src="https://github.com/nicholasgonzalez1/Data_Integration_FLD/blob/main/images/la_sql_insertion.png?raw=true" width="500"/>
    </p>
  </figure>
</kbd>


<!-- ACKNOWLEDGEMENTS -->
## Acknowledgements
Thank you to my other team members:
* [Jessica Silang](https://www.linkedin.com/in/jessica-silang/)
* [Natalie Danner](https://www.linkedin.com/in/natalie-danner-166a97165/)
