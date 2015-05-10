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

`POST https://Domain Name/api/v1/clients/{clientId}?command=activate`
