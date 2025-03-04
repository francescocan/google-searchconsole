# Getting all your search data

** check the official source for updates:** https://developers.google.com/webmaster-tools/search-console-api-original/v3/how-tos/all-your-data


Query details: You can query data grouped by **page or property**.

## Grouped by page

**For accurate counts, you must omit the page and query dimensions, like this:**


* "startDate": "2018-06-01",
* "endDate": "2018-06-01",
**"dimensions": ["country", "device"],**
* "searchType": "web",
* "aggregationType": "byPage"
* startDate / endDate: Choose a one-day window by selecting the same date.
* dimensions: Optionally include country and/or device.
* searchType: Enumerate over web, image, and video as desired in separate queries.
* aggregationType: Must be byPage.

**For greater detail, including page and/or query information, at the expense of losing some data, run a query like this:**

* "startDate": "2018-06-01",
* "endDate": "2018-06-01",
* "dimensions": ["page", "query", "country", "device"],
* "searchType": "web"
* startDate / endDate: Choose a one-day window by selecting the same date.
* dimensions: Include page. Optionally include any combination of query, country, or device.
* searchType: Enumerate over "web", "image", and "video" as desired in separate queries.



## Grouped by property
**For accurate counts, you must omit the page and query dimensions, like this:**


* "startDate": "2018-06-01",
* "endDate": "2018-06-01",
* "dimensions": ["country", "device"],
* "searchType": "web"
* startDate / endDate: Choose a one-day window by selecting the same date.
* dimensions: Optionally include country and/or device.
* searchType: Optionally enumerate over web, image, and video as desired in separate queries.


**For greater detail, including query, country, and/or device information, at the expense of losing some data, run a query like this:**


* "startDate": "2018-06-01",
* "endDate": "2018-06-01",
* "dimensions": ["query", "country", "device"],
* "searchType": "web"
* startDate / endDate: Choose a one-day window by selecting the same date.
* dimensions: Optionally include any combination of query, country, or device.
* searchType: Enumerate over "web", "image", and "video" as desired in separate queries.
* Grouping results by page or property
* Impressions, clicks, position, and click-through-rate are are calculated differently when grouping results by page rather than by property. Learn more.


### Why do I lose data when asking for more detail?

When you group by page and/or query, our system may drop some data in order to be able to calculate results in a reasonable time using a reasonable amount of computing resources.


### Getting search appearance data

**Search appearance is not available as a column along with any other dimensions.** Therefore, if you want to see search appearance information for your site, you must follow this process:

Specify searchAppearance as the **only dimension**, which will group all data by search appearance type with no other dimensions.
**Optionally run a second query**, filtering by one of the search appearance types listed in step 1, adding any desired dimensions to the query (page, country, query, etc).
To retrieve data about multiple search appearance types, you must run the second step once per search appearance type listed step 1.

**First query:**

Get list of search appearance types on your site.


{
  "startDate": "2018-05-01",
  "endDate": "2018-05-31",
  "searchType": "web",
  "dimensions": [
    "searchAppearance"
  ]
}
Results:

Your site has type INSTANT_APP, AMP_BLUE_LINK, and so on.


 "rows": [
  {
   "keys": [
    "INSTANT_APP"
   ],
   "clicks": 443024.0,
   "impressions": 4109826.0,
   "ctr": 0.10779629113251997,
   "position": 1.088168452873674
  },
  {
   "keys": [
    "AMP_BLUE_LINK"
   ],
   "clicks": 429887.0,
   "impressions": 1.7090884E7,
   "ctr": 0.025152999692701676,
   "position": 7.313451603790653
  },...

**Second query:**

*Filter by one of the search appearance types found in step 1, along with any dimensions that you like (page, device, etc). Here we filter by AMP_BLUE_LINK.*


{
  "startDate": "2018-05-01",
  "endDate": "2018-05-31",
  "searchType": "web",
  "dimensions": [
    "device" // and/or page, country, ...
  ],
  "dimensionFilterGroups": [
    {
      "filters": [
        {
          "dimension": "searchAppearance",
          "operator": "equals",
          "expression": "AMP_BLUE_LINK"
        }
      ]
    }
  ]
}
Results:

**Breakdown of AMP_BLUE_LINK by device types.**


"rows": [
  {
   "keys": [
    "MOBILE"
   ],
   "clicks": 429887.0,
   "impressions": 1.7090783E7,
   "ctr": 0.025153148337323107,
   "position": 7.31339517914422
  },
  {
   "keys": [
    "DESKTOP"
   ],
   "clicks": 0.0,
   "impressions": 66.0,
   "ctr": 0.0,
   "position": 12.257575757575758
  },
...
