---
layout: default
title: Tutorials - UPGRADE Order
---


# How to Upgrade a Device and/or Service Plan

**This tutorial provides step-by-step instructions for creating, confirming, and submitting an order to upgrade a specific device and/or service plan.**

<br />

## Authentication

This API uses the OAuth2 standard for authentication. Specifically, it supports two grant types: *Implicit* and *Resource Owner Password Credentials*. For details regarding how to use this standard to authenticate when making your API calls, please refer to the  [Security and Authentication]({{site.url}}/concepts/security/) page.

<br />

## Step 1. Build the request body that is required. 

Next, you will need to compile the JSON that will be submitted in the request body. This JSON includes all of the data that the backend system requires to process this order.

For an UPGRADE, this JSON typically includes the following pieces:

* Transaction type (i.e., UPGRADE).

* Service asset ID for the cellular service, and/or associated device, that you wish to upgrade. This ID can be obtained via the **/assets/service** endpoint.

* Region ID for the country in which the device and/or service will be used. This ID can be obtained via the **/regions** endpoint.

* The postal code for the zone in which the device and/or service will be used. (Note: This is only required when the selected region is the United States of America.)

* Shopping cart object containing the IDs for what is being ordered (i.e., devices, plans, and/or plan features). These IDs can be obtained via the catalog endpoints (i.e., **/catalog/devices**, **/catalog/plans**, **/catalog/features**).
  * Optional feature IDs for a specific plan can be obtained from the **/catalog/plans/{id}** endpoint.
  * Please note that when upgrading to a plan offered by a different carrier, the new carrier's cellular network may use an different channel access method than your existing carrier (e.g., CDMA, GSM, etc.). In such case, your existing device will not be compatible with the new plan, so a new device must be selected as part of this order.

* All required and optional order properties. Refer to the <a href="{{site.url}}/tutorials/properties">Obtaining Order Properties</a> page for steps how to identify the properties that are relevant to your order.

* Shipping information. This is required whenever an order includes physical items. Refer to the <a href="{{site.url}}/tutorials/addresses">Formatting an Address</a> page for steps how to assemble the shipping components that are required for your order.


Here is an example of what the fully-assembled request body JSON might look like when upgrading both a device and its related serivce plan:

```
{
  "orderRequest": {
    "externalOrderNumber": "upgrade_device_and_service_test",
    "transactionType": "UPGRADE",
    "serviceId": "38382349",
    "regionId": "70144640",
    "postalCode": "78759",    
    "shoppingCart": {     
      "deviceId": "9194713904",
      "planId": "6568886698",
      "optionalFeatureIds": ["293","2082"]
    },
    "properties": [ 
      {
        "groupId": "MISCELLANEOUS",
        "fields": [
          {
            "id": "CONTACT_NUMBER",
            "type": "TEXT",
            "text": "1234567890"
          },
          {
            "id": "ADDITIONAL_INSTRUCTIONS",            
            "type": "TEXT",
            "text": "TEST BY USING Rest based API."
          },
          {
            "id": "SERVICE_NUMBER",            
            "type": "TEXT",
            "text": "2035557676"
          },
          {
      	    "id": "Reason",
            "type": "CHOICE",
            "choice": {
	     		"id": "18"
            }
          }
        ]
      }
    ],
    "shipTo": { 
      "name": "Peter Edwards",
      "regionId": "70144640",
      "address": [
        {
          "id": "LINE_1",
          "value": "35 Executive Blvd"
        },
        {
          "id": "CITY",
          "value": "Orange"
        },
        {
          "id": "STATE",
          "value": "CT"
        },
        {
          "id": "POSTAL_CODE",
          "value": "06477"
        }
      ],
      "expedite": true
    }
  }
}
```
<br />



## Step 2: Set required request headers.

There are multiple HTTP headers that may be passed as part of your API request. They are used to confirm that the caller is authorized to make this request, along with optional filtering (when appropriate). See the [Request Headers]({{site.url}}/concepts/headers/) page for more information about our supported headers.

### Required Headers:

* **X-TNGO-TENANT** - Used to identify the specific tenant (i.e., customer) for which the API is being called.

