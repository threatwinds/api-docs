---
layout: default
title: Advanced Search
parent: Search API
nav_order: 2
permalink: /search/advanced-search
---

# Advanced Search

This API endpoint allows you to perform complex searches for threat intelligence entities using advanced filters and aggregations. It provides more flexibility and power than the simple search endpoint, allowing you to construct sophisticated queries using OpenSearch-like query syntax.

**Endpoint:** https://intelligence.threatwinds.com/api/search/v1/entities/advanced

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

* **AdvancedSearch** body _object_ (required)  
  The advanced search query parameters.

> Note: You must use either the Authorization header OR the API key and secret combination.

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

* **query** _object_ (required) - The query object using OpenSearch query DSL
  * **must** - Conditions that must match (AND)
  * **should** - Conditions that should match (OR)
  * **must_not** - Conditions that must not match (NOT)
  * **filter** - Conditions that must match but don't affect the score
  * **minimum_should_match** - Minimum number of should clauses that must match

> Note: In the example above, we're using a `term` query to filter by the `type` field with a value of `"ip"`. For a comprehensive list of all possible entity types, see the [Entity Types](/search/entity-types) page.
* **aggs** _object_ (optional) - Aggregations to perform on the data
* **source** _object_ (optional) - Fields to include or exclude in the response
* **collapse** _object_ (optional) - Field to collapse results on
* **script_fields** _object_ (optional) - Computed fields
* **search_after** _array_ (optional) - Pagination using search_after

## OpenSearch Query DSL

The advanced search endpoint uses OpenSearch Query DSL (Domain Specific Language), which provides a rich and flexible way to define queries. The query is structured as a JSON object with specific fields and operators.

### Query Types

The following query types can be used within the `must`, `should`, `must_not`, and `filter` clauses:

* **term** - Exact match on a field
* **terms** - Match if field has any of the specified values
* **ids** - Match documents with the specified IDs
* **range** - Match if field value is within a specified range
* **exists** - Match if field exists
* **prefix** - Match if field value starts with a specified prefix
* **fuzzy** - Match similar terms based on Levenshtein distance
* **wildcard** - Match using wildcards
* **regexp** - Match using regular expressions
* **match** - Full-text search on a field
* **multi_match** - Full-text search on multiple fields
* **match_bool_prefix** - Match prefix of words
* **match_phrase** - Match exact phrase
* **match_phrase_prefix** - Match phrase prefix
* **query_string** - Query with a syntax (AND, OR, NOT)
* **simple_query_string** - Simplified query string syntax

### Aggregation Types

The `aggs` field allows you to perform various aggregations on your data:

* **terms** - Group by field values
* **range** - Group by ranges of values
* **date_histogram** - Group by date intervals
* **histogram** - Group by numeric intervals
* **avg** - Calculate average of a field
* **sum** - Calculate sum of a field
* **min** - Find minimum value of a field
* **max** - Find maximum value of a field
* **cardinality** - Count unique values
* **stats** - Calculate statistics (count, min, max, avg, sum)
* **extended_stats** - Calculate extended statistics
* **percentiles** - Calculate percentiles
* **top_hits** - Return top matching hits

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

Search for IP entities with high reputation that are not tagged as malicious:

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

Search for entities with a field value that is similar to a specific value (allowing for typos):

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

Group by entity type and then calculate average reputation for each type:

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
        "value": "8.8.4.4",
        "text": "Google Public DNS"
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

* **items** - Total number of items matching the query
* **pages** - Total number of pages available
* **results** - Array of entities matching the search criteria
* **aggregations** - Results of any aggregations requested in the query

## Error Codes

* **204** - No content (no results found)
* **400** - Bad request
* **401** - Unauthorized
* **403** - Forbidden
* **500** - Internal server error

> For more detailed information about responses and error codes, please refer to the [Restful API definition](https://intelligence.threatwinds.com/api/search/v1/swagger/index.html).
