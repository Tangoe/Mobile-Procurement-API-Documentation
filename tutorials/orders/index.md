---
layout: default
title: Tutorials
logoTangoe: tangoe-logo_125.png
---

# How to Create and Submit an Order

**This tutorial provides a high-level overview of how orders are created and submitted. Please click on a specific transaction type (below) for step-by-step instructions, along with request and response body examples, needed to successfully create and submit an order for that transaction type.**

The types of orders that can be created with this API are classified in one of two catagories: *Procurement* or *Care*. Procurement transactions are orders for obtaining a new device, plan, and/or accessories. Care transactions are orders for modifying an existing device or service asset. 

Within each of these categories are specific types of transactions, as listed in the table below. Click on a transaction type to view the specific steps required to create that type of order, along with exanples of the request/response body for that type.


| ***Transaction Type*** 							| ***Catagory*** 	| ***Description*** 															|
| ----------------------------------------------	| ----------------- | ----------------------------------------------------------------------------- |
| NEW_ACTIVATION									| Procurement 		| Obtain a new device, service, and/or accessory 								|
| UPGRADE 											| Procurement 		| Upgrade an existing device and/or service.									|
| ORDER_ACCESSORIES 								| Procurement 		| Obtain one or more new accessory item(s).  									|
| PORT_NUMBER		 								| Procurement 		| Port an existing service asset from one carrier to another. 					|
| [SUSPEND](/tutorials/orders/suspend/)	 			| Care 				| Suspend an existing active cellular service asset.							|
| [UNSUSPEND](/tutorials/orders/unsuspend/) 	 	| Care 				| Reactivate an existing cellular service asset that was previously suspended. 	|
| [DEACTIVATE](/tutorials/orders/deactivate/) 	 	| Care 				| Deactivate an existing active cellular service asset.							|
| UPDATE_FEATURES 	 								| Care 				| Add or remove optional features on an existing cellular service asset. 		|
| [SWAP_DEVICES](/tutorials/orders/swap_devices/) 	| Care 				| Reassign cellular service from one existing device asset to another. 			|	
| ----------------------------------------------	| ----------------- | ----------------------------------------------------------------------------- |


Along with the required request headers, API consumers will need to pass a block of JSON in the request body. This JSON code provides all of the data needed by the backend system to process the order request (e.g., transaction type ID, requester's employee ID, catalog ID of for the device to be obtained, etc.). In addition to the **/orders** endpoint, the API also provides many other supporting endpoints that can be used to gather all of this data.

After preparing your order request, it can be passed to the server for validation and processing. This processing supports two modes: *confirmation* and *submission*:

* **Confirmation** -- When set to confirmation mode, the order will NOT be submitted for fulfillment. Instead, after validating the data in the request, the server returns a response containing a status value of "SUCCESS," along with JSON in the response body containing human-readable data that fully describes what was referenced by ID in the request body. This expanded data provides the UI with enough information to display meaningful request data that can be presented to the end user to verify the order, correct any errors, and submit for fulfillment.

* **Submission** -- Once the request is successfully validated and without errors -- and confirmation mode is NOT explicitly set -- the order can be submitted to the server for fulfillment. Again, the server will return a status value of "SUCCESS", along with enough meaningful information for display to the end user, as well as with a new, system-generated order ID.

* **Errors** -- If during validation the server determines that there are issues with the request that prevent it from being processed, the server will return a status value of "ERROR." The respinse will also include an array containing data describing the error(s). For a detailed description of what might be returned for an unsuccessful response, please go to the [Faults and Error Handling](/concepts/errors/) page.



## Steps for building your order


All orders require the following process steps in order to create, confirm, and submit an order:

### Step 1. Build the order request JSON code that is required for yur specific transaction type.
The transaction type identifies the specific action that you wish to take (e.g., obtain a new phone and cellular plan, suspend cellular service, etc.). See the chart below for a full list of available transaction types

For the specific order creation steps required for each transaction type, including JSON samples, please refer to the following pages:
 
### Step 2. Compile the request headers needed for your desired transaction.
Please refer to the [Headers](/concepts/headers/) page for details regarding what headers are available and how to configure them.

### Step 3. Submit the order request via HTTP POST to the orders resource (/orders).
Typically, you will want to first ask for a confirmation. To do so, simply include the appropriate query parameter (confirmation=true). This confirmation is useful because it:

* Returns a successful confirmation response will return  human readable data that fully describes what was passed in as an ID in the request (e.g., device make/model, vendor name, account holder details, etc.).
* Provides API consumer with useful feedback to present to end user, allowing them to confirm that the request is accurate.

### Step 4. Submit the Order.
After confirming the request, the API consumer can re-submit, setting the confirmation parameter to false (or do not pass at all since the default value is false). 

### Step 5. Order ID is assigned.
If the request is successful, the response will new order ID. Otherwise, errors will be returned describing why the order request could not be processed.



