---
layout: default
title: States and Provinces
---


# Get a List of States and Provinces

**This tutorial provides instructions for optaining a list of states or provinces that are located within a specific country.**

<br/>

When creating forms where users are required to enter address information, a dropdown list of states or provinces might need to be displayed. The **/regions** endpoint is used to obtain this data by . 

To begin, you must first determine the region ID for the relevant country. If you do not know this ID, call the **/regions** endpoint to get a list of countries, each with their own unique region ID.

Next, call the **/regions** endpoint, setting the request's **parentRegion** query parameter with the country's region ID. The API will return all of the states or provinces that are located within that country. Please note that each item in the list will include a TYPE property set to STATE_PROVINCE.

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
    {
      "_meta": {
        "href": "https://tg-mobility.cloudhub.io/mobility/v1/regions?parentRegion=70144640",
        "hrefParent": "https://tg-mobility.cloudhub.io/mobility/v1/regions?parentRegion=70144640&parentRegion=70144640"
      },
      "abbreviation": "AS",
      "id": "158385989",
      "name": "American Samoa",
      "parentId": "70144640",
      "type": "STATE_PROVINCE"
    },
    {
      "_meta": {
        "href": "https://tg-mobility.cloudhub.io/mobility/v1/regions?parentRegion=70144640",
        "hrefParent": "https://tg-mobility.cloudhub.io/mobility/v1/regions?parentRegion=70144640&parentRegion=70144640"
      },
      "abbreviation": "AZ",
      "id": "158385990",
      "name": "Arizona",
      "parentId": "70144640",
      "type": "STATE_PROVINCE"
    },
    {
      "_meta": {
        "href": "https://tg-mobility.cloudhub.io/mobility/v1/regions?parentRegion=70144640",
        "hrefParent": "https://tg-mobility.cloudhub.io/mobility/v1/regions?parentRegion=70144640&parentRegion=70144640"
      },
      "abbreviation": "AR",
      "id": "158385988",
      "name": "Arkansas",
      "parentId": "70144640",
      "type": "STATE_PROVINCE"
    },
    {
      "_meta": {
        "href": "https://tg-mobility.cloudhub.io/mobility/v1/regions?parentRegion=70144640",
        "hrefParent": "https://tg-mobility.cloudhub.io/mobility/v1/regions?parentRegion=70144640&parentRegion=70144640"
      },
      "abbreviation": "CA",
      "id": "158385991",
      "name": "California",
      "parentId": "70144640",
      "type": "STATE_PROVINCE"
    },

    ... 

    {
      "_meta": {
        "href": "https://tg-mobility.cloudhub.io/mobility/v1/regions?parentRegion=70144640",
        "hrefParent": "https://tg-mobility.cloudhub.io/mobility/v1/regions?parentRegion=70144640&parentRegion=70144640"
      },
      "abbreviation": "WI",
      "id": "158386040",
      "name": "Wisconsin",
      "parentId": "70144640",
      "type": "STATE_PROVINCE"
    },
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
