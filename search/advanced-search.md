---
layout: default
title: Advanced Search
parent: Search API
nav_order: 2
permalink: /search/advanced-search
---

# Advanced Search

This API endpoint allows you to perform complex searches for threat intelligence entities using advanced filters and aggregations. It provides more flexibility and power than the [simple search](/search/simple-search) endpoint, allowing you to construct sophisticated queries using OpenSearch-like query syntax. If you need a more straightforward search interface, consider using the [Simple Search](/search/simple-search) instead.

**Endpoint:** https://intelligence.threatwinds.com/api/search/v1/entities/advanced

## Parameters

### Headers

| Header            | Type   | Required  | Description                                  |
|-------------------|--------|-----------|----------------------------------------------|
| **Authorization** | string | Optional* | Bearer token obtained from an active session |
| **api-key**       | string | Optional* | Your API key                                 |
| **api-secret**    | string | Optional* | Your API secret                              |

> **Note:** The `user-id` and `groups` headers are added automatically by the API gateway when required and should not be provided by the client.

### Query Parameters

| Parameter | Type    | Required | Description                                                 |
|-----------|---------|----------|-------------------------------------------------------------|
| **limit** | integer | No       | Maximum number of results to return. Default is 10          |
| **page**  | integer | No       | Page number for pagination. Default is 1                    |
| **sort**  | string  | No       | Field to sort results by                                    |
| **order** | string  | No       | Sort order, either "asc" (ascending) or "desc" (descending) |

### Request Body

| Parameter          | Type   | Required | Description                          |
|--------------------|--------|----------|--------------------------------------|
| **AdvancedSearch** | object | Yes      | The advanced search query parameters |

> **Note:** You must use either the Authorization header OR the API key and secret combination. For more details on authentication, see the [Authentication](/auth) documentation.

## Request

To perform an advanced search, use a **POST** request with a JSON body containing your search parameters:

```bash
curl -X 'POST' \
  'https://intelligence.threatwinds.com/api/search/v1/entities/advanced?limit=10&page=1' \
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
        "range": {
          "reputation": {
            "gte": 1
          }
        }
      }
    ],
    "should": [
      {
        "match": {
          "attributes.ip": {
            "query": "8.8.8.8"
          }
        }
      }
    ],
    "must_not": [
      {
        "term": {
          "tags": {
            "value": "malicious"
          }
        }
      }
    ]
  },
  "aggs": {
    "types": {
      "terms": {
        "field": "type",
        "size": 10
      }
    },
    "reputation_ranges": {
      "range": {
        "field": "reputation",
        "ranges": [
          { "from": -3, "to": 0 },
          { "from": 0, "to": 1 },
          { "from": 1, "to": 3 }
        ]
      }
    }
  },
  "source": {
    "includes": ["id", "type", "attributes", "reputation", "accuracy", "tags"],
    "excludes": []
  }
}'
```

The request body parameters include:

| Parameter         | Type   | Required | Description                                  |
|-------------------|--------|----------|----------------------------------------------|
| **query**         | object | Yes      | The query object using OpenSearch query DSL  |
| **aggs**          | object | No       | Aggregations to perform on the data          |
| **source**        | object | No       | Fields to include or exclude in the response |
| **collapse**      | object | No       | Field to collapse results on                 |
| **script_fields** | object | No       | Computed fields                              |
| **search_after**  | array  | No       | Pagination using search_after                |

The **query** object can contain the following clauses:

| Clause                   | Description                                           | Logic                       |
|--------------------------|-------------------------------------------------------|-----------------------------|
| **must**                 | Conditions that must match                            | AND                         |
| **should**               | Conditions that should match                          | OR                          |
| **must_not**             | Conditions that must not match                        | NOT                         |
| **filter**               | Conditions that must match but don't affect the score | AND (without scoring)       |
| **minimum_should_match** | Minimum number of should clauses that must match      | Threshold for OR conditions |

> **Note:** In the example preceding, it's in use a `term` query to filter by the `type` field with a value of `"ip"`. For a comprehensive list of all possible entity types, see the [Entity Types](/search/entity-types) page.

## OpenSearch Query DSL

The advanced search endpoint uses OpenSearch Query DSL (Domain Specific Language), which provides a rich and flexible way to define queries. The query is structured as a JSON object with specific fields and operators.

> **Note:** To effectively use the advanced search capabilities, it's important to understand the entity structure and field types. For detailed information about entity mapping, including field types, searchability, and how to use `.keyword` fields, see the [Entity Mapping](/search/entity-mapping) documentation.

### Query Types

The following query types can be used within the `must`, `should`, `must_not`, and `filter` clauses:

