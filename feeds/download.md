---
layout: default
title: Download Feeds
parent: Feeds API
nav_order: 2
permalink: /feeds/download
---

# Download Feeds

This API endpoint allows you to download feed files containing threat intelligence data.

**Endpoint:** https://intelligence.threatwinds.com/api/feeds/v1/download/list/{level}/{type}/{name}

## Parameters

### Headers

| Parameter         | Type   | Required  | Description                                                        |
|-------------------|--------|-----------|-------------------------------------------------------------------|
| **Authorization** | string | Optional* | Bearer token obtained from an active session                       |
| **api-key**       | string | Optional* | Your API key                                                       |
| **api-secret**    | string | Optional* | Your API secret                                                    |

### Path Parameters

| Parameter | Type   | Required | Description                                                          |
|-----------|--------|----------|----------------------------------------------------------------------|
| **level** | string | Yes      | Accuracy level of the feed. Possible values: "level1", "level2", "level3" |
| **type**  | string | Yes      | Type of feed. Possible values: "accumulative", "daily"               |
| **name**  | string | Yes      | Name of the feed (e.g., "ip", "md5", etc.)                           |

> **Note:** You must use either the Authorization header OR the API key and secret combination.

## Request

To download a feed file, use a **GET** request, for example:

```bash
curl -X 'GET' \
  'https://intelligence.threatwinds.com/api/feeds/v1/download/list/level1/accumulative/ip' \
  -H 'accept: application/gzip' \
  -H 'Authorization: Bearer fq6JoEFTsxiXAl1cVdPDnK4emIQCwaUBfq9JoEFTsxhXAl1cVxPDnK4emIQCwaUB'
```

Or using API key and secret:

```bash
curl -X 'GET' \
  'https://intelligence.threatwinds.com/api/feeds/v1/download/list/level1/accumulative/ip' \
  -H 'accept: application/gzip' \
  -H 'api-key: your-api-key' \
  -H 'api-secret: your-api-secret'
```

## Response

A successful response will return the feed file content in one of the following formats:

| Content Type             | Description                  |
|--------------------------|------------------------------|
| **application/gzip**     | Compressed feed file         |
| **application/x-ndjson** | Newline-delimited JSON       |
| **text/plain**           | Plain text                   |

The format depends on the feed type and the Accept header in your request.

## Error Codes

| Status Code | Description           | Possible Cause                                          |
|-------------|-----------------------|---------------------------------------------------------|
| **204**     | No Content            | File not found                                          |
| **400**     | Bad Request           | Invalid request parameters or malformed JSON            |
| **500**     | Internal Server Error | Server-side error; please contact support if persistent |

# Checksum Verification

You can also verify the integrity of downloaded feed files using the checksum endpoint.

**Endpoint:** https://intelligence.threatwinds.com/api/feeds/v1/download/checksum

This endpoint follows the same authentication and parameter patterns as the download endpoint.

## Request

To get the checksum for a feed file, use a **GET** request, for example:

```bash
curl -X 'GET' \
  'https://intelligence.threatwinds.com/api/feeds/v1/download/checksum?level=level1&type=accumulative&name=ip' \
  -H 'accept: text/plain' \
  -H 'Authorization: Bearer fq6JoEFTsxiXAl1cVdPDnK4emIQCwaUBfq9JoEFTsxhXAl1cVxPDnK4emIQCwaUB'
```

## Response

A successful response will return the checksum of the feed file as plain text.
