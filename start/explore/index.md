---
layout: default
title: Getting Started Test and Explore the API
---


# Test and Explore the API

Once you have registered yuor application to use this API, Tangoe will provide you with access to the Tangoe Developer Portal. Through this site

<img src="{{site.url}}images/screens/devportal.jpg" style="border:1px solid #666;" />
<br/>




## **Before You Start**
In order to use the Tangoe Developer Portal to test and explore this API, you must first 
<br/>

## **Exploring the all the API Endpoints**

Once your application has been granted access to the Mobile Procurement API, you will be able to make your first API in a matter of minutes. Simply follow these easy steps:

1. Click on the API Reference link.
1. Scroll down to the Resources section and locate the **/ping** endpoint. 
1. Click on the **GET** tab to expand the panel that describes this endpoint.
1. Click on the **"Try it"** link right below the GET tab. This reveals the form for testing this API call. 
1. In the AUTHENTICATION section, enter your **Client ID** and check the appropriate scope (i.e., MOBILE_PROCUREMENT_API). 
1. In the HEADERS section, enter your tenant header (i.e., X-TNGO-TENANT).
1. In the QUERY PARAMETERS section, enter some text in the **echoText** parameter input. This text will be returned with a successful response.
1. Click the **GET** button.


A successful response (i.e., HTTP status 200) should be returned. The response body will include your echoText value, along with some data identifying the backend system that processed your request.

<img src="{{site.url}}images/screens/ping.png" style="border:1px solid #666;" />
<br/>

## **Explore**

After you make your first successful Ping call, then you are ready to begin exploring the rest of the Mobile Procurement API.

Every endpoint in the API offers a similar reference page that explains what it does and the query parameters that it takes. Furthermore, all these pages offer the same form for testing that endpoint. Simply enter the appropriate headers and query parameter values, then click the button labeled with the desired HTTP method (e.g., GET, POST). A sample response will be returned.