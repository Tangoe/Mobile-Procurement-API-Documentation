---
layout: default
title: Tutorials - SWAP_DEVICE Order
---


# How to Transfer Service from One Device to Another

**This tutorial provides step-by-step instructions for changing the assignment of a service from one device to another. Please note that both devices must be known assets in the possession of the user. In other words, service cannot be transferred to a brand new device that is purchased during the order process.**
<br />

## Authentication

This API uses the OAuth2 standard for authentication. Specifically, it supports two grant types: *Implicit* and *Resource Owner Password Credentials*. For details regarding how to use this standard to authenticate when making your API calls, please refer to the  [Security and Authentication](/concepts/security/) page.
<br />

## Step 1. Build the request body (in JSON) that is required 

Next, you will need to compile the JSON that will be submitted in the request body. This JSON includes all of the data that the backend system requires to process this order.

For a SWAP_DEVICE, this JSON typically includes the following pieces:

* Transaction type (i.e., SWAP_DEVICE).

* All required and optional order properties. Refer to the <a href="/tutorials/properties">Obtaining Order Properties</a> page for steps how to identify the properties that are relevant for your order.




Here is an example of what the fully-assembled request body JSON might look like:

```

```
<br />



## Step 2: Set required request headers.

There are multiple HTTP headers that may be passed as part of your API request. They are used to confirm that the caller is authorized to make this request, along with optional filtering (when appropriate). See the [Headers](/concepts/headers/) page for more information about our supported headers.

### Required Headers:

* **X-TNGO-TENANT** - Used to identify the specific tenant (i.e., customer) for which the API is being called.

### Optional Headers:

* **X-TNGO-CONTEXT-COMPANYEMPLOYEEID** -- Used to specify a company employee ID that defines the context of the call. This header is only checked if the X-TNGO-CONTEXT-EMPLOYEEID header is blank or does not exist. If this header does not exist, the context will be set to the authenticated user. 
*Security Note:* The authenticated user must be authorized to access (at a minimum) all the same data that the employee referenced in this header is authorized to access.

* **X-TNGO-CONTEXT-EMPLOYEEID** -- Used to specify an employee ID that defines the context of the call. If this header does not exist or is blank, then the X-TNGO-CONTEXT-COMPANYEMPLOYEEID header will be used instead. 
*Security Note:* The authenticated user must be authorized to access (at a minimum) all the same data that the employee referenced in this header is authorized to access.

* **X-TNGO-CONTEXT-HIERARCHYID** -- Used to specify the organizational hierarchy to be used for the API call. If this header is blank or does not exist, the default hierarchy will be used.

<br />


## Step 3: Indicate if you wish to obtain an order confirmation.

Prior to submitting the order for vendor processing, you will probably want to first obtain an order confirmation. The confirmation returns all of the IDs passed in with the request body, along with the human-readable data that fully describes what was referenced by those IDs (e.g., device name, vendor, price). This is useful for API consumers who wish to present the order request back to their end user so they can verify it for accuracy.

To obtain a confirmation, set the **confirm** query parameter to true.

<br />

## Step 4: Obtain the order confirmation by calling the /orders endpoint via HTTP POST. 

Your request, containing the JSON request body and confirm query parameter set to true, is now be ready. Submit it via the HTTP POST method to the **/orders** endpoint.

If successful, the API will return a response with a 200 HTTP status code and containing JSON that fully describes the identifiers that were submitted. This data can then be presented back to the end user for verification and correction (if necessary). 

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
