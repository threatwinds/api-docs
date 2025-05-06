---
layout: default
title: Simple Search
parent: Search API
nav_order: 1
permalink: /search/simple-search
---

# Simple Search

This API endpoint allows you to search for threat intelligence entities using a simple query language. It's designed for straightforward searches where you want to find entities matching specific criteria without constructing complex queries.

**Endpoint:** https://intelligence.threatwinds.com/api/search/v1/entities/simple

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

* **limit** query _integer_ (optional)  
  Maximum number of results to return. Default is 10.

* **page** query _integer_ (optional)  
  Page number for pagination. Default is 1.

* **sort** query _string_ (optional)  
  Field to sort results by.

* **order** query _string_ (optional)  
  Sort order, either "asc" (ascending) or "desc" (descending).

* **SimpleSearch** body _object_ (required)  
  The search query parameters.

> Note: You must use either the Authorization header OR the API key and secret combination.

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

* **query** _string_ (required) - The search term (e.g., an IP address, domain, hash, etc.). For a comprehensive list of all possible entity types, see the [Entity Types](/search/entity-types) page.
* **accuracy** _integer_ (optional) - Minimum accuracy level (0 to 3)
* **reputation** _integer_ (optional) - Minimum reputation score (-3 to 3)
* **source** _object_ (optional) - Fields to include or exclude in the response

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
        "value": "8.8.8.8"
      },
      "reputation": 3,
      "accuracy": 3
    }
  ]
}
```

The response includes:

* **items** - Total number of items matching the query
* **pages** - Total number of pages available
* **results** - Array of entities matching the search criteria

## Error Codes

* **204** - No content (no results found)
* **400** - Bad request
* **401** - Unauthorized
* **403** - Forbidden
* **500** - Internal server error
