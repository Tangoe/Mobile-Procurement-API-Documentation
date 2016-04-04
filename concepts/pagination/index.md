---
layout: default
title: Pagination
---

# Pagination

API calls that retrieve collections of items frequently return large, unbounded result sets. In these cases, results are almost always paginated to improve performance and help the user more easily manage handling of the results.

<br />

## Input Parameters

All paginated API calls require two input parameters: **limit** and **offset**. 

The limit sets the maximum number of items to be included in single “page.” In other words, it is the number of items to be returned within the response, out of the total result set found by the query for that API call. 

The limit value sets the maximum number of items to be included in the current page that gets returned from the total result set. The offset value identifies the index of the first item in that current page. For example, assume you have a result set of 50 items and you set the limit to 10 and the offset to 30. The result set would be grouped into 5 pages (50 / 10 = 5) and return the fourth page. (Indices 0 through 29 equals 30 items; 30 items divided by 10 equals 3 pages; so index 30 is the first item on the fourth page.)

<br />

## Response Body

Paginated results always include a [metadata block]({{site.url}}concepts/metadata/) containing the details needed by the API consumer to navigate the result set. These details include: 

* **totalCount** - The total number of items in the entire result set (i.e., across all pages).

* **hrefStart** - A fully-qualified URL for an API call that will return the FIRST page in the result set.

* **hrefPrevious** - A fully-qualified URL for an API call that will return the page that occurs immediately BEFORE the current page in the result set.

* **hrefNext** - A fully-qualified URL that will return the the page that occurs immediately AFTER the current page in the result set.

* **hrefEnd** - A fully-qualified URL that will return the LAST page in the result set.

Each of these pagination position properties (i.e., hrefStart, hrefPrevious, hrefNext, hrefEnd) will be returned only when the link resolves to a page other than the current page. For example, say the offset is set to zero. The API response will return the first page in the result set. Therefore, the metadata will not include the hrefStart and hrefPrevious properties since they would both point to the same page as the one that is returned. This behavior is intended to make it easier for user interface logic to populate a pagination control. The logic can simply check for the absence of these properties to know when to disable navigation links.

Here is an example of what the pagination information in the metadata block might look like:

```
"_meta": {
  "href": "https://tngo-mobproc.cloudhub.io/mobproc/v1/employees?limit=10&offset=30&sortAscending=true",
  "hrefEnd": "https://tngo--mobproc.cloudhub.io/mobproc/v1/employees?sortAscending=true&offset=70&limit=10",
  "hrefNext": "https://tngo-mobproc.cloudhub.io/mobproc/v1/employees?sortAscending=true&offset=40&limit=10",
  "hrefPrevious": "https://tngo-mobproc.cloudhub.io/mobproc/v1/employees?sortAscending=true&offset=20&limit=10",
  "hrefStart": "https://tngo-mobproc.cloudhub.io/mobproc/v1/employees?sortAscending=true&offset=0&limit=10",
  "limit": 10,
  "offset": 30,
  "totalCount": 72
}
```
