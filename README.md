# Documentation
Scalar_File_Documentation
# Overview
Scalar-Event-Log-Fetcher is a tool designed to integrate with the BOX application, facilitating file sharing among users for audit purposes. It provides functionality to view files assigned for audit.
# Application Components
Scalar-Box-Event-Log-Fetcher-App (App 1):
Service account managing access to user files/folders and actively listening to file events.
Service Account Email: AutomationUser_2140095_0hbK7pz8HB@boxdevedition.com
Manages folders/files shared with external auditors and controls access to user files/folders.

Scalar-Tampering-Detection-Tool (App 2):
Integrates with the Box web app for user interaction and adding files/folders to the audit set.
Allows users to view file copies, versions, and check file events.

Scalar DL:
Listens and stores event data into the asset table if files are in a monitored state.

##  Integration with Audit By Scalar (Summary)
•	Audit By Scalar Integration: Integrated with Audit By Scalar from Box, enabling users to monitor file events and access basic file details.
•	ScalarDL Integration: Integrated ScalarDL for secure data storage and verification purposes.
•	Box Event Listener: Implemented a listener to track file activities within the Box environment.
•	APIs for Audit Set and Collaboration: Developed APIs for managing audit sets and facilitating collaboration.
•	APIs for Audit Group and Auditor User Creation: Created APIs to manage audit groups and auditor users, enhancing organizational auditing capabilities.
•	Files/Folder Addition to Audit Set: Implemented functionality to add files and folders to audit sets seamlessly.
•	Sign-Up and Sign-In for Box Users: Enabled sign-up and sign-in functionalities for Box users to access auditing features.
•	Login for External Auditors: Developed login functionalities specifically tailored for external auditors, ensuring secure access to audit-related data.
•	File Validation: Implemented robust validation processes to maintain file integrity and security.
•	Audit Set Validation: Enhanced validation capabilities to ensure the integrity of files within audit sets, further bolstering auditing measures.


# Project Portal

Hosted at https://test7.jeeni.in/scalar-box, serves as the project portal.


# Functional Block of the Audit By Scalar

Authorization and User Role Management

•	Box Users can sign up.
•	Users with admin/co-admin roles in the Box enterprise account are assigned the Audit admin role.
•	Users with member roles in the Box enterprise account are assigned the GENERAL_USER role.
•	External auditor registration involves user creation, deletion, and role assignment.
•	Audit admins and external auditors use login credentials to access their dashboards.
Event Types Monitored
•	ITEM_CREATE
•	ITEM_UPLOAD
•	ITEM_MOVE
•	ITEM_COPY
•	ITEM_TRASH
•	ITEM_UNDELETE_VIA_TRASH
•	ITEM_RENAME
•	ITEM_MODIFY

# Audit Set Management


•	Audit sets are lists of files/folders shared with Scalar-Box-Event-Log-Fetcher-App.
•	Audit creation is performed by users with the Audit_admin role.
•	Co-owners (AUDIT_ADMIN ROLE) and members (GENERAL USER) can add files/folders to audit sets.
•	Audit Admins can set the allowed list of folders to be shared with external auditors.
•	External auditors can be assigned to audit and audit groups for auditing purposes.

# Audit Group Management


•	Audit admins can create audit groups and add external auditor users to them.
•	Groups can be assigned to audit sets.

# User Management

Audit Admins can create and delete users and assign roles from the dashboard.

# Validate Audit Set 
•	The Validate Audit Set functionality ensures the integrity of files within the audit set by validating them against the allowed list of files. It provides an overall status indicating the number of files checked, the total count of files in the audit set, and the number of files found to be tampered or defective, along with their names and paths.

# Event Log Viewing


•	Users can access a comprehensive log of events related to files that are actively being monitored.

•	Events may include actions such as creation, upload, move, copy, trash, undelete, rename, and modification of files.

# File Inspection:


•	Users have the ability to inspect copies and versions of files that are under monitoring.

•	This functionality allows users to track the history and evolution of a file over time.

