---
layout: default
title: Tutorials - SWAP_DEVICE Order
---


# How to Changing the Device that is Assigned to a Service Asset

**This tutorial provides step-by-step instructions for creating, confirming, and submitting an order to change the device that is associated with a specific service asset.**
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

For a SWAP_DEVICE, this JSON typically includes the following pieces:

* Transaction type (i.e., SWAP_DEVICE).
* Service ID of the service asset to be updated with a different device assignment.
* Account Holder Company ID (or Account Holder ID) for the user to whom the asset is assigned.
* The properties that are required for changing the device associated with a specific service asset for the specific vendor.
 
 
Here is an example:

```
{
  "orderRequest": {
    "externalOrderNumber": "AZ99087547-001",
    "transactionType": "SWAP_DEVICES",
    "serviceId": "31647614",
    "newDevice": {
      "manufacturerId": "Apple",
      "modelId": "iPhone 6 (64GB) - Space Gray",
      "otherMakeModelName": "",
      "serialNumber": {
        "type": "IMEI",
        "value": "123456789001230"
      }
    },
    "existingDevice": {
      "manufacturerId": "Other",
      "modelId": "Other",
      "otherMakeModelName": "Acme StarPhone 2000",
      "serialNumber": {
        "type": "IMEI",
        "value": "012345678901007"
      },
      "simId": "89 91 10 1200 00 320451 3"
    },    
    "properties": [
      {
        "groupId": "MISCELLANEOUS",
        "fields": [
          {
            "id": "CONTACT_NUMBER",
            "label": "Contact Phone Number",
            "type": "TEXT",
            "text": "2035559360"
          },
          {
            "id": "ADDITIONAL_INSTRUCTIONS",
            "label": "Additional Instructions",
            "type": "TEXT",
            "text": "Please confirm serial number for new device"
          },
          {
            "id": "SERVICE_NUMBER",
            "label": "Service Number",
            "type": "TEXT",
            "text": "7654388307"
          },
          {
            "id": "PREFERRED_AREA_CODE",
            "label": "Preferred Area Code",
            "type": "TEXT",
            "text": "203"
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
        "href": "https://tg-mobility.cloudhub.io/mobility/v1/employees/28671465"
      },
      "companyEmployeeId": "mike.mcpadden.xx1",
      "department": {
        "id": "10654876521",
        "name": "Marketing"
      },
      "email": "michael.mcpadden@tangoe.com",
      "firstName": "Michael",
      "id": "28671465",
      "lastName": "McPadden",
      "officePhone": "2038599360",
      "status": "ACTIVE"
    },
    "existingDevice": {
      "manufacturer": {
        "_meta": {
          "href": "https://tg-mobility.cloudhub.io/mobility/v1/manufacturers/Other"
        },
        "id": "Other",
        "logoUrl": "https://qa3.traq.com/manage/images/manufacturers/Other.gif",
        "name": "Other"
      },
      "model": {
        "_meta": {
          "href": "https://tg-mobility.cloudhub.io/mobility/v1/manufacturers/Other/models/Other"
        },
        "id": "Other",
        "name": "Other"
      },
      "serialNumber": {
        "type": "IMEI",
        "value": "012345678901007"
      },
      "simId": "89 91 10 1200 00 320451 3"
    },
    "externalOrderNumber": "AZ99087547-001",
    "newDevice": {
      "manufacturer": {
        "_meta": {
          "href": "https://tg-mobility.cloudhub.io/mobility/v1/manufacturers/Apple"
        },
        "id": "Apple",
        "logoUrl": "https://qa3.traq.com/manage/images/manufacturers/Apple.gif",
        "name": "Apple"
      },
      "model": {
        "_meta": {
          "href": "https://tg-mobility.cloudhub.io/mobility/v1/manufacturers/Apple/models/iPhone%206%20(64GB)%20-%20Space%20Gray"
        },
        "id": "iPhone 6 (64GB) - Space Gray",
        "name": "iPhone 6 (64GB) - Space Gray"
      },
      "serialNumber": {
        "type": "IMEI",
        "value": "123456789001230"
      }
    },
    "properties": [
      {
        "fields": [
          {
            "id": "CONTACT_NUMBER",
            "label": "Contact Phone Number",
            "text": "2035559360",
            "type": "TEXT"
          },
          {
            "id": "ADDITIONAL_INSTRUCTIONS",
            "label": "Additional Instructions",
            "text": "Please confirm serial number for new device",
            "type": "TEXT"
          },
          {
            "id": "SERVICE_NUMBER",
            "label": "Service Number",
            "text": "7654388307",
            "type": "TEXT"
          },
          {
            "id": "PREFERRED_AREA_CODE",
            "label": "Preferred Area Code",
            "text": "203",
            "type": "TEXT"
          }
        ],
        "groupId": "MISCELLANEOUS"
      }
    ],
    "requestedBy": {
      "_meta": {
        "href": "https://tg-mobility.cloudhub.io/mobility/v1/employees/28671465"
      },
      "companyEmployeeId": "mike.mcpadden.xx1",
      "department": {
        "id": "10654876521",
        "name": "Marketing"
      },
      "email": "michael.mcpadden@tangoe.com",
      "firstName": "Michael",
      "id": "28671465",
      "lastName": "McPadden",
      "status": "ACTIVE"
    },
    "service": {
      "_meta": {
        "href": "https://tg-mobility.cloudhub.io/mobility/v1/assets/services/31647614"
      },
      "contractDates": {
        "activated": "2014-05-17T05:00:00Z",
        "end": "2016-05-17T05:00:00Z",
        "lastDeviceUpgrade": "2014-05-17T05:00:00Z",
        "start": "2014-05-17T05:00:00Z"
      },
      "id": "31647614",
      "serviceNumber": "7654388307",
      "status": "ACTIVE"
    },
    "transactionType": "SWAP_DEVICES"
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
      "href": "https://tg-mobility.cloudhub.io/mobility/v1/orders/8066456"
    },
    "accountHolder": {
      "_meta": {
        "href": "https://tg-mobility.cloudhub.io/mobility/v1/employees/28671465"
      },
      "companyEmployeeId": "mike.mcpadden.xx1",
      "department": {
        "id": "10654876521",
        "name": "Marketing"
      },
      "email": "michael.mcpadden@tangoe.com",
      "firstName": "Michael",
      "id": "28671465",
      "lastName": "McPadden",
      "officePhone": "2038599360",
      "status": "ACTIVE"
    },
    "dateSubmitted": "2015-11-24T21:34:09Z",
    "externalOrderNumber": "AZ99087547-001",
    "orderId": "8066456",
    "orderSegments": {
      "items": []
    },
    "properties": [
      {
        "fields": [
          {
            "id": "CONTACT_NUMBER",
            "label": "Contact Phone Number",
            "text": "2035559360",
            "type": "TEXT"
          },
          {
            "id": "ADDITIONAL_INSTRUCTIONS",
            "label": "Additional Instructions",
            "text": "Please confirm serial number for new device",
            "type": "TEXT"
          },
          {
            "id": "SERVICE_NUMBER",
            "label": "Service Number",
            "text": "7654388307",
            "type": "TEXT"
          },
          {
            "id": "PREFERRED_AREA_CODE",
            "label": "Preferred Area Code",
            "text": "203",
            "type": "TEXT"
          }
        ],
        "groupId": "MISCELLANEOUS"
      }
    ],
    "requestedBy": {
      "_meta": {
        "href": "https://tg-mobility.cloudhub.io/mobility/v1/employees/28671465"
      },
      "companyEmployeeId": "mike.mcpadden.xx1",
      "department": {
        "id": "10654876521",
        "name": "Marketing"
      },
      "email": "michael.mcpadden@tangoe.com",
      "firstName": "Michael",
      "id": "28671465",
      "lastName": "McPadden",
      "status": "ACTIVE"
    },
    "serviceNumber": "7654388307",
    "shipTo": {
      "address": []
    },
    "status": "ORDER_SUBMITTED",
    "transactionType": "SWAP_DEVICES"
  },
  "status": "SUCCESS"
}
```
<br/>


