---
layout: default
title: Entity Details
parent: Analytics API
nav_order: 1
permalink: /analytics/entity-details
---

# Entity Details

This API endpoint provides detailed information about a specific threat intelligence entity, including its attributes, metadata, geolocations, and latest associations.

**Endpoint:** https://intelligence.threatwinds.com/api/analytics/v1/entity/{id}/details

## Parameters

* **Authorization** header _string_ (optional)  
  This authorization header can be obtained from an active session of the account.

* **api-key** header _string_ (optional)  
  Your API key.

* **api-secret** header _string_ (optional)  
  Your API secret.

> **Note**: the `user-id` and `groups` headers are added automatically by the API gateway when required and shouldn't be provided by the client.

* **id** path _string_ (required)  
  The unique identifier of the entity you want to retrieve details for.

> Note: you must use either the Authorization header OR the API key and secret combination.

## Request

To get details for a specific entity, use a **GET** request, for example:

```bash
curl -X 'GET' \
  'https://intelligence.threatwinds.com/api/analytics/v1/entity/ip-a1b2c3d4e5f6g7h8i9j0k1l2m3n4o5p6q7r8s9t0u1v2w3x4y5z6a7b8c9d0e1f2/details' \
  -H 'accept: application/json' \
  -H 'Authorization: Bearer fq6JoEFTsxiXAl1cVdPDnK4emIQCwaUBfq9JoEFTsxhXAl1cVxPDnK4emIQCwaUB'
```

Or using API key and secret:

```bash
curl -X 'GET' \
  'https://intelligence.threatwinds.com/api/analytics/v1/entity/ip-a1b2c3d4e5f6g7h8i9j0k1l2m3n4o5p6q7r8s9t0u1v2w3x4y5z6a7b8c9d0e1f2/details' \
  -H 'accept: application/json' \
  -H 'api-key: your-api-key' \
  -H 'api-secret: your-api-secret'
```

## Response

A successful response returns a JSON object containing detailed information about the entity, including:

* **attributes:** core attributes of the entity (type, value, reputation, accuracy, etc.). Note that the `value` field only contains strings, numbers, or booleans, never maps, arrays, or other complex structures.
* **metadata:** additional metadata associated with the entity
* **geolocations:** geolocation information if applicable (for IP addresses, domains, etc.)
* **latest_associations:** recent entities associated with this entity
* **extended_metadata:** additional detailed metadata

Example response:

```json
{
  "attributes": {
    "id": "ip-a1b2c3d4e5f6g7h8i9j0k1l2m3n4o5p6q7r8s9t0u1v2w3x4y5z6a7b8c9d0e1f2",
    "type": "ip",
    "value": "8.8.8.8",
    "label": "Google Public DNS",
    "description": "Google's public DNS server",
    "reputation": "benign",
    "reputation_score": 3,
    "best_reputation": "benign",
    "best_reputation_score": 3,
    "worst_reputation": "benign",
    "worst_reputation_score": 3,
    "accuracy": "high",
    "accuracy_score": 3,
    "first_seen": "2021-01-01T00:00:00Z",
    "last_seen": "2023-06-15T14:30:00Z",
    "tags": ["dns", "google", "public"]
  },
  "metadata": {
    "asn": 15169,
    "aso": "Google LLC",
    "country": "United States"
  },
  "geolocations": [
    {
      "city": "Mountain View",
      "country": "United States",
      "latitude": 37.4056,
      "longitude": -122.0775,
      "accuracy_radius": 1000,
      "asn": 15169,
      "aso": "Google LLC",
      "object": "8.8.8.0/24"
    }
  ],
  "latest_associations": [
    {
      "id": "domain-a1b2c3d4e5f6g7h8i9j0k1l2m3n4o5p6q7r8s9t0u1v2w3x4y5z6a7b8c9d0e1f2",
      "type": "domain",
      "value": "dns.google",
      "reputation": "benign",
      "reputation_score": 3,
      "accuracy": "high",
      "accuracy_score": 3,
      "first_seen": "2021-02-15T00:00:00Z",
      "last_seen": "2023-06-10T12:00:00Z"
    }
  ],
  "extended_metadata": [
    {
      "id": "registrar-a1b2c3d4e5f6g7h8i9j0k1l2m3n4o5p6q7r8s9t0u1v2w3x4y5z6a7b8c9d0e1f2",
      "type": "registrar",
      "value": "Google LLC",
      "reputation": "benign",
      "reputation_score": 3,
      "accuracy": "high",
      "accuracy_score": 3,
      "first_seen": "2021-02-15T00:00:00Z",
      "last_seen": "2023-06-10T12:00:00Z"
    }
  ]
}
```

## Error Codes

* **204:** no content (entity not found)
* **400:** bad request
* **401:** unauthorized
* **403:** forbidden
* **500:** internal server error
