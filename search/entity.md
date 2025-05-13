---
layout: default
title: Entity Retrieval
parent: Search API
nav_order: 3
permalink: /search/entity
---

# Entity Retrieval

This API endpoint allows you to retrieve detailed information about a specific threat intelligence entity by its ID. If you don't know the entity ID, you can use the [Simple Search](/search/simple-search) or [Advanced Search](/search/advanced-search) endpoints to find entities based on various criteria.

**Endpoint:** https://intelligence.threatwinds.com/api/search/v1/entity/{id}

## Parameters

* **Authorization** header _string_ (optional)  
  This authorization header can be obtained from an active session of the account.

* **api-key** header _string_ (optional)  
  Your API key.

* **api-secret** header _string_ (optional)  
  Your API secret.

> **Note**: the `user-id` and `groups` headers are added automatically by the API gateway when required and should not be provided by the client.

* **id** path _string_ (required)  
  The unique identifier of the entity you want to retrieve.

> Note: You must use either the Authorization header OR the API key and secret combination. For more details on authentication, see the [Authentication](/auth) documentation.

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

* **id:** unique identifier of the entity
* **type:** type of the entity (e.g., "ip", "domain", "hash"). For a comprehensive list of all possible entity types, see the [Entity Types](/search/entity-types) page.
* **@timestamp:** timestamp when the entity was last updated
* **reputation:** current reputation score
* **bestReputation:** best historical reputation score
* **worstReputation:** worst historical reputation score
* **accuracy:** accuracy score
* **lastSeen:** timestamp when the entity was last seen
* **tags:** array of tags associated with the entity
* **visibleBy:** array of visibility settings
* **attributes:** object containing entity-specific attributes

For detailed information about the entity structure and attributes, see the [Entity Mapping](/search/entity-mapping) documentation.

## Entity Relations

To retrieve the relations of an entity, you can use the related endpoint:

**Endpoint:** https://intelligence.threatwinds.com/api/search/v1/entity/{id}/relations

This endpoint accepts the same authentication parameters as the entity retrieval endpoint, plus additional query parameters:

* **limit** query _integer_ (optional)—Maximum number of relations to return
* **page** query _integer_ (optional)—Page number for pagination
* **sort** query _string_ (optional)—Field to sort relations by
* **order** query _string_ (optional)—Sort order ("asc" or "desc")
* **types** query _string_ (optional)—Filter relations by entity types

Example request:

```bash
curl -X 'GET' \
  'https://intelligence.threatwinds.com/api/search/v1/entity/5f35d2c4-5633-4b16-bbf0-5ca22ef8ea2e/relations?limit=10&types=domain,url' \
  -H 'accept: application/json' \
  -H 'Authorization: Bearer fq6JoEFTsxiXAl1cVdPDnK4emIQCwaUBfq9JoEFTsxhXAl1cVxPDnK4emIQCwaUB'
```

## Error Codes

* **204:** no content (entity not found)
* **400:** bad request
* **401:** unauthorized
* **403:** forbidden
* **500:** internal server error
