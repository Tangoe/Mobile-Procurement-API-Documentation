---
layout: default
title: Release 1.1.0
---


# Release 1.1.0


<br/>

## New Features


### Porting an Existing Service to a Different Carrier (i.e., porting a number)
This feature enables the creation of an order to port an existing service asset, including the service number, from one carrier to another. For a tutorial explaining how to use this feature, please refer to the [Approving or Rejecting an Order]({{site.url}}tutorials/orders/approvals/) page. 

To support this feature, the following endpoints were created or modified:

* **/vendors** – Added the **type** filter to the request. This is enables filtering the response to only return carriers. Uses cases for such data includes populating a carrier selection list. 
* **/vendors/{id}** – Added the **accessMethod** property to the response. This is used by the API caller to determine if a specific carrier supports a technology that is compatible with a specific device.


<br/>

### Approving an Order
This feature enables the approving of a specific order. To support this, the following endpoints were created or modified:

* **/orders/{id}/approve** – This new endpoint is used to approve or reject an order that is currently pending approval.
* **/me** – Added the **isApprover** property to the response. This property returns a boolean. 

<br/>

### Moving a Service
This feature enables moving the assignment of charges associated with a specific service asset from one employee to another. For a tutorial explaining how to use this feature, please refer to the [Moving a Service]({{site.url}}tutorials/move/) page.

To support this feature, the following endpoints were created or modified:

* **/assets/services/{id}/move** – This new endpoint is used to execute a "move" operation. 
* **/me** – Added the **canMoveService** property to the response. This property returns a boolean.

<br/>

### Filtering for Accessories that are Compatible with a Specific Device Asset
The **compatibleWithDeviceAssetId** filter parameter was added to the **/catalog/accessories** endpoint. Previously, API consumers could only filter the accessories catalog by a device catalog ID. This filter enables filtering by an device asset ID as well.

<br/>


## Fixed Issues

### /vendors and /vendors/{id} endpoints
The region associated with a vendor returned in these endpoints' responses were missing the **_meta** block. Responses are now be returning that data.
