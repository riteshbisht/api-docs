# User

An API capability to support administration of application users.

## Verify authentication

> The above command returns JSON structured like this:

```json
{
    "username": "mifos",
    "userId": 1,
    "base64EncodedAuthenticationKey": "bWlmb3M6cGFzc3dvcmQ=",
    "authenticated": true,
    "officeId": 1,
    "officeName": "Head Office",
    "roles": [
        {
            "id": 1,
            "name": "Super user",
            "description": "This role provides all application permissions."
        }
    ],
    "permissions": [
        "ALL_FUNCTIONS"
    ]
}
```

Authenticates the credentials provided and returns the set roles and permissions allowed.

### HTTP Request

`POST https://DomainName/api/v1/authentication`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
username | mifos | Username to LogIn to Mifos instance
password | password | Password to LogIn to Mifos instance

## Create a User

```shell
curl "https://DomainName/api/v1/users"
  -X POST
  -H "Authorization: Basic meowmeowmeow"
  -H "Content-Type: application/json"
  -H "Accept: application/json"
  -d "{
    "username": "newuser",
    "firstname": "Test",
    "lastname": "User",
    "email": "whatever@mifos.org",
    "officeId": 1,
    "staffId": 1,
    "roles": [2,3],
    "sendPasswordToEmail": true
}
"
```

> The above command returns JSON structured like this:

```json
{
    "username": "newuser",
    "firstname": "Test",
    "lastname": "User",
    "email": "whatever@mifos.org",
    "officeId": 1,
    "staffId": 1,
    "roles": [2,3],
    "sendPasswordToEmail": true
}
```

Adds new application user.

Note: Password information is not required (or processed). Password details at present are auto-generated and then sent to the email account given (which is why it can take a few seconds to complete).

Mandatory Fields
username, firstname, lastname, email, officeId, roles, sendPasswordToEmail

Optional Fields
staffId,passwordNeverExpires

### HTTP Request

`POST https://DomainName/api/v1/users`

## Retrieve list of users

`GET https://DomainName/api/v1/users`

> The above command returns JSON structured like this:

```json
[
    {
        "id": 1,
        "username": "mifos",
        "officeId": 1,
        "officeName": "Head Office",
        "firstname": "App",
        "lastname": "Administrator",
        "email": "demomfi@mifos.org",
        "passwordNeverExpires": false,
        "staff": {
		   "id": 1,
		   "firstname": "Test",
		   "lastname": "123",
		   "displayName": "123, Test",
		   "mobileNo": "12312312",
		   "officeId": 1,
		   "officeName": "Head Office",
		   "isLoanOfficer": true,
		   "isActive": true
		}
	"selectedRoles": [
            {
                "id": 1,
                "name": "Super user",
                "description": "This role provides all application permissions."
            }
        ]
    }
]
```
Example Requests:

users

users?fields=id,username,email,officeName


## Retrieve a User

`GET https://DomainName/api/v1/users/{userId}`

> The above command returns JSON structured like this:

```json
{
    "id": 1,
    "username": "mifos",
    "officeId": 1,
    "officeName": "Head Office",
    "firstname": "App",
    "lastname": "Administrator",
    "email": "demomfi@mifos.org",
    "passwordNeverExpires": true,
    "staff": {
	 "id": 1,
	 "firstname": "Test",
	 "lastname": "123",
	 "displayName": "123, Test",
	 "mobileNo": "12312312",
	 "officeId": 1,
	 "officeName": "Head Office",
	 "isLoanOfficer": true,
	 "isActive": true
	}
    "availableRoles": [],
    "selectedRoles": [
        {
            "id": 1,
            "name": "Super user",
            "description": "This role provides all application permissions."
        }
    ]
}
```
Example Requests:

users/1


users/1?template=true


users/1?fields=username,officeName

## Update a User

`PUT https://DomainName/api/v1/users/{userId)`

  > The above command returns JSON structured like this:

```json
{
    "officeId": 1,
    "resourceId": 3,
    "changes": {
        "firstname": "Test",
        "passwordEncoded": "abc3326b1bb376351c7baeb4175f5e0504e33aadf6a158474a6d71de1befae51"
    }
}
```
Note: When updating a password you must provide the repeatPassword parameter also.


## Delete a User

`DELETE https://DomainName/api/v1/users/{userId}`

> The above command returns JSON structured like this:

```json
{
    "officeId": 1,
    "resourceId": 20,
    "changes": {}
}
```
``

# Roles

An API capability to support management of application roles for user administration.

## List Roles

`GET https://DomainName/api/v1/roles`

> The above command returns JSON structured like this:

```json
[
  {
    "id": 1,
    "name": "Super user",
    "description": "This role provides all application permissions."
  }
]
```
Example Requests:

roles

roles?fields=name

## Delete a Role

`DELETE https://DomainName/api/v1/roles/{roleId}`

> The above command returns JSON structured like this:

```json
{
	"resourceId":1
}
```
Description : Delete the role in case role is not associated with any users.

## Retrieve a Role

`GET https://DomainName/api/v1/roles/{roleId}`

> The above command returns JSON structured like this:

```json
{
  "id": 1,
  "name": "Super user",
  "description": "This role provides all application permissions."
}
```
Example Requests:

roles/1


roles/1?fields=name

## Create a New Role

`POST https://DomainName/api/v1/roles/{roleId}`

> The above command returns JSON structured like this:

```json
{
  "resourceId": 2
}
```
Mandatory Fields
name, description

## Update a Role

`PUT https://DomainName/api/v1/roles/{roleId}`

> The above command returns JSON structured like this:

```json
{
	"resourceId": 1,
	"changes": {
		"description": "some description(changed)"
	}
}
```
## Retrieve a Role's Permissions

`GET https://DomainName/api/v1/roles/{roleId}/permissions`

> The above command returns JSON structured like this:

```json
{
    "id": 1,
    "name": "Super user",
    "description": "This role provides all application permissions.",
    "permissionUsageData": [
        {
            "grouping": "authorisation",
            "code": "READ_PERMISSION",
            "entityName": "PERMISSION",
            "actionName": "READ",
            "selected": false
        },
        {
            "grouping": "transaction_loan",
            "code": "WRITEOFF_LOAN_CHECKER",
            "entityName": "LOAN",
            "actionName": "WRITEOFF",
            "selected": false
        }
    ]
}
```

Example Requests:

roles/1/permissions

## Enable Role

`POST https://DomainName/api/v1/roles/{roleId}?command=enable`

> The above command returns JSON structured like this:

```json
{
	"resourceId":1
}
```

Description : Enable role in case role is disabled.

## Disable Role

`POST https://DomainName/api/v1/roles/{roleId}?command=disable`

> The above command returns JSON structured like this:

```json
{
	"resourceId":1
}
```
Description : Disable the role in case role is not associated with any users.

## Update a Role's Permissions

`PUT https://DomainName/api/v1/roles/{roleId}/permissions`

> The above command returns JSON structured like this:

```json
{
    "resourceId": 8,
    "changes": {
        "permissions": {
            "ALL_FUNCTIONS_READ": true
        }
    }
}
```
