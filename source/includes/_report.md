# Report

## Retrieve Report Template


> The above command returns JSON structured like this:

```json
{
  "allowedReportTypes": [
    "Table",
    "Pentaho",
    "Chart"
  ],
  "allowedReportSubTypes": [
    "Bar",
    "Pie"
  ],
  "allowedParameters": [
    {
      "id": 1,
      "parameterName": "startDateSelect"
    },
    {
      "id": 2,
      "parameterName": "endDateSelect"
    },
    {
      "id": 3,
      "parameterName": "obligDateTypeSelect"
    },
    {
      "id": 5,
      "parameterName": "OfficeIdSelectOne"
    },
    {
      "id": 6,
      "parameterName": "loanOfficerIdSelectAll"
    },
    {
      "id": 10,
      "parameterName": "currencyIdSelectAll"
    },
    {
      "id": 20,
      "parameterName": "fundIdSelectAll"
    },
    {
      "id": 25,
      "parameterName": "loanProductIdSelectAll"
    },
    {
      "id": 26,
      "parameterName": "loanPurposeIdSelectAll"
    },
    {
      "id": 100,
      "parameterName": "parTypeSelect"
    }
  ]
}
```

This is a convenience resource. It can be useful when building maintenance user interface screens for client applications. The template data returned consists of any or all of:

* Field Defaults
* Allowed Value Lists

### HTTP Request

`GET https://DomainName/api/v1/reports/template`

``

## Create a Report

```shell
curl "https://DomainName/api/v1/reports/template"
  -X POST
  -H "Authorization: Basic meowmeowmeow"
  -H "Content-Type: application/json"
  -H "Accept: application/json"
  -d "{
 "reportName":"Completely New Report",
 "reportType":"Table",
 "reportSubType":"",
 "reportCategory":"Loan",
 "useReport":"false",
 "description":"Just\nAn\nExample",
 "reportSql":"select 'very good sql' as AComment",
 "reportParameters":[{"id":"","parameterId":"5","reportParameterName":""},{"id":"","parameterId":"6","reportParameterName":""}]
}
"
```

> The above command returns JSON structured like this:

```json
{
	"resourceId": 132
}
```

### HTTP Request

`POST https://DomainName/api/v1/report`

``

## List Report


> The above command returns JSON structured like this:

```json
[
  {
    "id": 1,
    "reportName": "Client Listing",
    "reportType": "Table",
    "reportCategory": "Client",
    "description": "Individual Client Report\r\n\r\nLists the small number of defined fields on the client table.  Would expect to copy this \n\nreport and add any \u0027one to one\u0027 additional data for specific tenant needs.\r\n\r\nCan be run for any size MFI but you\u0027d expect it only to be run within a branch for \n\nlarger ones.  Depending on how many columns are displayed, there is probably is a limit of about 20/50k clients returned for html display (export to excel doesn\u0027t \n\nhave that client browser/memory impact).",
    "coreReport": true,
    "useReport": true,
    "reportParameters": [
      {
        "id": 1,
        "parameterId": 5,
        "parameterName": "OfficeIdSelectOne"
      }
    ]
  },
  {
    "id": 2,
    "reportName": "Client Loans Listing",
    "reportType": "Table",
    "reportCategory": "Client",
    "description": "Individual Client Report\n\nPretty \n\nwide report that lists the basic details of client loans.  \n\nCan be run for any size MFI but you\u0027d expect it only to be run within a branch for larger ones.  \n\nThere is probably is a limit of about 20/50k clients returned for html display (export to excel doesn\u0027t have that client browser/memory impact).",
    "coreReport": false,
    "useReport": true,
    "reportParameters": [
      {
        "id": 2,
        "parameterId": 5,
        "parameterName": "OfficeIdSelectOne"
      },
      {
        "id": 3,
        "parameterId": 6,
        "parameterName": "loanOfficerIdSelectAll"
      },
      {
        "id": 4,
        "parameterId": 10,
        "parameterName": "currencyIdSelectAll"
      },
      {
        "id": 5,
        "parameterId": 20,
        "parameterName": "fundIdSelectAll"
      },
      {
        "id": 6,
        "parameterId": 25,
        "parameterName": "loanProductIdSelectAll"
      },
      {
        "id": 7,
        "parameterId": 26,
        "parameterName": "loanPurposeIdSelectAll"
      }
    ]
  }
]
```

### HTTP Request

`GET https://DomainName/api/v1/reports`

``

## Retrieve a report


> The above command returns JSON structured like this:

```json
{
  "id": 1,
  "reportName": "Client Listing",
  "reportType": "Table",
  "reportCategory": "Client",
  "description": "Individual Client Report\r\n\r\nLists the small number of defined fields on the client table.  Would expect to copy this \n\nreport and add any \u0027one to one\u0027 additional data for specific tenant needs.\r\n\r\nCan be run for any size MFI but you\u0027d expect it only to be run within a branch for \n\nlarger ones.  Depending on how many columns are displayed, there is probably is a limit of about 20/50k clients returned for html display (export to excel doesn\u0027t \n\nhave that client browser/memory impact).",
  "reportSql": "select \nconcat(repeat(\"..\",   \n   ((LENGTH(ounder.`hierarchy`) - LENGTH(REPLACE(ounder.`hierarchy`, \u0027.\u0027, \u0027\u0027)) - 1))), ounder.`name`) as \"Office/Branch\",\n c.account_no as \"Client Account No.\",  \nc.display_name as \"Name\",  \nr.enum_message_property as \"Status\",\nc.activation_date as \"Activation\", c.external_id as \"External Id\"\nfrom m_office o \njoin m_office ounder on ounder.hierarchy like concat(o.hierarchy, \u0027%\u0027)\nand ounder.hierarchy like concat(\u0027${currentUserHierarchy}\u0027, \u0027%\u0027)\njoin m_client c on c.office_id \u003d ounder.id\nleft join r_enum_value r on r.enum_name \u003d \u0027status_enum\u0027 and r.enum_id \u003d c.status_enum\nwhere o.id \u003d ${officeId}\norder by ounder.hierarchy, c.account_no",
  "coreReport": true,
  "useReport": true,
  "reportParameters": [
    {
      "id": 1,
      "parameterId": 5,
      "parameterName": "OfficeIdSelectOne"
    }
  ]
}
```

### HTTP Request

`GET https://DomainName/api/v1/reports/`

