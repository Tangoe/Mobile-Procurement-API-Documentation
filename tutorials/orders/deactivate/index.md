---
layout: default
title: Tutorials - DEACTIVATE Order
---


# How to Deactivcate a Service Asset

**This tutorial provides step-by-step instructions for creating, confirming, and submitting an order to deactivate a specific service asset.**
<br />

## Authentication

See the [Security and Authentication](/concepts/security/) page for info regarding the supported OAuth2 grant types: Implicit, Resource Owner Password Credentials.
<br />

## Step 1: Set Necessary Headers

A few HTTP headers are required to be passed as part of your API request. They are used to confirm that the caller is authorized to make this request, along with optional filtering (when appropriate). 

### Required headers:

* **X-TNGO-TENANT** - Used to identify the specific tenant (i.e., customer) for which the API is being called.
See the [Headers](/concepts/headers/) page for more information about these headers.

### Optional headers:

* **X-TNGO-CONTEXT-COMPANYEMPLOYEEID**  -- Used to specify a company employee ID that defines the context of the call. This header is only checked if the X-TNGO-CONTEXT-EMPLOYEEID header is blank or does not exist. If this header does not exist, the context will be set to the authenticated user. 
*SECURITY NOTE:* The authenticated user must be authorized to access (at a minimum) all the same data that the employee referenced in this header is authorized to access.

* **X-TNGO-CONTEXT-EMPLOYEEID** --  Used to specify an employee ID that defines the context of the call. If this header does not exist or is blank, then the X-TNGO-CONTEXT-COMPANYEMPLOYEEID header will be used instead. 
*SECURITY NOTE:* The authenticated user must be authorized to access (at a minimum) all the same data that the employee referenced in this header is authorized to access.

* **X-TNGO-CONTEXT-HIERARCHYID** --  Used to specify the organizational hierarchy to be used for the API call. If this header is blank or does not exist, the default hierarchy will be used.
<br />

## Step 2: Build JSON for Request Body

Next, you will need to compile the JSON that will be submitted in the request body. This JSON includes all of the data that the backend system requires to process this order.

For a DEACTIVATE order, this JSON typically includes the following pieces:

* Transaction type (i.e., DEACTIVATE).
* Service ID of the service asset to be deactivated.
* Account Holder Company ID (or Account Holder ID) for the user to whom the asset is assigned.
* The properties that are required for deactivating a service asset or the specific vendor.
 
 
Here is an example:

```
 {
  "orderRequest": {
    "externalOrderNumber": "AZ99087547-001",
    "transactionType": "DEACTIVATE",
    "serviceId": "31647891",
    "properties": [
      {
        "groupId": "MISCELLANEOUS",
        "fields": [
          {
            "id": "CONTACT_NUMBER",
            "type": "TEXT",
            "text": "2038599360"
          },
          {
            "id": "ADDITIONAL_INSTRUCTIONS",
            "type": "TEXT",
            "text": "Please confirm with account holder that service has been successfully deactivated."
          },
          {
            "id": "PREFERRED_DEACTIVATION_DATE",
            "type": "DATE",
            "date": "2016-12-28T00:00:00+00:00"
          },
          {
            "id": "SERVICE_NUMBER",
            "type": "TEXT",
            "text": "2489535406"
          },
          {
            "id": "Reason",
            "type": "CHOICE",
            "choice": {
              "id": "1"
            }
          }
        ]
      }
    ]
  }
}
```
<br />

## Step 3: Set Query Parameter(s), as Needed

The only query parameter that can be set when creating an order is:

* confirm  (boolean) - If true, the order will not be posted. Instead, the response will return a complete description of the order that can be used to create an order confirmation for the end user to review, correct, and then submit for processing.
<br />

## Step 4: Submit Order Request Confirmation

Your request, containing the JSON request body and confirm query parameter set to true, should now be ready. Submit it via HTTP POST to the orders endpoint.

If successful, the API will return a response with a 200 HTTP status code and containing JSON that fully describes the identifiers that were submitted. This data can then be presented back to the end user for verification and correction, if necessary. 

Here is an example of what the order confirmation JSON might look like:

```
{
  "orderConfirmation": {
    "accountHolder": {
      "_meta": {
        "href": "https://tg-mobility.cloudhub.io/mobility/v1/employees/28671524"
      },
      "companyEmployeeId": "mike.mcpadden.xx1",
      "department": {
        "id": "10652965372",
        "name": "Accounting"
      },
      "email": "michael.mcpadden@tangoe.com",
      "firstName": "Michael",
      "id": "28671524",
      "lastName": "McPadden",
      "officePhone": "203-859-9360",
      "status": "ACTIVE"
    },
    "externalOrderNumber": "AZ99087547-001",
    "properties": [
      {
        "fields": [
          {
            "id": "CONTACT_NUMBER",
            "label": "Contact Phone Number",
            "text": "2038599360",
            "type": "TEXT"
          },
          {
            "id": "ADDITIONAL_INSTRUCTIONS",
            "label": "Additional Instructions",
            "text": "Please confirm with account holder that service has been successfully deactivated.",
            "type": "TEXT"
          },
          {
            "date": "2016-12-28T00:00:00Z",
            "id": "PREFERRED_DEACTIVATION_DATE",
            "type": "DATE"
          },
          {
            "id": "SERVICE_NUMBER",
            "label": "Service Number",
            "text": "2489535406",
            "type": "TEXT"
          },
          {
            "choice": {
              "id": "1"
            },
            "id": "Reason",
            "type": "CHOICE"
          }
        ],
        "groupId": "MISCELLANEOUS"
      }
    ],
    "requestedBy": {
      "_meta": {
        "href": "https://tg-mobility.cloudhub.io/mobility/v1/employees/28671524"
      },
      "companyEmployeeId": "mike.mcpadden.xx1",
      "department": {
        "id": "10652965372",
        "name": "Accounting"
      },
      "email": "michael.mcpadden@tangoe.com",
      "firstName": "Michael",
      "id": "28671524",
      "lastName": "McPadden",
      "officePhone": "203-859-9360",
      "status": "ACTIVE"
    },
    "service": {
      "_meta": {
        "href": "https://tg-mobility.cloudhub.io/mobility/v1/assets/services/31647891"
      },
      "contractDates": {
        "activated": "2013-06-05T05:00:00Z",
        "end": "2016-06-05T05:00:00Z",
        "lastDeviceUpgrade": "2013-06-05T05:00:00Z",
        "start": "2013-06-05T05:00:00Z"
      },
      "id": "31647891",
      "serviceNumber": "2489535406",
      "status": "ACTIVE"
    },
    "transactionType": "DEACTIVATE"
  },
  "status": "SUCCESS"
}
```
<br/>

## Step 5: Submit Order Request
Once the request has been confirmed, it can be re-submitted for processing via HTTP POST. Before submitting, be sure to set the confirm query parameter to false (or remove it, since the default is false). 

If the submission is successful, the API will once again return a response containing JSON with an HTTP status code of 200. This response also will include the newly-assigned order number (i.e., orderId). 
 
Here is an example of what the order JSON might look like:

```
{
  "order": {
    "_meta": {
      "href": "https://tg-mobility.cloudhub.io/mobility/v1/orders/8054738"
    },
    "accountHolder": {
      "companyEmployeeId": "mike.mcpadden.xx1",
      "department": {
        "id": "10652965372",
        "name": "Accounting"
      },
      "email": "michael.mcpadden@tangoe.com",
      "firstName": "Michael",
      "id": "28671524",
      "lastName": "McPadden",
      "officePhone": "203-859-9360",
      "status": "ACTIVE"
    },
    "dateSubmitted": "2015-12-16T19:14:04Z",
    "externalOrderNumber": "AZ99087547-001",
    "orderId": "8054738",
    "properties": [
      {
        "fields": [
          {
            "id": "CONTACT_NUMBER",
            "label": "Contact Phone Number",
            "text": "2038599360",
            "type": "TEXT"
          },
          {
            "id": "ADDITIONAL_INSTRUCTIONS",
            "label": "Additional Instructions",
            "text": "Please confirm with account holder that service has been successfully deactivated.",
            "type": "TEXT"
          },
          {
            "date": "2016-12-28T00:00:00Z",
            "id": "PREFERRED_DEACTIVATION_DATE",
            "type": "DATE"
          },
          {
            "id": "SERVICE_NUMBER",
            "label": "Service Number",
            "text": "2489535406",
            "type": "TEXT"
          },
          {
            "choice": {
              "id": "1"
            },
            "id": "Reason",
            "type": "CHOICE"
          }
        ],
        "groupId": "MISCELLANEOUS"
      }
    ],
    "requestedBy": {
      "_meta": {
        "href": "https://tg-mobility.cloudhub.io/mobility/v1/employees/28671524"
      },
      "companyEmployeeId": "mike.mcpadden.xx1",
      "department": {
        "id": "10652965372",
        "name": "Accounting"
      },
      "email": "michael.mcpadden@tangoe.com",
      "firstName": "Michael",
      "id": "28671524",
      "lastName": "McPadden",
      "officePhone": "203-859-9360",
      "status": "ACTIVE"
    },
    "serviceNumber": "2489535406",
    "status": "ORDER_SUBMITTED",
    "transactionType": "DEACTIVATE"
  },
  "status": "SUCCESS"
}
```
<br/>