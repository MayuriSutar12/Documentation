#	Setup of ‘Scalar Auditor for BOX’ (User OAuth 2.0 Authentication)

Overview

*	Box App, named Scalar Auditor for BOX, operates under OAuth 2.0 with both User and Client Authentication. 
*	This authentication type provides developers with the capability to publish applications either to the App Center or integrate them into the Box Web App.
This app serves below purpose:
*	Its primary usage involves the creation of a Web App Integration, specifically designed as the Scalar File Auditing Tool. 
*	This integration utilizes the BOX API, using OAuth 2.0 authentication, to gather essential information related to users, files, or folders as required.

Steps to Create the App:

1.	Create a New App in Box Developer Console:

*	Navigate to the Box Developer Console.
*	Click on "Create New App".
*	Enter the name "Scalar Auditor for BOX", add a description, and select the purpose as "Integration".
*	App Category Selection:
*	External System Integration: File Auditing Tool by Scalar Inc.
	

2.	Authentication Method Selection

•	Choose "User Authentication (OAuth 2.0)".


3.	Configuration:

*	Go to the Configuration tab.
*	Configure OAuth 2.0 Redirect URI and CORS Domains.
*	Save Client ID and Client Secret provided under OAuth 2.0 credentials.
*	Application Scopes: Define necessary application scopes that are,
*	Write all files and store in box 
*	Manage users.
*	Manage groups.
*	Manage enterprise properties,



*	Enable "Make API calls using as-user header" in Advanced Features.

4.	Integrations Tab:

*	Click on "Create New Web App Integration".
*	Fill in the Integration Name and Description.
*	App Info Section:
*	Integration Name: Scalar Auditor for BOX
*	Description: This tool serves the purpose of detecting tampering within files and folders.
*	Supported File Extensions: Support all file extensions
*	Permissions Requirement: Download permissions are required.
*	Integration Scopes:
*	The file/folder from which this integration is invoked
*	Integration Type: Both

5.	Configure Callback Configuration settings.
●	Integration Status: Set the status to "Development".
Please include the four parameters as depicted in the snapshot below.


![image](https://github.com/MayuriSutar12/Documentation/assets/124232796/0e6b5806-5cbc-4060-912c-0510f3acfdae)

##	App Center Tab:

*	Add short and long descriptions.
*	Upload app screenshots and an app icon.