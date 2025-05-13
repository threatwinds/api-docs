---
layout: default
title: Quickstart
nav_order: 2
permalink: /quickstart
---

# Quickstart

Welcome to the ThreatWinds API Quickstart Guide! This guide provides practical examples to help you quickly start using the ThreatWinds API for searching, analyzing, and reporting cybersecurity threat intelligence.

## Available APIs

ThreatWinds offers several RESTful APIs to help you work with threat intelligence data:

| API                | Description                                    | Documentation               |
|--------------------|------------------------------------------------|-----------------------------|
| Authentication API | Manage user sessions and API keys              | [Documentation](/auth)      |
| Search API         | Find and query threat intelligence entities    | [Documentation](/search)    |
| Analytics API      | Analyze relationships and details of threats   | [Documentation](/analytics) |
| Ingest API         | Submit new threat intelligence to the platform | [Documentation](/ingest)    |

## Authentication

ThreatWinds supports two authentication methods:

| Authentication Method            | Description                                      | Best For                                       |
|----------------------------------|--------------------------------------------------|------------------------------------------------|
| **Session-based authentication** | Uses an Authorization header with a bearer token | Web applications and interactive sessions      |
| **API key authentication**       | Uses API key and secret pairs                    | Third-party integrations and automated systems |

### Creating a Session (Authorization Header)

For web applications or interactive sessions, use the session-based authentication:

```bash
curl -X 'POST' \
  'https://intelligence.threatwinds.com/api/auth/v2/session' \
  -H 'accept: application/json' \
  -H 'Content-Type: application/json' \
  -d '{
  "email": "your.email@example.com"
}'
```

Response:
```json
{
  "bearer": "m5orsH7hqidILzwkGUcjf9gl93r9KqOj",
  "sessionID": "5f35d2c4-5633-4b16-bbf0-5ca22ef8ea2e",
  "expireAt": 1674492894,
  "ip": "1.1.1.1",
  "userAgent": "Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/51.0.2704.103 Safari/537.36",
  "verificationCodeID": "5f35d2c4-5633-4b16-bbf0-5ca22ef8ea2e"
}
```

> **Note:** The session is not fully active until it is verified with the verification code sent to your email.

Verify the session with the code received in your email:

```bash
curl -X 'PUT' \
  'https://intelligence.threatwinds.com/api/auth/v2/session/verification' \
  -H 'accept: application/json' \
  -H 'Content-Type: application/json' \
  -d '{
  "verificationCodeID": "5f35d2c4-5633-4b16-bbf0-5ca22ef8ea2e",
  "code": "123456"
}'
```

Use the `bearer` token in subsequent requests:

```bash
curl -X 'GET' \
  'https://intelligence.threatwinds.com/api/search/v1/entities/simple' \
  -H 'accept: application/json' \
  -H 'Authorization: Bearer m5orsH7hqidILzwkGUcjf9gl93r9KqOj' \
  -H 'Content-Type: application/json' \
  -d '{
  "query": "example.com"
}'
```

### Creating a Key Pair (for Third-Party Integrations)

For automated systems, services, or third-party integrations, create and use API key pairs:

```bash
# First, create a session to get the bearer token
curl -X 'POST' \
  'https://intelligence.threatwinds.com/api/auth/v2/session' \
  -H 'accept: application/json' \
  -H 'Content-Type: application/json' \
  -d '{
  "email": "your.email@example.com"
}'

# Then verify the session with the code received in your email
curl -X 'PUT' \
  'https://intelligence.threatwinds.com/api/auth/v2/session/verification' \
  -H 'accept: application/json' \
  -H 'Content-Type: application/json' \
  -d '{
  "verificationCodeID": "5f35d2c4-5633-4b16-bbf0-5ca22ef8ea2e",
  "code": "123456"
}'

# Then use the bearer token to create a key pair
curl -X 'POST' \
  'https://intelligence.threatwinds.com/api/auth/v2/keypair' \
  -H 'accept: application/json' \
  -H 'Authorization: Bearer m5orsH7hqidILzwkGUcjf9gl93r9KqOj' \
  -H 'Content-Type: application/json' \
  -d '{
  "name": "My Integration Key",
  "days": 365
}'
```

