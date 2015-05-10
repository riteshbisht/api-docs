# Overview

## Introduction

Mifos X is a secure, multi-tenanted microfinance platform.

The goal of the Mifos X API is to empower developers to build apps on top of the Mifos X Platform. The [reference app](https://demo.openmf.org) (username: mifos, password: password) works on the same demo 'tenant' as the interactive links in this documentation.

The API is organized around [REST](http://en.wikipedia.org/wiki/Representational_State_Transfer).

The API is designed to have:

* predictable, resource-oriented URLs
* to use HTTP response codes to indicate API errors
* to use built-in HTTP features, like HTTP authentication and HTTP verbs, which can be understood by off-the-shelf HTTP clients.

[JSON](http://www.json.org/) is returned in all responses from the API, including errors.

Much of the API presentation and design ideas are owed to the excellent [Apigee "Web API Design" eBook/PDF](http://info.apigee.com/Portals/62317/docs/web%20api.pdf) and the very good [Stripe API reference](https://stripe.com/docs/api).

## Try API From Your Browser

Try The API From Your Browser
GET (read) examples can be run directly from this documentation. It is just a matter of clicking a link. Most browsers display the output on the same page (or another tab if you right click and select that option). Internet Explorer will probably treat the output as if you wanted to download a file. In that case just elect to open the output in a text editor.

If you want to check out the POST, PUT and DELETE (update) examples a good approach is to take a moment to install a REST plugin for your browser e.g.

* [RESTClient](https://addons.mozilla.org/en-US/firefox/addon/restclient/) for Mozilla FireFox
* [Postman](https://chrome.google.com/webstore/detail/postman-rest-client-packa/fhbjgbiflinjbdggehcddcbncdddomop) for Google Chrome

The REST plugins will allow you to

* Select the "Verb" (e.g. POST)
* Enter the resource name (e.g. offices)
* Add a header to indicate you are sending JSON data as part of the request body (Content-Type: application/json)
* Add a header to indicate your 'tenant' (X-Mifos-Platform-TenantId: default)
* Paste the example JSON into a Request Body
* Send the Request (and receive a Response)

## Generic Options

### Convenience Templates

There are a list of convenience resources (see Template menu option). These resources end with "/template" and can be useful when building maintenance user interface screens for client applications. The template data returned may consist of any or all of:

* Field Defaults
* Allowed Value Lists

Also, many "Retrieve a" type resources (Retrieve a Client for example) allow the parameter option "template=true". This appends any "Allowed Value Lists" which can be useful when building update functionality.

### Restrict Returned Fields

Parameter "fields={fieldlist}" can be used on GET requests to restrict the fields returned.

Normal Request:

[offices/1](https://demo.openmf.org/mifosng-provider/api/v1/offices/1?tenantIdentifier=default)

Request (restricting fields returned):

[offices/1?fields=id,name](https://demo.openmf.org/mifosng-provider/api/v1/offices/1?fields=id,name&tenantIdentifier=default)

### Pretty JSON Formatting

Parameter "pretty=true" can be used to display JSON from GET requests in an easy-to-read format. This parameter is used in this documentation.

Easy-to-read JSON output for POSTs, PUTs and DELETEs will available in the REST plugin you use e.g. RESTClient for FireFox

Normal Request (with pretty printing/formatting):

[offices/1?pretty=true](https://demo.openmf.org/mifosng-provider/api/v1/offices/1?pretty=true&tenantIdentifier=default)

### Creating and Updating

When you want to 'Create a ...' you have to at least supply the mandatory fields. The mandatory fields are listed in this documentation under the relevant 'Create a ...' heading.

When you want to 'Update a ...' you can update individual fields or a combination of fields (subject to data integrity rules).

### Updating Dates and Numbers

> JSON examples for Dates:

```shell
{
	"locale": "en_US",
	"dateFormat": "dd MMMM yyyy",
	"openingDate": "01 July 2007"
}

{
	"locale": "en_US",
	"dateFormat": "yyyy-MM-dd",
	"openingDate": "2007-03-21"
}
```

**Dates**

Dates are returned in GET requests as an array e.g. [ 2007, 4, 11]. However, the API accepts them as strings in POST and PUT requests. If there are any dates in your POST or PUT requests, you need to provide the "locale" and "dateFormat". This can be any date pattern supported by [Joda-Time](http://joda-time.sourceforge.net/api-release/org/joda/time/format/DateTimeFormat.html). This capability can help you when saving data in your client application as you shouldn't need to do any date format conversion prior to issuing your POST or PUT request.

<br><br><br><br><br><br><br><br>

> JSON examples for Numbers:

```shell
{
	"locale": "en_US",
	"principal": "240,400.88"
}
{
	"locale": "fr_CH",
	"principal": "240 400.88"
}
```

**Numbers**

You must provide a "locale" when updating numbers. Numbers are not "Ids" or "Types" but are typically money amounts or percentages that relate to loans. In any case, the API will send back an error message if you forget.

<br><br><br><br><br><br><br><br><br><br>

### Field Descriptions

Most fields are self-explanatory. Fields that aren't are described under the relevant resource heading e.g. AUTHENTICATION

### Authentication Overview
Authentication to the API occurs via [HTTP Basic Auth](http://en.wikipedia.org/wiki/Basic_access_authentication).

The platform has been configured to reject plain HTTP requests and to expect all API requests to be made over [HTTPS](http://en.wikipedia.org/wiki/HTTP_Secure). All requests must be authenticated.

## Batch API

> Example Request

```shell
[
   {
      "requestId":1,
      "relativeUrl":"clients",
      "method":"POST",
      "headers":[
         {
            "name":"Content-type",
            "value":"text/html"
         }
      ],
      "body":"{
	    \"officeId\": 1,
	    \"firstname\": \"Petra\",
	    \"lastname\": \"Yton\",
	    \"externalId\": \"ex_externalId1\",
	    \"dateFormat\": \"dd MMMM yyyy\",
	    \"locale\": \"en\",
	    \"active\": true,
	    \"activationDate\": \"04 March 2009\",
	    \"submittedOnDate\": \"04 March 2009\"
   	 }"
   },
   {
      "requestId":2,
      "relativeUrl":"loans",
      "method":"POST",
      "headers":[
         {
            "name":"Content-type",
            "value":"text/html"
         }
      ],
      "reference":1,
      "body":"{ \"dateFormat\": \"dd MMMM yyyy\",
                \"locale\": \"en_GB\",
                \"clientId\": \"$.clientId\",
                \"productId\": 26,
                \"principal\": \"10,000.00\",
                \"loanTermFrequency\": 12,
                \"loanTermFrequencyType\": 2,
                \"loanType\": \"individual\",
                \"numberOfRepayments\": 10,
                \"repaymentEvery\": 1,
                \"repaymentFrequencyType\": 2,
                \"interestRatePerPeriod\": 10,
                \"amortizationType\": 1,
                \"interestType\": 0,
                \"interestCalculationPeriodType\": 1,
                \"transactionProcessingStrategyId\": 1,
                \"expectedDisbursementDate\": \"10 Jun 2013\",
                \"submittedOnDate\": \"10 Jun 2013\"
              }"
   },
   {
      "requestId":3,
      "relativeUrl":"loans/$.loanId/charges",
      "method":"POST",
      "reference":2,
      "headers":[
         {
            "name":"Content-type",
            "value":"text/html"
         }
      ],
      "body":"{
              \"chargeId\": \"2\",
              \"locale\": \"en\",
              \"amount\": \"100\",
              \"dateFormat\": \"dd MMMM yyyy\",
              \"dueDate\": \"29 April 2013\"
            }"
   },
   {
      "requestId":4,
      "relativeUrl":"loans/$.loanId/charges",
      "method":"GET",
      "reference":2,
      "headers":[
         {
            "name":"Content-type",
            "value":"text/html"
         }
      ],
      "body":"{}"
   }
]
```

> Example Response

```shell
[
   {
      "requestId":1,
      "statusCode":200,
      "headers":[
         {
            "name":"Content-type",
            "value":"text/html"
         },
         {
            "name":"X-Mifos-Platform-TenantId",
            "value":"text/html"
         }
      ],
      "body":"{\"officeId\":1,\"clientId\":909,\"resourceId\":909}"
   },
   {
      "requestId":2,
      "statusCode":200,
      "headers":[
         {
            "name":"Content-type",
            "value":"text/html"
         }
      ],
      "body":"{\"officeId\":1,\"clientId\":909,\"loanId\":212,\"resourceId\":212}"
   },
   {
      "requestId":3,
      "statusCode":200,
      "headers":[
         {
            "name":"Content-type",
            "value":"text/html"
         }
      ],
      "body":"{\"officeId\":1,\"clientId\":909,\"loanId\":212,\"resourceId\":155}"
   },
   {
      "requestId":4,
      "statusCode":200,
      "headers":[
         {
            "name":"Content-type",
            "value":"text/html"
         }
      ],
      "body":"[
	   {
	      \"id\":155,
	      \"chargeId\":2,
	      \"name\":\"Charge_Loans_GEQJC5\",
	      \"chargeTimeType\":{
	         \"id\":1,
	         \"code\":\"chargeTimeType.disbursement\",
	         \"value\":\"Disbursement\"
	      },
	      \"chargeCalculationType\":{
	         \"id\":2,
	         \"code\":\"chargeCalculationType.percent.of.amount\",
	         \"value\":\"% Amount\"
	      },
	      \"percentage\":100.000000,
	      \"amountPercentageAppliedTo\":10000.000000,
	      \"currency\":{
	         \"code\":\"USD\",
	         \"name\":\"USDollar\",
	         \"decimalPlaces\":2,
	         \"displaySymbol\":\"\$\",
	         \"nameCode\":\"currency.USD\",
	         \"displayLabel\":\"US Dollar \$\"
	      },
	      \"amount\":10000.000000,
	      \"amountPaid\":0,
	      \"amountWaived\":0,
	      \"amountWrittenOff\":0,
	      \"amountOutstanding\":10000.000000,
	      \"amountOrPercentage\":100.000000,
	      \"penalty\":false,
	      \"chargePaymentMode\":{
	         \"id\":0,
	         \"code\":\"chargepaymentmode.regular\",
	         \"value\":\"Regular\"
	      },
	      \"paid\":false,
	      \"waived\":false,
	      \"chargePayable\":false
	   }
	]"
   }
]
```

### HTTP Request
`POST https://DomainName/api/v1/batches`

The Mifos X Batch API enables a consumer to access significant amounts of data in a single call or to make changes to several objects at once. Batching allows a consumer to pass instructions for several operations in a single HTTP request. A consumer can also specify dependencies between related operations. Once all operations have been completed, a consolidated response will be passed back and the HTTP connection will be closed.

The Batch API takes in an array of logical HTTP requests represented as JSON arrays - each request has a requestId (the id of a request used to specify the sequence and as a dependency between requests), a method (corresponding to HTTP method GET/PUT/POST/DELETE etc.), a relativeUrl (the portion of the URL after https://example.org/api/v2/), optional headers array (corresponding to HTTP headers), optional reference parameter if a request is dependent on another request and an optional body (for POST and PUT requests). The Batch API returns an array of logical HTTP responses represented as JSON arrays - each response has a requestId, a status code, an optional headers array and an optional body (which is a JSON encoded string).

Batch API uses Json Path to handle dependent parameters. For example, if request '2' is referencing request '1' and in the "body" or in "relativeUrl" of request '2', there is a dependent parameter (which will look like "$.parameter_name"), then Batch API will internally substitute this dependent parameter from the response body of request '1'.

Batch API is able to handle deeply nested dependent requests as well nested parameters. As shown in the example, requests are dependent on each other as, 1<--2<--6, i.e a nested dependency, where request '6' is not directly dependent on request '1' but still it is one of the nested child of request '1'. In the same way Batch API could handle a deeply nested dependent value, such as {..[..{..,$.parameter_name,..}..]}.

## Batch requests in a single transaction

> Example Request Payload

```shell
[
  {
    "requestId":1,
    "relativeUrl":"clients",
    "method":"POST",
    "headers":[
      {
        "name":"Content-type",
        "value":"text/html"
      },
      {
        "name":"X-Mifos-Platform-TenantId",
        "value":"text/html"
      }
    ],
    "body":"{
    	 \"officeId\": 1,
    	 \"firstname\": \"Petra\",
    	 \"lastname\": \"Yton\",
    	 \"externalId\": \"externalId_4\",
    	 \"dateFormat\": \"dd MMMM yyyy\",
    	 \"locale\": \"en\", \"active\": true,
    	 \"activationDate\": \"04 March 2009\",
    	 \"submittedOnDate\": \"04 March 2009\"
    	 }"
  },
  {
    "requestId":2,
    "relativeUrl":"savingsaccounts",
    "method":"POST",
    "reference":1,
    "headers":[
      {
        "name":"Content-type",
        "value":"text/html"
      }
    ],
    "body":"{
              \"clientId\": \"$.clientId\",
              \"productId\": 1,
              \"locale\": \"en\",
              \"dateFormat\": \"dd MMMM yyyy\",
              \"submittedOnDate\": \"01 March 2011\"
            }"
  }
]
```

> Example Response

```shell
[
  {
    "requestId":1,
    "statusCode":200,
    "headers":[
      {
        "name":"Content-type",
        "value":"text/html"
      },
      {
        "name":"X-Mifos-Platform-TenantId",
        "value":"text/html"
      }
    ],
    "body":"{\"officeId\":1,\"clientId\":922,\"resourceId\":922}"
  },
  {
    "requestId":2,
    "statusCode":200,
    "headers":[
      {
        "name":"Content-type",
        "value":"text/html"
      }
    ],
    "body":"{\"officeId\":1,\"clientId\":922,\"savingsId\":116,\"resourceId\":116}"
  }
]
```
The Mifos X Batch API is also capable of executing all the requests in a single transaction, by setting a Query Parameter, "enclosingTransaction=true". So, if one or more of the requests in a batch returns an erroneous response all of the Data base transactions made by other successful requests will be rolled back.

If there has been a rollback in a transaction then a single response will be provided, with a '400' status code and a body consisting of the error details of the first failed request.

### HTTP Request
`POST https://DomainName/api/v1/batches?enclosingTransaction=true`

## Batch API Errors

In Batch API without "enclosingTransaction=true", if one of the response is erroneous, then an appropriate status code will be set for that request and the error message will be returned in it's "body", while all other requests will return successful response with a status code of "200".

## Available Command Strategies

These are the currently available command strategies within the Mifos Batch API. So, these listed operations can be executed using a Batch Request.

**Batch API - available commands**

* [Create a new Client](https://demo.openmf.org/api-docs/apiLive.htm#clients_create)
* [Update an existing Client](https://demo.openmf.org/api-docs/apiLive.htm#clients_update)
* [Apply a Loan to a Client](https://demo.openmf.org/api-docs/apiLive.htm#loans_create)
* [Apply Savings to a Client](https://demo.openmf.org/api-docs/apiLive.htm#savingsaccounts_create)
* [Add a new Loan Charge](https://demo.openmf.org/api-docs/apiLive.htm#loans_charges_create)
* [Collect an existing Loan Charge](https://demo.openmf.org/api-docs/apiLive.htm#loans_charges_retrieve)
* [Activate a Pending Client](https://demo.openmf.org/api-docs/apiLive.htm#clients_activate)
* [Approve a Pending Loan](https://demo.openmf.org/api-docs/apiLive.htm#loans_approve)
* [Disburse a Loan](https://demo.openmf.org/api-docs/apiLive.htm#loans_disburse)
