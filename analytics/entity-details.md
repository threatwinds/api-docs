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

* **user-id** header _string_ (optional)  
  User ID for filtering results.

* **groups** header _string_ (optional)  
  Groups for filtering results.

* **id** path _string_ (required)  
  The unique identifier of the entity you want to retrieve details for.

> Note: You must use either the Authorization header OR the API key and secret combination.

## Request

To get details for a specific entity, use a **GET** request, for example:

```bash
curl -X 'GET' \
  'https://intelligence.threatwinds.com/api/analytics/v1/entity/5f35d2c4-5633-4b16-bbf0-5ca22ef8ea2e/details' \
  -H 'accept: application/json' \
  -H 'Authorization: Bearer fq6JoEFTsxiXAl1cVdPDnK4emIQCwaUBfq9JoEFTsxhXAl1cVxPDnK4emIQCwaUB'
```

Or using API key and secret:

```bash
curl -X 'GET' \
  'https://intelligence.threatwinds.com/api/analytics/v1/entity/5f35d2c4-5633-4b16-bbf0-5ca22ef8ea2e/details' \
  -H 'accept: application/json' \
  -H 'api-key: your-api-key' \
  -H 'api-secret: your-api-secret'
```

## Response

A successful response will return a JSON object containing detailed information about the entity, including:

* **attributes** - Core attributes of the entity (type, value, reputation, accuracy, etc.). Note that the `value` field only contains strings, numbers, or booleans, never maps, arrays, or other complex structures.
* **metadata** - Additional metadata associated with the entity
* **geolocations** - Geolocation information if applicable (for IP addresses, domains, etc.)
* **latest_associations** - Recent entities associated with this entity
* **extended_metadata** - Additional detailed metadata

Example response:

```json
{
  "attributes": {
    "id": "5f35d2c4-5633-4b16-bbf0-5ca22ef8ea2e",
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
    "organization": "Google LLC",
    "country": "US"
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
      "id": "7a1b3c2d-4e5f-6g7h-8i9j-0k1l2m3n4o5p",
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
      "id": "7a1b3c2d-4e5f-6g7h-8i9j-0k1l2m3n4o5p",
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

* **204** - No content (entity not found)
* **400** - Bad request
* **401** - Unauthorized
* **403** - Forbidden
* **500** - Internal server error
