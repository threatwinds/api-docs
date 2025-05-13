---
layout: default
title: Comments
parent: Search API
nav_order: 4
permalink: /search/comments
---

# Comments

This API endpoint allows you to retrieve comments associated with a specific threat intelligence entity by its ID.

**Endpoint:** https://intelligence.threatwinds.com/api/search/v1/comments/{id}

## Parameters

### Headers

| Parameter         | Type   | Required  | Description                                  |
|-------------------|--------|-----------|----------------------------------------------|
| **Authorization** | string | Optional* | Bearer token obtained from an active session |
| **api-key**       | string | Optional* | Your API key                                 |
| **api-secret**    | string | Optional* | Your API secret                              |

> **Note:** The `user-id` and `groups` headers are added automatically by the API gateway when required and should not be provided by the client.

### Path Parameters

| Parameter | Type   | Required | Description                                                                 |
|-----------|--------|----------|-----------------------------------------------------------------------------|
| **id**    | string | Yes      | The unique identifier of the entity for which you want to retrieve comments |

### Query Parameters

| Parameter | Type    | Required | Description                                                 |
|-----------|---------|----------|-------------------------------------------------------------|
| **limit** | integer | No       | Maximum number of results to return. Default is 10          |
| **page**  | integer | No       | Page number for pagination. Default is 1                    |
| **sort**  | string  | No       | Field to sort comments by                                   |
| **order** | string  | No       | Sort order, either "asc" (ascending) or "desc" (descending) |

> **Note:** You must use either the Authorization header OR the API key and secret combination.

## Request

To retrieve comments for a specific entity, use a **GET** request:

```bash
curl -X 'GET' \
  'https://intelligence.threatwinds.com/api/search/v1/comments/5f35d2c4-5633-4b16-bbf0-5ca22ef8ea2e?limit=10&page=1' \
  -H 'accept: application/json' \
  -H 'Authorization: Bearer fq6JoEFTsxiXAl1cVdPDnK4emIQCwaUBfq9JoEFTsxhXAl1cVxPDnK4emIQCwaUB'
```

Or using API key and secret:

```bash
curl -X 'GET' \
  'https://intelligence.threatwinds.com/api/search/v1/comments/5f35d2c4-5633-4b16-bbf0-5ca22ef8ea2e?limit=10&page=1' \
  -H 'accept: application/json' \
  -H 'api-key: your-api-key' \
  -H 'api-secret: your-api-secret'
```

## Response

A successful response will return a JSON object containing the comments:

```json
{
  "items": 2,
  "pages": 1,
  "results": [
    {
      "id": "7a1b3c2d-4e5f-6g7h-8i9j-0k1l2m3n4o5p",
      "entityID": "5f35d2c4-5633-4b16-bbf0-5ca22ef8ea2e",
      "comment": "This IP is a known Google DNS server.",
      "userID": "6a2b4c5d-6e7f-8g9h-0i1j-2k3l4m5n6o7p",
      "parentID": "00000000-0000-0000-0000-000000000000",
      "@timestamp": "2023-06-15T14:30:00Z",
      "visibleBy": ["public"]
    },
    {
      "id": "8b2c4d5e-6f8g-9h0i-1j2k-3l4m5n6o7p8q",
      "entityID": "5f35d2c4-5633-4b16-bbf0-5ca22ef8ea2e",
      "comment": "Confirmed as benign.",
      "userID": "7b3c5d6e-7f8g-9h0i-1j2k-3l4m5n6o7p8q",
      "parentID": "7a1b3c2d-4e5f-6g7h-8i9j-0k1l2m3n4o5p",
      "@timestamp": "2023-06-16T09:45:00Z",
      "visibleBy": ["public", "private"]
    }
  ]
}
```

The response includes:

| Field       | Description                                    |
|-------------|------------------------------------------------|
| **items**   | Total number of comments matching the query    |
| **pages**   | Total number of pages available                |
| **results** | Array of comments associated with the entity   |

Each comment object includes:

| Field          | Description                                         |
|----------------|-----------------------------------------------------|
| **id**         | Unique identifier of the comment                    |
| **entityID**   | The ID of the entity the comment is associated with |
| **comment**    | The comment text                                    |
| **userID**     | The ID of the user who created the comment          |
| **parentID**   | The ID of the parent comment (if it's a reply)      |
| **@timestamp** | Timestamp when the comment was created              |
| **visibleBy**  | Array of visibility settings for the comment        |

## Error Codes

| Status Code | Description           | Possible Cause                                          |
|-------------|-----------------------|---------------------------------------------------------|
| **204**     | No Content            | No comments found for the specified entity              |
| **400**     | Bad Request           | Invalid request parameters or malformed JSON            |
| **401**     | Unauthorized          | Missing or invalid authentication credentials           |
| **403**     | Forbidden             | Authenticated user lacks permission for this operation  |
| **500**     | Internal Server Error | Server-side error; please contact support if persistent |
