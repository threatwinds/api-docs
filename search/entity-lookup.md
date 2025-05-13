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

> **Note**: the `user-id` and `groups` headers are added automatically by the API gateway when required and shouldn't be provided by the client.

* **EntityGet** body _object_ (required)  
  The entity type and value to look up.

> Note: you must use either the Authorization header OR the API key and secret combination.

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

* **type** _string_ (required)—The type of entity (e.g., "ip", "domain", "hash", etc.). For a comprehensive list of all possible entity types, see the [Entity Types](/search/entity-types) page.
* **value** _string_ (required)—The value of the entity (e.g., "8.8.8.8" for an IP address)

## Response

A successful response will return a JSON object containing the entity details:

```json
{
  "id": "ip-5ca22ef8ea2e234234",
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
    "aso": "Google LLC",
    "country": "United States"
  }
}
```

The response includes all available information about the entity, including:

* **id:** unique identifier of the entity
* **type:** type of the entity (e.g., "ip", "domain", "hash")
* **@timestamp:** timestamp when the entity was last updated
* **reputation:** current reputation score
* **bestReputation:** best historical reputation score
* **worstReputation:** worst historical reputation score
* **accuracy:** accuracy score
* **lastSeen:** timestamp when the entity was last seen
* **tags:** array of tags associated with the entity
* **visibleBy:** array of visibility settings
* **attributes:** object containing entity-specific attributes

## Error Codes

* **204:** no content (entity not found)
* **400:** bad request
* **401:** unauthorized
* **403:** forbidden
* **500:** internal server error
