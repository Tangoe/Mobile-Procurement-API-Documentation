---
layout: default
title: Tutorials
logoTangoe: tangoe-logo_125.png
---

# How to Create an Order

[NAV FOR TRANSACTION TYPE-SPECIFIC PAGES]

Orders can be created to procure new devices, plans, and/or accessories.  An order can also be created for modifying an existing device or service asset. Regardless of the transaction type, the following process steps apply to creating an order:

1. Build the order request JSON required by the desired type of transaction. For the specific order creation steps required for each transaction type, including JSON samples, please refer to the following pages:
	***Procurement Transactions***
		* NEW_ACTIVATION
		* UPGRADE (device | service | device and service)
		* ORDER_ACCESSORIES 
		* PORT_NUMBER
	***Care Transactions***
		* SUSPEND
		* UNSUSPEND
		* DEACTIVATE
		* UPDATE_FEATURES
		* SWAP_DEVICES

2. Compile the request headers needed for your desired transaction. [Link to headers page]
3. Submit the order request via HTTP POST to the orders resource (/orders).Typically, you will want to first ask for a confirmation. To do so, simply include the appropriate query parameter (confirmation=true). This confirmation is useful because it:
 * Returns a successful confirmation response will return  human readable data that fully describes what was passed in as an ID in the request (e.g., device make/model, vendor name, account holder details, etc.).
 * Provides API consumer with useful feedback to present to end user, allowing them to confirm that the request is accurate.
4. After confirming the request, the API consumer can re-submit, setting the confirmation parameter to false (or do not pass at all since the default value is false). 
5. If the request is successful, the response will new order ID. Otherwise, errors will be returned describing why the order request could not be processed.

 