Response:
```json
{
  "apiKey": "fq6JoEFTsxiXAl1cVxPDnK4emIQCwaUB",
  "apiSecret": "fq6JoEFTsxiXAl1cVxPDnK4emIQCwaUBfq6JoEFTsxiXAl1cVxPDnK4emIQCwaUB",
  "keyID": "5f35d2c4-5633-4b16-bbf0-5ca22ef8ea2e",
  "keyName": "My Integration Key",
  "expireAt": 1674492894,
  "verified": false,
  "verificationCodeID": "5f35d2c4-5633-4b16-bbf0-5ca22ef8ea2e"
}
```

> **Important:** Store the API secret securely. It won't be displayed again.

After creating a key pair, you need to verify it:

```bash
curl -X 'PUT' \
  'https://intelligence.threatwinds.com/api/auth/v2/keypair/verification' \
  -H 'accept: application/json' \
  -H 'Authorization: Bearer m5orsH7hqidILzwkGUcjf9gl93r9KqOj' \
  -H 'Content-Type: application/json' \
  -d '{
  "verificationCodeID": "5f35d2c4-5633-4b16-bbf0-5ca22ef8ea2e",
  "code": "123456"
}'
```

Use the API key and secret in subsequent requests:

```bash
curl -X 'GET' \
  'https://intelligence.threatwinds.com/api/search/v1/entities/simple' \
  -H 'accept: application/json' \
  -H 'api-key: fq6JoEFTsxiXAl1cVxPDnK4emIQCwaUB' \
  -H 'api-secret: fq6JoEFTsxiXAl1cVxPDnK4emIQCwaUBfq6JoEFTsxiXAl1cVxPDnK4emIQCwaUB' \
  -H 'Content-Type: application/json' \
  -d '{
  "query": "example.com"
}'
```

## Searching for Threat Intelligence

ThreatWinds provides multiple ways to search for threat intelligence entities.

### Simple Search

Use simple search for straightforward queries. The following parameters can be used in your search:

| Parameter         | Type    | Required | Description                              |
|-------------------|---------|----------|------------------------------------------|
| `query`           | string  | Yes      | The search term (IP, domain, hash, etc.) |
| `accuracy`        | integer | No       | Minimum accuracy level (0 to 3)          |
| `reputation`      | integer | No       | Minimum reputation score (-3 to 3)       |
| `source.includes` | array   | No       | Fields to include in the response        |
| `source.excludes` | array   | No       | Fields to exclude from the response      |

Example request:

```bash
curl -X 'POST' \
  'https://intelligence.threatwinds.com/api/search/v1/entities/simple?limit=10&page=1' \
  -H 'accept: application/json' \
  -H 'api-key: your-api-key' \
  -H 'api-secret: your-api-secret' \
  -H 'Content-Type: application/json' \
  -d '{
  "query": "8.8.8.8",
  "accuracy": 1,
  "reputation": -2,
  "source": {
    "includes": ["id", "type", "attributes.value", "reputation", "accuracy"],
    "excludes": []
  }
}'
```

This searches for the IP address "8.8.8.8" with a minimum accuracy of 1 and a minimum reputation of -2.

### Advanced Search

For more complex queries, use advanced search. The query structure follows a Boolean logic format:

| Query Section | Purpose              | Description                                       |
|---------------|----------------------|---------------------------------------------------|
| `must`        | Required conditions  | All conditions must match (AND logic)             |
| `should`      | Preferred conditions | At least one condition should match (OR logic)    |
| `must_not`    | Excluded conditions  | None of these conditions should match (NOT logic) |

Common query types:

| Query Type | Purpose       | Example                                                |
|------------|---------------|--------------------------------------------------------|
| `term`     | Exact match   | `{"term": {"type": {"value": "ip"}}}`                  |
| `match`    | Partial match | `{"match": {"attributes.ip": {"query": "203.0.113"}}}` |
| `range`    | Numeric range | `{"range": {"reputation": {"gte": -3, "lte": -1}}}`    |

Example request:

```bash
curl -X 'POST' \
  'https://intelligence.threatwinds.com/api/search/v1/entities/advanced?limit=10&page=1' \
  -H 'accept: application/json' \
  -H 'api-key: your-api-key' \
  -H 'api-secret: your-api-secret' \
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
            "gte": -3,
            "lte": -1
          }
        }
      }
    ],
    "should": [
      {
        "match": {
          "attributes.ip": {
            "query": "203.0.113"
          }
        }
      }
    ],
    "must_not": [
      {
        "term": {
          "tags": {
            "value": "benign"
          }
        }
      }
    ]
  }
}'
```

