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


