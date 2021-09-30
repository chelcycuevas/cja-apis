---
keywords:
  - Experience Platform
  - API Documentation
  - JavaScript
title: Audit Logs
description: Get a list of audit logs using the API.
---

# CJA Audit Logs

The Audit Log API allows you to retrieve a list of audit log records using a GET or POST method through Adobe I/O. The GET endpoint provides a way to add filters through query string parameters. The POST endpoint offers greater flexibility to your search criteria.

## Get Audit Logs

The GET endpoint is designed for use when few or no filters are needed. If filters are included by adding query string parameters, each query string parameter will filter the list of audit logs using an 'AND' condition.

`GET https://cja.adobe.io/auditlogs`

Hitting this endpoint with no query string parameters will return the last 1000 audit log records in descending order.


### Query String Parameters
| Query String | Type | Description | Possible Values|
| --- | --- | ---------- | ----- |
| startDate | String | Begin date range. If startDate is set, endDate must also be defined. | 2021-01-01T00:00:00-07 |
| endDate | String | End date range. If endDate is set, startDate must also be defined. | 2021-02-01T00:00:00-07 |
| action | Enum | The type of action a user or system can make. | CREATE, EDIT, DELETE, LOGIN_FAILED, LOGIN_SUCCESSFUL, API_REQUEST |
| component | Enum | The type of component. | CALCULATED_METRIC, CONNECTION, DATA_GROUP, DATA_VIEW, DATE_RANGE, FILTER, MOBILE, PROJECT, REPORT, SCHEDULED_PROJECT |
| componentId | String | The id of the component. | -- |
| userType | Enum | The type of user. | IMS, OKTA |
| userId | String | The id of the user. | -- |
| userEmail | String | The email address of the user. | User defined |
| description | String | The description of the audit log. | -- |
| pageSize | Integer | Number of results per page. If left null, the default size will be set to 100. | 10 |
| pageNumber | Integer | Page number (base 0 - first page is "0") | 0 |

## Search Audit Logs

If you need to retrieve a list of audit logs using 'OR' criteria, you will want to utilize the POST /auditlogs/search endpoint.

`POST https://cja.adobe.io/auditlogs/search`


An example POST request body:

```
{
  "criteria": {
    "fieldOperator": "AND",
    "fields": [
      {
        "fieldType": "COMPONENT",
        "value": [
          "FILTER",
          "CALCULATED_METRIC"
        ],
        "operator": "IN"
      },
      {
        "fieldType": "DESCRIPTION",
        "value": [
          "created"
        ],
        "operator": "CONTAINS"
      }
    ],
    "subCriteriaOperator": "AND",
    "subCriteria": {
      "fieldOperator": "OR",
      "fields": [
        {
          "fieldType": "USER_EMAIL",
          "value": [
            "jane"
          ],
          "operator": "CONTAINS"
        },
        {
          "fieldType": "USER_EMAIL",
          "value": [
            "john"
          ],
          "operator": "CONTAINS"
        }
      ],
      "subCriteriaOperator": null,
      "subCriteria": null
    }
  },
  "pageSize": 100,
  "pageNumber": 0
}
```

### fieldType
  * COMPONENT
  * COMPONENT_ID
  * USER
  * USER_ID
  * USER_EMAIL
  * BEGIN_DATE_RANGE
  * END_DATE_RANGE
  * ACTION
  * DESCRIPTION

#### Notes

_If 'BEGIN_DATE_RANGE' is set as a fieldType, 'END_DATE_RANGE' must also be set. These fieldTypes must use 'EQUALS' as their 'operator'._

### operator

* EQUALS
* NOT_EQUALS
* CONTAINS
* IN

### fieldOperator

* AND
* OR
