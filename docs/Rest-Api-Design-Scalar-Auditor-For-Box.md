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
1.28	/eventLog/getEventsByDateRangeAndFileId	41
1.29	/eventLog/getEventsByDateRangeAndUserAndItemId	43
1.30	/eventLog/getEventsByDateRangeAndEventTypeAndItemIdAndUserId	45
1.31	/eventLog/getEventsByDateRangeAndEventTypeAndItemId	46
1.32	/eventLog/getEventsByDateRangeAndEventTypeAndUser	48
6.	Audit Set Item Controller	50
1.33	/auditSetItem/addItemToAuditSet	50
1.34	/auditSetItem/viewItemsFromSelectedAuditSet	52
1.35	/auditSetItem/getAllowListFromAuditSet	54
1.36	/auditSetItem/getItemFromAuditSet	54
7.	Audit Set Controller	56
1.37	/auditSet/createAuditSet	56
1.38	/auditSet/deleteAuditSet	58
1.39	/auditSet/getMyAuditSetList	59
1.40	/auditSet/viewExternalAuditorAccessLog	61
1.41	/auditSet/updateAuditSetInfo	62
1.42	/auditSet/validateAuditSet	64
1.43	/auditSet/updateAuditSetsForItemId	65
1.44	/auditSet/getMyAuditSetListForItemId	66
8.	Audit Set Collaborator Controller	68
1.45	/auditSetCollaborator/changeAuditSetOwner	68
1.46	/auditSetCollaborator/getCollaboratorsForAuditSet	71
9.	Audit Group Controller	73
1.47	/auditGroup/createAuditGroup	73
1.48	/auditGroup/updateAuditGroup	75
1.49	/auditGroup/deleteAuditGroup	76
1.50	/auditGroup/getListOfAuditGroupMembers	78
 
REST API Design Document
1.	User Controller
Overview
User Controller is a REST API Controller for user-related operations related to ‘Scalar Auditor for BOX’ Application

Following are the REST APIs for the user-related operations The API documentation and execution approach is documented on Swagger here.
1.1	/user/createUser
The API createUser is a POST API that creates a user in the system and stores the user details. These details include:
-	Name of the user, 
-	Email id of the user, 
-	Password, 
-	Organization name,
-	Role of user
Input Parameters
Name of the User: Should be non-null or empty Size must be less than 64 chars, Also, it shall not contain any special characters
Email of the user:	Should be non-null, should be a valid email with @
Password:		Should be non-null, and should contain a minimum 8 characters
Response 
As a response, a new user will be created in the system.
Exceptions
Following would be the exception conditions

Exception	Error Message
For invalid user emails	Incorrect user Email
For invalid username	Invalid user name
For invalid password	Invalid password
If logged in, the user does not have the authority to create a user.	User does not have the required role to create the user
If the user is already present	User is already exist
If  a general exception	Failed to create user
Operations performed at each layer of the framework by this API are as below:
1.	Controller
In this REST API, Following Object is accepted - {name,email,password,organization,roleJson,imageUrl}

2.	Business
Following Object is accepted
{name,email,password,organization,roleJson,imageUrl}
●	Start transaction
●	Call the createUser method from UserService, and if no exception occurs, submit the transaction
●	
3.	UserService
Following Object is accepted –
{name,email,password,organization,roleJson,imageUrl}
●	Ensure there are no duplicate emails; throw an exception if one is found.
●	Use PasswordEncoder to encrypt the password.
●	Construct a user object from the provided data.
●	Assign a role to the user in RoleJson.
●	Create a role-user object and store it.
●	Retrieve the user object by email.
●	Determine the user's role in the security context.
●	Proceed with user creation if the role is owner.
●	Validate the user's email, name, and password.
●	Set the user's role list in JSON format.
●	Create or update users in the repository.
●	If roles include AUDIT_ADMIN and GENERAL_USER, set userId; otherwise, null.
●	Save the user to the repository.
●	Save the user's role to UserRoleRepository.
●	Handle exceptions and return appropriate responses.


1.2	/user/getManagedUser
The API getManagedUser is a GET API that accepts details of users based on orgId and retrieves data including userEmail, id, name, password, roleJson, organizationName, and imageUrl.
Input Parameters:
       Token(currentUser)  is important for fetching user list
Response 
As a response, to fetch a list of users from the system.
The Response DTO
Field name	Description
Id	User id 
Name	Name of user
Email	Email of user
Password	Password
roleJson	Role of user
(AUDIT_ADMIN,GENERAL_USER,EXTERNAL_AUDITOR)
organizationName	Organization name
imageUrl	User image

Exceptions
Following would be the exception conditions

Exception	Error Message
For user not present	User not found

Operations performed at each layer of the framework by this API are as below:
1.	Controller
      In this REST API, Following Object is accepted –{orgId}

2.	Business
     Following Object is accepted –{orgId} 
●	Start transaction
●	Call the getManagedList method from UserService, and if no exception occurs, submit the transaction

3.	UserService:
     Following params are accepted- {orgId}	
●	Retrieve information about the user from the organization
●	Utilize the user repository or database to fetch a list of users.
●	User the userInfo object or method to obtain the list of users.
●	Iterate through the list of users to retrieve individual user information.
●	Handle any potential errors or exceptions during the retrieval process.
●	Utilize the retrieved user information for further processing or analysis as needed.

1.3	 /user/deleteUser
The API deleteUser is a DELETE API. It takes user email as input and deletes the corresponding user from the system.
Input Parameters:
User email:Should check if the user is already deleted or not.
Response 
As a response,a user will be deleted from the system.
Exceptions
Following would be the exception conditions

Exception	Error Message
For user is not present	User not found
If the user does not have the authority to delete the user	User does not have the required role to delete the user
If general exception	An error occurred while deleting the user
Operations performed at each layer of the framework by this API are as below:
1.	Controller
      In this REST API, Following Object is accepted - {userEmail}

2.	Business
     Following Object is accepted - {userEmail}
●	Start transaction
●	Call deleteUser method from UserService and if no exception occurs submit transaction
●	
3.	UserService
      Following Object is accepted - {userEmail}
●	Construct a user object.
●	check if the  user is present or not.
●	Only AUDIT_ADMINs have permission to delete users from the system.
●	If a user is deleted, they will also be removed from the user_roles table, as well as from the audit_set, audit_set_collaborators, audit_group, and user_audit_group tables.
●	Get message is user deleted successfully.
1.4	/user/login
The API login is a Post API used for user authentication. It accepts details such as the user's email and password to authenticate the user.
Input Parameters:
User email: should be a valid input
password:  should not be empty
Response 
As a response, a user will be logged into the system.
Exceptions
Following would be the exception conditions

Exception	Error Message
For user is not present	User not found
If the user does not have the authority to delete the user	User does not have the required role to delete the user
If general exception	An error occurred while deleting the user
Operations performed at each layer of the framework by this API are as below:
1.	Controller
      In this REST API, Following Object is accepted - {userEmail,password}
2.	Business
      Following Object is accepted - {userEmail,password}
●	Start transaction
●	The login method initiates a distributed transaction and attempts to login.
●	It authenticates the user credentials, generates a JWT token, retrieves user details, and generates Box access tokens. Upon successful login, it commits the transaction and returns user data along with tokens.
●	If an error occurs during the transaction, it rolls back and throws a generic exception.
●	Call the login method from UserService, and if no exception occurs, submit the transaction

3.	UserService
      Following Object is accepted – {userEmail,password}
●	The generateToken method creates a JWT token for a given user, including additional claims. 
●	It utilizes the doGenerateToken method to construct the token with specified claims, subject, issuance, expiration, and signing algorithms.
●	The getByUserName method retrieves a user entity from the repository based on the provided username within the context of a distributed transaction.
 
1.5	 /user/updateUserRole
The API updateUserRole is a PUT API used to update the role of a user. It accepts details such as user email and a list of user roles to update the user's role.
Input Parameters:
userEmail:	should be non null
list of user roles
Response 
As a response, a new user role will be updated in the system.
Exceptions
Following would be the exception conditions

Exception	Error Message
For invalid userEmail	Incorrect userEmail
If the user does not have the right to update user roles	User don't have access to update role
Operations performed at each layer of the framework by this API are as below:
1.	Controller
       In this REST API, Following Object is accepted - {userEmail,list of roles}

2.	Business
      Following Object is accepted - {userEmail,list of roles}
●	Start transaction
●	Call the updateUserRole method from UserService, and if no exception occurs, submit the transaction

3.	UserService
Following Object is accepted - {userEmail,list of roles}
●	Construct a user object.
●	Retrieve the existing user based on the provided email from the userRepository.
●	Deserialize the stored JSON representation of user roles into a List of Strings.
●	Extract the current roles associated with the user.
●	Fetch the roles from the security token for authority verification.
●	Check if the user has access to update roles, specifically checking for AUDIT_ADMIN role.
●	Iterate through the new roles provided.
●	If a new role is not already present, create a RoleUser entry and add it to the database.
●	Update the list of added roles.
●	Iterate through the existing roles.
●	If an existing role is not in the new roles list, delete its corresponding RoleUser entry from the database.
●	Update the list of deleted roles.
●	Serialize the new roles list into JSON format.
●	Update the user's role JSON representation in the database with the new roles.
●	Create a new user and save it to the user repository.
●	Create a role for the specific user and save it to the UserRoleRepository.

1.6	 /user/editUser
The API editUser is a PUT API that accepts details of the user to be edited, including the user's name, email, and organization name.
Input Parameters:
Name of the User- should be non null or empty Size must be less than 64 chars Also it shall not contain any special characters
Email of user - Should be non null or empty
Response 
As a response,a new user will be created in the system.
Exceptions
Following would be the exception conditions

Exception	Error Message
For invalid userEmail	Incorrect userEmail
For invalid user name	Invalid user name
If  the user is already present	User is already exist
If a general exception	Failed to updated user
Operations performed at each layer of the framework by this API are as below:
1.	Controller
In this REST API, Following Object is accepted - {name,email,organization}
2.	Business
Following Object is accepted - {name,email,organization}
●	Start transaction
●	Call the createUser method from UserService, and if no exception occurs, submit the transaction

