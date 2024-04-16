#	Setup of Scalar Box Event Log Fetcher App (Client Credentials Grant with Server Authentication)

## Overview

Operates through the Client Credentials Grant with Server Authentication Upon authorization, the app generates its user, referred to as the Service Account user, with the username AutomationUser_2140095_0hbK7pz8HB@boxdevedition.com.

This app serves a dual purpose:

*	Events Retrieval:
The primary function involves fetching user events from Box.
Additionally, the app is used when a user adds a file or folder to the audit set. In such cases, the app adds AutomationUser_2140095_0hbK7pz8HB@boxdevedition.com as a collaborator to the respective file or folder.

# Steps to Create a Server Authentication App (Client Credentials)

To create a Server Authentication App using Client Credentials in the Box Developer Console, follow these steps:

*	Create New App:

*	Navigate to the Box Developer Console.

*	Provide a name for your app, such as "Scalar-Box-Event-Log-Fetcher-App", and add a description.
*	Select the purpose as "Automation" and click on "Create App".

Configuration:

*	After creating the app, go to the configurations tab.

*	Scroll down to the "App Access Level" section and select "App + Enterprise Access".
*	Application Scopes:
Define the required application scopes:
*	Write all files and store in Box.
*	Manage users.
*	Manage groups.
*	Manage enterprise properties.
*	Enable integrations.


OAuth 2.0 Credentials:

*	Scroll down to the OAuth 2.0 credentials section.
*	Save the Client ID and Client secret in a secure location for later use.

In Advanced Features, enable "Make API calls using the as-user header".
Authorization Tab:
*	Go to the Authorization tab.
*	Click on "Review and Submit" to initiate the review request process.


# Approval Process for both the Apps after creation

To approve the review request for both the Scalar Box Event Log Fetcher App (Server Authentication) and the Scalar Auditor for BOX (User OAuth 2.0 Authentication), follow these steps:

*	Navigate to the Box Admin Console.
*	Go to the "Apps" tab and select "Custom Apps Manager".
*	In the Server Authentication tab, click on the "Add App" button.
*	Paste the Client ID of the Scalar Box Event Log Fetcher App (Server Authentication) and click "Next".
*	Check the application scopes and approve the app.
