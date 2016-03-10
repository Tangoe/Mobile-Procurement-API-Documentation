---
layout: default
title: States and Provinces
---


# Get a List of States and Provinces

**This tutorial provides instructions for obtaining a list of states or provinces that are located within a specific country.**

When creating forms where users are required to enter address information, a dropdown list of states or provinces might need to be displayed. The **/regions** endpoint is used to obtain the data needed for populating the options. 

To begin, you must first determine the region ID for a selected country. If you do not know this ID, call the **/regions** endpoint to get a full list of countries that you can search for the relevant region ID.

Next, call the **/regions** endpoint again, this time setting the request's **parentRegion** query parameter with your selected country's region ID. The API will return all of the states or provinces that are located within that country. Please note that each item in the list will include a TYPE property set to STATE_PROVINCE.

For example:


```

{
  "regions": [
    {
      "_meta": {
        "href": "https://tg-mobility.cloudhub.io/mobility/v1/regions?parentRegion=70144640",
        "hrefParent": "https://tg-mobility.cloudhub.io/mobility/v1/regions?parentRegion=70144640&parentRegion=70144640"
      },
      "abbreviation": "AL",
      "id": "158385987",
      "name": "Alabama",
      "parentId": "70144640",
      "type": "STATE_PROVINCE"
    },
    {
      "_meta": {
        "href": "https://tg-mobility.cloudhub.io/mobility/v1/regions?parentRegion=70144640",
        "hrefParent": "https://tg-mobility.cloudhub.io/mobility/v1/regions?parentRegion=70144640&parentRegion=70144640"
      },
      "abbreviation": "AK",
      "id": "158385986",
      "name": "Alaska",
      "parentId": "70144640",
      "type": "STATE_PROVINCE"
    },

    ... 

    {
      "_meta": {
        "href": "https://tg-mobility.cloudhub.io/mobility/v1/regions?parentRegion=70144640",
        "hrefParent": "https://tg-mobility.cloudhub.io/mobility/v1/regions?parentRegion=70144640&parentRegion=70144640"
      },
      "abbreviation": "WY",
      "id": "158386042",
      "name": "Wyoming",
      "parentId": "70144640",
      "type": "STATE_PROVINCE"
    }
  ]
}

```