3.	UserService -
            Following Object is accepted – {name,email,organization}
●	Fetch the current user object based on the provided email within the transaction.
●	If the current user is not found, throw a generic exception.
●	Obtain the roles of the current user.
●	Retrieve the user to be edited using their previous email.
●	If the user to be edited is not found, throw a generic exception.
●	Check if the new email is different from the previous one and if it already exists for another user.
●	Retrieve the roles of the previous user from JSON.
●	If the current user has the authority to update user details,
●	Update user details like email, name, and organization.
●	Update the roles associated with the user.
●	Update collaborators, audit sets, and audit group memberships accordingly.
●	Update the auditor logs if the email is changed.
●	Return a success response if the user details are updated successfully; otherwise, return an appropriate error response.
●	Update the email of collaborators, including owners, co-owners, members, and reviewers, if necessary.
●	Parse JSON to retrieve the member list of an audit group.
●	Update the email of members within the audit group, if necessary.


1.7	 /user/submitToken
The API submitToken is a PUT API used for submitting user tokens. It accepts details such as accessToken, refreshToken, and expiryIn for the purpose of submitting the token.
Input Parameters:
accessToken
refreshToken
expiryIn
Response 
As a response, a user will be submitted token to the system.
Exceptions
Following would be the exception conditions

Exception	Error Message
For invalid userEmail	Incorrect userEmail
Operations performed at each layer of the framework by this API are as below:
1.	Controller
      In this REST API, Following Object is accepted
 -{  accessToken,refreshToken,expiryIn}
2.	Business
      Following Object is accepted - {accessToken,refreshToken,expiryIn}    
●	Start transaction
●	The submitToken method begins a distributed transaction and initializes a BoxAPIConnection with the provided access token. 
●	It retrieves user information from Box, attempts to register the user and save the token, then commits the transaction. 
●	If successful, it proceeds with user login using default credentials and returns the login response. 
●	If an error occurs during the transaction, it rolls back and throws a generic exception.
●	Call the submitToken method from UserService, and if no exception occurs, submit the transaction

3.UserService
 Following Object is accepted –{ accessToken,refreshToken,expiryIn }
●	The registerUserAndSaveToken method checks if the user is already registered; if not, it creates a new user entry in the database with default role(s) and organization details. 
●	Then, it saves the user's access token information, including expiration details. 
●	If the user already exists, it updates the access token information.
●	Finally, it returns a response, indicating the success of the operation.

1.8	 /user/userSignIn
The API userSignIn is a PUT API used for user sign-in.  It accepts details such as user email to log in to the system.
Input Parameters:
userEmail
Response 
As a response, a user will be signed in to the system.
Exceptions
Following would be the exception conditions

Exception	Error Message
For invalid userEmail	Incorrect userEmail
       Operations performed at each layer of the framework by this API are as below:
1.	Controller
      In this REST API, Following Object is accepted - {userEmail}
2.	Business
      Following Object is accepted - {userEmail}
         	Start transaction
Call userSignIn method from UserService and if no exception occurs submit transaction
3.	UserService
       Following Object is accepted - {userEmail}
●	Generate a token for a user based on their username and user details, which include their roles. 
●	This token is created with an expiration time and signed using a secret key. 
●	The token is then returned along with other relevant information, such as a refresh token, access token, and user details, in a response object. 
●	Additionally, a Box API connection is established to obtain a service account access token for further operations.

1.9	 /user/getServiceToken
The API getServiceToken is a GET API designed to retrieve a token by providing details of the user token.
Input Parameters:
Response 
As a response, a user will get a service token from the system.
Operations performed at each layer of the framework by this API are as below:
1.	Controller
      In this REST API, Following Object is accepted - {token }

2.	Business
      Following Object is accepted - {token}
●	Start transaction
●	   Call the getServiceToken method from UserService, and if no exception occurs, submit the transaction

3.	UserService
    Following Object is accepted – {token }
●	Get access and refresh tokens for a service account from Box, a cloud storage provider.
●	It starts by establishing a connection to Box's enterprise account using a utility method. Then, it sets the connection to act as itself.
●	After that, it constructs a response object with an HTTP status of OK, an empty message, and a status of true. 
●	The response includes data containing the access token, refresh token, and expiration time obtained from the Box connection.
1.10	/user/getListOfExternalAuditors 
The API getListOfExternalAuditors is a GET API that retrieves data including userEmail, id, name, password, roleJson, organizationName, and imageUrl.
Input Parameters:
User Token is important for fetching list of external auditors
Response 
As a response, a list of external auditors from the system
The Response DTO:userInfo

Field name	Description
id	User id
name	Name of user
email	Email of user
password	Password
organizationName	Organization name
imageUrl	User image
Exceptions
Following would be the exception conditions

Exception	Error Message
For external auditors, the list is empty.	User list is empty 
Operations performed at each layer of the framework by this API are as below:
1.	Controller
    In this REST API, Following Object is accepted - {User Token}

2. Business
     Following Object is accepted - {User Token}
●	Start transaction
●	Call getListOfExternalAuditors method from UserService and if no exception occurs submit transaction
3 . UserService:
      Following params are accepted- User Token	
●	Fetch a list of external auditors using the getByRole method from the role user repository.
●	Obtain the list of external auditors using the userInfo object.


1.11	/user/getOrgList
The API getOrgList is a GET API that accepts details of the user based on orgId and retrieves data including the user's email, ID, name, password, role JSON, organization name, and image URL.
Input Parameters:
organization id
Response 
As a response, a list of organizations will be fetched from the system.
The Response DTO:orgList

Field name	Description
orgId	Organization id 
organizationName	Name of organization
Operations performed at each layer of the framework by this API are as below:
1.	Controller
       In this REST API, Following Object is accepted - {User Token}
2.	Business
      Following Object is accepted - {User Token}
●	Start transaction
●	Call the getOrgList method from UserService, and if no exception occurs, submit the transaction

3 .UserService:
     Following params are accepted- User Token	
●	Fetch a list of organizations using the getOrganizationList method from the organization repository.
●	Obtain a list of organizations using the Organization object.

1.12	/user/sendResetPasswordOTP
The API sendResetPasswordOTP is a GET method. that accepts details such as the user's email and sends a request to generate and send the reset password OTP (one-time password).
      Input Parameters:
email: This parameter contains the email id of the user
Response 
As a response, an OTP is sent to existing users' email.
The Response DTO:userInfo

Field name	Description
userId	User Id 
emailId	Email Id of user
otpValidityTime	Validity time for OTP
OTP	OTP 
Exceptions
Following would be the exception conditions

Exception	Error Message
If you failed to send an email	Unable to send email
For invalid userEmail	Invalid EmailNo User found with this email
Operations performed at each layer of the framework by this API are as below:
1.	Controller 
       sendResetPasswordOTP
In this REST API, Following parameter is accepted - {useremail}
2. Business : 
    Following parameter is accepted -  {useremail}
●	Start transaction
●	Call sendResetPasswordOTP method from UserService and if no exception occurs submit transaction
3.UserService
Following Object is accepted - {userEmail}
●	Check if the given email is valid or not.
●	Generate OTP by calling GenericUtilitygenerateOtp().
●	Send OTP to email to reset password.
●	Save otp details in the user otp repository.
●	Send user ID, user email, validity time, and OTP in response.

1.13	 /user/forgotPassword
The API forgotPassword is a POST API used for facilitating password resets. It accepts details such as user email, OTP (one-time password), and new password to set a new password.
Input Parameters:
newpassword: It should not be null or empty It should contain a minimum of 8 characters 
Email: It should not be null or empty
OTP: It should not be null or empty
Response 
As a response, the password is updated form the user.
Exceptions
Following would be the exception conditions

Exception	Error Message
If user not present in the system	User Not Found !
If entered OTP is invalid	Invalid OTP
If Otp time is expired	OTP has expired
      Operations performed at each layer of the framework by this API are as below:
1.	Controller
       In this REST API, Following Object is accepted - {OTP,userEmail,newPassword}

2. Business
   Following Object is accepted - {OTP,userEmail,newPassword }
●	Start transaction
●	Call the forgotPassword method from UserService, and if no exception occurs, submit the transaction

3.UserService:
Following Object is accepted - {OTP,userEmail,newPassword }
●	Retrieve the user object using the provided email from the userRepository.
●	Check if the user exists or if they are marked as deleted; if so, throw a NotFoundException.
●	Retrieve the UserOtp object associated with the user's email from the userOptRepository.
●	Verify if the provided OTP matches the OTP stored in the database; if not, throw a GenericException.
●	Ensure that the OTP has not expired by comparing its expiry date with the current date; if expired, throw a GenericException.
●	Encrypt the new password provided in the updatePasswordDTO using the password encoder.
●	Update the user's password in the database with the newly encrypted password.

 
2.	Item Controller
Overview
Item Controller is REST API Controller for item related operations on box application   
Following are the REST API’s for the get integrated item detail related operations The API documentation and execution approach is documented on Swagger here
1.14	 /item/getIntegratedItemDetails
The API getIntegratedItemDetails is a GET API that accepts details of the item, including itemId, itemType, and userId, in order to retrieve integrated item details.
Input Parameters:
itemId
itemType
userId
Response 
As a response, fetch the information related to integrated items from the system.
The Response DTO:
Field name	Description
userEmail	User not found
Name	Name of user
jwtToken	Token 
List<String> userRoles	List of user roles(audit_admin,general_user,external_auditor)
refreshToken	Refresh token of user
accessToken	Access token of user
serviceAccAccessToken	Enterprise access token
objectItemDetails	   Item details object


Exceptions
Following would be the exception conditions

Exception	Error Message
For user not present	User not found
Operations performed at each layer of the framework by this API are as below:
                     
1.Controller
   		In this REST API, Following Object is accepted - {itemId,itemType,userId}

2.Business
   Following Object is accepted - {itemId,itemType,userId}
