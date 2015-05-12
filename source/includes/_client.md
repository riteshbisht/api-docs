# Client

## Retrieve Client Details Template

```shell
curl "https://DomainName/api/v1/clients/template"
```

> The above command returns JSON structured like this:

```json
{
	"activationDate":[2014,3,4],
	"officeId":1,
	"officeOptions":[{
		"id":1,
		"name":"Head Office",
		"nameDecorated":"Head Office"
	}],
	"staffOptions":[{
		"id":1,
		"firstname":"xyz",
		"lastname":"sjs",
		"displayName":"sjs, xyz",
		"officeId":1,
		"officeName":"Head Office",
		"isLoanOfficer":true,
		"isActive":true
	}],
	"savingProductOptions":[{
		"id":4,
		"name":"account overdraft",
		"withdrawalFeeForTransfers":false,
		"allowOverdraft":false
	}]
}
```

This is a convenience resource. It can be useful when building maintenance user interface screens for client applications. The template data returned consists of any or all of:

* Field Defaults
* Allowed Value Lists

### HTTP Request

`GET https://DomainName/api/v1/clients/template`

### Query Parameters

**ARGUMENTS**

 | |
--------- | ------- | -----------
**officeId:** |**Integer** _optional_ |
**staffInSelectedOfficeOnly:** | **Boolean** _optional_  Defaults to false if not provided. If staffInSelectedOfficeOnly=true only staff who are associated with the selected branch are returned.|
**commandParam:** | **String** _optional_ If commandParam=close retrieves all closureReasons which are associated with "ClientClosureReason" value.|

## Create a Client


```shell
curl "https://DomainName/api/v1/clients"
  -X POST
  -H "Authorization: Basic meowmeowmeow"
  -H "Content-Type: application/json"
  -H "Accept: application/json"
  -d "{
	"officeId": 1,
	"firstname": "Petra",
	"lastname": "Yton",
	"externalId": "786YYH7",
	"dateFormat": "dd MMMM yyyy",
	"locale": "en",
	"active": true,
	"activationDate": "04 March 2009",
    "submittedOnDate":"04 March 2009",
    "savingsProductId" : 4
}"
```

> The above command returns JSON structured like this:

```json
{
  "id": 2,
  "name": "Isis",
  "breed": "unknown",
  "fluffiness": 5,
  "cuteness": 10
}
```

**Note:** You can enter either:  
firstname/middlename/lastname - for a person (middlename is optional) OR  
fullname - for a business or organisation (or person known by one name).

### HTTP Request

`POST https://DomainName/api/v1/clients`  

### URL Parameters

|**Mandatory Fields**|
|--------- |
|firstname and lastname OR fullname|
|officeId|
|active=true |
|activationDate|
|active=false|

|**Mandatory Fields**|
|--------- |
|groupId|
|externalId|
|accountNo|
|staffId|
|mobileNo|
|savingsProductId|
|genderId|
|clientTypeId|
|clientClassificationId|


##Activate a Client


```shell
curl "https://DomainName/api/v1/clients/{clientId}?command=activate"
  -X POST
  -H "Authorization: Basic meowmeowmeow"
  -H "Content-Type: application/json"
  -H "Accept: application/json"
  -d "Request Body:
{
  "locale": "en",
  "dateFormat": "dd MMMM yyyy",
  "activationDate": "01 March 2011"
}"
```

> The above command returns JSON structured like this:

```json
{
  "officeId": 1,
  "clientId": 1,
  "resourceId": 1
}
```
Clients can be created in a Pending state. This API exists to enable client activation (for when a client becomes an approved member of the financial Institution).

If the client happens to be already active this API will result in an error.


### HTTP Request

`POST https://DomainName/api/v1/clients/{clientId}?command=activate`



##Close a Client


```shell
curl "https://DomainName/api/v1/clients/{clientId}?command=close"
  -X POST
  -H "Authorization: Basic meowmeowmeow"
  -H "Content-Type: application/json"
  -H "Accept: application/json"
  -d "Request Body:
	{
	  "dateFormat":"dd MMMM yyyy",
	  "locale":"en",
	  "closureDate":"25 June 2013",
	  "closureReasonId":"11"
	}"
```

> The above command returns JSON structured like this:

