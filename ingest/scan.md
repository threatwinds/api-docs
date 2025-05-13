---
layout: default
title: Scan
parent: Ingest API
nav_order: 4
---

# Scan Endpoint

The Scan endpoint allows you to schedule and manage scans for IPs or hostnames in the ThreatWinds platform.

## Schedule a Scan

This endpoint allows you to schedule a scan for an IP address or hostname.

### Endpoint

```
POST /api/ingest/v1/scan
```

### Request Headers

| Header          | Description                                                        |
|-----------------|--------------------------------------------------------------------|
| `Authorization` | Bearer token for authentication (optional if using API key/secret) |
| `api-key`       | Your API key (optional if using Authorization header)              |
| `api-secret`    | Your API secret (optional if using Authorization header)           |

> **Note:** The `user-id` and `groups` headers are added automatically by the API gateway when required and should not be provided by the client.

### Required Roles

Access to this endpoint is controlled by role-based permissions defined in the gateway. Users must have at least one of the required roles assigned to their account to access this endpoint.

Required role: `reporter`

This endpoint requires the `reporter` role, which allows users to submit threat intelligence data to the platform.

### Request Body

The request body should be a JSON object with the following structure:

```json
{
  "target": "string",
  "webhook": {
    "url": "string",
    "headers": [
      {
        "key": "string",
        "value": "string"
      }
    ]
  }
}
```

#### Parameters

| Parameter         | Type   | Description                                                      |
|-------------------|--------|------------------------------------------------------------------|
| `target`          | string | The IP address or hostname to scan (must be a valid IP or FQDN)  |
| `webhook`         | object | Optional webhook configuration for scan completion notifications |
| `webhook.url`     | string | URL to call when the scan is complete                            |
| `webhook.headers` | array  | Optional headers to include in the webhook request               |

### Response

#### Success Response (200 OK or 202 Accepted)

```json
{
  "id": "unique-task-id",
  "status": "new"
}
```

The API returns different status codes based on the task status:
- 202 Accepted: when a new scan task is created (status: "new")
- 200 OK: when returning an existing scan task (status: "queued", "running", or "done")

#### Error Responses

| Status Code | Description                          |
|-------------|--------------------------------------|
| 400         | Bad Request - Invalid input data     |
| 401         | Unauthorized - Authentication failed |
| 403         | Forbidden - Insufficient permissions |

### Example

```bash
curl -X POST "https://intelligence.threatwinds.com/api/ingest/v1/scan" \
  -H "Content-Type: application/json" \
  -H "api-key: your-api-key" \
  -H "api-secret: your-api-secret" \
  -d '{
    "target": "203.0.113.1",
    "webhook": {
      "url": "https://your-webhook-url.com/callback",
      "headers": [
        {
          "key": "Authorization",
          "value": "Bearer your-token"
        }
      ]
    }
  }'
```

Example response:

```json
{
  "id": "a1b2c3d4-e5f6-7890-abcd-ef1234567890",
  "status": "new"
}
```

## About Scans

When you submit a target for scanning, the ThreatWinds platform automatically performs a comprehensive scan that includes:

- Port scanning to identify open ports
- Service detection to identify running services
- SSL/TLS certificate information collection and analysis

> **Note:** You don't need to specify a scan type in your request. The system automatically determines the appropriate scans to run based on the target.

## Scan Results

After a scan is completed, the results are automatically ingested into the ThreatWinds platform as entities and associations. You can access these results through the Analytics API.

> **Note:** The scan endpoint only returns a task ID and status. To retrieve the actual scan results, you need to use the Analytics API once the scan is complete.

### Data Generated from Scans

When a scan completes, it creates various types of data in the ThreatWinds platform:

**Entity Types:**
- Port entities for open ports found during the scan
- Service entities for services identified on ports
- SSL/TLS certificate entities with certificate information
- Banner entities containing service banner information

**Association Types:**
- Associations between the target and its open ports
- Associations between ports and the services running on them
- Associations between targets/services and their SSL/TLS certificates
- Associations between services and their banners

This rich data structure allows for comprehensive analysis of the scanned target through the Analytics API.
