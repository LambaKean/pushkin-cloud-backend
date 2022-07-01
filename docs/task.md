# 1. Project goal

The goal of the project is to develop a web application that allows users to store their files in the cloud.
Users will be able to create folders on their cloud and upload their files to the cloud, edit and delete them.

# 2. Project description

The project consists of the following main blocks:

1. Registration, authentication and authorization
2. User functionality
3. Notification functionality

## 2.1 User authentication

Users are authenticated using JWT tokens. On successful registration or on successful login to the account, 
the server sends two cookies to the client: an access token cookie and a refresh token cookie. The access token cookie 
will then be sent to the server each time a request is made. The refresh token, on the other hand, will be sent to
the server when the access token expires. The refresh token is used to update a pair of tokens and is sent only to 
a specific endpoint.

If an unauthorized client makes a request that requires authorization, the server returns a 401 response.

## 2.2 Registration

When registering a user, the following data must be transferred to the server:

* username
* email
* password

All the fields are required.

After successfully submitting the registration form, the user will receive a notification with an email confirmation
code. The user will need to enter the received confirmation code to complete the registration. Once the
registration is complete, the user receives a pair of cookies with security tokens.

## 2.3 User login

The user is considered logged into his account if he has a valid access token or valid update token.

The following data must be transferred to the server in order for the user to log in to the account:

* email
* password

If the login form is successfully submitted, the user receives a pair of cookies with security tokens.

## 2.4 Functionality for a user who is not logged into an account

Users who are not logged in are not provided with any functionality (except, of course, the ability to log in
to their account or register).

## 2.5 Functionality for the user logged into the account

The user functionality consists of the following blocks:

1. Editing account data
2. Changing the folder structure
3. File management

### 2.5.1 Editing account data

#### 2.5.1.1 User account

The user account consists of the following fields:

* email

The user email must be unique among the emails of registered users.

* username

The username must be unique among the usernames of all registered users. The username must be 6 to 40 characters
long and consist only of Latin letters and numbers.

* password

The password must be 8 to 60 characters long.

#### 2.5.1.2 Field editing

User should be able to change his username, email and password. New values of fields must satisfy the rules
described in Section 2.5.1.1.

When the user changes email, the user should receive a notification with an email confirmation code at the 
new email address.

When changing the password, in addition to the new password, the user must also specify his current password.

All data about the changed field values comes to the server in a single request (i.e. if the user changes his 
password and username, the server sends not two requests for each field, but a single request for both fields).

### 2.5.2 Changing the folder structure

During registration, a root folder with the same name as the user's username is created for each user. The root
directory cannot be deleted or renamed, but when the user's username is changed, the root directory of that user
is automatically renamed, and when the user is deleted, the user's root folder is also deleted.

The user can create new folders in his root folder and subfolders in those folders. The names of the created 
folders can be changed. Created folders can be deleted, deleting all subfolders and files in the deleted folder. 

The following rules must be taken into account when creating folders or changing folder names:

* The folder name may contain any characters except **/**, **\n**, **\r**, **\t**, **\0**, **\f**, **`**, **?**,
  *, **\\**, **<**, **>**, **|**, **"**, **:**
* Folder name must be between 1 and 200 characters long
* No two subfolders with the same name should be in the same folder
* There can be no more than 15 levels of subfolders, including the root folder


### 2.5.3 File management

The user can upload files to his root folder and to any folder he creates. Uploaded files can be downloaded, 
renamed, or deleted. The file extension is considered part of the file name. 

The following rules must be taken into account when uploading files or changing file names:

* The file name may contain any characters except **/**, **\n**, **\r**, **\t**, **\0**, **\f**, **`**, **?**,
  *, **\\**, **<**, **>**, **|**, **"**, **:**
* File name must be between 1 and 200 characters long
* No two files with the same name should be in the same folder
* The weight of the uploaded file must not exceed 64 mb

## 2.6 Notification functionality

In some cases, users must receive notifications, such as those that contain an email confirmation code. The server
has two modes of sending notifications: directly by email or by output to the server console. The second mode can
be useful, for example, when you need to quickly deploy the server in a Docker container without connecting
the mail server.

# 3. Technologies

* Java 17
* Spring Boot
* PostgreSQL, HSQLDB
* Liquibase
* JUnit