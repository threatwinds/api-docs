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

#### Headers

| Parameter         | Type   | Required  | Description                                  |
|-------------------|--------|-----------|----------------------------------------------|
| **Authorization** | string | Optional* | Bearer token obtained from an active session |
| **api-key**       | string | Optional* | Your API key                                 |
| **api-secret**    | string | Optional* | Your API secret                              |

> **Note:** The `user-id` and `groups` headers are added automatically by the API gateway when required and shouldn't be provided by the client.

#### Query Parameters

| Parameter | Type    | Required | Description                                                 |
|-----------|---------|----------|-------------------------------------------------------------|
| **limit** | integer | No       | Maximum number of results to return. Default is 10          |
| **page**  | integer | No       | Page number for pagination. Default is 1                    |
| **sort**  | string  | No       | Field to sort results by                                    |
| **order** | string  | No       | Sort order, either "asc" (ascending) or "desc" (descending) |

#### Request Body

| Parameter        | Type   | Required | Description                 |
|------------------|--------|----------|-----------------------------|
| **SimpleSearch** | object | Yes      | The search query parameters |

> **Note:** You must use either the Authorization header OR the API key and secret combination.

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

| Parameter      | Type    | Required | Description                                                                                                                                                         |
|----------------|---------|----------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **query**      | string  | Yes      | The search term (e.g., an IP address, domain, hash, etc.). For a comprehensive list of all possible entity types, see the [Entity Types](/search/entity-types) page |
| **accuracy**   | integer | No       | Minimum accuracy level (0 to 3)                                                                                                                                     |
| **reputation** | integer | No       | Minimum reputation score (-3 to 3)                                                                                                                                  |
| **source**     | object  | No       | Fields to include or exclude in the response                                                                                                                        |

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

| Field       | Description                                                     |
|-------------|-----------------------------------------------------------------|
| **items**   | Total number of historical records matching the query           |
| **pages**   | Total number of pages available                                 |
| **results** | Array of historical entity records matching the search criteria |

## Advanced History Search

This API endpoint allows you to perform complex searches for historical records of threat intelligence entities using advanced filters and aggregations.

**Endpoint:** https://intelligence.threatwinds.com/api/search/v1/entities/history/advanced

### Parameters

#### Headers

| Parameter         | Type   | Required  | Description                                  |
|-------------------|--------|-----------|----------------------------------------------|
| **Authorization** | string | Optional* | Bearer token obtained from an active session |
| **api-key**       | string | Optional* | Your API key                                 |
| **api-secret**    | string | Optional* | Your API secret                              |

> **Note:** the `user-id` and `groups` headers are added automatically by the API gateway when required and shouldn't be provided by the client.

#### Query Parameters

| Parameter | Type    | Required | Description                                                 |
|-----------|---------|----------|-------------------------------------------------------------|
| **limit** | integer | No       | Maximum number of results to return. Default is 10          |
| **page**  | integer | No       | Page number for pagination. Default is 1                    |
| **sort**  | string  | No       | Field to sort results by                                    |
| **order** | string  | No       | Sort order, either "asc" (ascending) or "desc" (descending) |

#### Request Body

| Parameter          | Type   | Required | Description                          |
|--------------------|--------|----------|--------------------------------------|
| **AdvancedSearch** | object | Yes      | The advanced search query parameters |

> **Note:** you must use either the Authorization header OR the API key and secret combination.

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

| Parameter                      | Type    | Required | Description                                           |
|--------------------------------|---------|----------|-------------------------------------------------------|
| **query**                      | object  | Yes      | The query object using OpenSearch-like query DSL      |
| **query.must**                 | array   | No       | Conditions that must match (AND logic)                |
| **query.should**               | array   | No       | Conditions that should match (OR logic)               |
| **query.must_not**             | array   | No       | Conditions that must not match (NOT logic)            |
| **query.filter**               | array   | No       | Conditions that must match but don't affect the score |
| **query.minimum_should_match** | integer | No       | Minimum number of should clauses that must match      |
| **aggs**                       | object  | No       | Aggregations to perform on the data                   |
| **source**                     | object  | No       | Fields to include or exclude in the response          |

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

| Field            | Description                                                     |
|------------------|-----------------------------------------------------------------|
| **items**        | Total number of historical records matching the query           |
| **pages**        | Total number of pages available                                 |
| **results**      | Array of historical entity records matching the search criteria |
| **aggregations** | Results of any aggregations requested in the query              |

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
