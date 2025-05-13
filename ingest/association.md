---
layout: default
title: Association
parent: Ingest API
nav_order: 2
---

# Association Endpoint

The Association endpoint allows you to create relationships between entities in the ThreatWinds platform.

## Insert Association

This endpoint allows you to insert an association between two entities.

### Endpoint

```
POST /api/ingest/v1/association
```

### Request Headers

| Header          | Description                                                        |
|-----------------|--------------------------------------------------------------------|
| `Authorization` | Bearer token for authentication (optional if using API key/secret) |
| `api-key`       | Your API key (optional if using Authorization header)              |
| `api-secret`    | Your API secret (optional if using Authorization header)           |

> **Note:** The `user-id` and `groups` headers are added automatically by the API gateway when required and shouldn't be provided by the client.

### Required Roles

Access to this endpoint is controlled by role-based permissions defined in the gateway. Users must have at least one of the required roles assigned to their account to access this endpoint.

Required role: `reporter`

This endpoint requires the `reporter` role, which allows users to submit threat intelligence data to the platform.

### Request Body

The request body should be a JSON object with the following structure:

```json
{
  "entityID": "string",
  "relatedEntityID": "string"
}
```

#### Parameters

| Parameter         | Type   | Description                                    |
|-------------------|--------|------------------------------------------------|
| `entityID`        | string | The ID of the source entity in the association |
| `relatedEntityID` | string | The ID of the target entity in the association |

> **Note:** You need to provide valid entities IDs that already exist in the system. Entitiy IDs in ThreatWinds follow the format `[type]-[hash]` where `type` is the entity type and `hash` is the SHA3-256 hash of the entity's main attribute. You can get entity IDs by first creating entities using the `/entity` endpoint and/or by querying entities through the Search API.

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
curl -X POST "https://intelligence.threatwinds.com/api/ingest/v1/association" \
  -H "Content-Type: application/json" \
  -H "api-key: your-api-key" \
  -H "api-secret: your-api-secret" \
  -d '{
    "entityID": "ip-a1b2c3d4e5f67890abcdef1234567890abcdef1234567890abcdef1234567890",
    "relatedEntityID": "domain-f67890abcdef1234567890abcdef1234567890abcdef1234567890a1b2c3d4e5"
  }'
```