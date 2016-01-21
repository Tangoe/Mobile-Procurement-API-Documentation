---
layout: default
title: Tutorials
logoTangoe: tangoe-logo_125.png
---

# Pagination

API calls that retrieve collections of items frequently return large, unbounded result sets. In these cases, results are almost always paginated to both improve performance and help the user more easily manage handling of the results.
<br />

## Input Parameters

All paginated API calls require two input parameters: **limit** and **offset**. 

The limit value sets the maximum number of items to be included in single “page.” In other words, it is the number of items to be returned within the JSON, out of the total result set found by the query for that API call. The offset value identifies the index of first item contained in the current page. For example, assume you have a total result set of 50 items and you set the limit to 10 and the offset to 30. The result set would be grouped into 5 pages (50 / 10 = 5) and return 10 items starting with the third page (30 / 10 = 3).
<br />

## JSON Response Body

Paginated results always include a [metadata block](/concepts/pagination/) containing the details needed by the API consumer to navigate the resultset. These details include: 

* **totalCount** - The total number of items in the entire result set (i.e., across all pages).

* **hrefStart** - A fully-qualified URL for an API call that will return the FIRST page in the result set.

* **hrefPrevious** - A fully-qualified URL for an API call that will return the page that occurs immediately BEFORE the current page in the result set.

* **hrefNext** - A fully-qualified URL that will return the the page that occurs immediately AFTER the current page in the result set.

* **hrefEnd** - A fully-qualified URL that will return the LAST page in the result set.

These pagination position properties (i.e., hrefStart, hrefPrevious, hrefNext, hrefEnd) will only be returned when that link resolves to a page other than the current page. For example, say the offset is set to zero (i.e., asking to return the first page). The API response will then return the first page in the result set. Therefore, the metadata will not include the hrefStart and hrefPrevious properties since they would both point to the same page that is returned. This behavior is intended to make it easier for user interface logic to populate a pagination control. The logic can simply check for the absence of these properties to know when to disable navigation links.

Here is an example of what the pagination information in the metadata block might looks like:

```
"_meta": {
  ...
  "totalCount": 24,
  "offset": 10,
  "limit": 5,
  "hrefStart": "http://api.tangoe.com/mobileprocurement/v1/orders?sortAscending=true&maxStatistics=7&limit=5&offset=0",
  "hrefPrevious": "http://api.tangoe.com/mobileprocurement/v1/orders?sortAscending=true&maxStatistics=7&limit=5&offset=5",
  "hrefNext": "http://api.tangoe.com/mobileprocurement/v1/orders?sortAscending=true&maxStatistics=7&limit=5&offset=15",
  "hrefEnd": "http://api.tangoe.com/mobileprocurement/v1/orders?sortAscending=true&maxStatistics=7&limit=5&offset=20"
}
```
