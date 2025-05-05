---
layout: default
title: List Feeds
parent: Feeds API
nav_order: 1
permalink: /feeds/list
---

# List Feeds

This API endpoint returns a list of all available threat intelligence feeds.

**Endpoint:** https://intelligence.threatwinds.com/api/feeds/v1/list

## Parameters

* **Authorization** header _string_ (optional)  
  This authorization header can be obtained from an active session of the account.

* **api-key** header _string_ (optional)  
  Your API key.

* **api-secret** header _string_ (optional)  
  Your API secret.

> Note: You must use either the Authorization header OR the API key and secret combination.

## Request

To get a list of all available feeds, use a **GET** request, for example:

```bash
curl -X 'GET' \
  'https://intelligence.threatwinds.com/api/feeds/v1/list' \
  -H 'accept: application/json' \
  -H 'Authorization: Bearer fq6JoEFTsxiXAl1cVdPDnK4emIQCwaUBfq9JoEFTsxhXAl1cVxPDnK4emIQCwaUB'
```

Or using API key and secret:

```bash
curl -X 'GET' \
  'https://intelligence.threatwinds.com/api/feeds/v1/list' \
  -H 'accept: application/json' \
  -H 'api-key: your-api-key' \
  -H 'api-secret: your-api-secret'
```

## Response

A successful response will return a JSON array of feed items, each containing:

* **name** - The name of the feed (e.g., "ip", "md5")
* **type** - The type of the feed (e.g., "accumulative", "daily")
* **accuracy** - The accuracy level of the feed (e.g., "level1", "level2", "level3")

Example response:

```json
[
  {
    "name": "ip",
    "type": "accumulative",
    "accuracy": "level1"
  },
  {
    "name": "md5",
    "type": "daily",
    "accuracy": "level2"
  },
  {
    "name": "domain",
    "type": "accumulative",
    "accuracy": "level3"
  }
]
```

## Error Codes

* **400** - Bad request
* **500** - Internal server error

> For more detailed information about responses and error codes, please refer to the [Restful API definition](https://intelligence.threatwinds.com/api/feeds/v1/swagger/index.html).