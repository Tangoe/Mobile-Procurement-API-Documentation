---
layout: default
title: Tutorials
---

# Metadata

In many places, the Mobile Procurement API’s responses include blocks of metadata relating to the data points within that response. These supplementary blocks, which are always named “_meta,” typically contain fully-qualified URLs that resolve to other API calls for retrieving related detail data.

For example, the API call for a collection of employees might return items containing just a few selected details about each employee. However, each item also contains a “_meta” block populated a URL that can be used to obtain that specific employee’s full detail record. 

## Detail Record Example

Metadata blocks make it very easy for the API consumer to easily obtain details for selected items without having to bloat a collection with details for all items returned. This could be particularly useful for use cases such as a list page containing a “view details” button for every item in the list.The end user will likely want to scan a list and choose to find out more details about just one particular item, rather than view the details of every item. Therefore, it is unnecessary to bloat the API response with a huge volume of detail that the end user is unlikely to want. Moreover, a smaller result set of more targeted data will enable the API to return results much faster.

Below is an illustration of a metadata block contained within a response for an employee collection.

```

{
  "_meta": {
    "href": "http://api.tangoe.com/mobileprocurement/v1/employees/123456781"
  },
  "id": "123456781",
  "firstName": "Barbara",
  "lastName": "Andrews",
  "email": "barbara.andrews@example.com",
  "officePhone": "2035550399"
},
{
  "_meta": {
    "href": "http://api.tangoe.com/mobileprocurement/v1/employees/223456993"
  },
  "id": "223456993",
  "firstName": "James",
  "lastName": "Carpenter",
  "email": "james.carpenter@example.com",
  "officePhone": "2035550402"
},
...

```

## Pagination Data Example

The meta blocks are not limited to providing the URLs used to make API calls for more detailed data. Meta blocks can also hold data that is useful for supporting paginated lists, such as total count, items per page limit, etc. For example:


```

"_meta": {
  "href": "http://api.tangoe.com/mobileprocurement/v1/assets/devices?sortAscending=true&maxStatistics=7&limit=3&offset=9",
  "totalCount": 31,
  "offset": 9,
  "limit": 3,
  "hrefStart": "http://api.tangoe.com/mobileprocurement/v1/assets/devices?sortAscending=true&maxStatistics=7&limit=3&offset=0",
  "hrefPrevious": "http://api.tangoe.com/mobileprocurement/v1/assets/devices?sortAscending=true&maxStatistics=7&limit=3&offset=6",
  "hrefNext": "http://api.tangoe.com/mobileprocurement/v1/assets/devices?sortAscending=true&maxStatistics=7&limit=3&offset=12",
  "hrefEnd": "http://api.tangoe.com/mobileprocurement/v1/assets/devices?sortAscending=true&maxStatistics=7&limit=3&offset=30"
}

```