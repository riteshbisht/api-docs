# Errors

> Example Response

> Error Message returned when attempting to create an Office without passing any parameters

```shell
{
"developerMessage": "The request was invalid. This typically will happen due to validation errors which are provided.",
"developerDocLink": "https://github.com/openMF/mifosx/wiki/HTTP-API-Error-codes",
"httpStatusCode": "400",
"defaultUserMessage": "Validation errors exist.",
"userMessageGlobalisationCode": "validation.msg.validation.errors.exist",
"errors": [
	{
	"developerMessage": "The parameter name cannot be blank.",
	"defaultUserMessage": "The parameter name cannot be blank.",
	"userMessageGlobalisationCode": "validation.msg.office.name.cannot.be.blank",
	"parameterName": "name",
	"value": null,
	"args": []
	},
	{
	"developerMessage": "The parameter openingDate cannot be blank.",
	"defaultUserMessage": "The parameter openingDate cannot be blank.",
	"userMessageGlobalisationCode": "validation.msg.office.openingDate.cannot.be.blank",
	"parameterName": "openingDate", "value": null, "args": []
	},
	{
	"developerMessage": "The parameter parentId cannot be blank.",
	"defaultUserMessage": "The parameter parentId cannot be blank.",
	"userMessageGlobalisationCode":
	"validation.msg.office.parentId.cannot.be.blank", "parameterName":
	"parentId", "value": null, "args": []
	}
]
}
```

All errors are returned in JSON.

Error Code | Meaning
---------- | -------
200 | OK -- Everything Worked.
400 | Bad Request -- Invalid Parameter or Data Integrity Issue.
401 | Unauthorized -- Your Authorization Token is invalid
403 | Forbidden -- You don't have access to this resource
404 | Not Found -- The specified resource could not be found
405 | Method Not Allowed -- You tried to access a kitten with an invalid method
500 | Internal Server Error -- We had a problem with our server. Try again later.
503 | Service Unavailable -- We're temporarily offline for maintenance. Please try again later.
