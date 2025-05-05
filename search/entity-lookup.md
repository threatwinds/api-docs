---
layout: default
title: Entity Lookup
parent: Search API
nav_order: 6
permalink: /search/entity-lookup
---

# Entity Lookup

This API endpoint allows you to retrieve a threat intelligence entity by specifying its type and value, rather than its ID.

**Endpoint:** https://intelligence.threatwinds.com/api/search/v1/entity

## Parameters

* **Authorization** header _string_ (optional)  
  This authorization header can be obtained from an active session of the account.

* **api-key** header _string_ (optional)  
  Your API key.

* **api-secret** header _string_ (optional)  
  Your API secret.

* **user-id** header _string_ (optional)  
  User ID for filtering results.

* **groups** header _string_ (optional)  
  Groups for filtering results.

* **EntityGet** body _object_ (required)  
  The entity type and value to look up.

> Note: You must use either the Authorization header OR the API key and secret combination.

## Request

To retrieve an entity by its type and value, use a **POST** request with a JSON body containing the entity details:

```bash
curl -X 'POST' \
  'https://intelligence.threatwinds.com/api/search/v1/entity' \
  -H 'accept: application/json' \
  -H 'Authorization: Bearer fq6JoEFTsxiXAl1cVdPDnK4emIQCwaUBfq9JoEFTsxhXAl1cVxPDnK4emIQCwaUB' \
  -H 'Content-Type: application/json' \
  -d '{
  "type": "ip",
  "value": "8.8.8.8"
}'
```

The request body parameters include:

* **type** _string_ (required) - The type of entity (e.g., "ip", "domain", "hash", etc.). For a comprehensive list of all possible entity types, see the [Entity Types](/search/entity-types) page.
* **value** _string_ (required) - The value of the entity (e.g., "8.8.8.8" for an IP address)

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
    "value": "8.8.8.8",
    "label": "Google Public DNS",
    "description": "Google's public DNS server",
    "asn": 15169,
    "organization": "Google LLC",
    "country_code": "US"
  }
}
```

The response includes all available information about the entity, including:

* **id** - Unique identifier of the entity
* **type** - Type of the entity (e.g., "ip", "domain", "hash")
* **@timestamp** - Timestamp when the entity was last updated
* **reputation** - Current reputation score
* **bestReputation** - Best historical reputation score
* **worstReputation** - Worst historical reputation score
* **accuracy** - Accuracy score
* **lastSeen** - Timestamp when the entity was last seen
* **tags** - Array of tags associated with the entity
* **visibleBy** - Array of visibility settings
* **attributes** - Object containing entity-specific attributes

## Error Codes

* **204** - No content (entity not found)
* **400** - Bad request
* **401** - Unauthorized
* **403** - Forbidden
* **500** - Internal server error

> For more detailed information about responses and error codes, please refer to the [Restful API definition](https://intelligence.threatwinds.com/api/search/v1/swagger/index.html).
