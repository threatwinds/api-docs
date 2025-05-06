---
layout: default
title: History Search
parent: Search API
nav_order: 5
permalink: /search/history-search
---

# History Search

The History Search endpoints allow you to search for historical records of threat intelligence entities. This is useful for tracking how entities have changed over time and analyzing trends in threat intelligence data.

## Simple History Search

This API endpoint allows you to search for historical records of threat intelligence entities using a simple query language.

**Endpoint:** https://intelligence.threatwinds.com/api/search/v1/entities/history/simple

### Parameters

* **Authorization** header _string_ (optional)  
  This authorization header can be obtained from an active session of the account.

* **api-key** header _string_ (optional)  
  Your API key.

* **api-secret** header _string_ (optional)  
  Your API secret.

> **Note**: The `user-id` and `groups` headers are added automatically by the API gateway when required and should not be provided by the client.

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

### Request

To search for entity history records, use a **POST** request with a JSON body containing your search parameters:

```bash
curl -X 'POST' \
  'https://intelligence.threatwinds.com/api/search/v1/entities/history/simple?limit=10&page=1' \
  -H 'accept: application/json' \
  -H 'Authorization: Bearer fq6JoEFTsxiXAl1cVdPDnK4emIQCwaUBfq9JoEFTsxhXAl1cVxPDnK4emIQCwaUB' \
  -H 'Content-Type: application/json' \
  -d '{
  "query": "8.8.8.8",
  "accuracy": 1,
  "reputation": -2,
  "source": {
    "includes": ["id", "type", "attributes.value", "reputation", "accuracy", "@timestamp"],
    "excludes": []
  }
}'
```

The request body parameters include:

* **query** _string_ (required) - The search term (e.g., an IP address, domain, hash, etc.). For a comprehensive list of all possible entity types, see the [Entity Types](/search/entity-types) page.
* **accuracy** _integer_ (optional) - Minimum accuracy level (0 to 3)
* **reputation** _integer_ (optional) - Minimum reputation score (-3 to 3)
* **source** _object_ (optional) - Fields to include or exclude in the response

### Response

A successful response will return a JSON object containing the historical entity records:

```json
{
  "items": 3,
  "pages": 1,
  "results": [
    {
      "id": "5f35d2c4-5633-4b16-bbf0-5ca22ef8ea2e",
      "entityID": "ip-a1b2c3d4e5f6g7h8i9j0k1l2m3n4o5p6q7r8s9t0u1v2w3x4y5z6a7b8c9d0e1f2",
      "type": "ip",
      "@timestamp": "2023-06-15T14:30:00Z",
      "attributes": {
        "value": "8.8.8.8"
      },
      "reputation": 3,
      "accuracy": 3
    },
    {
      "id": "5f35d2c4-5633-4b16-bbf0-5ca22ef8ea2e",
      "entityID": "ip-a1b2c3d4e5f6g7h8i9j0k1l2m3n4o5p6q7r8s9t0u1v2w3x4y5z6a7b8c9d0e1f2",
      "@timestamp": "2023-05-20T10:15:00Z",
      "type": "ip",
      "attributes": {
        "value": "8.8.8.8"
      },
      "reputation": 2,
      "accuracy": 2
    },
    {
      "id": "5f35d2c4-5633-4b16-bbf0-5ca22ef8ea2e",
      "entityID": "ip-a1b2c3d4e5f6g7h8i9j0k1l2m3n4o5p6q7r8s9t0u1v2w3x4y5z6a7b8c9d0e1f2",
      "@timestamp": "2023-04-10T08:45:00Z",
      "type": "ip",
      "attributes": {
        "value": "8.8.8.8"
      },
      "reputation": 1,
      "accuracy": 1
    }
  ]
}
```

The response includes:

* **items** - Total number of historical records matching the query
* **pages** - Total number of pages available
* **results** - Array of historical entity records matching the search criteria

## Advanced History Search

This API endpoint allows you to perform complex searches for historical records of threat intelligence entities using advanced filters and aggregations.

**Endpoint:** https://intelligence.threatwinds.com/api/search/v1/entities/history/advanced

### Parameters

* **Authorization** header _string_ (optional)  
  This authorization header can be obtained from an active session of the account.

* **api-key** header _string_ (optional)  
  Your API key.

* **api-secret** header _string_ (optional)  
  Your API secret.

> **Note**: The `user-id` and `groups` headers are added automatically by the API gateway when required and should not be provided by the client.

* **limit** query _integer_ (optional)  
  Maximum number of results to return. Default is 10.

* **page** query _integer_ (optional)  
  Page number for pagination. Default is 1.

* **sort** query _string_ (optional)  
  Field to sort results by.

* **order** query _string_ (optional)  
  Sort order, either "asc" (ascending) or "desc" (descending).

