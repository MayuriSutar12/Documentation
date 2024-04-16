# Overview
Audit by Scalar is an application that is integrated with the BOX application. The primary objective is to store user event logs on files managed by BOX outside the BOX application. These logs are then analyzed in different ways and this data is made available to the external auditors for performing audits of important files.
The necessary administrative functionalities required by the users are also implemented in this application.
The application is developed in Springboot framework for the Backend and React JS for front end. It uses Cassandra as a database and it is managed by ScalarDB. The file records are also managed by the application in ScalarDL so that file tampering can be detected using the file validation feature of ScalarDL.

## Features Implemented
1.	Store User Event Logs and display data in various formats     
	Store user event logs in the database       
	View the event logs after applying the filtering as below     
        •	Start date-time and End date-time     
        •	Type of user event      
        •	User who performed the events      
 
2.	View the file item details      
	View file details such as name, created by, created time and date, updated by, updated time and date, size of the file, and validation status     
	View files with the same hash      
	View all versions of the selected file     
 
3.	Manage organization user roles      
	Manage the organization users (BOX app users of the organization) having roles as Audit Admin and General User      

4.	Manage External users (Non-BOX users or users from external organizations performing Audits)     
	Create an external Auditor with name, mail ID, and organization name      
	Update external auditor details        
	Delete external auditor user         

5.	Assign files or folders for auditing to external auditors       
	Create Audit Sets which are groups of files and folders to be audited.        
	Select the file or folder (owned by the user or shared to the user) from BOX and add it to the Audit Set for Audit purpose      
	Partial selections of files and folders when assigned for audit to the external auditor          

6.	Manage the Audit sets             
	Create Audit sets           
	Assign collaborators to the Audit sets          
	Assign reviewers (external auditors) to the Audit sets          

7.	View logs of external auditor operations – for BOX users         
	View access logs of external auditors        

8.	Preview and download files for while auditing             

9.	Validate the file to check if it is tampered          

10.	Validate the Audit Set to check if any of the items in the Audit set are tampered.          

**Note**: More details are available in the document: Audit By Scalar_ HL Design Document
please refer below documents as per the topic metioned

* [BOX application types used](docs/box-application.md)    
* Installation and Setup Procedure[Setup of ‘Scalar Auditor for BOX’ (User OAuth 2.0 Authentication)](docs/Installation-and-Setup-Procedure.md)
* [Setup of Scalar Box Event Log Fetcher App-Client Credentials Grant with Server Authentication](docs/installation-and-setup-procedure2.md) 
     
* [Scalar DB Setup](docs/scalarDB.md)       
* [Scalar DL Setup](docs/scalarDL.md)      	
* [Configuration of the backend Spring Boot application for ScalarDL](docs/springbootBackedDL.md)          
* [SpringBoot Application Setup](docs/springboot-application-setup.md)	   
* Frontend application Setup
  There are two projects 
  1.[Scalar-WebApp-Integration-Menu](docs/Scalar-WebApp-Integration-Menu.md)     
  2.[Scalar-Box-WebApp](docs/Scalar-Box-WebApp.md) 
        
* [Features Planned for Release V2.0]()                      