* **client_id** - Used to identify your client application as the one calling the API.

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
      "companyEmployeeId": "mike.mcpadden.xx1",
      "department": {
        "id": "11569020385",
        "name": "Development"
      },
      "email": "michael.mcpadden@tangoe.com",
      "firstName": "Mike",
      "id": "28673732",
      "lastName": "McPadden",
      "officePhone": "2035559999",
      "status": "ACTIVE"
    },
    "costSummary": [
      {
        "amount": 199.99,
        "currencyCode": "USD",
        "recurrence": "ONETIME"
      },
      {
        "amount": 261.65,
        "currencyCode": "USD",
        "recurrence": "MONTHLY"
      }
    ],
    "device": {
      "accessMethod": "CDMA",
      "companyAssetId": "Mike's Cookie",
      "id": "35362368",
      "macAddress": "90:33:20:94:31",
      "manufacturer": {
        "_meta": {
          "href": "https://tg-mobility.cloudhub.io/mobility/v1/manufacturers/LG"
        },
        "id": "LG",
        "logoUrl": "https://commcareqa.tangoe.com/manage/images/manufacturers/LG.gif",
        "name": "LG"
      },
      "model": "Cookie Plus",
      "serialNumber": {
        "type": "ESN",
        "value": "4289183847832"
      },
      "simId": "83924758342478392342"
    },
    "externalOrderNumber": "upgrade_device_and_service_test",
    "postalCode": "78759",
    "properties": [
      {
        "fields": [
          {
            "id": "CONTACT_NUMBER",
            "label": "Contact Phone Number",
            "text": "1234567890",
            "type": "TEXT"
          },
          {
            "id": "ADDITIONAL_INSTRUCTIONS",
            "label": "Additional Instructions",
            "text": "TEST BY USING Rest based API.",
            "type": "TEXT"
          },
          {
            "id": "SERVICE_NUMBER",
            "label": "Service Number",
            "text": "2035557676",
            "type": "TEXT"
          },
          {
            "choice": {
              "id": "18"
            },
            "id": "Reason",
            "type": "CHOICE"
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
      "companyEmployeeId": "mike.mcpadden.xx1",
      "department": {
        "id": "11569020385",
        "name": "Development"
      },
      "email": "michael.mcpadden@tangoe.com",
      "firstName": "Mike",
      "id": "28673732",
      "lastName": "McPadden",
      "officePhone": "2035559999",
      "status": "ACTIVE"
    },
    "service": {
      "_meta": {
        "href": "https://tg-mobility.cloudhub.io/mobility/v1/assets/services/38382349"
      },
      "contractDates": {
        "activated": "2010-01-20T06:00:00Z",
        "end": "2016-01-21T06:00:00Z",
        "lastDeviceUpgrade": "2014-01-21T06:00:00Z",
        "start": "2014-01-21T06:00:00Z"
      },
      "id": "38382349",
      "serviceNumber": "2035557676",
      "status": "ACTIVE"
    },
    "shipTo": {
      "address": [
        {
          "id": "LINE_1",
          "label": "Address Line1",
          "value": "35 Executive Blvd"
        },
        {
          "id": "CITY",
          "label": "City",
          "value": "Orange"
        },
        {
          "id": "STATE",
          "label": "State",
          "value": "CT"
        },
        {
          "id": "POSTAL_CODE",
          "label": "Zip Code",
          "value": "06477"
        }
      ],
      "expedite": true,
      "name": "Peter Edwards",
      "region": {
        "_meta": {
          "href": "https://tg-mobility.cloudhub.io/mobility/v1/regions/70144640"
        },
        "id": "70144640",
        "name": "United States"
      }
    },
    "shoppingCart": {
      "device": {
        "_meta": {
          "href": "https://tg-mobility.cloudhub.io/mobility/v1/catalog/devices/9194713904"
        },
        "id": "9194713904",
        "imageUrl": "https://commcareqa.tangoe.com/manage/images/devices/VER/iphone5_blk_l.jpg",
        "manufacturer": {
          "id": "Apple",
          "logoUrl": "https://commcareqa.tangoe.com/manage/images/manufacturers/Apple.gif",
          "name": "Apple"
        },
        "name": "Apple iPhone 5 (16GB) - Black",
        "price": {
          "amount": 199.99,
          "currencyCode": "USD",
          "recurrence": "ONETIME"
        },
        "vendor": {
          "id": "98",
          "logoUrl": "https://commcareqa.tangoe.com/manage/images/carrier/logo_VER.gif",
          "name": "Verizon Wireless"
        }
      },
      "plan": {
        "id": "6568886698",
        "includedFeatures": [
          {
            "description": "Total Equipment Coverage"
          },
          {
            "description": "VZ Navigator"
          },
          {
            "description": "Base Package"
          }
        ],
        "name": "Nationwide for Business Talk Share Plan",
        "optionalFeatures": [
          {
            "description": "Roadside Assistance",
            "id": "293",
            "price": {
              "amount": 200,
              "currencyCode": "USD",
              "recurrence": "MONTHLY"
            }
          },
          {
            "description": "Visual Voice Mail",
            "id": "2082",
            "price": {
              "amount": 2.99,
              "currencyCode": "USD",
              "recurrence": "MONTHLY"
            }
          }
        ],
        "price": {
          "amount": 58.67,
          "currencyCode": "USD",
          "recurrence": "MONTHLY"
        },
        "vendor": {
          "_meta": {
            "href": "https://tg-mobility.cloudhub.io/mobility/v1/vendors/98"
          },
          "id": "98",
          "logoUrl": "https://commcareqa.tangoe.com/manage/images/carrier/logo_VER.gif",
          "name": "Verizon Wireless"
        }
      },
      "quantity": 0
    },
    "transactionType": "UPGRADE"
  },
  "status": "SUCCESS"
}
```
<br/>


## Step 5: Submit order request.

Once the request has been confirmed, it can be re-submitted for processing via HTTP POST. Before submitting, be sure to set the confirm query parameter to false (or remove it, since the default is false). 

If the submission is successful, the API will once again return a response containing JSON with an HTTP status code of 200. This response also will include the newly-assigned order number (i.e., orderId). 
 
Here is an example of what the order response might look like:

```
{
  "order": {
    "_meta": {
      "href": "https://tg-mobility.cloudhub.io/mobility/v1/orders/8712282"
    },
    "accountHolder": {
      "_meta": {
        "href": "https://tg-mobility.cloudhub.io/mobility/v1/employees/28673732"
      },
      "companyEmployeeId": "mike.mcpadden.xx1",
      "department": {
        "id": "11569020385",
        "name": "Development"
      },
      "email": "michael.mcpadden@tangoe.com",
      "firstName": "Mike",
      "id": "28673732",
      "lastName": "McPadden",
      "officePhone": "2035559999",
      "status": "ACTIVE"
    },
    "dateSubmitted": "2016-02-01T16:36:45Z",
    "externalOrderNumber": "upgrade_device_and_service_test",
    "orderId": "8712282",
    "orderSegments": {
      "items": [
        {
          "_meta": {
            "hrefHistory": "https://tg-mobility.cloudhub.io/mobility/v1/orders/8712282/history?orderSegment=8712283",
            "hrefShipments": "https://tg-mobility.cloudhub.io/mobility/v1/orders/8712282/shipments?orderSegment=8712283"
          },
          "accessories": [],
          "device": {
            "imageUrl": "https://commcareqa.tangoe.com/manage/images/devices/iphone5_blk_l.jpg",
            "manufacturer": {
              "_meta": {
                "href": "https://tg-mobility.cloudhub.io/mobility/v1/manufacturers/Apple"
              },
              "id": "Apple",
              "logoUrl": "https://commcareqa.tangoe.com/manage/images/manufacturers/Apple.gif",
              "name": "Apple"
            },
            "name": "Apple iPhone 5 (16GB) - Black",
            "price": {
              "amount": 199.99,
              "currencyCode": "USD",
              "recurrence": "ONETIME"
            }
          },
          "features": [],
          "orderSegmentId": "8712283",
          "plan": {
            "name": "Nationwide for Business Talk Share Plan",
            "optionalFeatures": [
              {
                "description": "Roadside Assistance",
                "price": {
                  "amount": 200,
                  "currencyCode": "USD",
                  "recurrence": "MONTHLY"
                }
              },
              {
                "description": "Visual Voice Mail",
                "price": {
                  "amount": 2.99,
                  "currencyCode": "USD",
                  "recurrence": "MONTHLY"
                }
              }
            ],
            "price": {
              "amount": 58.67,
              "currencyCode": "USD",
              "recurrence": "MONTHLY"
            }
          },
          "status": "ORDER_SUBMITTED",
          "vendor": {
            "_meta": {
              "href": "https://tg-mobility.cloudhub.io/mobility/v1/vendors/98"
            },
            "id": "98",
            "logoUrl": "https://commcareqa.tangoe.com/manage/images/carrier/logo_VER.gif",
            "name": "Verizon Wireless"
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
            "text": "1234567890",
            "type": "TEXT"
          },
          {
            "id": "ADDITIONAL_INSTRUCTIONS",
            "label": "Additional Instructions",
            "text": "TEST BY USING Rest based API.",
            "type": "TEXT"
          },
          {
            "id": "SERVICE_NUMBER",
            "label": "Service Number",
            "text": "2035557676",
            "type": "TEXT"
          },
          {
            "choice": {
              "id": "18"
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
        "href": "https://tg-mobility.cloudhub.io/mobility/v1/employees/28673732"
      },
      "companyEmployeeId": "mike.mcpadden.xx1",
      "department": {
        "id": "11569020385",
        "name": "Development"
      },
      "email": "michael.mcpadden@tangoe.com",
      "firstName": "Mike",
      "id": "28673732",
      "lastName": "McPadden",
      "officePhone": "2035559999",
      "status": "ACTIVE"
    },
    "serviceNumber": "2035557676",
    "shipTo": {
      "address": [
        {
          "id": "LINE_1",
          "label": "Address Line1",
          "value": "35 Executive Blvd"
        },
        {
          "id": "CITY",
          "label": "City",
          "value": "Orange"
        },
        {
          "id": "STATE",
          "label": "State",
          "value": "CT"
        },
        {
          "id": "POSTAL_CODE",
          "label": "Zip Code",
          "value": "06477"
        }
      ],
      "expedite": true,
      "name": "Peter Edwards",
      "region": {
        "_meta": {
          "href": "https://tg-mobility.cloudhub.io/mobility/v1/regions/70144640"
        },
        "id": "70144640",
        "name": "United States"
      }
    },
    "status": "ORDER_SUBMITTED",
    "transactionType": "UPGRADE"
  },
  "status": "SUCCESS"
}
```
<br/>