* **AdvancedSearch** body _object_ (required)  
  The advanced search query parameters.

> Note: You must use either the Authorization header OR the API key and secret combination.

### Request

To perform an advanced search for entity history records, use a **POST** request with a JSON body containing your search parameters:

```bash
curl -X 'POST' \
  'https://intelligence.threatwinds.com/api/search/v1/entities/history/advanced?limit=10&page=1' \
  -H 'accept: application/json' \
  -H 'Authorization: Bearer fq6JoEFTsxiXAl1cVdPDnK4emIQCwaUBfq9JoEFTsxhXAl1cVxPDnK4emIQCwaUB' \
  -H 'Content-Type: application/json' \
  -d '{
  "query": {
    "must": [
      {
        "term": {
          "type": {
            "value": "ip"
          }
        }
      },
      {
        "match": {
          "attributes.value": {
            "query": "8.8.8.8"
          }
        }
      }
    ],
    "filter": [
      {
        "range": {
          "@timestamp": {
            "gte": "2023-01-01T00:00:00Z",
            "lte": "2023-06-30T23:59:59Z"
          }
        }
      }
    ]
  },
  "aggs": {
    "reputation_over_time": {
      "date_histogram": {
        "field": "@timestamp",
        "interval": "month"
      },
      "aggs": {
        "avg_reputation": {
          "avg": {
            "field": "reputation"
          }
        }
      }
    }
  },
  "source": {
    "includes": ["id", "type", "attributes", "reputation", "accuracy", "@timestamp"],
    "excludes": []
  }
}'
```

The request body parameters include:

* **query** _object_ (required) - The query object using OpenSearch-like query DSL
  * **must** - Conditions that must match (AND)
  * **should** - Conditions that should match (OR)
  * **must_not** - Conditions that must not match (NOT)
  * **filter** - Conditions that must match but don't affect the score
  * **minimum_should_match** - Minimum number of should clauses that must match
* **aggs** _object_ (optional) - Aggregations to perform on the data
* **source** _object_ (optional) - Fields to include or exclude in the response

### Response

A successful response will return a JSON object containing the historical entity records and aggregations:

```json
{
  "items": 3,
  "pages": 1,
  "results": [
    {
      "id": "5f35d2c4-5633-4b16-bbf0-5ca22ef8ea2e",
      "entityID": "ip-a1b2c3d4e5f6g7h8i9j0k1l2m3n4o5p6q7r8s9t0u1v2w3x4y5z6a7b8c9d0e1f2",
      "type": "ip",
      "@timestamp": "2023-06-15T14:30:00Z",
      "attributes": {
        "value": "8.8.8.8",
        "label": "Google Public DNS"
      },
      "reputation": 3,
      "accuracy": 3
    },
    {
      "id": "5f35d2c4-5633-4b16-bbf0-5ca22ef8ea2e",
      "entityID": "ip-a1b2c3d4e5f6g7h8i9j0k1l2m3n4o5p6q7r8s9t0u1v2w3x4y5z6a7b8c9d0e1f2",
      "type": "ip",
      "@timestamp": "2023-05-20T10:15:00Z",
      "attributes": {
        "value": "8.8.8.8",
        "label": "Google Public DNS"
      },
      "reputation": 2,
      "accuracy": 2
    },
    {
      "id": "5f35d2c4-5633-4b16-bbf0-5ca22ef8ea2e",
      "entityID": "ip-a1b2c3d4e5f6g7h8i9j0k1l2m3n4o5p6q7r8s9t0u1v2w3x4y5z6a7b8c9d0e1f2",
      "type": "ip",
      "@timestamp": "2023-04-10T08:45:00Z",
      "attributes": {
        "value": "8.8.8.8",
        "label": "Google DNS"
      },
      "reputation": 1,
      "accuracy": 1
    }
  ],
  "aggregations": {
    "reputation_over_time": {
      "buckets": [
        {
          "key_as_string": "2023-04-01T00:00:00.000Z",
          "key": 1680307200000,
          "doc_count": 1,
          "avg_reputation": {
            "value": 1
          }
        },
        {
          "key_as_string": "2023-05-01T00:00:00.000Z",
          "key": 1682899200000,
          "doc_count": 1,
          "avg_reputation": {
            "value": 2
          }
        },
        {
          "key_as_string": "2023-06-01T00:00:00.000Z",
          "key": 1685577600000,
          "doc_count": 1,
          "avg_reputation": {
            "value": 3
          }
        }
      ]
    }
  }
}
```

The response includes:

* **items** - Total number of historical records matching the query
* **pages** - Total number of pages available
* **results** - Array of historical entity records matching the search criteria
* **aggregations** - Results of any aggregations requested in the query

## Error Codes

* **204** - No content (no results found)
* **400** - Bad request
* **401** - Unauthorized
* **403** - Forbidden
* **500** - Internal server error