●	Start a distributed transaction
●	Retrieve user details and validate the user.
●	Update the user's latest token for authentication.
●	Generate a JWT token for the user.
●	Establish a connection with the Box API using the user's access token.
●	Fetch details of the item (file or folder) based on the provided ID and type.
●	Convert item details into DTO objects.
●	Commit the transaction and construct the integrated response, including item details and user information.
●	Return an API response with the integrated response.
●	Rollback the transaction in case of errors during the commit process.

 
3.	Folder Controller
Overview
Folder Controller is REST API Controller for folder related operations on box application   
Following are the REST API’s for the get root folder related operations The API documentation and execution approach is documented on Swagger here
1.15	/folder/getRootFolder
The API getRootFolder is a GET API that accepts details of the root folder, including the name of the audit set, description, ownedBy, and auditSetId, to retrieve the root folder.
Input Parameters:
       Token(currentUser)  is important for fetching audit setlist.
Response 
As a response, fetch the root folder from the system.
Exceptions
Following would be the exception conditions

Exception	Error Message
For user not present	User not found
Operations performed at each layer of the framework by this API are as below:
1.Controller
   In this REST API, Following Object is accepted 

2. Business
     Following Object is accepted 
●	Start transaction
●	Call the getListAuditSet method from AuditSetService, and if no exception occurs, submit the transaction
3.AuditSetService:
           Following params are accepted	
●	Retrieve information about the root folder from the box connection.
●	Get the folder ID using a box connection.

1.16	 /folder/getItemList
The API getItemList is a GET API that retrieves a list of items. It accepts details such as id, name, type, description, createdAt, modifiedAt, size, ownedBy, modifiedBy, and path.
Input Parameters:
           ItemId:-item id must not be null.
Response 
As a response, a fetch list of items from the system using a box connection
The Response DTO:FolderDetailsDto

Field name	Description
Id	FileId
Name	File name
description	File description
created time	Create time when file is created
modifiedAt	Modified time of file 
Size	Size of file
ownedBy	ownedBy is a file owner details :-user type,id,username,login
modifiedBy	Modified by is a user modified a file details:-user type,id,username,login
Path	Folder path from box api

Exceptions
Following would be the exception conditions

Exception	Error Message
For item not present	Item not found

Operations performed at each layer of the framework by this API are as below:
1.Controller
   In this REST API, Following Object is accepted - {itemId}

2.Business
    Following Object is accepted - {itemId}
●	Start transaction
●	Call the getItemList method from FileService, and if no exception occurs, submit the transaction
3.AuditSetService:
           Following params are accepted - {itemId}
●	Retrieve information about the folder using a box connection.
●	Fetch a list of items from the box application using the box connection.
●	Obtain the list of items using the itemDetailsList object.


















4.	File Controller
Overview
File Controller is REST API Controller for file related operations on Box Application   
Following are the REST API’s for the file related operations The API documentation and execution approach is documented on Swagger here

1.17	 /file/getFileCopies
The API getFileCopies is a GET API that accepts details of the file copies, including sha1Hash and itemId, and retrieves data such as sha1Hash, itemId, itemVersionId, itemVersionNumber, itemName, ownerByJson, path, and createdAt.
  
Input Parameters:
Item id
Sha1Hash
Response 
As a response, get file copies from the system.
The Response DTO:copyItems

Field name	Description
sha1Hash	Sha1 Hash id of the file
itemId	File id
itemVersionId	File version id
itemVersionNumber	File version number
itemName	File name
ownerByJson	Owner json(typeofUser,userid,Username,Userlogin)
Path	File path from folder
createdAt	File copy datei

Exceptions
Following would be the exception conditions

Exception	Error Message
If item ID is null	Item id not found

Operations performed at each layer of the framework by this API are as below:
1.Controller
    In this REST API, Following Object is accepted - {itemId,sha1Hash}

2. Business
     Following Object is accepted - {itemId, sha1Hash }

●	Start transaction
●	Call getFileCopies method from FileService and if no exception occurs submit transaction

3.FileService
   Following Object is accepted - {itemId, sha1Hash }
●	Verify that the item ID is not empty.
●	Obtain details about the file copies from the item by SHA1 table.
●	Get the file owned by objects
●	Get details of file copies from the item BySha1Repsitory
●	Obtain the CopyItems object as a response

1.18	/file/getFileVersion
The API getFileVersion is a GET API that accepts details of the file version, such as fileId and auditSetId, and retrieves data including uploaderName, sha1Hash, itemVersionId, itemVersionNumber, itemName, and modifiedAt.
Input Parameters:
fileId
auditSetId
Response 
As a response, get the file version using a box connection.

 The Response DTO:FileVersions

Field name	Description
uploaderName	Name of who upload the version
sha1Hash	Sha1 Hash id of the file
itemVersionId	File version id
itemVersionNumber	File version number
itemName	File name
modifiedAt	Modified date of file version
Operations performed at each layer of the framework by this API are as below:
1.Controller
    In this REST API, Following Object is accepted - {fileId,auditSetId}

  2.Business
     Following Object is accepted - {fileId,auditSetId}


●	Start transaction
●	Call the getFileVersion method from FileService, and if no exception occurs, submit the transaction

3.FileService
   Following Object is accepted - {fileId,auditSetId}
●	Verify that the item ID is not empty.
●	Obtain details about the file version using the box connection.
●	Get details of file versions from the Box application.
●	Get details in the fileVersions object.

1.19	/file/getFileDetails
The API getFileDetails is a GET API that accepts details of the file, such as itemId and auditSetId, and retrieves data including name, type, description, createdAt, modifiedAt, size, sha1, ownedBy object, modifiedBy object, tamperedStatus, and path.
Input Parameters:
Item id- should be non null
Audit Set Id - Optional
Response 
As a response, get file details from the box connection.
The Response DTO:FileDetailsDto

Field name	Error Message
Id	FileId
name	File name
description	File description
created time	Create time when file is created
modifiedAt	Modified time of file 
Size	Size of file
ownedBy	OwnedBy is a file owner details :-user type,id,username,login
modifiedBy	modifiedBy is a user modified a file details:-user type,id,username,login
temperedStatus	File is monitored or not-monitored
Path	File path from folder


Exceptions
Following would be the exception conditions

Exception	Error Message
If item ID is null	Item id not found


Operations performed at each layer of the framework by this API are as below:
1.Controller
    In this REST API, Following Object is accepted - {itemId,auditSetId}

2. Business
     Following Object is accepted - {itemId,auditSetId}

●	Start transaction
●	Call the getFileDetails method from FileService, and if no exception occurs, submit the transaction

3.FileService
   Following Object is accepted - {itemId,auditSetId}
●	Verify that the item ID is not empty.
●	Retrieve the token from the security context holder.
●	If the user has the roles AUDIT_ADMIN or GENERAL_USER, use the token; otherwise, connect to Box.
●	Obtain details about the file using the Box file object.
●	Specify the user making the action or the owner of the file in the ownedBy and modifiedBy objects.
●	Check the tampering status; if the item status is null, set it as non-monitored; otherwise, get the status from the database.
●	Create an object for auditor logs and save it to the AuditorLogRepository.

1.20	/file/getFolderDetails
The API getFolderDetails is a GET API that accepts details of the folder, such as folderId and auditSetId, and retrieves data including name, type, description, createdAt, modifiedAt, size, ownedBy object, modifiedBy object, and path.
Input Parameters:
folder id- should be non null
Audit Set Id - Optional
Response 
As a response, get folder details from the box connection.

The Response DTO:FolderDetailsDto

Field name	Error Message
Id	fileId
name	File name
description	file description
created time	Create time when file is created
modifiedAt	Modified time of file 
Size	Size of file
ownedBy	ownedBy is a file owner details :-user type,id,username,login
modifiedBy	modified by is a user modified a file details:-user type,id,username,login
Path	File path from folder


Exceptions
Following would be the exception conditions

Exception	Error Message
If folder ID is null	folder id not found

Operations performed at each layer of the framework by this API are as below:
1.Controller
    In this REST API, Following Object is accepted - {folderId,auditSetId}

2.Business
   Following Object is accepted - {folderId,auditSetId}
●	Start transaction
●	Call the getFolderDetails method from FileService, and if no exception occurs, submit the transaction

3.FileService
            Following Object is accepted - {folderId,auditSetId}
●	Confirm that the folder ID is not empty.
●	Retrieve the token from the security context holder.
●	If the user has the roles AUDIT_ADMIN or GENERAL_USER, use the token; otherwise, connect to Box.
●	Obtain the details of the file using the Box folder object.
●	Specify the user making the action or the owner of the folder in the ownedBy and modifiedBy objects.
●	Create an object for auditor logs and save it to the AuditorLogRepository.

1.21	/file/addExternalAuditorEventLog
The API addExternalAuditorEventLog is a POST API used to add event logs for files or folders. It accepts parameters such as auditSetId, itemId, actionType, timestamp, and itemType to log external auditor events.
Input Parameters:
auditSetId: Should not null or empty
itemId
actionType
timestamp

Response 
As a response, an event log will be created in the system.
Exceptions
Following would be the exception conditions

Exception	Error Message
For user not present	User not found
If audit set not present	Audit Set Not Found
Only external auditor action on the file or folder must be added	no need to add event log
If you have other exceptions	An error occurred while adding event logs
       Operations performed at each layer of the framework by this API are as below:
  1.Controller
      In this REST API, Following Object is accepted -  {auditSetId,itemId,actionType,timestamp}


2.Business
   Following Object is accepted - {auditSetId,itemId,actionType,timestamp}
●	Start transaction
●	Call the addExtAuditorEventLog method from FileService, and if no exception occurs, submit the transaction
3.FileService:
           Following params are accepted -  {auditSetId,itemId,actionType,timestamp}
●	Retrieve information about the current user using their email address.
●	Ensure that the user exists or not; raise an exception.
●	Verify if the audit set exists or not; raise a not-found exception.
●	Retrieve the user's role from the authentication token.
●	Create a UTC-formatted timestamp for the event.
●	If the user has the role of an external auditor, proceed with adding event logs; otherwise, return a forbidden response.
●	Build an auditor log object with relevant details, such as audit set ID, item ID, user email, event date, and custom JSON event details.
●	Determine the item type (file or folder) based on the provided information.
●	Depending on the action type (ITEM_DOWNLOAD,ITEM_PREVIEW, ITEM_VIEW), perform specific actions:
●	For item view, fetch file details.
●	For item preview, set the event type accordingly.
●	For item download, set the event type accordingly.
●	Save the auditor log to the AuditorLogRepository.

