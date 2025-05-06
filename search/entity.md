---
layout: default
title: Entity Retrieval
parent: Search API
nav_order: 3
permalink: /search/entity
---

# Entity Retrieval

This API endpoint allows you to retrieve detailed information about a specific threat intelligence entity by its ID.

**Endpoint:** https://intelligence.threatwinds.com/api/search/v1/entity/{id}

## Parameters

* **Authorization** header _string_ (optional)  
  This authorization header can be obtained from an active session of the account.

* **api-key** header _string_ (optional)  
  Your API key.

* **api-secret** header _string_ (optional)  
  Your API secret.

> **Note**: The `user-id` and `groups` headers are added automatically by the API gateway when required and should not be provided by the client.

* **id** path _string_ (required)  
  The unique identifier of the entity you want to retrieve.

> Note: You must use either the Authorization header OR the API key and secret combination.

## Request

To retrieve an entity by its ID, use a **GET** request:

```bash
curl -X 'GET' \
  'https://intelligence.threatwinds.com/api/search/v1/entity/5f35d2c4-5633-4b16-bbf0-5ca22ef8ea2e' \
  -H 'accept: application/json' \
  -H 'Authorization: Bearer fq6JoEFTsxiXAl1cVdPDnK4emIQCwaUBfq9JoEFTsxhXAl1cVxPDnK4emIQCwaUB'
```

Or using API key and secret:

```bash
curl -X 'GET' \
  'https://intelligence.threatwinds.com/api/search/v1/entity/5f35d2c4-5633-4b16-bbf0-5ca22ef8ea2e' \
  -H 'accept: application/json' \
  -H 'api-key: your-api-key' \
  -H 'api-secret: your-api-secret'
```

## Response

A successful response will return a JSON object containing the entity details:

```json
{
  "id": "5f35d2c4-5633-4b16-bbf0-5ca22ef8ea2e",
  "type": "ip",
  "@timestamp": "2023-06-15T14:30:00Z",
  "reputation": 3,
  "bestReputation": 3,
  "worstReputation": 3,
  "accuracy": 3,
  "lastSeen": "2023-06-15T14:30:00Z",
  "tags": ["dns", "google", "public"],
  "visibleBy": ["public"],
  "attributes": {
    "ip": "8.8.8.8",
    "asn": 15169,
    "oso": "Google LLC",
    "country": "United States"
  }
}
```

The response includes all available information about the entity, including:

* **id** - Unique identifier of the entity
* **type** - Type of the entity (e.g., "ip", "domain", "hash"). For a comprehensive list of all possible entity types, see the [Entity Types](/search/entity-types) page.
* **@timestamp** - Timestamp when the entity was last updated
* **reputation** - Current reputation score
* **bestReputation** - Best historical reputation score
* **worstReputation** - Worst historical reputation score
* **accuracy** - Accuracy score
* **lastSeen** - Timestamp when the entity was last seen
* **tags** - Array of tags associated with the entity
* **visibleBy** - Array of visibility settings
* **attributes** - Object containing entity-specific attributes

## Entity Relations

To retrieve the relations of an entity, you can use the related endpoint:

**Endpoint:** https://intelligence.threatwinds.com/api/search/v1/entity/{id}/relations

This endpoint accepts the same authentication parameters as the entity retrieval endpoint, plus additional query parameters:

* **limit** query _integer_ (optional) - Maximum number of relations to return
* **page** query _integer_ (optional) - Page number for pagination
* **sort** query _string_ (optional) - Field to sort relations by
* **order** query _string_ (optional) - Sort order ("asc" or "desc")
* **types** query _string_ (optional) - Filter relations by entity types

Example request:

```bash
curl -X 'GET' \
  'https://intelligence.threatwinds.com/api/search/v1/entity/5f35d2c4-5633-4b16-bbf0-5ca22ef8ea2e/relations?limit=10&types=domain,url' \
  -H 'accept: application/json' \
  -H 'Authorization: Bearer fq6JoEFTsxiXAl1cVdPDnK4emIQCwaUBfq9JoEFTsxhXAl1cVxPDnK4emIQCwaUB'
```

## Error Codes

* **204** - No content (entity not found)
* **400** - Bad request
* **401** - Unauthorized
* **403** - Forbidden
* **500** - Internal server error
