---
layout: default
title: Request Headers 
---


# Request Headers

Request headers are simply key/value pairs of metadata relating to what is contained within the body of a request and/or response. The Mobile Procurement API primarily uses them for identifying users and limiting context. 

<br />

## General Headers

General headers are added by the consumer of the API and are passed along to the appropriate backend system.

### X-TNGO-TENANT
Used to identify the specific tenant for which the API is being called (i.e., the customer).

* Required
* Data type: string

<br>

### client_id
Used to identify your client application as the one calling the API.

* Required
* Data type: string

<br />

## Context ID Headers

Frequently, the logged-in/authenticated user calling the API may be a service account. As such, a service account does not specifically identify the actual end user who is placing the order, viewing assets, etc. The true end user (also known as "the actor"), must be identified by setting a context header. The API will use these identifiers to set the context for making the API call. 

For example, an application called the API using a service account ID (i.e., admin.acme.com). The request also included the X-TNGO-CONTEXT-COMPANYEMPLOYEEID header set to the company ID for an end user, Peter Edwards (i.e., pedwards.acme.com). When the request for the device catalog was executed, only devices that Peter Edwards was authorized to see were returned rather than the larger set that the service account was authorized to access.

<br>

### X-TNGO-CONTEXT-COMPANYEMPLOYEEID

This is the employee ID assigned by the tenant/customer (e.g., employee's email address, etc.). This header is only used when the X-TNGO-CONTEXT-EMPLOYEEID header is not set. If this header is NOT set, the context will be set to the authenticated user. 

* Optional
* Data type: string

<br>

### X-TNGO-CONTEXT-EMPLOYEEID 

This is the employee ID assigned by Tangoe. If this header is set, it will be used in place of the X-TNGO-CONTEXT-COMPANYEMPLOYEEID header. If it is NOT set, then the X-TNGO-CONTEXT-COMPANYEMPLOYEEID header will be used (if set).

* Optional
* Data type: string

<br>

### X-TNGO-CONTEXT-HIERARCHYID 

The Tangoe-assigned ID that is used to specify the organizational hierarchy to be used for the API call. If this header is not set, the default hierarchy will be used.

* Optional
* Data type: string

