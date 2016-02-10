---
layout: default
title: Errors
---

# Errors

This API returns a standard HTTP status code with every response to indicate whether your request had succeeded or failed. 

Status codes within the 200 range typically indicate that the request was processed successfully. Codes within the 400 range indicate an error occurred that resulted from the request data provided by the caller (e.g., a required parameter was missing in the request). And codes within the 500 range indicate an error triggered by something on Tangoe's servers. 

<br/>

## Supported HTTP Status Codes

The following status codes are currently supported:

| ***Status Code*** 	| ***Description*** 																				|
| --------------------	| ------------------------------------------------------------------------------------------------- |
| **200**				| *Success:* Your request was received, understood, and processed. 									|
| --------------------	| ------------------------------------------------------------------------------------------------- |
| **400**				| *Bad Request:* Your request is missing required data or is malformed. 							|
| --------------------	| ------------------------------------------------------------------------------------------------- |
| **403**				| *Forbidden:* You are not authorized to receive what was requested. 								|
| --------------------	| ------------------------------------------------------------------------------------------------- |
| **404**				| *Not Found:* The resource you requested cannot be found.											|
| --------------------	| ------------------------------------------------------------------------------------------------- |
| **500**				| *Server Error:* Your request failed due to a problem on Tangoe's servers.							|
| --------------------	| ------------------------------------------------------------------------------------------------- |

<br/>

## Error Messages

If an error does occur, the following error message information will be included in the response body:

* **status** -- One of the HTTP status codes listed above (e.g., 400).
* **id** -- Unique string identifying this specific occurance of the error.
* **errorCode** -- Tangoe-assigned code indicating the type of error that occurred.
* **message** -- Description of the what caused the message and/or how to resolve it.


Here is an example of an error message for a missing (required) parameter:

```
{
  "errorCode": "400-Region_id-Account-Id-error",
  "id": "tghwn",
  "message": "Region ID or Service ID must be passed in.",
  "status": 400
}
```