| Query Type              | Description                                             | Use Case                                                      |
|-------------------------|---------------------------------------------------------|---------------------------------------------------------------|
| **term**                | Exact match on a field                                  | When you need exact matching on a specific field value        |
| **terms**               | Match if the field has any of the specified values      | When you want to match multiple possible values               |
| **ids**                 | Match documents with the specified IDs                  | When you want to retrieve specific entities by ID             |
| **range**               | Match if the field value is within a specified range    | For numeric or date ranges (e.g., reputation scores)          |
| **exists**              | Match if field exists                                   | When you need to find entities that have a specific attribute |
| **prefix**              | Match if the field value starts with a specified prefix | For partial matching at the beginning of a value              |
| **fuzzy**               | Match similar terms based on Levenshtein distance       | When you need to account for typos or spelling variations     |
| **wildcard**            | Match using wildcards                                   | When you need pattern matching with * and ? wildcards         |
| **regexp**              | Match using regular expressions                         | For complex pattern matching                                  |
| **match**               | Full-text search on a field                             | For general text searching with relevance scoring             |
| **multi_match**         | Full-text search on multiple fields                     | When you want to search the same term across different fields |
| **match_bool_prefix**   | Match prefix of words                                   | For search-as-you-type functionality                          |
| **match_phrase**        | Match exact phrase                                      | When word order matters in your search                        |
| **match_phrase_prefix** | Match phrase prefix                                     | For phrase matching with the last term as a prefix            |
| **query_string**        | Query with a syntax (AND, OR, NOT)                      | For advanced users who need complex query syntax              |
| **simple_query_string** | Simplified query string syntax                          | For user-facing search boxes with simple syntax               |

### Aggregation Types

The `aggs` field allows you to perform various aggregations on your data:

| Aggregation Type   | Description                                      | Use Case                                                         |
|--------------------|--------------------------------------------------|------------------------------------------------------------------|
| **terms**          | Group by field values                            | When you want to see distribution of values in a field           |
| **range**          | Group by ranges of values                        | For grouping numeric data into buckets (e.g., reputation ranges) |
| **date_histogram** | Group by date intervals                          | For time-based analysis (e.g., entities per month)               |
| **histogram**      | Group by numeric intervals                       | For numeric data distribution analysis                           |
| **avg**            | Calculate average of a field                     | When you need the mean value of a numeric field                  |
| **sum**            | Calculate sum of a field                         | When you need the total of a numeric field                       |
| **min**            | Find minimum value of a field                    | When you need the smallest value in a dataset                    |
| **max**            | Find maximum value of a field                    | When you need the largest value in a dataset                     |
| **cardinality**    | Count unique values                              | When you need to know how many distinct values exist             |
| **stats**          | Calculate statistics (count, min, max, avg, sum) | For basic statistical analysis in one request                    |
| **extended_stats** | Calculate extended statistics                    | For more detailed statistical analysis                           |
| **percentiles**    | Calculate percentiles                            | For understanding data distribution                              |
| **top_hits**       | Return top matching hits                         | When you want to see examples from each bucket                   |

### Source Filtering

The `source` field allows you to include or exclude specific fields from the response:

```json
{
  "source": {
    "includes": ["id", "type", "attributes.ip"],
    "excludes": ["attributes.country"]
  }
}
```

### Collapse

The `collapse` field allows you to collapse search results based on field values:

```json
{
  "collapse": {
    "field": "type"
  }
}
```

## Sample Requests

Here are some sample requests that demonstrate various query types and aggregation types:

### Term Query

Search for entities of type "ip":

```json
{
  "query": {
    "must": [
      {
        "term": {
          "type": {
            "value": "ip"
          }
        }
      }
    ]
  }
}
```

### Terms Query

Search for entities with specific tags:

```json
{
  "query": {
    "must": [
      {
        "terms": {
          "tags": ["malicious", "suspicious"]
        }
      }
    ]
  }
}
```

### Range Query

Search for entities with a reputation between 1 and 3:

```json
{
  "query": {
    "must": [
      {
        "range": {
          "reputation": {
            "gte": 1,
            "lte": 3
          }
        }
      }
    ]
  }
}
```

### Match Query

Full-text search for entities with "google" in their attributes:

```json
{
  "query": {
    "must": [
      {
        "match": {
          "attributes.ip": {
            "query": "8.8.8.8"
          }
        }
      }
    ]
  }
}
```

### Query with Multiple Clauses

Search for IP entities with a high reputation that aren't tagged as malicious:

```json
{
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
        "range": {
          "reputation": {
            "gte": 2
          }
        }
      }
    ],
    "must_not": [
      {
        "term": {
          "tags": {
            "value": "mail-server"
          }
        }
      }
    ]
  }
}
```

### Exists Query

Search for entities that have a specific field:

```json
{
  "query": {
    "must": [
      {
        "exists": {
          "field": "attributes.country_code"
        }
      }
    ]
  }
}
```

### Prefix Query

Search for entities with a field value that starts with a specific prefix:

```json
{
  "query": {
    "must": [
      {
        "prefix": {
          "attributes.aso": "Google"
        }
      }
    ]
  }
}
```

### Fuzzy Query

Search for entities with a field value that's similar to a specific value (allowing for typos):

```json
{
  "query": {
    "must": [
      {
        "fuzzy": {
          "attributes.aso": {
            "value": "googl",
            "fuzziness": 2
          }
        }
      }
    ]
  }
}
```

