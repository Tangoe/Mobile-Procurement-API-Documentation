---
layout: default
title: Tutorials - Submit an Order DEACTIVATE 
---


# How to Deactivcate a Service

The following is a step-by-step illustration of how to permanently [*****VERIFY WITH BECKY*****] deactivate a specific active mobile service asset.

```
<div class="parent">
  <div class="puffNstuff">Block here</div>
</div>
```


## Step Zero: Authentication

See Authentication page for info regarding the supported OAuth2 grant types: Implicit, Resource Owner Password Credentials

## Step One: Set Necessary Headers

A few HTTP headers are required to be passed as part of your API request. They are used to confirm that the caller is authorized to make this request, along with optional filtering (when appropriate). 

### Required headers:

* **X-TNGO-TENANT** - Used to identify the specific tenant (i.e., customer) for which the API is being called.
See headers page [LINK] for more information about these headers.

### Optional headers:   (Do any/all of these apply to DEACTIVATE?)

* **X-TNGO-CONTEXT-COMPANYEMPLOYEEID**  

* **X-TNGO-CONTEXT-EMPLOYEEID** 

* **X-TNGO-CONTEXT-HIERARCHYID** 

## Step Two: Build JSON for Request Body

Next, you will need to compile the JSON that will be submitted in the request body. This JSON includes all of the data that the backend system requires to process this order.

For DEACTIVATE, this JSON typically includes the following pieces:

* External Order Number
* Transaction type (i.e., DEACTIVATE).
* Service ID of the service asset to be deactivated.
* Account Holder Company ID (or Account Holder ID) for the user to whom the asset is assigned.
* The properties that are required for deactivating a service for the specific vendor associated with the service asset.
 
 
Here is an example:

***** UPDATE THIS JSON -- CONFIRM THAT THIS WILL SUCCESSFULLY POST *****

```
  {
    "orderRequest": {
    "externalOrderNumber": "AZ99087547-001",
    "transactionType": "DEACTIVATE",
    "serviceId": "99034551",
    "accountHolderCompanyEmployeeId": "PETER_EDWARDS@EXAMPLE.COM",
    "properties": [
      {
        "groupId": "MISCELLANEOUS",
        "fields": [
          {
            "id": "CONTACT_PHONE",
            "type": "TEXT",
            "text": "2035559360"
          },
          {
            "id": "ADDITIONAL_INSTRUCTIONS",
            "type": "TEXT",
            "text": "Please confirm with account holder that service has been successfully deactivated."
          },
          {
            "id": "PREFERRED_DEACTIVATION_DATE",
            "type": "DATE",
            "date": "2015-06-28T00:00:00+00:00"
          }
        ]
      }
    ]
  }
```


## Step Three: Set Query Parameter(s), as Needed

The only query parameter that can be set when creating an order is:

* confirm  (boolean) - If true, the order will not be posted. Instead, the response will return a complete description of the order that can be used to create an order confirmation for the end user to review, correct, and then submit for processing.

## Step Four: Submit Order Request Confirmation

Your request, containing the JSON request body and confirm query parameter set to true, should now be ready. Submit it via HTTP POST to the orders endpoint.

If successful, the API will return a response with a 200 HTTP status code and containing JSON that fully describes the identifiers that were submitted. This data can then be presented back to the end user for verification and correction, if necessary. 

Here is an example of what the order confirmation JSON might look like:
[*** ORDER CONFIRMATION JSON ***]
Step Five: Submit Order Request
Once the request has been confirmed, it can be re-submitted for processing via HTTP POST. Before submitting, be sure to set the confirm query parameter to false (or remove it, since the default is false). 

If the submission is successful, the API will once again return a response containing JSON with an HTTP status code of 200. This response also will include the newly-assigned order number (i.e., orderId). 
 
Here is an example of what the order JSON might look like:
[*** ORDER SUBMIT JSON ***]
 
