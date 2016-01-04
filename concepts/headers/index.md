---
layout: default
title: API Concepts - Headers 
---


# Setting Request Headers

Request headers are simply key value pairs of metadata relating to what is contained within the body of a request and/or response. The Mobile Procurement API primarily use them to pass along various important user and context identifiers to the relevant backend system. Some of these headers are added by the user of the API (e.g., X-TNGO-TENANT, X-TNGO-EMPLOYEEID). 

### A Note about Empty Values and Blanks
If a header value is returned as an empty or blank field, the backend system using the header should consider it to have NOT been set. In other words, they should discard the empty value or spaces and act as if that particular header to have not been passed at all.

Below are descriptions the various types of headers available, along with the call required to obtain each one.

## General Headers

General headers are added by the consumer of the API and are passed through the Synergy layer to the appropriate backend system.

### X-TNGO-TENANT
* Required
* Data type of value: string
* Description: Used to identify the specific tenant (i.e., customer) for which the API is being called.


## Context ID Headers
User ID headers are added after the logged in user is authenticated. A single user could have IDs for one or more backend systems. Each backend system can extract their relevant ID and simply ignore the rest. 

### X-TNGO-CONTEXT-COMPANYEMPLOYEEID

* Optional
* Data type of value: string
* Description: Used to specify a company employee ID that defines the context of the call. This header is only checked if the X-TNGO-CONTEXT-EMPLOYEEID header is blank or does not exist. If this header does not exist, the context will be set to the authenticated user. 
***SECURITY NOTE*** - The authenticated user must be authorized to access (at a minimum) all the same data that the employee referenced in this header is authorized to access.

### X-TNGO-CONTEXT-EMPLOYEEID 
* Optional
* Data type of value: string
* Description: Used to specify an employee ID that defines the context of the call. If this header does not exist or is blank, then the X-TNGO-CONTEXT-COMPANYEMPLOYEEID header will be used instead.
***SECURITY NOTE*** - The authenticated user must be authorized to access (at a minimum) all the same data that the employee referenced in this header is authorized to access.

### X-TNGO-CONTEXT-HIERARCHYID 

* Optional
* Data type of value: string
* Description: Used to specify the organizational hierarchy to be used for the API call. If this header is blank or does not exist, the default hierarchy will be used.
