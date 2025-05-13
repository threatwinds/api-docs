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

* **Authorization** header _string_ (optional)  
  This authorization header can be obtained from an active session of the account.

* **api-key** header _string_ (optional)  
  Your API key.

* **api-secret** header _string_ (optional)  
  Your API secret.

> **Note**: the `user-id` and `groups` headers are added automatically by the API gateway when required and should not be provided by the client.

* **id** path _string_ (required)  
  The unique identifier of the entity for which you want to retrieve comments.

* **limit** query _integer_ (optional)  
  Maximum number of comments to return. Default is 10.

* **page** query _integer_ (optional)  
  Page number for pagination. Default is 1.

* **sort** query _string_ (optional)  
  Field to sort comments by.

* **order** query _string_ (optional)  
  Sort order, either "asc" (ascending) or "desc" (descending).

> Note: You must use either the Authorization header OR the API key and secret combination.

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

* **items:** total number of comments matching the query
* **pages:** total number of pages available
* **results:** array of comments associated with the entity

Each comment object includes:
* **id:** unique identifier of the comment
* **entityID:** the ID of the entity the comment is associated with
* **comment:** the comment text
* **userID:** the ID of the user who created the comment
* **parentID:** the ID of the parent comment (if it's a reply)
* **@timestamp:** timestamp when the comment was created
* **visibleBy:** array of visibility settings for the comment

## Error Codes

* **204:** no content (no comments found)
* **400:** bad request
* **401:** unauthorized
* **403:** forbidden
* **500:** internal server error