```json
{
  "clientId":15,
  "resourceId":15
}
```
Clients can be closed if they do not have any non-closed loans/savingsAccount. This API exists to close a client .

If the client have any active loans/savingsAccount this API will result in an error.

### HTTP Request

`POST https://DomainName/api/v1/clients/{clientId}?command=close`

##Reject a Client


```shell
curl "https://DomainName/api/v1/clients/{clientId}?command=reject"
  -X POST
  -H "Authorization: Basic meowmeowmeow"
  -H "Content-Type: application/json"
  -H "Accept: application/json"
  -d "Request Body:
	{
"rejectionDate":"28 November 2014",
"rejectionReasonId":16,
"locale":"en",
"dateFormat":"dd MMMM yyyy"
}"
```

> The above command returns JSON structured like this:

```json
{
  "clientId":15,
  "resourceId":15
}
```
Clients can be rejected when client is in pending for activation status.

If the client is any other status, this API throws an error.

|Mandatory Fields|
|-----------------|
|rejectionDate|
|rejectionReasonId|

### HTTP Request

`POST https://DomainName/api/v1/clients/{clientId}?command=reject`

##Withdraw a Client


```shell
curl "https://DomainName/api/v1/clients/{clientId}?command=withdraw"
  -X POST
  -H "Authorization: Basic meowmeowmeow"
  -H "Content-Type: application/json"
  -H "Accept: application/json"
  -d "Request Body:
	{
"withdrawalDate":"28 November 2014",
"withdrawalReasonId":17,
"locale":"en",
"dateFormat":"dd MMMM yyyy"
}"
```

> The above command returns JSON structured like this:

```json
{
  "officeId":1,
  "clientId":15,
  "resourceId":15
}
```
Client applications can be withdrawn when client is in a pending for activation status.

If the client is any other status, this API throws an error.

|Mandatory Fields|
|-----------------|
|withdrawalDate|
|withdrawalReasonId|

### HTTP Request

`POST https://DomainName/api/v1/clients/{clientId}?command=withdraw`


##Reactivate a Client


```shell
curl "https://DomainName/api/v1/clients/{clientId}?command=reactivate"
  -X POST
  -H "Authorization: Basic meowmeowmeow"
  -H "Content-Type: application/json"
  -H "Accept: application/json"
  -d "Request Body:
	{
"reactivationDate":"28 November 2014",
"locale":"en",
"dateFormat":"dd MMMM yyyy"
}"
```

> The above command returns JSON structured like this:

```json
{

  "clientId":15,
  "resourceId":15
}
```
Clients can be reactivated after they have been closed.

Trying to reactivate a client in any other state throws an error.

|Mandatory Fields|
|-----------------|
|reactivationDate|

### HTTP Request

`POST https://DomainName/api/v1/clients/{clientId}?command=reactivate`


##assign a Staff


```shell
curl "https://DomainName/api/v1/clients/{clientId}?command=assignStaff"
  -X POST
  -H "Authorization: Basic meowmeowmeow"
  -H "Content-Type: application/json"
  -H "Accept: application/json"
  -d "Request Body:
	{
  "staffId": "1"
}"
```

> The above command returns JSON structured like this:

```json
{
  "officeId": 1,
  "clientId": 1,
  "resourceId": 1,
  "changes": {"staffId":1}
}

```
Allows you to assign a Staff for existed Client.

The selected Staff should belong to the same office (or an officer higher up in the hierarchy) as the Client he manages.

### HTTP Request

`POST https://DomainName/api/v1/clients/{clientId}?command=assignStaff`



##Unassign a Staff


```shell
curl "https://Domain Name/api/v1/clients/{clientId}?command=unassignStaff"
  -X POST
  -H "Authorization: Basic meowmeowmeow"
  -H "Content-Type: application/json"
  -H "Accept: application/json"
  -d "Request Body:
	{
  "staffId": "1"
}"
```

> The above command returns JSON structured like this:

```json
{
  "officeId": 1,
  "clientId": 1,
  "resourceId": 1,
  "changes": {"staffId":1}
}

```
Allows you to unassign the Staff assigned to a Client.

### HTTP Request

`POST https://DomainName/api/v1/clients/{clientId}?command=unassignStaff`
