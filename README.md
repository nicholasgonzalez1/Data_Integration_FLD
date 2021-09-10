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

Our UF Senior Design Team was tasked with automating the Florida Lottery System’s current file storage process. The scope of this project included retrieving a file (Player, Retailer, or Game Focused) and having the creation of the file in a storage trigger a workflow to insert JSON objects into a SQL server. Our team explored two different methods to achieve this goal - Azure Logic Apps and Azure Data Factory. We noted the pros and cons of each platform and documented how to recreate these workflows.

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
<br><br>
<kbd>
  <figure>
    <p align="center">
      <img src="https://github.com/nicholasgonzalez1/Data_Integration_FLD/blob/main/images/file_system_trigger.png?raw=true" width="700"/>
    </p>
    <p align="center">
      <center>Logic Apps connector: “When a file is added or modified (properties only)"</center>
    </p>
  </figure>
</kbd><br><br>

### Branching Logic

Once the contents are the file were retrieved, the type of file needed to be determined so that it could be further processed by a correct script. For the Logic Apps pipeline, all files being passed through were of XML type. However, the contents of the file were divided by 3 different types: Retailer, Player, and Game. Since actual company was restricted from being given to us, these file types were meant to mimic different classifications of data used at the Florida Lottery. The image below shows the logic we used to determine the type of file being processed.

<br><br>
<kbd>
  <figure>
<img src="https://github.com/nicholasgonzalez1/Data_Integration_FLD/blob/main/images/branching_logic.png?raw=true" width="700">
  </figure>
</kbd><br><br>


<!-- ACKNOWLEDGEMENTS -->
## Acknowledgements
Thank you to my other team members:
* [Jessica Silang](https://www.linkedin.com/in/jessica-silang/)
* [Natalie Danner](https://www.linkedin.com/in/natalie-danner-166a97165/)
