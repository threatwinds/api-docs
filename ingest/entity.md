---
layout: default
title: Entity
parent: Ingest API
nav_order: 1
---

# Entity Endpoints

The Entity endpoints allow you to submit and manage threat intelligence entities in the ThreatWinds platform.

## Insert Entity

This endpoint allows you to insert a new entity with optional associations.

### Endpoint

```
POST /api/ingest/v1/entity
```

### Request Headers

| Header          | Description                                                        |
|-----------------|--------------------------------------------------------------------|
| `Authorization` | Bearer token for authentication (optional if using API key/secret) |
| `api-key`       | Your API key (optional if using Authorization header)              |
| `api-secret`    | Your API secret (optional if using Authorization header)           |

> **Note**: The `user-id` and `groups` headers are added automatically by the API gateway when required and should not be provided by the client.

### Required Roles

Access to this endpoint is controlled by role-based permissions defined in the gateway. Users must have at least one of the required roles assigned to their account to access this endpoint.

Required role: `reporter`

This endpoint requires the `reporter` role, which allows users to submit threat intelligence data to the platform.

### Request Body

The request body should be a JSON object with the following structure:

```json
{
  "type": "string",
  "attributes": {
    "additionalProp1": "string",
    "additionalProp2": "string",
    "additionalProp3": "string"
  },
  "associations": [
    {
      "mode": "string",
      "type": "string",
      "attributes": {
        "additionalProp1": "string",
        "additionalProp2": "string",
        "additionalProp3": "string"
      }
    }
  ],
  "reputation": 0,
  "correlate": ["string"],
  "tags": ["string"],
  "visibleBy": ["string"]
}
```

#### Parameters

| Parameter      | Type    | Description                                                                                 |
|----------------|---------|---------------------------------------------------------------------------------------------|
| `type`         | string  | The type of the entity (e.g., "ip", "domain", "url")                                        |
| `attributes`   | object  | Attributes for the entity. Must include a key matching the entity type                      |
| `associations` | array   | Optional associations with other entities                                                   |
| `reputation`   | integer | Reputation score for the entity (e.g., -1 for malicious, 0 for neutral, 1 for benign)       |
| `correlate`    | array   | Optional list of attribute names to create additional entities from                         |
| `tags`         | array   | Optional tags to categorize the entity                                                      |
| `visibleBy`    | array   | Optional list of groups that can see the entity (defaults to user's groups if not provided) |

> **Note**: the main attribute must exist, and its key is the same as the type of the entity. For example, an IP entity with the value "203.0.113.1" must have a field "attributes.ip" with the value "203.0.113.1" otherwise the request returns error code 400.

### Response

#### Success Response (202 Accepted)

```json
{
  "message": "acknowledged"
}
```

#### Error Responses

| Status Code | Description                          |
|-------------|--------------------------------------|
| 400         | Bad Request - Invalid input data     |
| 401         | Unauthorized - Authentication failed |
| 403         | Forbidden - Insufficient permissions |

### Example

```bash
curl -X POST "https://intelligence.threatwinds.com/api/ingest/v1/entity" \
  -H "Content-Type: application/json" \
  -H "api-key: your-api-key" \
  -H "api-secret: your-api-secret" \
  -d '{
    "type": "ip",
    "attributes": {
      "ip": "203.0.113.1",
      "text": "manual",
      "value": "high"
    },
    "reputation": -1,
    "tags": ["malicious", "scanner"],
    "visibleBy": ["group1", "group2"]
  }'
```

## Well-Known Entity

This endpoint allows you to insert a well-known (trusted) entity that will never be evaluated with negative reputation.

### Endpoint

```
POST /api/ingest/v1/well-known
```

### Request Headers

Same as the Insert Entity endpoint.

### Required Roles

Access to this endpoint is controlled by role-based permissions defined in the gateway. Users must have at least one of the required roles assigned to their account to access this endpoint.

Required role: `trusted`

This endpoint requires the `trusted` role, which allows access to trusted or verified endpoints.

### Request Body

```json
{
  "type": "string",
  "attributes": {
    "additionalProp1": "string",
    "additionalProp2": "string",
    "additionalProp3": "string"
  }
}
```

### Response

Same as the Insert Entity endpoint.

### Example

```bash
curl -X POST "https://intelligence.threatwinds.com/api/ingest/v1/well-known" \
  -H "Content-Type: application/json" \
  -H "api-key: your-api-key" \
  -H "api-secret: your-api-secret" \
  -d '{
    "type": "domain",
    "attributes": {
      "domain": "example.com",
      "text": "trusted_source",
      "value": "high"
    }
  }'
```

## Get Definitions

This endpoint returns entity definitions.

### Endpoint

```
GET /api/ingest/v1/definitions
```

### Request Headers

| Header          | Description                                                        |
|-----------------|--------------------------------------------------------------------|
| `Authorization` | Bearer token for authentication (optional if using API key/secret) |
| `api-key`       | Your API key (optional if using Authorization header)              |
| `api-secret`    | Your API secret (optional if using Authorization header)           |

> **Note**: The `user-id` and `groups` headers are added automatically by the API gateway when required and should not be provided by the client.

### Required Roles

Access to this endpoint is controlled by role-based permissions defined in the gateway. Users must have at least one of the required roles assigned to their account to access this endpoint.

Required role: `trusted`

This endpoint requires the `trusted` role, which allows access to trusted or verified endpoints.

### Response

#### Success Response (200 OK)

```json
[
  {
    "type": "ip",
    "description": "IP address",
    "attributes": ["text", "value", "country"]
  },
  {
    "type": "domain",
    "description": "Domain name",
    "attributes": ["text", "value", "whois-registrar"]
  }
]
```

### Example

```bash
curl -X GET "https://intelligence.threatwinds.com/api/ingest/v1/definitions" \
  -H "api-key: your-api-key" \
  -H "api-secret: your-api-secret"
```
