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

### Headers

| Parameter         | Type   | Required  | Description                                  |
|-------------------|--------|-----------|----------------------------------------------|
| **Authorization** | string | Optional* | Bearer token obtained from an active session |
| **api-key**       | string | Optional* | Your API key                                 |
| **api-secret**    | string | Optional* | Your API secret                              |

> **Note:** The `user-id` and `groups` headers are added automatically by the API gateway when required and shouldn't be provided by the client.

### Request Body

| Parameter     | Type   | Required | Description                          |
|---------------|--------|----------|--------------------------------------|
| **EntityGet** | object | Yes      | The entity type and value to look up |

> **Note:** You must use either the Authorization header OR the API key and secret combination.

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

| Parameter | Type   | Required | Description                                                                                                                                                       |
|-----------|--------|----------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **type**  | string | Yes      | The type of entity (e.g., "ip", "domain", "hash", etc.). For a comprehensive list of all possible entity types, see the [Entity Types](/search/entity-types) page |
| **value** | string | Yes      | The value of the entity (e.g., "8.8.8.8" for an IP address)                                                                                                       |

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

| Field               | Description                                       |
|---------------------|---------------------------------------------------|
| **id**              | Unique identifier of the entity                   |
| **type**            | Type of the entity (e.g., "ip", "domain", "hash") |
| **@timestamp**      | Timestamp when the entity was last updated        |
| **reputation**      | Current reputation score                          |
| **bestReputation**  | Best historical reputation score                  |
| **worstReputation** | Worst historical reputation score                 |
| **accuracy**        | Accuracy score                                    |
| **lastSeen**        | Timestamp when the entity was last seen           |
| **tags**            | Array of tags associated with the entity          |
| **visibleBy**       | Array of visibility settings                      |
| **attributes**      | Object containing entity-specific attributes      |

## Error Response Headers

For responses with status codes other than 200 and 202, the following headers are included:

| Header        | Description                                                |
|---------------|------------------------------------------------------------|
| **x-error**   | Contains a description of the error that occurred          |
| **x-error-id**| Contains a unique identifier for the error for support     |

## Error Codes

| Status Code | Description           | Possible Cause                                          |
|-------------|-----------------------|---------------------------------------------------------|
| **204**     | No Content            | Entity not found                                        |
| **400**     | Bad Request           | Invalid request parameters or malformed JSON            |
| **401**     | Unauthorized          | Missing or invalid authentication credentials           |
| **403**     | Forbidden             | Authenticated user lacks permission for this operation  |
| **500**     | Internal Server Error | Server-side error; please contact support if persistent |
