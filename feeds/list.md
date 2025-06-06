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

| Parameter         | Location | Type   | Required  | Description                                                        |
|-------------------|----------|--------|-----------|-------------------------------------------------------------------|
| **Authorization** | header   | string | Optional* | Bearer token obtained from an active session                       |
| **api-key**       | header   | string | Optional* | Your API key                                                       |
| **api-secret**    | header   | string | Optional* | Your API secret                                                    |

> **Note:** You must use either the Authorization header OR the API key and secret combination.

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

| Field        | Description                                                                |
|--------------|----------------------------------------------------------------------------|
| **name**     | The name of the feed (for example, "ip", "md5")                            |
| **type**     | The type of the feed (for example, "accumulative", "daily")                |
| **accuracy** | The accuracy level of the feed (for example, "level1", "level2", "level3") |

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

## Error Response Headers

For responses with status codes other than 200 and 202, the following headers are included:

| Header        | Description                                                |
|---------------|------------------------------------------------------------|
| **x-error**   | Contains a description of the error that occurred          |
| **x-error-id**| Contains a unique identifier for the error for support     |

## Error Codes

| Status Code | Description           | Possible Cause                                          |
|-------------|-----------------------|---------------------------------------------------------|
| **400**     | Bad Request           | Invalid request parameters or malformed JSON            |
| **500**     | Internal Server Error | Server-side error; please contact support if persistent |
