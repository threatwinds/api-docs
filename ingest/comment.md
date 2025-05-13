---
layout: default
title: Comment
parent: Ingest API
nav_order: 3
---

# Comment Endpoint

The Comment endpoint allows you to add comments to entities or relations in the ThreatWinds platform.

## Insert Comment

This endpoint allows you to insert a comment for an entity or a relation.

### Endpoint

```
POST /api/ingest/v1/comment
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
  "entityID": "string",
  "comment": "string",
  "parentID": "string",
  "visibleBy": [
    "string"
  ]
}
```

> **Note**: you need to provide valid entity IDs that already exist in the system. Entity IDs in ThreatWinds follow the format `[type]-[hash]` where `type` is the entity type and `hash` is the SHA3-256 hash of the entity's main attribute. You can obtain entity IDs by first creating entities using the `/entity` endpoint and/or by querying entities through the Search API.

#### Parameters

| Parameter   | Type   | Description                                                                                   |
|-------------|--------|-----------------------------------------------------------------------------------------------|
| `entityID`  | string | The ID of the entity to which the comment is attached                                         |
| `comment`   | string | The comment text                                                                              |
| `parentID`  | string | Optional ID of a parent comment (for threaded comments)                                       |
| `visibleBy` | array  | Optional array of groups that can see the comment (defaults to user's groups if not provided) |

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
curl -X POST "https://intelligence.threatwinds.com/api/ingest/v1/comment" \
  -H "Content-Type: application/json" \
  -H "api-key: your-api-key" \
  -H "api-secret: your-api-secret" \
  -d '{
    "entityID": "ip-a1b2c3d4e5f67890abcdef1234567890abcdef1234567890abcdef1234567890",
    "comment": "This IP has been observed in multiple phishing campaigns.",
    "visibleBy": ["group1", "group2"]
  }'
```

### Use Cases

Comments can be used for various purposes in threat intelligence:

1. **Analyst Notes**: add observations or analysis of an entity
2. **Context Information**: provide additional context that might not fit in standard attributes
3. **Investigation Updates**: document findings during an investigation
4. **Collaboration**: share insights with team members
