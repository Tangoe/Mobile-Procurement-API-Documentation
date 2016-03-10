---
layout: default
title: Formatting an Address
---


# How to Format an Address

**This tutorial dscribes the steps for obtaining the format that is required for properly formatting an address for a delivery to a selected country.**

<br/>

When placing an order that requires the physical shipment of a device or accessory, shipping address information will need to be included in the request body. Moreover, different countries often require differing sets of components in order for an address to be valid for delivery to an address within that country.

In order to determine the required mailing address components for a selected country, an API call must first be made to the **/regions/{id}** endpoint. Please note that this endpoint requires passing the ID as a URI parameter for the country to which the shipment is to be delivered. This call will return a response that includes a block called addressFormat that identifies all of the specific address components that are required. 

For example:

```

{
  "_meta": {
    "href": "http://api.tangoe.com/mobileprocurement/v1/regions/7088567",
    "hrefParent": "",
    "hrefChildren": "http://api.tangoe.com/mobileprocurement/v1/regions?parentRegion=7088567"
  },
  "id": "7088567",
  "name": "United States of America",
  "type": "COUNTRY",
  "parentId": "",
  "addressFormat": [
    {
      "id": "LINE_1",
      "label": "Address 1"
    },
    {
      "id": "LINE_2",
      "label": "Address 2"
    },
    {
      "id": "CITY",
      "label": "City"
    },
    {
      "id": "STATE",
      "label": "State"
    },
    {
      "id": "POSTAL_CODE",
      "label": "Postal Code"
    }
  ]
}
```


Once a selected country's address format is returned, the response can be parsed to assemble a **shipTo** block containing all of the required address components, along with the appropriate values. For example:

```
{
  "shipTo": {  
    "name": "Peter Edwards",
    "region": { 
      "id": "7088567"
    },
    "address": [
      {
        "id": "LINE_1",
        "value": "35 Executive Blvd"
      },
      {
        "id": "CITY",
        "value": "Orange"
      },
      {
        "id": "STATE",
        "value": "CT"
      },
      {
        "id": "POSTAL_CODE",
        "value": "06477"
      }
    ],
    "expedite": true
  }
}

```
