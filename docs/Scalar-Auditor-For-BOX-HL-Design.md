### Scalar Auditor for BOX

#### 1. Overview
Cloud storage services like Box.com lack audit logs for external audits, and their retention periods may not align with external audit requirements. Scalar Auditor for BOX addresses these challenges by storing audit logs outside of cloud storage services, enabling external audits. It fetches and stores BOX event logs, records file operations, and facilitates creating audit sets for external auditors.

#### 2. Functional Blocks
- **BOX Event Log Fetching and Storing**: Fetches and stores user events from BOX, including file creation, upload, move, copy, delete, rename, and modification.
- **Query Box Events**: Allows users to query stored event log data based on date range, user, and event type.
- **User Role Management**: Administers roles for BOX app users, including Audit Admin and General User.
- **Audit Set Management**: Manages groups of files and folders selected for monitoring.
- **Auditor and Audit Group Management**: Creates and manages external auditor accounts and audit groups.
- **Audit Data Management and Display**: Provides file and folder details, event history, and validation functionality for auditing.
- **Validation Functionality**: Validates files and entire audit sets for tampering using ScalarDL.
- **Managing Addition of Items to Audit Set**: Adds files or folders to audit sets for monitoring.

#### 3. Use Cases
- Accessing Scalar Box Integration Application from BOX Integration Menu.
- Viewing file properties, versions, and copies.
- Adding files/folders to audit sets.
- Managing user roles, including external auditors.
- Creating, updating, and deleting audit sets.
- Signing up/login for organization users and external auditors.

#### 4. User Roles and Operations
- **Audit Admin**: Full access, including app usage, login, role management, audit set creation, and user/group management.
- **General User**: Limited access, including app usage, audit set management, and viewing file details.
- **External Auditor**: Limited access for auditing assigned files, viewing file details, and performing audit-related actions.

#### 5. User Functionality Flow
- Using BOX Integrations Menu for organization users.
- Using WEB UI for organization users and external auditors.

#### 6. Architecture
- Backend integrates with two BOX apps for event logging and other functionalities.
- Frontend uses React JS for the UI.

#### 7. Rest APIs Implemented
A comprehensive set of REST APIs facilitates various functionalities, including user management, audit set operations, event querying, and validation.

For detailed API documentation, refer to "Rest API Design_Scalar Auditor for BOX.docx".

[GitHub Documentation](github doc)