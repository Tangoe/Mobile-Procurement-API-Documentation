---
layout: default
title: Metadata
---

# Metadata

In many places, this API’s responses include blocks of metadata relating to data points contained within that response. These supplementary blocks, which are always named “_meta,” typically contain fully-qualified URLs that resolve to other API calls for retrieving related detail data.

For example, the API call for a collection of employees might return items containing just a few selected details about each employee. However, each item also contains a “_meta” block populated with a URL that can be used to obtain that specific employee’s full detail record. 

<br />

## Detail Record Example

Metadata blocks make it very easy for the API consumer to easily obtain additional detail data for a selected item without having to bloat the collection by supplying those same details for every item returned within that collection. 

This pattern is particularly useful for use cases such as a list page containing a “view details” button for every item in the list. The end user will likely want to scan a list and choose to find out more details about just one particular item, rather than view all the details of every item. Therefore, it is unnecessary to bloat the API response with a huge volume of detail that the end user is unlikely to want. Moreover, a smaller result set of more targeted data will enable the API to return results much faster.

Below is an illustration of a metadata block contained within a response for an employee collection.

```

{
  "_meta": {
    "href": "https://tngo-mobproc.cloudhub.io/mobproc/v1/employees/48068931",
    "hrefDevices": "https://tngo-mobproc.cloudhub.io/mobproc/v1/assets/devices?limit=10&offset=0&employee=48068931",
    "hrefServices": "https://tngo-mobproc.cloudhub.io/mobproc/v1/assets/services?limit=10&offset=0&employee=48068931"
  },
  "companyEmployeeId": "barbara.andrews@example.com",
  "department": {
    "id": "80022",
    "name": "Marketing"
  },
  "firstName": "Barbara",
  "id": "48068931",
  "lastName": "Andrews",
  "status": "ACTIVE"
},
{
  "_meta": {
    "href": "https://tngo-mobproc.cloudhub.io/mobproc/v1/employees/48068987",
    "hrefDevices": "https://tngo-mobproc.cloudhub.io/mobproc/v1/assets/devices?limit=10&offset=0&employee=48068987",
    "hrefServices": "https://tngo-mobproc.cloudhub.io/mobproc/v1/assets/services?limit=10&offset=0&employee=48068987"
  },
  "companyEmployeeId": "james.carpenter@example.com",
  "department": {
    "id": "80022",
    "name": "Marketing"
  },
  "firstName": "James",
  "id": "48068987",
  "lastName": "Carpenter",
  "status": "ACTIVE"
},
...

```
<br />

## Pagination Data Example

The meta blocks are not limited to providing the URLs used to make API calls for more detailed data. Meta blocks can also hold data that is useful for supporting paginated lists, such as total count, items per page limit, etc. For example:


```

"_meta": {
  "href": "http://tngo-mobproc.cloudhub.io/mobproc/v1/assets/devices?sortAscending=true&maxStatistics=7&limit=3&offset=9",
  "totalCount": 31,
  "offset": 9,
  "limit": 3,
  "hrefStart": "http://tngo-mobproc.cloudhub.io/mobproc/v1/assets/devices?sortAscending=true&maxStatistics=7&limit=3&offset=0",
  "hrefPrevious": "http://tngo-mobproc.cloudhub.io/mobproc/v1/assets/devices?sortAscending=true&maxStatistics=7&limit=3&offset=6",
  "hrefNext": "http://tngo-mobproc.cloudhub.io/mobproc/v1/assets/devices?sortAscending=true&maxStatistics=7&limit=3&offset=12",
  "hrefEnd": "http://tngo-mobproc.cloudhub.io/mobproc/v1/assets/devices?sortAscending=true&maxStatistics=7&limit=3&offset=30"
}

```