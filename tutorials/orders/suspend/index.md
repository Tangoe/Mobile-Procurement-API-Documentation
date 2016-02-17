---
layout: default
title: Tutorials - SUSPEND Order
---


# How to Suspend a Service Asset

**This tutorial provides step-by-step instructions for creating, confirming, and submitting an order to temporarily suspend a specific service asset that is currently active.**

<br />

## Authentication

This API uses the OAuth2 standard for authentication. Specifically, it supports two grant types: *Implicit* and *Resource Owner Password Credentials*. For details regarding how to use this standard to authenticate when making your API calls, please refer to the  [Security and Authentication](/concepts/security/) page.

<br />

## Step 1. Build the request body that is required 

Next, you will need to compile the JSON that will be submitted in the request body. This JSON includes all of the data that the backend system requires to process this order.

For a SUSPEND, this JSON typically includes the following pieces:

* Transaction type (i.e., SUSPEND).

* Service asset ID for the cellular service that is to be suspended. This ID can be obtained via the **/assets/service** endpoint.

* All required and optional order properties. Refer to the <a href="/tutorials/properties">Obtaining Order Properties</a> page for steps how to identify the properties that are relevant for your order.



Here is an example of what the fully-assembled request body JSON might look like:

```
{
  "orderRequest": {
    "externalOrderNumber": "AZ99087547-001",
    "transactionType": "SUSPEND",
    "serviceId": "38382335",
    "properties": [
      {
        "groupId": "MISCELLANEOUS",
        "fields": [
          {
            "id": "CONTACT_NUMBER",
            "type": "TEXT",
            "text": "2035559999"
          },
          {
            "id": "ADDITIONAL_INSTRUCTIONS",
            "type": "TEXT",
            "text": "Please confirm with account holder that service has been successfully suspended."
          },
          {
            "id": "SERVICE_NUMBER",
            "type": "TEXT",
            "text": "2035554468"
          },
          {
            "id": "HOLD_SERVICE",
            "type": "CHOICE",
            "choice": {
              "id": "peter.edwards.acme" 
            }
          },
          {
            "id": "PREFERRED_SUSPENSION_DATE",
            "type": "DATE",
            "date": "2016-12-22T00:00:00+00:00"
          }
        ]
      }
    ]
  }
}
```
<br />



## Step 2: Set required request headers.

There are multiple HTTP headers that may be passed as part of your API request. They are used to confirm that the caller is authorized to make this request, along with optional filtering (when appropriate). See the [Request Headers](/concepts/headers/) page for more information about our supported headers.

### Required Headers:

* **X-TNGO-TENANT** - Used to identify the specific tenant (i.e., customer) for which the API is being called.

### Optional Headers:

* **X-TNGO-CONTEXT-COMPANYEMPLOYEEID** -- The employee ID assigned by the tenant/customer (e.g., employee’s email address, etc.). 

* **X-TNGO-CONTEXT-EMPLOYEEID** -- The employee ID assigned by Tangoe. 

* **X-TNGO-CONTEXT-HIERARCHYID** -- The Tangoe-assigned ID that is used to specify the organizational hierarchy to be used for the API call.

<br />


## Step 3: Indicate if you wish to obtain an order confirmation.

Prior to submitting the order for vendor processing, you will probably want to first obtain an order confirmation. The confirmation returns all of the IDs passed in with the request body, along with the human-readable data that fully describes what was referenced by those IDs (e.g., device name, vendor, price). This is useful for API consumers who wish to present the order request back to their end user so they can verify it for accuracy.

To obtain a confirmation, set the **confirm** query parameter to true.

<br />

## Step 4: Obtain the order confirmation by calling the /orders endpoint via HTTP POST. 

Your request, containing the JSON request body and confirm query parameter set to true, is now ready. Submit it via the HTTP POST method to the **/orders** endpoint.

If successful, the API will return a response with a 200 HTTP status code and containing JSON that fully describes the identifiers that were submitted. This data can then be presented back to the end user for verification and correction (if necessary). 

Here is an example of what the order confirmation response might look like:

```
{
  "orderConfirmation": {
    "accountHolder": {
      "_meta": {
        "href": "https://tg-mobility.cloudhub.io/mobility/v1/employees/28673732"
      },
      "companyEmployeeId": "peter.edwards.acme",
      "department": {
        "id": "11569020385",
        "name": "Development"
      },
      "email": "peter.edwards@acme.com",
      "firstName": "Peter",
      "id": "28673732",
      "lastName": "Edwards",
      "officePhone": "2038599360",
      "status": "ACTIVE"
    },
    "device": {
      "accessMethod": "CDMA_PCS",
      "companyAssetId": "0123ABC",
      "id": "35362361",
      "macAddress": "90:33:20:94:33",
      "manufacturer": {
        "_meta": {
          "href": "https://tg-mobility.cloudhub.io/mobility/v1/manufacturers/Apple"
        },
        "id": "Apple",
        "logoUrl": "https://commcareqa.tangoe.com/manage/images/manufacturers/Apple.gif",
        "name": "Apple"
      },
      "model": "iPhone 6 Plus (16GB) gold",
      "serialNumber": {
        "type": "IMEI",
        "value": "7738293847839"
      },
      "simId": "830002222211123"
    },
    "externalOrderNumber": "AZ99087547-001",
    "properties": [
      {
        "fields": [
          {
            "id": "CONTACT_NUMBER",
            "label": "Contact Phone Number",
            "text": "2035559999",
            "type": "TEXT"
          },
          {
            "id": "ADDITIONAL_INSTRUCTIONS",
            "label": "Additional Instructions",
            "text": "Please confirm with account holder that service has been successfully suspended.",
            "type": "TEXT"
          },
          {
            "id": "SERVICE_NUMBER",
            "label": "Service Number",
            "text": "2035554468",
            "type": "TEXT"
          },
          {
            "choice": {
              "id": "peter.edwards.acme"
            },
            "id": "HOLD_SERVICE",
            "label": "Hold Service",
            "type": "CHOICE"
          },
          {
            "date": "2016-12-22T00:00:00Z",
            "id": "PREFERRED_SUSPENSION_DATE",
            "label": "Preferred Suspension Date",
            "type": "DATE"
          }
        ],
        "groupId": "MISCELLANEOUS"
      }
    ],
    "region": {
      "_meta": {
        "href": "https://tg-mobility.cloudhub.io/mobility/v1/regions/70144640"
      },
      "id": "70144640",
      "name": "United States"
    },
    "requestedBy": {
      "_meta": {
        "href": "https://tg-mobility.cloudhub.io/mobility/v1/employees/28673732"
      },
      "companyEmployeeId": "peter.edwards.acme",
      "department": {
        "id": "11569020385",
        "name": "Development"
      },
      "email": "peter.edwards@acme.com",
      "firstName": "Peter",
      "id": "28673732",
      "lastName": "Edwards",
      "officePhone": "2038599360",
      "status": "ACTIVE"
    },
    "service": {
      "_meta": {
        "href": "https://tg-mobility.cloudhub.io/mobility/v1/assets/services/38382335"
      },
      "contractDates": {
        "activated": "2014-02-15T06:00:00Z",
        "end": "2017-02-18T06:00:00Z",
        "lastDeviceUpgrade": "2015-02-18T06:00:00Z",
        "start": "2015-02-18T06:00:00Z"
      },
      "id": "38382335",
      "serviceNumber": "2035554468",
      "status": "ACTIVE"
    },
    "transactionType": "SUSPEND"
  },
  "status": "SUCCESS"
}
```
<br/>


## Step 5: Submit Order Request

Once the request has been confirmed, it can be re-submitted for processing via HTTP POST. Before submitting, be sure to set the confirm query parameter to false (or remove it, since the default is false). 

If the submission is successful, the API will once again return a response containing JSON with an HTTP status code of 200. This response also will include the newly-assigned order number (i.e., orderId). 
 
Here is an example of what the order response might look like:

```
{
  "order": {
    "_meta": {
      "href": "https://tg-mobility.cloudhub.io/mobility/v1/orders/8711660"
    },
    "accountHolder": {
      "_meta": {
        "href": "https://tg-mobility.cloudhub.io/mobility/v1/employees/28673732"
      },
      "companyEmployeeId": "peter.edwards.acme",
      "department": {
        "id": "11569020385",
        "name": "Development"
      },
      "email": "peter.edwards@acme.com",
      "firstName": "Peter",
      "id": "28673732",
      "lastName": "Edwards",
      "officePhone": "2038599360",
      "status": "ACTIVE"
    },
    "dateSubmitted": "2016-01-29T21:14:05Z",
    "externalOrderNumber": "AZ99087547-001",
    "orderId": "8711660",
    "orderSegments": {
      "items": [
        {
          "_meta": {
            "hrefHistory": "https://tg-mobility.cloudhub.io/mobility/v1/orders/8711660/history?orderSegment=8711661",
            "hrefShipments": "https://tg-mobility.cloudhub.io/mobility/v1/orders/8711660/shipments?orderSegment=8711661"
          },
          "accessories": [],
          "features": [],
          "orderSegmentId": "8711661",
          "plan": {
            "optionalFeatures": []
          },
          "status": "ORDER_SUBMITTED",
          "vendor": {
            "_meta": {
              "href": "https://tg-mobility.cloudhub.io/mobility/v1/vendors/105"
            },
            "id": "105",
            "logoUrl": "https://commcareqa.tangoe.com/manage/images/carrier/logo_SPR.gif",
            "name": "Sprint"
          }
        }
      ]
    },
    "properties": [
      {
        "fields": [
          {
            "id": "CONTACT_NUMBER",
            "label": "Contact Phone Number",
            "text": "2035559999",
            "type": "TEXT"
          },
          {
            "id": "ADDITIONAL_INSTRUCTIONS",
            "label": "Additional Instructions",
            "text": "Please confirm with account holder that service has been successfully suspended.",
            "type": "TEXT"
          },
          {
            "id": "SERVICE_NUMBER",
            "label": "Service Number",
            "text": "2035554468",
            "type": "TEXT"
          },
          {
            "choice": {
              "id": "peter.edwards.acme"
            },
            "id": "HOLD_SERVICE",
            "label": "Hold Service",
            "type": "CHOICE"
          },
          {
            "date": "2016-12-22T00:00:00Z",
            "id": "PREFERRED_SUSPENSION_DATE",
            "label": "Preferred Suspension Date",
            "type": "DATE"
          }
        ],
        "groupId": "MISCELLANEOUS"
      }
    ],
    "requestedBy": {
      "_meta": {
        "href": "https://tg-mobility.cloudhub.io/mobility/v1/employees/28673732"
      },
      "companyEmployeeId": "peter.edwards.acme",
      "department": {
        "id": "11569020385",
        "name": "Development"
      },
      "email": "peter.edwards@acme.com",
      "firstName": "Peter",
      "id": "28673732",
      "lastName": "Edwards",
      "officePhone": "2038599360",
      "status": "ACTIVE"
    },
    "serviceNumber": "2035554468",
    "shipTo": {
      "address": []
    },
    "status": "ORDER_SUBMITTED",
    "transactionType": "SUSPEND"
  },
  "status": "SUCCESS"
}
```
<br/>