1.22	/file/getItemCollaborator
The API getItemCollaborator is a GET API that accepts details of the item collaborator, such as itemId and itemType, and retrieves information about the item's collaborators (userEmail, username, userId) as well as its owner (ownerName, ownerEmail, ownerId).

Input Parameters:
        temId and itemType  is important for fetching item collaborator list
Response 
As a response, a list of item collaborators will be fetched from the system.
The Response DTO:itemCollaborator

Field name	Description
itemCollaborator object	In this object we get item collaborator(userEmail,username,userId)
itemOwner object	In this object we get item owner(userEmail,username,userId)
      Exceptions
Following would be the exception conditions

Exception	Error Message
The user item is present or not present.	item not found

Operations performed at each layer of the framework by this API are as below:
1.Controller
   In this REST API, Following Object is accepted – {itemId,itemType}

2.Business
   Following Object is accepted - {itemId,itemType}
●	Start transaction
●	Call the getItemCollaborator method from FileService, and if no exception occurs, submit the transaction

3.FileService:
   Following params are accepted- {itemId,itemType}
●	Retrieve information about the item collaborator using the box connection.
●	Get a list of item collaborators using a box connection, and also get details about the owner of the file 
●	Obtain the list of item collaborator and owner details using the item Collaborator object.

1.23	/file/getFileVersionForExternal
The API getFIleVersionForExternal is a GET API that accepts details of the file version, such as uploaderName, sha1Hash, itemVersionId, itemVersionNumber, itemName, and modifiedAt.
Input Parameters:
fileId
Response 
As a response, get the file version using the box connection.
The Response DTO:fileVersions

Field name	Description
uploaderName	Name of who upload the version
sha1Hash	Sha1 Hash id of the file
itemVersionId	File version id
itemVersionNumber	File version number
itemName	File name
modifiedAt	Modified date of file version
      Operations performed at each layer of the framework by this API are as below:
1.Controller
In this REST API, Following Object is accepted - {fileId}

2.Business
            Following Object is accepted - {fileId}
●	Start transaction
●	Call the getFileVersionForExternal method from FileService, and if no exception occurs, submit the transaction

3.FileService
            Following Object is accepted - {fileId}
●	Verify that the item ID is not empty.
●	Obtain details about the file version using the box connection.
●	Get details of file versions from the Box application.
●	Get details in the fileVersions object.

1.24	/file/checkTamperingStatus
The API checkTamperingStatus is a GET API that accepts details of the file's tampering status, such as fileId, in order to check its tampering status.
Input Parameters:
fileId
Response 
As a response, check the file tampering status on the system.
 The Response DTO:

Field name	Description
tamperingStatus	Tampering status of file

Operations performed at each layer of the framework by this API are as below:
1.Controller
    In this REST API, Following Object is accepted - {fileId}

2.Business
   Following Object is accepted - {fileId}
●	Start transaction
●	This method checks the tampering status of a file. It retries the process up to three times in the event of failure. 
●	It starts a transaction, retrieves the item status, and then checks for tampering. 
●	If successful, it updates the status and commits the transaction
●	If any errors occur during the transaction, it rolls back and retries.
●	If retries are exceeded three times, it throws a generic exception.
3.FileService
            Following Object is accepted - {fileId}
●	The method checks if a file has been tampered with by comparing its details in Box with its details in Scalar DL. 
●	If the ledger status is "OK,"  it retrieves the file's information from Box and validates it against Scalar DL ledger entries. 
●	If the details match, it returns "NOT_TAMPERED"; if not monitored in Scalar DL, it returns "NOT_MONITORED"; otherwise, it returns "TAMPERED."



























5.	Event Log Controller
Overview
Event log Controller is REST API Controller for event log  related operations on box application.
Following are the REST API’s for the event log collaborators related operations The API documentation and execution approach is documented on Swagger here
1.25	/eventLog/getEventsByDateRange
The API getEventsByDateRange is a GET API that accepts details of events such as startDate and endDate, and retrieves a list of events including eventId, eventType, eventCreatedUserName, eventCreatedUserId, eventCreatedAt, itemId, and itemName.
Input Parameters:
startDate
endDate
Response 
As a response,a list of events will be fetched from the system.
The Response DTO:EventList
Field name	Description
eventId	Event id
eventType	Event type(item_create,item_upload,item_move,item_copy,item_trash,item_undelete_via_trash,item_rename,item_modify)
eventCreatedUserName	Name of event created by
eventCreatedUserId	User id of event created user
eventCreatedAt	Event created date
itemId	File id
itemName	File id

       Exceptions
Following would be the exception conditions

Exception	Error Message
The user item is present or not present.	Item not found
       Operations performed at each layer of the framework by this API are as below:
1.Controller
   In this REST API, Following Object is accepted – {startDate,endDate}

2.Business
   Following Object is accepted - {startDate,endDate}
●	Start transaction
●	Call the getEventsByDateRange method from eventService, and if no exception occurs, submit the transaction
3.eventService:
            Following params are accepted- {startDate,endDate}
●	This method retrieves events within a specified date range.
●	It first parses the input timestamps into the desired format (yyyy/MM/dd HH:mm:ss:SSS). 
●	Then, it extracts the start and end dates from the parsed timestamps. 
●	Next, it iterates through each date within the range and retrieves events for each date.
●	If there's only one day in the range, it fetches events for that day. For the first date, it fetches events until midnight. 
●	For the last date, it fetches events until the end of the day. For dates in between, it fetches events for the entire day. 
●	Finally, it returns a list of event details gathered during the process.

1.26	/eventLog/getEventsByDateRangeAndUser
The API getEventByDateRangeAndUser is a GET API that accepts details of events such as startDate and endDate, and retrieves a list of events including eventId, eventType, eventCreatedUserName, eventCreatedUserId, eventCreatedAt, itemId, and itemName.
Input Parameters:
startDate
endDate
userId
Response 
As a response, a list of events will be fetched from the system.
The Response DTO:EventList

Field name	Description
eventId	Event id
eventType	Event type(item_create,item_upload,item_move,item_copy,item_trash,item_undelete_via_trash,item_rename,item_modify)
eventCreatedUserName	Name of event created by
eventCreatedUserId	User id of event created user
eventCreatedAt	Event created date
itemId	File id
itemName	File id
      Exceptions
Following would be the exception conditions

Exception	Error Message
For user item is present or not present	Item not found
      Operations performed at each layer of the framework by this API are as below:
1.Controller
    In this REST API, Following Object is accepted – {startDate,endDate,userId}

2.Business
   Following Object is accepted - {startDate,endDate,userId}
●	Start transaction
●	Call the getEventsByDateRangeAndUser method from eventService, and if no exception occurs, submit the transaction


3.eventService:
   Following parameters are accepted: {startDate,endDate,userId}
●	This method retrieves events within a specified date range and user ID.
●	It first parses the input timestamps into the desired format (yyyy/MM/dd HH:mm:ss:SSS). 
●	Then, it extracts the start and end dates from the parsed timestamps for the user
●	Next, it iterates through each date within the range and retrieves events for each date.
●	If there's only one day in the range, it fetches events for that day. For the first date, it fetches events until midnight. 
●	For the last date, it fetches events until the end of the day. For dates in between, it fetches events for the entire day. 
●	Finally, it returns a list of event details gathered during the process.


1.27	/eventLog/getEventsByDateRangeAndEventType
The API getEventsByDateRangeAndEventType is a GET API that accepts details of events such as startDate and endDate, and retrieves a list of events including eventId, eventType, eventCreatedUserName, eventCreatedUserId, eventCreatedAt, itemId, and itemName.
Input Parameters:
startDate
endDate
eventType
Response 
As a response, a list of events will be fetched from the system.
The Response DTO:EventList:

Field name	Description
eventId	Event id
eventType	Event type(item_create,item_upload,item_move,item_copy,item_trash,item_undelete_via_trash,item_rename,item_modify)
eventCreatedUserName	Name of event created by
eventCreatedUserId	User id of event created user
eventCreatedAt	Event created date
itemId	File id
itemName	File id

Exceptions
Following would be the exception conditions

Exception	Error Message
For user item is present or not present	Item not found
       Operations performed at each layer of the framework by this API are as below:
1.Controller
   In this REST API, Following Object is accepted - {startDate,endDate,eventType}

                2.Business
   Following Object is accepted - {startDate,endDate,eventType}

●	Start transaction
●	Call the getEventsByDateRangeAndEventType method from eventService, and if no exception occurs, submit the transaction
3.eventService:
            Following params are accepted- {startDate,endDate,eventType}
●	This method retrieves events within a specified date range and event type.
●	It first parses the input timestamps into the desired format (yyyy/MM/dd HH:mm:ss:SSS). 
●	Then, it extracts the start and end dates from the parsed timestamps for eventType.
●	Next, it iterates through each date within the range and retrieves events for each date.
●	If there's only one day in the range, it fetches events for that day. For the first date, it fetches events until midnight. 
●	For the last date, it fetches events until the end of the day. For dates in between, it fetches events for the entire day. 
●	Finally, it returns a list of event details gathered during the process.

1.28	/eventLog/getEventsByDateRangeAndFileId
The API getEventsByDateRangeAndFileId is a GET API that accepts details of events such as startDate and endDate, and retrieves a list of events including eventId, eventType, eventCreatedUserName, eventCreatedUserId, eventCreatedAt, itemId, and itemName.
Input Parameters:
startDate
endDate
fileId
Response 
As a response, a list of events will be fetched from the system.
The Response DTO:EventList:

Field name	Description
eventId	Event id
eventType	Event type(item_create,item_upload,item_move,item_copy,item_trash,item_undelete_via_trash,item_rename,item_modify)
eventCreatedUserName	Name of event created by
eventCreatedUserId	User id of event created user
eventCreatedAt	Event created date
itemId	File id
itemName	File id


Exceptions
Following would be the exception conditions

Exception	Error Message
For user item is present or not present	Item not found
       Operations performed at each layer of the framework by this API are as below:
1.Controller
    In this REST API, Following Object is accepted - {startDate,endDate,fileId}

2.Business
   Following Object is accepted - {startDate,endDate,fileId}
●	Start transaction
●	Call the getEventsByDateRangeAndFileId method from eventService, and if no exception occurs, submit the transaction
3.eventService:
           Following params are accepted- {startDate,endDate,fileId}
●	This method retrieves events within a specified date range and file ID.
●	It first parses the input timestamps into the desired format (yyyy/MM/dd HH:mm:ss:SSS). 
●	Then, it extracts the start and end dates from the parsed timestamps for fileId.
●	Next, it iterates through each date within the range and retrieves events for each date.
●	If there's only one day in the range, it fetches events for that day. For the first date, it fetches events until midnight. 
●	For the last date, it fetches events until the end of the day. For dates in between, it fetches events for the entire day. 
●	Finally, it returns a list of event details gathered during the process.

1.29	/eventLog/getEventsByDateRangeAndUserAndItemId
The API getEventsByDateRangeAndUserAndItemId is a GET API that accepts details of events such as startDate and endDate, and retrieves a list of events including eventId, eventType, eventCreatedUserName, eventCreatedUserId, eventCreatedAt, itemId, and itemName.  
Input Parameters:
startDate
endDate
itemId
Response 
As a response, a list of events will be fetched from the system.
The Response DTO:EventList:

Field name	Description
eventId	Event id
eventType	Event type(item_create,item_upload,item_move,item_copy,item_trash,item_undelete_via_trash,item_rename,item_modify)
eventCreatedUserName	Name of event created by
eventCreatedUserId	User id of event created user
eventCreatedAt	Event created date
itemId	File id
itemName	File id


Exceptions
Following would be the exception conditions

Exception	Error Message
For user item is present or not present	Item not found
       Operations performed at each layer of the framework by this API are as below:
1.Controller
    In this REST API, Following Object is accepted – {startDate,endDate, userId,itemId }

2.Business
   Following Object is accepted - {startDate,endDate, userId,itemId}

●	Start transaction
●	Call the getEventsByDateRangeAndUserAndItemId method from eventService, and if no exception occurs, submit the transaction.

     3.eventService:
            Following params are accepted- {startDate,endDate,userId,itemId}
●	This method retrieves events within a specified date range (userId, itemId).
●	It first parses the input timestamps into the desired format (yyyy/MM/dd HH:mm:ss:SSS). 
●	Then, it extracts the start and end dates from the parsed timestamps for userId and itemId.
●	Next, it iterates through each date within the range and retrieves events for each date.
●	If there's only one day in the range, it fetches events for that day. For the first date, it fetches events until midnight. 
●	For the last date, it fetches events until the end of the day. For dates in between, it fetches events for the entire day. 
●	Finally, it returns a list of event details gathered during the process.

1.30	/eventLog/getEventsByDateRangeAndEventTypeAndItemIdAndUserId
The API getEventsByDateRangeAndEventTypeAndItemIdAndUserId is a GET APIthat accepts details of events such as startDate and endDate, and retrieves a list of events including eventId, eventType, eventCreatedUserName, eventCreatedUserId, eventCreatedAt, itemId, and itemName.
Input Parameters:
startDate
endDate
eventType
itemId
userId
Response 
As a response, a list of events will be fetched from the system.
The Response DTO:EventList:

Field name	Description
eventId	Event id
eventType	Event type(item_create,item_upload,item_move,item_copy,item_trash,item_undelete_via_trash,item_rename,item_modify)
eventCreatedUserName	Name of event created by
eventCreatedUserId	User id of event created user
eventCreatedAt	Event created date
itemId	File id
itemName	File id

Exceptions
Following would be the exception conditions

Exception	Error Message
For user item is present or not present	item not found
        Operations performed at each layer of the framework by this API are as below:
1.Controller
    In this REST API, Following Object is accepted - {startDate,endDate, userId,itemId, eventType }

2.Business
   Following Object is accepted - {startDate,endDate, userId,itemId,eventType}

●	Start transaction
●	Call the getEventsByDateRangeAndEventTypeAndItemIdAndUserId method from eventService, and if no exception occurs, submit the transaction.

      3.eventService:
   Following params are accepted- {startDate,endDate,userId,itemId, eventType }
●	This method retrieves events within a specified date range, user ID, item ID,eventType
●	It first parses the input timestamps into the desired format (yyyy/MM/dd HH:mm:ss:SSS). 
●	Then, it extracts the start and end dates from the parsed timestamps for userId, itemId, and eventType.
●	Next, it iterates through each date within the range and retrieves events for each date.
●	If there's only one day in the range, it fetches events for that day. For the first date, it fetches events until midnight. 
●	For the last date, it fetches events until the end of the day. For dates in between, it fetches events for the entire day. 
●	Finally, it returns a list of event details gathered during the process.

1.31	 /eventLog/getEventsByDateRangeAndEventTypeAndItemId
The API getEventsByDateRangeAndEventTypeAndItemId is a GET API that accepts details of events such as startDate and endDate, and retrieves a list of events including eventId, eventType, eventCreatedUserName, eventCreatedUserId, eventCreatedAt, itemId, and itemName.
Input Parameters:
startDate
endDate
eventType
itemId
Response 
As a response, a list of events will be fetched from the system.
The Response DTO:EventList:

Field name	Description
eventId	Event id
eventType	Event type(item_create,item_upload,item_move,item_copy,item_trash,item_undelete_via_trash,item_rename,item_modify)
eventCreatedUserName	Name of event created by
eventCreatedUserId	User id of event created user
eventCreatedAt	Event created date
itemId	File id
itemName	File id

Exceptions
Following would be the exception conditions

Exception	Error Message
For user item is present or not present	item not found
Operations performed at each layer of the framework by this API are as below:
1.Controller
   In this REST API, Following Object is accepted - {startDate,endDate,itemId, eventType}

2.Business
   Following Object is accepted - {startDate,endDate,itemId,eventType}

●	Start transaction
●	Call the getEventsByDateRangeAndEventTypeAndItemId method from eventService, and if no exception occurs, submit the transaction

3.eventService:
           Following params are accepted- {startDate,endDate,itemId, eventType }
●	 This method retrieves events within a specified date range,itemId,eventType
●	It first parses the input timestamps into the desired format (yyyy/MM/dd HH:mm:ss:SSS) 
●	Then, it extracts the start and end dates from the parsed timestamps for itemId,eventType
●	Next, it iterates through each date within the range and retrieves events for each date
●	If there's only one day in the range, it fetches events for that day For the first date, it fetches events until midnight 
●	For the last date, it fetches events until the end of the day For dates in between, it fetches events for the entire day 
●	Finally, it returns a list of event details gathered during the process


1.32	/eventLog/getEventsByDateRangeAndEventTypeAndUser
The API getEventsByDateRangeAndEventTypeAndUser a GET API that accepts details of events such as startDate and endDate, and retrieves a list of events including eventId, eventType, eventCreatedUserName, eventCreatedUserId, eventCreatedAt, itemId, and itemName.
Input Parameters:
startDate
endDate
eventType
userId
Response 
As a response, a list of events will be fetched from the system.
The Response DTO:EventList:


Field name	Description
eventId	Event id
eventType	Event type(item_create,item_upload,item_move,item_copy,item_trash,item_undelete_via_trash,item_rename,item_modify)
eventCreatedUserName	Name of event created by
eventCreatedUserId	User id of event created user
eventCreatedAt	Event created date
itemId	File id
itemName	File id



Exceptions
Following would be the exception conditions

Exception	Error Message
For user item is present or not present	Item not found

Operations performed at each layer of the framework by this API are as below:
1.Controller
   In this REST API, Following Object is accepted -   {startDate,endDate,userId,eventType }

2.Business
   Following Object is accepted - {startDate,endDate,userId,eventType }
●	Start transaction
●	Call the getEventsByDateRangeAndEventTypeAndItemId method from eventService, and if no exception occurs, submit the transaction

3.eventService:
            Following params are accepted - {startDate,endDate, userId, eventType }
●	This method retrieves events within a specified date range, user ID, and event type.
●	It first parses the input timestamps into the desired format (yyyy/MM/dd HH:mm:ss:SSS). 
●	Then, it extracts the start and end dates from the parsed timestamps for userId, itemId, and eventType.
●	Next, it iterates through each date within the range and retrieves events for each date.
●	If there's only one day in the range, it fetches events for that day. For the first date, it fetches events until midnight. 
●	For the last date, it fetches events until the end of the day. For dates in between, it fetches events for the entire day. 
●	Finally, it returns a list of event details gathered during the process.

6.	Audit Set Item Controller
Overview
Audit Set  Item Controller is REST API Controller for audit set item related operations on box application   
Following are the REST API’s for the audit set item related operations The API documentation and execution approach is documented on Swagger here

1.33	/auditSetItem/addItemToAuditSet
The API addItemToAuditSet is a POST API used to add an item to an audit set. It accepts details such as auditSetId, itemId, itemType, itemName, accessListType, and a list of items to be added to the audit set.
Input Parameters:
auditSetId
itemId
itemType
itemName
accessListType
list of items(id,type)
Response 
As a response, an item will be added to the audit set in the system.
Exceptions
Following would be the exception conditions

Exception	Error Message
If audit set is not present	Audit set not found
 if a user is not a member of the audit set	User is not the member of auditSet to add an item
If the user does not have authority to create audit set	User does not have the required role to create the audit set
Operations performed at each layer of the framework by this API are as below:
1.Controller
    In this REST API, Following Object is accepted - {    auditSetId,itemId,itemType,itemName, accessListType,list of items
List(userEmail,userRole),list of groupIds}

2.Business
    Following Object is accepted - { auditSetId, itemId,itemType,itemName,    accessListType,list of items}
●	Start transaction
●	Call the createAuditSet method from AuditSetService, and if no exception occurs, submit the transaction

3.AuditSetServie-
   Following params are accepted - {auditSetId, itemId,itemType,itemName, accessListType,list of items}
