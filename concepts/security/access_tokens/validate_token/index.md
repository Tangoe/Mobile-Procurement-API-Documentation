---
layout: default
title: Validate Access Token
---

# Access Tokens


## How to Validate an Access Token


The following information is required to validate an access token: 

* **access_token**: The access token to be validated.

<br>

### Endpoints 
An access token is validated by making an HTTP GET to our validation endpoint. The specific endpoint used will depend upon your instance's environment type:

* For **Production**: <code>http://tngo-prod-oauth-ping-validator.cloudhub.io</code>
* For **QA**: <code>http://tngo-qa-oauth-ping-validator.cloudhub.io</code>
 
<br>

### Request Query Parameters 
To validate an access token, the following query parameters need to be passed with this GET request: 
 
| ***Query Parameter*** | ***Description*** | ***Example*** |
| --- | --- | --- |
| **access_token** | The access token to be validated. | <code>jf8ANPs5ETFT1RPnfJnZpbcWjz2f</code> |

<br>

### Example 
The following is an example of a GET request to validate a token: 

```
https://oauthqa.tangoe.com/as/token.oauth2?grant_type=refresh_token&client_id=376af94124f4400e9227c89937c12354&client_secret=81f40d2777ea4a41A992535F17AC92EC&scope=MOBPROC&refresh_token=a9AhNvcffquYkV4bSw0O6gt4gKZRvTUGR2lfR8nJf4 
```

The following is an example of a response that might be returned: 

```
 { 
   "expires_in": 7188, 
   "scope": "MOBPROC", 
   "client_id": "a98b70cddd5f1432221360bd732f5ec1", 
   "username": "sn2.admin.xx1", 
   "platform": "command", 
   "identityProvider": "pcv_edge" 
 } 
```

The following properties are returned in the token validation response: 

| ***Property*** | ***Description*** | ***Example*** |
| --- | --- | --- |
| **expires_in** | Number of seconds that the access token will be valid. | <code>7188</code> |
| **scope** | OAuth scopes that are associated with the access token being validated. | <code>MOBPROC</code> |
| **client_id** | Tangoe-assigned client ID for the API client application that requested the access token being validated. | <code>a98b70cddd5f1432221360bd732f5ec1</code> |
| **username** | Username that is associated with the access token. | <code>sn2.admin.xx1</code> |
| **platform** | Source system that is associated with the access token. | <code>command</code> |
| **identityProvider** | Identifies the authenticating entity. | <code>pcv_edge</code> |

