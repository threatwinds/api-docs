---
layout: default
title: Simple Search
parent: Search API
nav_order: 1
permalink: /search/simple-search
---

# Simple Search

This API endpoint allows you to search for threat intelligence entities using a simple query language. It's designed for straightforward searches where you want to find entities matching specific criteria without constructing complex queries. For more sophisticated search capabilities, see the [Advanced Search](/search/advanced-search) documentation.

**Endpoint:** https://intelligence.threatwinds.com/api/search/v1/entities/simple

## Parameters

### Headers

| Header            | Type   | Required  | Description                                  |
|-------------------|--------|-----------|----------------------------------------------|
| **Authorization** | string | Optional* | Bearer token obtained from an active session |
| **api-key**       | string | Optional* | Your API key                                 |
| **api-secret**    | string | Optional* | Your API secret                              |

> **Note:** The `user-id` and `groups` headers are added automatically by the API gateway when required and shouldn't be provided by the client.

> **Note:** You must use either the Authorization header OR the API key and secret combination. For more details on authentication, see the [Authentication](/auth) documentation.

### Query Parameters

| Parameter | Type    | Required | Description                                                 |
|-----------|---------|----------|-------------------------------------------------------------|
| **limit** | integer | No       | Maximum number of results to return. Default is 10          |
| **page**  | integer | No       | Page number for pagination. Default is 1                    |
| **sort**  | string  | No       | Field to sort results by                                    |
| **order** | string  | No       | Sort order, either "asc" (ascending) or "desc" (descending) |

### Request Body

| Parameter        | Type   | Required | Description                 |
|------------------|--------|----------|-----------------------------|
| **SimpleSearch** | object | Yes      | The search query parameters |

The SimpleSearch object can contain the following fields:

| Field               | Type    | Required | Description                              |
|---------------------|---------|----------|------------------------------------------|
| **query**           | string  | Yes      | The search term (IP, domain, hash, etc.) |
| **accuracy**        | integer | No       | Minimum accuracy level (0 to 3)          |
| **reputation**      | integer | No       | Minimum reputation score (-3 to 3)       |
| **source.includes** | array   | No       | Fields to include in the response        |
| **source.excludes** | array   | No       | Fields to exclude from the response      |

## Request

To search for entities, use a **POST** request with a JSON body containing your search parameters:

```bash
curl -X 'POST' \
  'https://intelligence.threatwinds.com/api/search/v1/entities/simple?limit=10&page=1' \
  -H 'accept: application/json' \
  -H 'Authorization: Bearer fq6JoEFTsxiXAl1cVdPDnK4emIQCwaUBfq9JoEFTsxhXAl1cVxPDnK4emIQCwaUB' \
  -H 'Content-Type: application/json' \
  -d '{
  "query": "8.8.8.8",
  "accuracy": 1,
  "reputation": -2,
  "source": {
    "includes": ["id", "type", "attributes.value", "reputation", "accuracy"],
    "excludes": []
  }
}'
```

The request body parameters include:

| Parameter      | Type    | Required | Description                                                                                                                                                                |
|----------------|---------|----------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **query**      | string  | Yes      | The search term (for example, an IP address, domain, hash, etc.). For a comprehensive list of all possible entity types, see the [Entity Types](/search/entity-types) page |
| **accuracy**   | integer | No       | Minimum accuracy level (0 to 3)                                                                                                                                            |
| **reputation** | integer | No       | Minimum reputation score (-3 to 3)                                                                                                                                         |
| **source**     | object  | No       | Fields to include or exclude in the response                                                                                                                               |

## Response

A successful response will return a JSON object containing the search results:

```json
{
  "items": 1,
  "pages": 1,
  "results": [
    {
      "id": "5f35d2c4-5633-4b16-bbf0-5ca22ef8ea2e",
      "type": "ip",
      "attributes": {
        "ip": "8.8.8.8"
      },
      "reputation": 3,
      "accuracy": 3
    }
  ]
}
```

### Response Structure

| Field       | Type    | Description                                    |
|-------------|---------|------------------------------------------------|
| **items**   | integer | Total number of items matching the query       |
| **pages**   | integer | Total number of pages available                |
| **results** | array   | Array of entities matching the search criteria |

### Entity Object Structure

Each entity in the "results" array contains:

| Field          | Type    | Description                                                       |
|----------------|---------|-------------------------------------------------------------------|
| **id**         | string  | Unique identifier for the entity                                  |
| **type**       | string  | Type of the entity (ip, domain, hash, etc.)                       |
| **attributes** | object  | Entity-specific attributes (varies by type)                       |
| **reputation** | integer | Reputation score (-3 to 3, where -3 is malicious and 3 is benign) |
| **accuracy**   | integer | Accuracy level (0 to 3, where 3 is highest confidence)            |
| **tags**       | array   | Optional array of tags associated with the entity                 |
| **createdAt**  | string  | ISO timestamp when the entity was created                         |
| **updatedAt**  | string  | ISO timestamp when the entity was last updated                    |

## Error Response Headers

For responses with status codes other than 200 and 202, the following headers are included:

| Header        | Description                                                |
|---------------|------------------------------------------------------------|
| **x-error**   | Contains a description of the error that occurred          |
| **x-error-id**| Contains a unique identifier for the error for support     |

## Error Codes

| Status Code | Description           | Possible Cause                                          |
|-------------|-----------------------|---------------------------------------------------------|
| **204**     | No Content            | No results found matching your query criteria           |
| **400**     | Bad Request           | Invalid request parameters or malformed JSON            |
| **401**     | Unauthorized          | Missing or invalid authentication credentials           |
| **403**     | Forbidden             | Authenticated user lacks permission for this operation  |
| **500**     | Internal Server Error | Server-side error; please contact support if persistent |
