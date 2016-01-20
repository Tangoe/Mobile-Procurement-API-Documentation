---
layout: default
title: Tutorials - NEW_ACTIVATION Order
---


# How to Order a New Device and Service Plan

**This tutorial provides step-by-step instructions for creating, confirming, and submitting an order to procure a new device and service plan.**
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

For a NEW_ACTIVATION, this JSON typically includes the following pieces:

* Transaction type (i.e., NEW_ACTIVATION).
* Account Holder Company ID (or Account Holder ID) for the user to whom the assets will be assigned.
* The properties that are required for procuring a new device and service plan with the specific vendor from whom they will be purchased.
* ??? OTHER BULLETS HERE ???
 
 
Here is an example:

```
{
  "orderRequest": {
    "externalOrderNumber": "AZ99087547-001",
    "transactionType": "NEW_ACTIVATION",
    "regionId": "70144640",
    "postalCode": "06477",
    "shoppingCart": {
      "quantity": 1,
      "deviceId": "9194791812",
      "planId": "6568886494",
      "optionalFeatureIds": ["951", "88"],
      "accessoryIds": ["3628935899", "3628939022"]
    },
    "properties": [
      {
        "groupId": "MISCELLANEOUS",
        "fields": [
          {
            "id": "CONTACT_NUMBER",
            "type": "TEXT",
            "text": "2035559360"
          },
          {
            "id": "ADDITIONAL_INSTRUCTIONS",
            "type": "TEXT",
            "text": "Please ship via FedEx"
          },
          {
            "id": "PREFERRED_AREA_CODE",
            "type": "TEXT",
            "text": "512"
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
    ],
    "shipTo": { 
      "name": "Peter Edwards",
      "regionId": "70144640",
      "address": [
        {
          "id": "1",
          "value": "35 Executive Blvd"
        },
        {
          "id": "4",
          "value": "Orange"
        },
        {
          "id": "5",
          "value": "CT"
        },
        {
          "id": "6",
          "value": "06477"
        }
      ],
      "expedite": true
    }
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
    "costSummary": [
      {
        "amount": 244.97000000000003,
        "currencyCode": "USD",
        "recurrence": "ONETIME"
      },
      {
        "amount": 538.69,
        "currencyCode": "USD",
        "recurrence": "MONTHLY"
      }
    ],
    "externalOrderNumber": "AZ99087547-001",
    "postalCode": "06477",
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
            "text": "Please ship via FedEx",
            "type": "TEXT"
          },
          {
            "id": "PREFERRED_AREA_CODE",
            "label": "Preferred Area Code",
            "text": "512",
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
    "region": {
      "_meta": {
        "href": "https://tg-mobility.cloudhub.io/mobility/v1/regions/70144640"
      },
      "id": "70144640",
      "name": "United States"
    },
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
    "shipTo": {
      "address": [
        {
          "id": "1",
          "label": "Address Line1",
          "value": "35 Executive Blvd"
        },
        {
          "id": "4",
          "label": "City",
          "value": "Orange"
        },
        {
          "id": "5",
          "label": "State",
          "value": "CT"
        },
        {
          "id": "6",
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
      "accessories": [
        {
          "_meta": {
            "href": "https://tg-mobility.cloudhub.io/mobility/v1/catalog/accessories/3628935899"
          },
          "id": "3628935899",
          "imageUrl": "https://commcareqa.tangoe.com/manage/images/devices/VER/MICRDUALVPCF_m.jpg",
          "manufacturer": {
            "_meta": {
              "href": "https://tg-mobility.cloudhub.io/mobility/v1/manufacturers/Verizon%20Wireless"
            },
            "id": "Verizon Wireless",
            "logoUrl": "https://commcareqa.tangoe.com/manage/images/manufacturers/Verizon Wireless.gif",
            "name": "Verizon Wireless"
          },
          "name": "Verizon Wireless Vehicle Charger with Dual Output (MICRDUALVPC-F)",
          "price": {
            "amount": 22.49,
            "currencyCode": "USD",
            "recurrence": "ONETIME"
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
        {
          "_meta": {
            "href": "https://tg-mobility.cloudhub.io/mobility/v1/catalog/accessories/3628939022"
          },
          "id": "3628939022",
          "imageUrl": "https://commcareqa.tangoe.com/manage/images/devices/VER/EMICUSBDTVL_F_m.jpg",
          "manufacturer": {
            "_meta": {
              "href": "https://tg-mobility.cloudhub.io/mobility/v1/manufacturers/Verizon%20Wireless"
            },
            "id": "Verizon Wireless",
            "logoUrl": "https://commcareqa.tangoe.com/manage/images/manufacturers/Verizon Wireless.gif",
            "name": "Verizon Wireless"
          },
          "name": "Verizon Wireless Rapid Wall Charger with 6ft Cable for Micro USB (EMICUSBDTVL-F)",
          "price": {
            "amount": 22.49,
            "currencyCode": "USD",
            "recurrence": "ONETIME"
          },
          "vendor": {
            "_meta": {
              "href": "https://tg-mobility.cloudhub.io/mobility/v1/vendors/98"
            },
            "id": "98",
            "logoUrl": "https://commcareqa.tangoe.com/manage/images/carrier/logo_VER.gif",
            "name": "Verizon Wireless"
          }
        }
      ],
      "device": {
        "_meta": {
          "href": "https://tg-mobility.cloudhub.io/mobility/v1/catalog/devices/9194791812"
        },
        "id": "9194791812",
        "imageUrl": "https://commcareqa.tangoe.com/manage/images/devices/VER/sam_gal_s4_blk_l.jpg",
        "manufacturer": {
          "_meta": {
            "href": "https://tg-mobility.cloudhub.io/mobility/v1/manufacturers/Samsung"
          },
          "id": "Samsung",
          "logoUrl": "https://commcareqa.tangoe.com/manage/images/manufacturers/Samsung.gif",
          "name": "Samsung"
        },
        "name": "Samsung Galaxy S4 (16GB) - Black Mist",
        "price": {
          "amount": 199.99,
          "currencyCode": "USD",
          "recurrence": "ONETIME"
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
      "plan": {
        "_meta": {
          "href": "https://tg-mobility.cloudhub.io/mobility/v1/catalog/plans/6568886494"
        },
        "id": "6568886494",
        "includedFeatures": [
          {
            "description": "Base Package"
          }
        ],
        "name": "Nationwide for Business Talk Share Plan",
        "optionalFeatures": [
          {
            "description": "Unlimited Push to Talk",
            "id": "951",
            "price": {
              "amount": 300,
              "currencyCode": "USD",
              "recurrence": "MONTHLY"
            }
          },
          {
            "description": "Extended Warranty",
            "id": "88",
            "price": {
              "amount": 200,
              "currencyCode": "USD",
              "recurrence": "MONTHLY"
            }
          }
        ],
        "price": {
          "amount": 38.69,
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
      "quantity": 1
    },
    "transactionType": "NEW_ACTIVATION"
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
    "costSummary": [
      {
        "amount": 244.97000000000003,
        "currencyCode": "USD",
        "recurrence": "ONETIME"
      },
      {
        "amount": 538.69,
        "currencyCode": "USD",
        "recurrence": "MONTHLY"
      }
    ],
    "externalOrderNumber": "AZ99087547-001",
    "postalCode": "06477",
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
            "text": "Please ship via FedEx",
            "type": "TEXT"
          },
          {
            "id": "PREFERRED_AREA_CODE",
            "label": "Preferred Area Code",
            "text": "512",
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
    "region": {
      "_meta": {
        "href": "https://tg-mobility.cloudhub.io/mobility/v1/regions/70144640"
      },
      "id": "70144640",
      "name": "United States"
    },
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
    "shipTo": {
      "address": [
        {
          "id": "1",
          "label": "Address Line1",
          "value": "35 Executive Blvd"
        },
        {
          "id": "4",
          "label": "City",
          "value": "Orange"
        },
        {
          "id": "5",
          "label": "State",
          "value": "CT"
        },
        {
          "id": "6",
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
      "accessories": [
        {
          "_meta": {
            "href": "https://tg-mobility.cloudhub.io/mobility/v1/catalog/accessories/3628935899"
          },
          "id": "3628935899",
          "imageUrl": "https://commcareqa.tangoe.com/manage/images/devices/VER/MICRDUALVPCF_m.jpg",
          "manufacturer": {
            "_meta": {
              "href": "https://tg-mobility.cloudhub.io/mobility/v1/manufacturers/Verizon%20Wireless"
            },
            "id": "Verizon Wireless",
            "logoUrl": "https://commcareqa.tangoe.com/manage/images/manufacturers/Verizon Wireless.gif",
            "name": "Verizon Wireless"
          },
          "name": "Verizon Wireless Vehicle Charger with Dual Output (MICRDUALVPC-F)",
          "price": {
            "amount": 22.49,
            "currencyCode": "USD",
            "recurrence": "ONETIME"
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
        {
          "_meta": {
            "href": "https://tg-mobility.cloudhub.io/mobility/v1/catalog/accessories/3628939022"
          },
          "id": "3628939022",
          "imageUrl": "https://commcareqa.tangoe.com/manage/images/devices/VER/EMICUSBDTVL_F_m.jpg",
          "manufacturer": {
            "_meta": {
              "href": "https://tg-mobility.cloudhub.io/mobility/v1/manufacturers/Verizon%20Wireless"
            },
            "id": "Verizon Wireless",
            "logoUrl": "https://commcareqa.tangoe.com/manage/images/manufacturers/Verizon Wireless.gif",
            "name": "Verizon Wireless"
          },
          "name": "Verizon Wireless Rapid Wall Charger with 6ft Cable for Micro USB (EMICUSBDTVL-F)",
          "price": {
            "amount": 22.49,
            "currencyCode": "USD",
            "recurrence": "ONETIME"
          },
          "vendor": {
            "_meta": {
              "href": "https://tg-mobility.cloudhub.io/mobility/v1/vendors/98"
            },
            "id": "98",
            "logoUrl": "https://commcareqa.tangoe.com/manage/images/carrier/logo_VER.gif",
            "name": "Verizon Wireless"
          }
        }
      ],
      "device": {
        "_meta": {
          "href": "https://tg-mobility.cloudhub.io/mobility/v1/catalog/devices/9194791812"
        },
        "id": "9194791812",
        "imageUrl": "https://commcareqa.tangoe.com/manage/images/devices/VER/sam_gal_s4_blk_l.jpg",
        "manufacturer": {
          "_meta": {
            "href": "https://tg-mobility.cloudhub.io/mobility/v1/manufacturers/Samsung"
          },
          "id": "Samsung",
          "logoUrl": "https://commcareqa.tangoe.com/manage/images/manufacturers/Samsung.gif",
          "name": "Samsung"
        },
        "name": "Samsung Galaxy S4 (16GB) - Black Mist",
        "price": {
          "amount": 199.99,
          "currencyCode": "USD",
          "recurrence": "ONETIME"
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
      "plan": {
        "_meta": {
          "href": "https://tg-mobility.cloudhub.io/mobility/v1/catalog/plans/6568886494"
        },
        "id": "6568886494",
        "includedFeatures": [
          {
            "description": "Base Package"
          }
        ],
        "name": "Nationwide for Business Talk Share Plan",
        "optionalFeatures": [
          {
            "description": "Unlimited Push to Talk",
            "id": "951",
            "price": {
              "amount": 300,
              "currencyCode": "USD",
              "recurrence": "MONTHLY"
            }
          },
          {
            "description": "Extended Warranty",
            "id": "88",
            "price": {
              "amount": 200,
              "currencyCode": "USD",
              "recurrence": "MONTHLY"
            }
          }
        ],
        "price": {
          "amount": 38.69,
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
      "quantity": 1
    },
    "transactionType": "NEW_ACTIVATION"
  },
  "status": "SUCCESS"
}
```
<br/>


