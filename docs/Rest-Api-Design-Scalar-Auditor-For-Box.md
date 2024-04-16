REST API Design Document	2
1.	User Controller	2
1.1	/user/createUser	2
1.2	/user/getManagedUser	4
1.3	/user/deleteUser	5
1.4	/user/login	6
1.5	/user/updateUserRole	8
1.6	/user/editUser	9
1.7	/user/submitToken	10
1.8	/user/userSignIn	12
1.9	/user/getServiceToken	12
1.10	/user/getListOfExternalAuditors	13
1.11	/user/getOrgList	15
1.12	/user/sendResetPasswordOTP	15
1.13	/user/forgotPassword	17
2.	Item Controller	19
1.14	/item/getIntegratedItemDetails	19
3.	Folder Controller	21
1.15	/folder/getRootFolder	21
1.16	/folder/getItemList	22
4.	File Controller	23
1.17	/file/getFileCopies	24
1.18	/file/getFileVersion	25
1.19	/file/getFileDetails	26
1.20	/file/getFolderDetails	28
1.21	/file/addExternalAuditorEventLog	30
1.22	/file/getItemCollaborator	31
1.23	/file/getFileVersionForExternal	33
1.24	/file/checkTamperingStatus	34
5.	Event Log Controller	35
1.25	/eventLog/getEventsByDateRange	36
1.26	/eventLog/getEventsByDateRangeAndUser	37
1.27	/eventLog/getEventsByDateRangeAndEventType	39


# REST API Design Document

## 1. User Controller

### Overview

User Controller is a REST API Controller for user-related operations related to the 'Scalar Auditor for BOX' Application.

### REST APIs

#### 1.1 /user/createUser

**Description:**  
This POST API creates a user in the system and stores the user details.

**Input Parameters:**
- Name: Non-null, less than 64 characters, no special characters
- Email: Non-null, valid email with '@'
- Password: Non-null, minimum 8 characters
- Organization Name
- Role of User

**Response:**  
A new user will be created in the system.

**Exceptions:**
- Incorrect user Email for invalid user emails
- Invalid user name for invalid username
- Invalid password for invalid password
- User does not have the required role to create the user if logged in
- User is already exist if the user is already present
- Failed to create user for a general exception

**Operations Performed:**

1. Controller:
   Accepts Object: {name, email, password, organization, roleJson, imageUrl}

2. Business:
   Accepts Object: {name, email, password, organization, roleJson, imageUrl}
   - Start transaction
   - Call createUser method from UserService, submit transaction if no exception occurs

3. UserService:
   Accepts Object: {name, email, password, organization, roleJson, imageUrl}
   - Ensure no duplicate emails, throw exception if found
   - Encrypt password
   - Construct user object
   - Assign role to user
   - Create and store role-user object
   - Retrieve user object by email
   - Determine user's role
   - Proceed with user creation if role is owner
   - Validate user's email, name, and password
   - Set user's role list in JSON format
   - Create or update users in repository
   - Save user to repository
   - Save user's role to UserRoleRepository
   - Handle exceptions and return appropriate responses

#### 1.2 /user/getManagedUser

**Description:**  
This GET API retrieves details of users based on orgId.

**Input Parameters:**  
Token (currentUser) for fetching user list

**Response:**  
Fetches a list of users from the system.

**Response DTO:**
- Id: User id
- Name: Name of user
- Email: Email of user
- Password: Password
- RoleJson: Role of user (AUDIT_ADMIN, GENERAL_USER, EXTERNAL_AUDITOR)
- OrganizationName: Organization name
- ImageUrl: User image

**Exceptions:**
- User not found for user not present

**Operations Performed:**

1. Controller:
   Accepts Object: {orgId}

2. Business:
   Accepts Object: {orgId}
   - Start transaction
   - Call getManagedList method from UserService, submit transaction if no exception occurs

3. UserService:
   Accepts Params: {orgId}
   - Retrieve information about the user from the organization
   - Fetch a list of users using user repository or database
   - Use userInfo object or method to obtain the list of users
   - Iterate through the list of users to retrieve individual user information
   - Handle any potential errors or exceptions during retrieval
   - Utilize retrieved user information for further processing or analysis