●	This method adds an item to an audit set.
●	First, it retrieves the audit set based on the provided ID. If the audit set doesn't exist, it throws an exception.
●	Then, it retrieves the user information based on the current user's email within the transaction context.
●	Next, it parses the access control list (ACL) JSON from the audit set. If parsing fails, it throws an exception.
●	The method checks if the current user is a member of the audit set by examining the ACL. If not, it throws an exception.
●	After that, it checks if the item is already present in the audit set. If not, it creates a new audit set item. For folders, it retrieves modification information from Box, constructs an audit set item, and adds it to the repository. For files, it directly constructs and adds the item.
●	If the item is already present, it updates the access control list and adds it to the repository.
●	For files, it also ensures that their status is set to "monitored" if not already done, updating the item status repository accordingly.
●	Finally, it returns a successful ApiResponse with the appropriate message and HTTP status, along with the added or updated items.




1.34	/auditSetItem/viewItemsFromSelectedAuditSet
The API viewItemFromSelectedAuditSet is a GET API that accepts details of the item from an audit set based on auditSetId and retrieves data including itemId, itemName, itemType, isAllowed, auditSetRootItemId, createdAt, modifiedAt, createdBy, modifiedBy, size, createdByEmail, and modifiedByEmail.
Input Parameters:
        AuditSetId:Should not be null or empty
Response 
If all the Input Parameters met properly, information about the audit set will be fetched from the system.

 The Response DTO:AuditSetItemVisibility

Field name	Description
itemId	File id
itemName	File name
itemType	Type of item(file,folder)
isAllowed	Only allowed item fro audit set
auditSetRootItemId	Audit set root item id
createdAt	Item created date
modifiedAt	Item modified date
createdBy	Name of item created by user
modifiedBy	Name of item modified by user
Size	Name of item modified by user
createdByEmail	Email id of created by 
modifiedByEmail	Email id of modified by

Operations performed at each layer of the framework by this API are as below:
1.Controller
   In this REST API, Following Object is accepted - {auditSetId }

2.Business
   Following Object is accepted - {auditSetId}
●	Start transaction
●	      Call the viewItemsFromSelectedAuditSet method from AuditSetItemService, and if no exception occurs, submit the transaction

                     3.AuditSetItemService:
  Following params are accepted- {auditSetId}	
●	The provided code retrieves audit set items and their details, including files and folders, within a specified audit set It then iterates through each item,fetching information such as name, type, creation/modification dates, creators, and sizes
●	Additionally, it updates the access status for audit set collaborators to "UNDER_REVIEW"
●	If an error occurs during processing, it logs the error and continues to the next item 
●	Finally, it returns an API response containing the fetched audit set items' details or a message indicating an empty item list


1.35	/auditSetItem/getAllowListFromAuditSet
The API getAllowListFromAuditSet is a GET API that retrieves details of the allow list item from an audit set based on auditSetId and itemId. It returns the allow list with itemId and itemType.
Input Parameters:
       AuditSetId:Should not be null or empty
Response 
As a response, get an approved list of audit sets from the system.
Operations performed at each layer of the framework by this API are as below:
1.Controller
    In this REST API, Following Object is accepted - {auditSetId,itemId}

2.Business
   Following Object is accepted - {auditSetId,itemid }
●	Start transaction
●	Call getAllowListFromAuditSetmethod from AuditSetItemService, and if no exception occurs, submit the transaction
3.AuditSetItemService:
 Following params are accepted - {auditSetId,itemid }	
●	retrieves all audit set items associated with a specific user within a distributed transaction context.
●	It first fetches the user details based on the email obtained from the security context within the provided transaction.
●	Then, it retrieves all audit set items related to this user from the repository within the transaction.

1.36	/auditSetItem/getItemFromAuditSet
The API getItemFromAuditSet is a GET API that accepts details of the item from the audit set based on auditSetId, itemId, and subfolderId. It retrieves data including itemId, itemName, itemType, isAllowed, auditSetRootItemId, createdAt, modifiedAt, createdBy, modifiedBy, size, createdByEmail, and modifiedByEmail.
Input Parameters:
auditSetId
itemId
subFolderId
Response 
As a response, fetch the item information from the audit set. 

 The Response DTO:AuditSetItemVisibility

Field name	Description
itemId	File id
itemName	File name
itemType	Type of item(file,folder)
isAllowed	Only allowed item fro audit set
auditSetRootItemId	Audit set root item id
createdAt	Item created date
modifiedAt	Item modified date
createdBy	Name of item created by user
modifiedBy	Name of item modified by user
Size	Name of item modified by user
createdByEmail	Email id of created by 
modifiedByEmail	Email id of modified by

       Operations performed at each layer of the framework by this API are as below:
1.Controller
   In this REST API, Following Object is accepted - {auditSetId,itemId,subfolderId }

2.Business
   Following Object is accepted - {auditSetId,itemid,subfolderId }
●	Start transaction
●	Call getItemFromAuditSet from AuditSetItemService, and if no exception occurs, submit the transaction

3.AuditSetItemService:
Following params are accepted - {auditSetId,itemid,subFolderId}	
●	This method retrieves item information from an Audit Set It first checks if the item exists in the Audit Set 
●	If not, it returns an empty response Then, it retrieves basic item information from JSON stored in the Audit Set
●	It iterates through items in a specified folder, checking if each item is allowed or denied based on the Audit Set
●	It updates lists of denied and allowed items accordingly If all items are allowed, it updates the Audit Set with the new allowed items 
●	Finally, it returns the updated list of item visibilities
●	
7.	Audit Set Controller
Overview
Audit Set Controller is REST API Controller for audit set related operations on box application 
Following are the REST API’s for the audit set related operations The API documentation and execution approach is documented on Swagger here

1.37	/auditSet/createAuditSet
The API createAuditSet is a POST API that accepts details of the audit set, including the name of the audit set, description, list of users (userEmail, userRole), and list of groupIds, to create an audit set.
Input Parameters:
Name of the Audit set- should be non null or empty Size must be less than 64 chars 
Description of audit set –description is optional 
User List(userEmail,userRole)
List of groupIds
Response 
As a response,a new audit set will be created in the system.
Exceptions
Following would be the exception conditions

Exception	Error Message
For user not present	User not found
If the audit set is empty or null,	Audit set name should not be empty
For duplicate name	Please use another audit set name as there exists a audit set with the same name
If the  user does not have authority to create an audit set	User does not have the required role to create the audit set
If having other error	An error occurred while creating the audit set
       Operations performed at each layer of the framework by this API are as below:
1.Controller
    In this REST API, Following Object is accepted - {name,description,user               List(userEmail,userRole),list of groupIds}

2.Business
   Following Object is accepted - {name,description,user List(userEmail,userRole),list of groupIds}
●	Start transaction
●	 Call the createAuditSet method from AuditSetService, and if no exception occurs, submit the transaction

                       3.AuditSetServie-
    Following params are accepted - {name,description,user List(userEmail,userRole),list of groupIds }
●	creates a new audit set based on input data, ensuring the uniqueness of the audit name, user roles, and permissions. It retrieves the user by email, formats the date, and checks for duplicate names. 
●	If the user has the necessary role, it constructs the audit set object with collaborators, validates and adds groups, and finally creates the audit set. 
●	The method returns an API response indicating success or failure.
●	The addGroupToAuditSet method adds an audit group to an audit set, ensuring both exist, and then creates a mapping entry. 
●	It updates the list of audit groups for the audit set and returns the updated list.
●	The updateAclJson method updates the access control list (ACL) JSON with the user's role in the audit set collaborator object. 
●	It calls appendUserToList to append the user with the specified role to the collaborator list, ensuring uniqueness and role-based permissions.

1.38	/auditSet/deleteAuditSet
The API deleteAuditSet is a DELETE API used to delete an audit set based on the provided auditSetId.
Input Parameters:
Audit set id- should be check audit set id is present or not 
Response 
If all the Input Parameters were met properly, an audit set would be deleted successfully.
Exceptions
Following would be the exception conditions

Exception	Error Message
For check audit set is present or not	Audit set not found
For user not present	User not found
If the user does not have authority to delete an audit set	User does not have the required role to delete the audit set
If having other error	An error occurred while deleting the audit set
       Operations performed at each layer of the framework by this API are as below:
1.Controller
   In this REST API, Following Object is accepted - {auditSetId}

2.Business
   Following Object is accepted - {auditSetId }

●	Start transaction
●	Call the deleteAuditSet method from AuditSetService, and if no exception occurs, submit the transaction

3.AuditSetService:
    Following params are accepted - {auditSetId }	
●	Retrieve the user and audit set objects.
●	Check if they exist; if not, throw a NotFoundException.
●	Verify if the audit set is already deleted; if so, return ApiResponse.
●	Deserialize ACL JSON into a Collaborator object.
●	Fetch user roles from the security token.
●	Determine if the user is the owner or co-owner.
●	If authorized, delete the audit set and associated collaborators.
●	Clear audit group mappings and update the audit set status.
●	Return a response indicating success or failure.


1.39	/auditSet/getMyAuditSetList
The API getMyAuditSetList is a GET API that accepts details of the audit set, including auditSetId, auditSetName, description, ownedBy, createdAt, accessStatus, and isFavourite.
Input Parameters:
Current user login
Response 
As a response,the user will get a list of audit sets from the system.

 The Response DTO:AuditSetList

Field name	Description
Id	AuditSetId
name	Audit set name
description	Audit set description description
ownedBy	Owner email of audit set
createdAt	Date when audit set is created
accessStatus	Access status of file (UNDER_REVIEW,NEWLY_ADDED)
isFavourite	To be implemented


Exceptions
Following would be the exception conditions

Exception	Error Message
For user not present	User not found
If having other errors	Error when get my audit setlist
       Operations performed at each layer of the framework by this API are as below:
1. Controller
    In this REST API, Following Object is accepted - {Token(currentUser)}

2.Business
   Following Object is accepted - {Token(currentUser)}
●	Start transaction
●	 Call the getMyAuditSetList method from AuditSetService, and if no exception occurs, submit the transaction

3.AuditSetService:
    Following params are accepted - {Token(currentUser)}