This searches for IP addresses with reputation between -3 and -1, preferably containing "203.0.113", and excluding those tagged as "benign".

### Entity Type Lookup

To find specific types of entities, you can use the definitions endpoint:

```bash
curl -X 'GET' \
  'https://intelligence.threatwinds.com/api/ingest/v1/definitions' \
  -H 'accept: application/json' \
  -H 'api-key: your-api-key' \
  -H 'api-secret: your-api-secret'
```

> **Note:** This endpoint requires the `trusted` role. Alternatively, you can view the comprehensive list of entity types in the [Entity Types](/search/entity-types) documentation.

## Analyzing Threat Intelligence

### Get Entity Details

Retrieve comprehensive details about a specific entity:

```bash
curl -X 'GET' \
  'https://intelligence.threatwinds.com/api/analytics/v1/entity/ip-a1b2c3d4e5f6g7h8i9j0k1l2m3n4o5p6q7r8s9t0u1v2w3x4y5z6a7b8c9d0e1f2/details' \
  -H 'accept: application/json' \
  -H 'api-key: your-api-key' \
  -H 'api-secret: your-api-secret'
```

### Get Entity Relations

Explore relationships between entities:

```bash
curl -X 'GET' \
  'https://intelligence.threatwinds.com/api/analytics/v1/entity/ip-a1b2c3d4e5f6g7h8i9j0k1l2m3n4o5p6q7r8s9t0u1v2w3x4y5z6a7b8c9d0e1f2/relations?depth=2&limit=10' \
  -H 'accept: application/json' \
  -H 'api-key: your-api-key' \
  -H 'api-secret: your-api-secret'
```

This retrieves entities related to the specified IP address up to a depth of 2 relationships.

## Reporting Threat Intelligence

### Submit a New Entity

Report a new threat intelligence entity:

```bash
curl -X 'POST' \
  'https://intelligence.threatwinds.com/api/ingest/v1/entity' \
  -H 'Content-Type: application/json' \
  -H 'api-key: your-api-key' \
  -H 'api-secret: your-api-secret' \
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

### Submit an Association Between Entities

Connect related entities:

```bash
curl -X 'POST' \
  'https://intelligence.threatwinds.com/api/ingest/v1/association' \
  -H 'Content-Type: application/json' \
  -H 'api-key: your-api-key' \
  -H 'api-secret: your-api-secret' \
  -d '{
    "entityID": "ip-a1b2c3d4e5f67890abcdef1234567890abcdef1234567890abcdef1234567890",
    "relatedEntityID": "domain-f67890abcdef1234567890abcdef1234567890abcdef1234567890a1b2c3d4e5"
  }'
```

### Add a Comment to an Entity

Provide additional context to an entity:

```bash
curl -X 'POST' \
  'https://intelligence.threatwinds.com/api/ingest/v1/comment' \
  -H 'Content-Type: application/json' \
  -H 'api-key: your-api-key' \
  -H 'api-secret: your-api-secret' \
  -d '{
    "entityID": "ip-a1b2c3d4e5f67890abcdef1234567890abcdef1234567890abcdef1234567890",
    "comment": "This IP was observed scanning for vulnerable WordPress installations.",
    "visibleBy": ["group1", "group2"]
  }'
```

## Next Steps

Now that you understand the basics of working with the ThreatWinds API, here are the recommended next steps:

| Step                          | Description                                  | Resources                                                                                                    |
|-------------------------------|----------------------------------------------|--------------------------------------------------------------------------------------------------------------|
| 1. Explore API documentation  | Dive deeper into each API's capabilities     | [Authentication API](/auth)<br>[Search API](/search)<br>[Analytics API](/analytics)<br>[Ingest API](/ingest) |
| 2. Create advanced queries    | Build more sophisticated search queries      | [Advanced Search](/search/advanced-search)                                                                   |
| 3. Build integrations         | Connect ThreatWinds with your security tools | [API Key Authentication](#creating-a-key-pair-for-third-party-integrations)                                  |
| 4. Set up automated reporting | Contribute threat intelligence data          | [Ingest API](/ingest)                                                                                        |
