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

