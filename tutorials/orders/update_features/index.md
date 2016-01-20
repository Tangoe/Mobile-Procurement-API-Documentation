---
layout: default
title: Tutorials - UPDATE_FEATURES Order
---


# How to Update the Features of a Service Asset

**This tutorial provides step-by-step instructions for creating, confirming, and submitting an order to update features of a specific service asset.**
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

For an UPDATE_FEATURES, this JSON typically includes the following pieces:

* Transaction type (i.e., UPDATE_FEATURES).
* ??? OTHER BULLETS HERE ???
 
 
Here is an example:

```

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

```
<br/>

## Step 5: Submit Order Request
Once the request has been confirmed, it can be re-submitted for processing via HTTP POST. Before submitting, be sure to set the confirm query parameter to false (or remove it, since the default is false). 

If the submission is successful, the API will once again return a response containing JSON with an HTTP status code of 200. This response also will include the newly-assigned order number (i.e., orderId). 
 
Here is an example of what the order JSON might look like:

```

```
<br/>