●	Retrieve the user object using the provided email from the userRepository.
●	Check if the user exists; if not, throw a NotFoundException.
●	Based on the user's role, fetch audit setlists:
●	For AUDIT_ADMIN role: Retrieve all audit sets.
●	For GENERAL_USER role: Retrieve audit sets where the user is a collaborator.
●	For EXTERNAL_AUDITOR role: Retrieve audit sets associated with user groups.
●	Construct AuditSetLists objects containing relevant information for each audit set.
●	Return ApiResponse with the constructed audit set lists and appropriate status.

1.40	/auditSet/viewExternalAuditorAccessLog
The API viewExternalAuditorAccessLog is a GET API that accepts details of the external auditor event log, such as auditSetId, itemId, and userEmail, and retrieves data including ownerName, eventType, itemType, and eventDate.
Input Parameters:
auditSetId
itemId
userEmail
Response 
As a response,get a list of external auditor logs from the system.

 The Response DTO:ExtAuditorAccessLog

Field name	Description
ownerName	Owner name of file id
eventType	Event types(item_create,item_upload,item_move,item_copy,item_trash,item_undelete_via_trash,item_rename,item_modify)
itemType	Item type(file,folder)
ownedBy	Owner email id
eventDate	Date when event is created

Exceptions
Following would be the exception conditions

Exception	Error Message
If having other errors	Error when getting external auditor access
      Operations performed at each layer of the framework by this API are as below:
1.Controller
In this REST API, Following Object is accepted - {auditSetId,itemId,userEmail}

2.Business
Following Object is accepted - {auditSetId,itemId,userEmail}

●	Start transaction
●	Call the getMyAuditSetList method from AuditSetService, and if no exception occurs, submit the transaction

3.AuditSetService:
    Following params are accepted - {auditSetId,itemId,userEmail}

●	Fetch information about the external auditor event log
●	Check the event log of an external auditor in the database
●	 Retrieve the event logs for a specific item in an audit set for a particular user
●	If the list of external auditor logs is not empty, retrieve the list successfully
●	Retrieve ExtAuditorAccessLog list
●	If the list is empty, return an empty list


1.41	/auditSet/updateAuditSetInfo
The API updateAuditSetInfo is a Put API used to update audit set information, including the name of the audit set, description, list of users (userEmail, userRole), and list of groupIds.
Input Parameters:
Name of the Audit set- should be non null or empty Size must be less than 64 chars 
Description of audit set –description is optional
User List(userEmail,userRole)
List of groupIds
Response 
As a response,an audit set will be updated in the system.

Exceptions
Following would be the exception conditions

Exception	Error Message
For user not present	User not found
If audit set is empty or null	Audit set name should not be empty
For duplicate name	Please use another audit set name as there exists a audit set with the same name
If the user does not have authority to create an audit set	User does not have the required role to create the audit set
If having other error	An error occurred while creating the audit set
       Operations performed at each layer of the framework by this API are as  below:
1.Controller
    In this REST API, Following Object is accepted - {name,description,user                            List(userEmail,userRole),list of groupIds}

2.Business
   Following Object is accepted - {name,description,user List(userEmail,userRole),list of groupIds}
●	Start transaction
●	Call the createAuditSet method from AuditSetService, and if no exception occurs, submit the transaction

3.AuditSetServie-
    Following params are accepted - {name,description,user List(userEmail,userRole),list of groupIds}	
●	Retrieve information about the current user.
●	Ensure that the audit set name is provided and not empty.
●	Fetch the list of existing audit sets to check for the uniqueness of the audit set name.
●	Verify if the current user, determined from the context, possesses owner authority to update the audit set.
●	Only owners have the privilege to update audit sets.
●	Create a new collaborator user instance.
●	Configure the collaborator lists for co-owners, members, and reviewers.
●	Assign roles accordingly: audit_admin is added as a co-owner, general_user is added as a member, and external_auditor is added as a reviewer in the audit set.
●	Replace the existing list of collaborators with the new one.
●	Convert the collaborator object into JSON format.
●	Embed the JSON object within the audit set.
●	Create a new audit set and store it in the AuditSetRepository.
●	Generate a new AuditSetCollaborator and save it in the AuditSetCollaboratorRepository.
●	Confirm the successful update of the audit set.

1.42	/auditSet/validateAuditSet
The API validateAuditSet is a GET API that validates an audit set based on the provided auditSetId.
Input Parameters:
AuditSetId
Response 
As a response, an audit set will be validated.
Exceptions
Following would be the exception conditions

Exception	Error Message
Exception	Something Went Wrong !!
       Operations performed at each layer of the framework by this API are as below:
1.Controller
     In this REST API, Following Object is accepted - {auditSetId}

2.Business
    Following Object is accepted - {auditSetId}
●	Start transaction
●	Call the createAuditSet method from AuditSetService, and if no exception occurs, submit the transaction

3.AuditSetServie-
   Following params are accepted - {auditSetId}
●	Get audit set items for the provided ID.
●	Handle CRUD exceptions with a generic error message.
●	Check if audit set item retrieval was successful; if not, return the response.
●	Prepare to collect verification statuses for each item.
●	Establish a Box API connection for enterprise access.
●	Iterate through each audit set item:
●	Retrieve the corresponding item object from the repository.
●	Log and throw an exception if the item is not found.
●	For file items:
●	Manage item status, update validation time, and check tampering status.
●	Collect file metadata and add verification status.
●	For non-file items:
●	Parse JSON data into basic item information.
●	Validate folder contents and update verification statuses.
●	Summarize the tampered files for the response.
●	Build and return a response with total files checked, tampered file count, and details.


1.43	/auditSet/updateAuditSetsForItemId
The API updateAuditSetInfo is a Put API used to update audit set information, including the name of the item, list of AuditSetLists ( auditSetId, auditSetName, description,ownedBy,
createdAt, accessStatus, isFavourite,isItemIdAdded)

Input Parameters:
Name of the Audit set- should be non null or empty Size must be less than 64 chars 
AuditSetLists ( auditSetId, auditSetName, description,ownedBy,
createdAt, accessStatus, isFavourite,isItemIdAdded)

Response 
As a response,an audit set for item id will be updated in the system.

       Operations performed at each layer of the framework by this API are as  below:
1.Controller
    In this REST API, Following Object is accepted - {itemName, AuditSetLists ( auditSetId, auditSetName, description,ownedBy,
createdAt, accessStatus, isFavourite,isItemIdAdded)}

2.Business
   Following Object is accepted - {itemName, AuditSetLists ( auditSetId, auditSetName, description,ownedBy,
createdAt, accessStatus, isFavourite,isItemIdAdded)}
●	Start transaction
●	Call the createAuditSet method from AuditSetService, and if no exception occurs, submit the transaction

3.AuditSetServie-
    Following params are accepted -  {itemName, AuditSetLists ( auditSetId, auditSetName, description,ownedBy,
createdAt, accessStatus, isFavourite,isItemIdAdded)}
●	Retrieve the current user from the repository using the email associated with the authenticated user.
●	Iterate through each audit set in the provided update request.
●	If an item is marked as added in the audit set:
●	Check if the item already exists in the audit set.
●	If not, create a new audit set item with the provided details and the current timestamp.
●	If it exists, update its creation timestamp.
●	If an item is not marked as added in the audit set:
●	Check if the item exists in the audit set.
●	If it does, delete the item from the audit set.
●	Return a successful response indicating that the item has been updated in the audit set.
1.44	/auditSet/getMyAuditSetListForItemId
The API getMyAuditSetList is a GET API that accepts details of the audit set using itemId, including auditSetId, auditSetName, description, ownedBy, createdAt, accessStatus, and isFavourite.
Input Parameters:
Current user login
Response 
As a response,the user will get a list of audit sets for itemId from the system.

 The Response DTO:AuditSetList

Field name	Description
Id	AuditSetId
name	Audit set name
description	Audit set description description
ownedBy	Owner email of audit set
createdAt	Date when audit set is created
accessStatus	Access status of file (UNDER_REVIEW,NEWLY_ADDED)
isFavourite	To be implemented
       Operations performed at each layer of the framework by this API are as below:
1. Controller
    In this REST API, Following Object is accepted - {itemId

2.Business
   Following Object is accepted - {itemId}
●	Start transaction
●	 Call the getMyAuditSetList method from AuditSetService, and if no exception occurs, submit the transaction
●	Attempt to retrieve the user's audit set list using their authentication name.
●	Iterate through each audit set in the response.
●	For each audit set, check if the specified item exists within it.

3.AuditSetService:
    Following params are accepted - {itemId}
●	Retrieve the user object using the provided email within the current transaction.
●	If the user is not found, throw a NotFoundException.
●	Initialize an empty list to store audit set lists.
●	Determine the roles of the user from their authorities.
●	If the user is an audit admin, retrieve all audit set lists.
●	If the user is a general user, retrieve audit set lists where they are collaborators.
●	If the user is an external auditor, retrieve audit set lists for collaborators and their user groups.
●	Combine the retrieved audit set lists.
●	If audit set lists are found, return a successful ApiResponse with HttpStatus OK.
●	If no audit set lists are found, return a successful ApiResponse with an empty list and HttpStatus OK.
●	Handle exceptions such as NotFoundException and other unexpected errors, returning appropriate ApiResponse objects with error messages and corresponding HttpStatus codes.
8.	Audit Set Collaborator Controller
Overview
Audit Set Collaborator Controller is REST API Controller for audit set collaborator related operations on box application   
Following are the REST APIs for the audit set related operations. The API documentation and execution approach is documented on Swagger here

1.45	/auditSetCollaborator/changeAuditSetOwner
The API changeAuditSetOwner is a Put API used to change the owner of an audit set. It accepts details such as auditSetId and newOwnerId to perform the owner change operation.
Input Parameters:
auditSetId- should be non null or empty
newOwnerId 
Response 
As a response,the owner of the audit set will be changed in the system.
Exceptions
Following would be the exception conditions

Exception	Error Message
For audit set is not present	Audit Set Not Found
If the new owner is not present	New owner not found
Authority to change owner	Only owner and co-owner can change audit set owner
If new owner is owner	The new owner is already an owner
The new owner must be co-owner of auditSet.	The new owner must be an Co Owner before becoming the owner
       Operations performed at each layer of the framework by this API are as below:
1.Controller
            In this REST API, Following Object is accepted - {auditSetId,newOwnerId}             

2.Business
           Following Object is accepted - {auditSetId,newOwnerId}      
●	Start transaction
●	Call the changeAuditSetOwner method from AuditSetCollaboratorService, and if no exception occurs, submit the transaction

3.AuditSetServie-
    Following params are accepted - {auditSetId,newOwnerId}      
●	Get auditSet object from auditSetId
●	If an object is not found, give a generic exception.
●	Check permission to see if the user is the owner or co-owner of this operation.
●	Fetch the user details for the new owner.
●	If a new owner is not found, give a generic exception.
●	Update the ACL JSON to change the owner and make the current owner a co-owner by calling the changeAuditSetOwner method of the same class.
●	Update the AuditSet with the new ACL JSON and set the new owner details.
●	Get the auditSetCollaborators object of the current owner from the auditSetCollaborator repository by providing the auditSet id, role, and previous owner email.
●	Set the current owner as co-owner and update it.
●	Get the auditSetCollaborators object of a new owner from the auditSetCollaborator repository by providing the auditSet id, new role, and new owner email.
●	Set the new owner as the owner and update it.
 

1.46	/auditSetCollaborator/getCollaboratorsForAuditSet
The API getCollaboratorsForAuditSet is a GET API that accepts details of the audit set collaborator auditSetId and retrieves data, including a list of collaborators and an audit group list.
Input Parameters:
         auditSetId:-Should not be null or empty
Response 
As a response, a list of audit set collaborators will be fetched from the system.

 The Response DTO:AuditSetICollaborators

Field name	Description
ownedBy	Owned by(userId, username,emailId,role)
coOwners	Co owners list with(userId, username,emailId,role)
members	Members list with (userId, username,emailId,role)
reviewers	Reviewers list with (userId, username,emailId,role)
auditGroupListForAuditSetList	Audit group list with(auditGroupId,
auditGroupName,description)

Exceptions
Following would be the exception conditions

Exception	Error Message
For audit set is not present	Audit Set Not Found
       Operations performed at each layer of the framework by this API are as below:
1.Controller
In this REST API, Following Object is accepted - {auditSetId}

2.Business
Following Object is accepted - {auditSetId}

●	Start transaction
●	Call the getCollaboratorForAuditSet method from the auditSet Collaborator service, and if no exception occurs, submit the transaction
3.AuditSetCollaboratorService:
 Following params are accepted- {auditSetId}
●	Retrieve the AuditSet using its ID.
●	Ensure the AuditSet exists and is not deleted; otherwise, raise an exception.
●	Parse the ACL JSON into a collaborator object.
●	Handle JSON parsing exceptions if any occur.
●	Extract the list of audit groups associated with the audit set.
●	Parse the audit group list JSON into objects.
●	Handle parsing exceptions.
●	Construct an AuditSetCollaboratorList with the collaborator and audit group lists.
●	Return ApiResponse with the constructed AuditSetCollaboratorList.
●	Handle exceptions and return the appropriate ApiResponse for errors.









9.	Audit Group Controller
Overview
Audit Group Controller is REST API Controller for audit group related operations on box application   
Following are the REST API’s for the audit group related operations The API documentation and execution approach is documented on Swagger here
1.47	/auditGroup/createAuditGroup
The API createAuditGroup is a POST API used to create an audit group. It accepts details such as the name of the audit group, a description, and a list of user emails to create the audit group. 
Input Parameters:
Name of the Audit group- should be non null or empty Size must be less than 64 chars 
Description of audit group–description is optionalList of user emails
Response 
As a response, a new audit group will be created in the system.
Exceptions
Following would be the exception conditions

Exception	Error Message
For user not present	User not found
If audit group is empty or null	Audit group name should not be empty
For duplicate name	Please use another audit group name as there exists a audit group with the same name
If the user does not have authority to create an audit group	User does not have the required role to create the audit group
If having other error	An error occurred while creating the audit group
        Operations performed at each layer of the framework by this API are as below:
1.Controller
In this REST API, Following Object is accepted - {name, description, List of users(userEmails)}

2.Business
Following Object is accepted - {name,description,List of users(userEmails)}
●	Start transaction
●	Call the createAuditGroup method from AuditGroupService, and if no exception occurs, submit the transaction

3.AuditGroupService:
           Following params are accepted -{name,description,List of users(userEmails)}
●	Retrieve the current user object using the provided email from the user repository.
●	Check if the user exists; if not, throw a NotFoundException.
●	Validate the provided audit group name; if empty or null, return ApiResponse, indicating a bad request.
●	Check for duplicate audit group names among active groups; if found, return an ApiResponse indicating a bad request.
●	Fetch user roles from the security token.
●	If the user has the AUDIT_ADMIN role:
●	Set the owner of the audit group.
●	Set members from the provided auditGroupUserList.
●	Combine the owner and members into a list.
●	Convert the list to JSON format.
●	Generate a unique audit group ID.
●	Create an AuditGroup object with the provided data.
●	Save the audit group in the database.
●	Save member details in the userAuditGroup table.
●	Return ApiResponse indicating successful creation of the audit group with appropriate details or failure with error messages and HTTP status codes, handling exceptions as needed.




1.48	/auditGroup/updateAuditGroup
The API update AuditGroup is a PUT API used to update an existing audit group. It accepts details such as the name of the audit group, a description, and a list of user emails to perform the update operation on the audit group.
Input Parameters:
Name of the Audit group- should be non null or empty Size must be less than 64 chars 
Description of audit group–description is optionalList of user emails
Response 
As a response, an audit group will be updated in the system.
Exceptions
Following would be the exception conditions

Exception	Error Message
For check audit group is present or not	Audit group not found
For user not present	User not found
If audit group is empty or null	Audit group name should not be empty
For duplicate name	Please use another audit group name as there exists a audit group with the same name
If the user does not have authority to create an audit group	User does not have the required role to create the audit group
If having other error	An error occurred while creating the audit group
Operations performed at each layer of the framework by this API are as below:
1.Controller
In this REST API, Following Object is accepted - {name,description,List of users(userEmails)}

2.Business
Following Object is accepted - {name,description,List of users(userEmails)}
●	Start transaction
●	Call the createAuditGroup method from AuditGroupService, and if no exception occurs, submit the transaction

3.AuditGroupService:
   Following params are accepted - {name, description,List of users(userEmails)}	
●	Retrieve the audit group object using the provided auditGroupId from the auditGroupRepository.
●	Check if the audit group exists and is not already deleted; if not, throw a NotFoundException.
●	Validate the provided audit group name; if empty or null, return ApiResponse, indicating a bad request.
●	Check for duplicate audit group names among active groups; if found, return an ApiResponse indicating a bad request.
●	Fetch user roles from the security token.
●	Deserialize existing member emails from JSON representations into a list of GroupUserPrivileges objects.
●	Determine which members to add and remove based on the provided update.
●	If the user has the AUDIT_ADMIN role:
●	Update the audit group name and description.
●	Add new members and update existing members accordingly.
●	Remove members as per the provided update.
●	Update the audit group's member list JSON in the database.
●	Update the associated audit set mappings and their group details.
●	Return ApiResponse, indicating a successful update or failure with appropriate messages and HTTP status codes, handling exceptions as needed.

1.49	/auditGroup/deleteAuditGroup
The API delete AuditGroup is a DELETE API used to delete an audit group. It accepts details such as the audit group ID to perform the deletion of the audit group.
Input Parameters:
Audit group id- should be check audit group id is present or not 
Response 
If all the Input Parameters met properly, an audit group was successfully deleted from the system.
Exceptions
Following would be the exception conditions

Exception	Error Message
For check audit group is present or not	Audit group not found
For user not present	User not found
If the user does not have authority to delete an audit group	User does not have the required role to delete the audit group
If having other error	An error occurred while deleting the audit group
       Operations performed at each layer of the framework by this API are as below:
1.Controller
In this REST API, Following Object is accepted - {auditGroupId}

2.Business
Following Object is accepted - {auditGroupId}
●	Start transaction
●	Call the createAuditGroup method from AuditGroupService, and if no exception occurs, submit the transaction

3.AuditGroupService:
           Following params are accepted - {auditGroupId}	
●	Retrieve the audit group object using the provided auditGroupId from the auditGroupRepository.
●	Deserialize the JSON representation of member emails into a list of GroupUserPrivileges objects.
●	Fetch user roles from the security token.
●	Verify if the user has the necessary authority (AUDIT_ADMIN role) to delete the audit group.
●	If authorized, iterate through each member email:
●	Delete user's association with the audit group from the userAuditGroupRepository.
●	Retrieve all audit set mappings associated with the audit group.
●	Delete all audit set mappings related to the audit group from the auditGrpAuditSetMappingRepository.
●	Mark the audit group as deleted by setting the isDeleted flag to true.
●	Update the audit group in the database with the new status.
●	Return an ApiResponse indicating successful deletion of the audit group.


1.50	/auditGroup/getListOfAuditGroupMembers
The API getListOfAuditGroupMembers is a GET API that retrieves a list of members belonging to an audit group. It accepts details such as user email and user name.
Input Parameters:
        auditGroupId  is important for fetching audit group member list
Response 
As a response,a list of audit group members will be fetched from the system.

 The Response DTO:AuditGroupMemberList

FieldName	Description
Email	User emails
Name	Name of user

Exceptions
Following would be the exception conditions

Exception	Error Message
For check audit group is present or not	Audit group not found
Operations performed at each layer of the framework by this API are as below:
1.Controller
In this REST API, Following Object is accepted - {auditGroupId}

2.Business
Following Object is accepted - {auditGroupId}
●	Start transaction
●	Call the getListAuditSet method from AuditSetService, and if no exception occurs, submit the transaction

3.AuditSetService:
           Following params are accepted- {auditGroupId}	
●	Retrieve information about the audit group. 
●	Verify if the audit group is present or not.
●	Convert memberList json into a list of groupUserPrivileges
●	Fetch a list of audit group member lists from the memberList json.
●	Obtain the list of audit groups using the auditGroupMemberList object.
 







