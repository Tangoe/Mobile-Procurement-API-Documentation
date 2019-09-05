---
layout: default
title: Migration to Atlas
---


# Migration to the Atlas Platform (Beta)

The Mobile Procurement API (MOBPROC) was originally designed to support two Tangoe backend platforms: Premium Mobile (Command) and Rivermine. We recently updated this API so that it can be adopted by subscribers to our new Atlas platform as well. 

Although Atlas offers a more robust experience than the older platforms, Tangoe has intentionally limited the scope of new Atlas functionality offered via MOBPROC to minimize effort required by API consumers when they migrate to Atlas. However, there are a few considerations to be aware of that could necessitate changes to the code for applications that consume the API. 

Currently, only a subset of the available MOBPROC endpoints have been implemented for Atlas. As additional endpoints are supported in the future, this page will be updated to identify any additional considerations that arise. The endpoints supported presently are:
* /assets/devices
* /assets/services
* /assets/services/{id}

* /catalog/devices
* /catalog/devices/{id}
* /catalog/plans
* /catalog/plans/{id}
* /catalog/accessories

<br/>

## Code Changes that Might Be Required
### Device Features
The Premium Mobile (Command) and Rivermine platforms both require that a unique device catalog item exist for every device feature option combination (e.g., iPhone 7 32GB silver; iPhone 7 128GB gold, etc.). However, the Atlas platform can support multiple features for a generic device catalog item (e.g., iPhone 7). Therefore, the response returned from the Atlas platform for the **/catalog/devices/{id}** endpoint includes an additional data block called <code>deviceFeatures</code> to provide available feature options. 

The following is an example of the device features block returned within the device asset detail response:
```
  ...
  "deviceFeatures": [
      {
        "id": "740500",
        "label": "Available Colors",
        "options": [
          {
            "id": "993500",
            "isDefault": false,
            "label": "Silver",
            "price": {
              "amount": 0,
              "currencyCode": "USD",
              "recurrence": "ONETIME"
            }
          },
          {
            "id": "994500",
            "isDefault": false,
            "label": "Gold",
            "price": {
              "amount": 0,
              "currencyCode": "USD",
              "recurrence": "ONETIME"
            }
          }
        ],
        "type": "CHOICE",
        "validation": "OPTIONAL"
      },
      {
        "id": "741500",
        "label": "Available Storage",
        "options": [
          {
            "id": "995500",
            "isDefault": false,
            "label": "8 GB",
            "price": {
              "amount": 5,
              "currencyCode": "USD",
              "recurrence": "ONETIME"
            }
          },
          {
            "id": "996500",
            "isDefault": false,
            "label": "16 GB",
            "price": {
              "amount": 10,
              "currencyCode": "USD",
              "recurrence": "ONETIME"
            }
          }
        ],
      "type": "CHOICE",
      "validation": "OPTIONAL"
    }
  ]
  ...
```

<br/>

## Changes Not Requiring Code Changes 

### Unique Catalog Items for Each Vendor
The Premium Mobile (Command) and Rivermine platforms both require that a unique device catalog item exist for every unique vendor. Although the Atlas platform can support multiple vendors for the same device catalog item, the MOBPROC API will continue to return a separate item for each vendor. Catalog item IDs for the Atlas platform contain two segments concatenated with an underscore (e.g., 123456_7778888). The first segment is the catalog item ID and the second segment is the vendor ID. 

Please note that all IDs returned by the Mobile Procurement API are assigned by the API and intended to be only used by the API. **The specific ID values returned should not be relied upon for use apart from the API.** Moreover, the structure of the ID can be different depending on the Tangoe backend system to which you subscribe and are subject to change in the future. 

<br/>

### Images Returned as a Data URI
For the Atlas platform, image values will be returned as data URIs. These URIs are prefixed with the <code>data:</code> scheme and allow content creators to embed small files inline into documents. Data URLs for images are supported on all modern browsers.

Data URLs are composed of four parts: a prefix (data:), a MIME type indicating the type of data, an optional base64 token if non-textual, and the data itself. That is, it adheres to the followign syntax:
```
 data:[<mediatype>][;base64],<data>
``` 
For example:
```
 data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAADoAAAA7CAIAAAClhQ7a
 AAAACXBIWXMAAAsTAAALEwEAmpwYAAAAB3RJTUUH4goXEh0KED2VsgAAAB1pVFh0Q2
 9tbWVudAAAAAAAQ3JlYXRlZCB3aXRoIEdJTVBkLmUHAAAAW0lEQVRo3u3asRHAIAwE
 QeRxtRTkdp8KHIOYvVTJBh+qkow+PaNVuLi4uO2579+hvtoCyowx4OLi4uLi4uLi4u
 Li4uLi4uLi4uLi4uLi4p5Web/AxcXFxb2GuwDC8glxFXZphAAAAABJRU5ErkJggg==
```