### Wildcard Query

Search for entities with a field value that matches a wildcard pattern:

```json
{
  "query": {
    "must": [
      {
        "wildcard": {
          "attributes.text": {
            "value": "Google*DNS"
          }
        }
      }
    ]
  }
}
```

### Regexp Query

Search for entities with a field value that matches a regular expression:

```json
{
  "query": {
    "must": [
      {
        "regexp": {
          "attributes.ip": "8\\.8\\.[0-9]\\.[0-9]"
        }
      }
    ]
  }
}
```

### Multi Match Query

Search for a value across multiple fields:

```json
{
  "query": {
    "must": [
      {
        "multi_match": {
          "query": "google dns",
          "fields": ["attributes.text", "attributes.description"]
        }
      }
    ]
  }
}
```

### Match Bool Prefix Query

Search for entities with a field value that starts with specific words:

```json
{
  "query": {
    "must": [
      {
        "match_bool_prefix": {
          "attributes.description": {
            "query": "public dns serv"
          }
        }
      }
    ]
  }
}
```

### Match Phrase Query

Search for entities with a field value that contains an exact phrase:

```json
{
  "query": {
    "must": [
      {
        "match_phrase": {
          "attributes.description": {
            "query": "public DNS server"
          }
        }
      }
    ]
  }
}
```

### Match Phrase Prefix Query

Search for entities with a field value that contains a phrase prefix:

```json
{
  "query": {
    "must": [
      {
        "match_phrase_prefix": {
          "attributes.description": {
            "query": "public DNS serv"
          }
        }
      }
    ]
  }
}
```

### Query String Query

Search using a query string syntax:

```json
{
  "query": {
    "must": [
      {
        "query_string": {
          "query": "attributes.label:\"Google DNS\" AND attributes.country_code:US",
          "default_operator": "AND"
        }
      }
    ]
  }
}
```

### Simple Query String Query

Search using a simplified query string syntax:

```json
{
  "query": {
    "must": [
      {
        "simple_query_string": {
          "query": "google dns",
          "fields": ["attributes.label", "attributes.description"],
          "default_operator": "AND"
        }
      }
    ]
  }
}
```

### Terms Aggregation

Group results by entity type:

```json
{
  "query": {
    "must": [
      {
        "range": {
          "reputation": {
            "gte": 0
          }
        }
      }
    ]
  },
  "aggs": {
    "entity_types": {
      "terms": {
        "field": "type",
        "size": 10
      }
    }
  }
}
```

### Date Histogram Aggregation

Group entities by the month they were last seen:

```json
{
  "query": {
    "must": [
      {
        "term": {
          "type": {
            "value": "ip"
          }
        }
      }
    ]
  },
  "aggs": {
    "entities_over_time": {
      "date_histogram": {
        "field": "lastSeen",
        "interval": "month"
      }
    }
  }
}
```

### Nested Aggregations

Group by entity type and then calculate an average reputation for each type:

```json
{
  "query": {
    "must": [
      {
        "range": {
          "reputation": {
            "gte": 3
          }
        }
      }
    ]
  },
  "aggs": {
    "entity_types": {
      "terms": {
        "field": "type",
        "size": 10
      },
      "aggs": {
        "avg_reputation": {
          "avg": {
            "field": "reputation"
          }
        }
      }
    }
  }
}
```

## Response

A successful response will return a JSON object containing the search results and aggregations:

```json
{
  "items": 2,
  "pages": 1,
  "results": [
    {
      "id": "5f35d2c4-5633-4b16-bbf0-5ca22ef8ea2e",
      "type": "ip",
      "attributes": {
        "ip": "8.8.8.8"
      },
      "reputation": 3,
      "accuracy": 3,
      "tags": ["dns", "google", "public"]
    },
    {
      "id": "6a2b4c5d-6e7f-8g9h-0i1j-2k3l4m5n6o7p",
      "type": "ip",
      "attributes": {
        "ip": "8.8.4.4"
      },
      "reputation": 3,
      "accuracy": 3,
      "tags": ["dns", "google", "public"]
    }
  ],
  "aggregations": {
    "types": {
      "buckets": [
        {
          "key": "ip",
          "doc_count": 2
        }
      ]
    },
    "reputation_ranges": {
      "buckets": [
        {
          "key": "-3.0-0.0",
          "from": -3,
          "to": 0,
          "doc_count": 0
        },
        {
          "key": "0.0-1.0",
          "from": 0,
          "to": 1,
          "doc_count": 0
        },
        {
          "key": "1.0-3.0",
          "from": 1,
          "to": 3,
          "doc_count": 2
        }
      ]
    }
  }
}
```

The response includes:

| Field            | Type    | Description                                        |
|------------------|---------|----------------------------------------------------|
| **items**        | integer | Total number of items matching the query           |
| **pages**        | integer | Total number of pages available                    |
| **results**      | array   | Array of entities matching the search criteria     |
| **aggregations** | object  | Results of any aggregations requested in the query |

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
