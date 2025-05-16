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

### Headers

| Parameter         | Type   | Required  | Description                                  |
|-------------------|--------|-----------|----------------------------------------------|
| **Authorization** | string | Optional* | Bearer token obtained from an active session |
| **api-key**       | string | Optional* | Your API key                                 |
| **api-secret**    | string | Optional* | Your API secret                              |

> **Note:** The `user-id` and `groups` headers are added automatically by the API gateway when required and should not be provided by the client.

### Path Parameters

| Parameter | Type   | Required | Description                                              |
|-----------|--------|----------|----------------------------------------------------------|
| **id**    | string | Yes      | The unique identifier of the entity you want to retrieve |

> **Note:** You must use either the Authorization header OR the API key and secret combination. For more details on authentication, see the [Authentication](/auth) documentation.

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

| Field               | Description                                                                                                                                                 |
|---------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **id**              | Unique identifier of the entity                                                                                                                             |
| **type**            | Type of the entity (e.g., "ip", "domain", "hash"). For a comprehensive list of all possible entity types, see the [Entity Types](/search/entity-types) page |
| **@timestamp**      | Timestamp when the entity was last updated                                                                                                                  |
| **reputation**      | Current reputation score                                                                                                                                    |
| **bestReputation**  | Best historical reputation score                                                                                                                            |
| **worstReputation** | Worst historical reputation score                                                                                                                           |
| **accuracy**        | Accuracy score                                                                                                                                              |
| **lastSeen**        | Timestamp when the entity was last seen                                                                                                                     |
| **tags**            | Array of tags associated with the entity                                                                                                                    |
| **visibleBy**       | Array of visibility settings                                                                                                                                |
| **attributes**      | Object containing entity-specific attributes                                                                                                                |

For detailed information about the entity structure and attributes, see the [Entity Mapping](/search/entity-mapping) documentation.

## Entity Relations

To retrieve the relations of an entity, you can use the related endpoint:

**Endpoint:** https://intelligence.threatwinds.com/api/search/v1/entity/{id}/relations

This endpoint accepts the same authentication parameters as the entity retrieval endpoint, plus additional query parameters:

| Parameter | Type    | Required | Description                                                 |
|-----------|---------|----------|-------------------------------------------------------------|
| **limit** | integer | No       | Maximum number of relations to return                       |
| **page**  | integer | No       | Page number for pagination                                  |
| **sort**  | string  | No       | Field to sort relations by                                  |
| **order** | string  | No       | Sort order, either "asc" (ascending) or "desc" (descending) |
| **types** | string  | No       | Filter relations by entity types (comma-separated list)     |

Example request:

```bash
curl -X 'GET' \
  'https://intelligence.threatwinds.com/api/search/v1/entity/5f35d2c4-5633-4b16-bbf0-5ca22ef8ea2e/relations?limit=10&types=domain,url' \
  -H 'accept: application/json' \
  -H 'Authorization: Bearer fq6JoEFTsxiXAl1cVdPDnK4emIQCwaUBfq9JoEFTsxhXAl1cVxPDnK4emIQCwaUB'
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
| **204**     | No Content            | Entity not found                                        |
| **400**     | Bad Request           | Invalid request parameters or malformed JSON            |
| **401**     | Unauthorized          | Missing or invalid authentication credentials           |
| **403**     | Forbidden             | Authenticated user lacks permission for this operation  |
| **500**     | Internal Server Error | Server-side error; please contact support if persistent |
