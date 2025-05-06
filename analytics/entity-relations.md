---
layout: default
title: Entity Relations
parent: Analytics API
nav_order: 2
permalink: /analytics/entity-relations
---

# Entity Relations

This API endpoint provides relationship information about a specific threat intelligence entity in a graph format, showing how the entity is connected to other entities in the ThreatWinds platform.

**Endpoint:** https://intelligence.threatwinds.com/api/analytics/v1/entity/{id}/relations

## Parameters

* **Authorization** header _string_ (optional)  
  This authorization header can be obtained from an active session of the account.

* **api-key** header _string_ (optional)  
  Your API key.

* **api-secret** header _string_ (optional)  
  Your API secret.

> **Note**: The `user-id` and `groups` headers are added automatically by the API gateway when required and should not be provided by the client.

* **id** path _string_ (required)  
  The unique identifier of the entity you want to retrieve relations for.

* **limit** query _integer_ (optional)  
  Maximum number of related entities to return.

* **levels** query _integer_ (optional)  
  Number of relationship levels to include in the graph.

> Note: You must use either the Authorization header OR the API key and secret combination.

## Request

To get relations for a specific entity, use a **GET** request, for example:

```bash
curl -X 'GET' \
  'https://intelligence.threatwinds.com/api/analytics/v1/entity/ip-a1b2c3d4e5f6g7h8i9j0k1l2m3n4o5p6q7r8s9t0u1v2w3x4y5z6a7b8c9d0e1f2/relations?limit=10&levels=2' \
  -H 'accept: application/json' \
  -H 'Authorization: Bearer fq6JoEFTsxiXAl1cVdPDnK4emIQCwaUBfq9JoEFTsxhXAl1cVxPDnK4emIQCwaUB'
```

Or using API key and secret:

```bash
curl -X 'GET' \
  'https://intelligence.threatwinds.com/api/analytics/v1/entity/ip-a1b2c3d4e5f6g7h8i9j0k1l2m3n4o5p6q7r8s9t0u1v2w3x4y5z6a7b8c9d0e1f2/relations?limit=10&levels=2' \
  -H 'accept: application/json' \
  -H 'api-key: your-api-key' \
  -H 'api-secret: your-api-secret'
```

## Response

A successful response will return a JSON object containing graph data with nodes and relations:

* **nodes** - Array of entity objects representing the nodes in the graph. Note that the `value` field in the `attributes` object only contains strings, numbers, or booleans, never maps, arrays, or other complex structures.
* **relations** - Array of relationship objects connecting the nodes

Example response:

```json
{
  "nodes": [
    {
      "id": "ip-a1b2c3d4e5f6g7h8i9j0k1l2m3n4o5p6q7r8s9t0u1v2w3x4y5z6a7b8c9d0e1f2",
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
        "ip": "8.8.8.8"
      }
    },
    {
      "id": "domain-a1b2c3d4e5f6g7h8i9j0k1l2m3n4o5p6q7r8s9t0u1v2w3x4y5z6a7b8c9d0e1f2",
      "type": "domain",
      "@timestamp": "2023-06-10T12:00:00Z",
      "reputation": 3,
      "bestReputation": 3,
      "worstReputation": 3,
      "accuracy": 3,
      "lastSeen": "2023-06-10T12:00:00Z",
      "tags": ["dns", "google"],
      "visibleBy": ["public"],
      "attributes": {
        "domain": "dns.google"
      }
    }
  ],
  "relations": [
    {
      "source": "ip-a1b2c3d4e5f6g7h8i9j0k1l2m3n4o5p6q7r8s9t0u1v2w3x4y5z6a7b8c9d0e1f2",
      "target": "domain-a1b2c3d4e5f6g7h8i9j0k1l2m3n4o5p6q7r8s9t0u1v2w3x4y5z6a7b8c9d0e1f2"
    }
  ]
}
```

## Error Codes

* **400** - Bad request
* **401** - Unauthorized
* **403** - Forbidden
* **500** - Internal server error
