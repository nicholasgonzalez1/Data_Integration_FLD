# Data Integration for the Florida Lottery Department

<!-- TABLE OF CONTENTS -->
<!-- TABLE OF CONTENTS -->
<details open="open">
  <summary>Table of Contents</summary>
  <ol>
    <li><a href="#about-the-project">About the Project</a></li>
    <li><a href="#built-with">Built With</a></li>
    <li><a href="#getting-started">Getting Started</a></li>
    <li><a href="#azure-logic-apps">Azure Logic Apps</a>
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


<!-- ACKNOWLEDGEMENTS -->
## Acknowledgements
Thank you to my other team members:
* [Jessica Silang](https://www.linkedin.com/in/jessica-silang/)
* [Natalie Danner](https://www.linkedin.com/in/natalie-danner-166a97165/)
