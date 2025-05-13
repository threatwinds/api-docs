---
layout: default
title: Ingest API
nav_order: 8
has_children: true
---

# Ingest API

The Ingest API allows you to submit and manage threat intelligence data in the ThreatWinds platform. It provides endpoints for ingesting entities, associations between entities, comments, and scheduling scans for IPs or hostnames.

## Overview

ThreatWinds Ingest API allows you to:

| Feature                             | Description                                             |
|-------------------------------------|---------------------------------------------------------|
| [Entities](/ingest/entity)          | Submit new threat intelligence entities to the platform |
| [Associations](/ingest/association) | Create associations between entities                    |
| [Comments](/ingest/comment)         | Add comments to entities                                |
| Well-Known Entities                 | Mark entities as "well-known" (trusted)                 |
| [Scans](/ingest/scan)               | Schedule scans for IPs or hostnames                     |
| Definitions                         | Retrieve entity definitions                             |

## Authentication

Like all ThreatWinds APIs, the Ingest API requires authentication. You can authenticate using either:

| Authentication Method    | Description                                            |
|--------------------------|--------------------------------------------------------|
| **Authorization Header** | Include a bearer token in the Authorization header     |
| **API Key and Secret**   | Include your API key and secret in the request headers |

For more details on authentication, see the [Authentication](/auth) section.

## Authorization

Access to the Ingest API endpoints is controlled by role-based permissions defined in the gateway. Users must have at least one of the required roles assigned to their account to access each endpoint.

The available roles include:

| Role         | Description                                       |
|--------------|---------------------------------------------------|
| **admin**    | Full administrative access                        |
| **user**     | Standard user access                              |
| **reporter** | Access to reporting and data submission endpoints |
| **trusted**  | Access to trusted or verified endpoints           |

### Role Requirements for Endpoints

| Endpoint       | Method | Required Role |
|----------------|--------|---------------|
| `/entity`      | POST   | `reporter`    |
| `/association` | POST   | `reporter`    |
| `/comment`     | POST   | `reporter`    |
| `/well-known`  | POST   | `trusted`     |
| `/scan`        | POST   | `reporter`    |
| `/definitions` | GET    | `trusted`     |

For more detailed information about each endpoint's role requirements, please refer to the individual endpoint documentation pages.

## API Endpoints

The base URL for the Ingest API is:

```
https://intelligence.threatwinds.com/api/ingest/v1
```

### Available Endpoints

| Endpoint                              | Method | Description                            |
|---------------------------------------|--------|----------------------------------------|
| [`/entity`](/ingest/entity)           | POST   | Insert a new entity with associations  |
| [`/association`](/ingest/association) | POST   | Insert an association between entities |
| [`/comment`](/ingest/comment)         | POST   | Add a comment to an entity             |
| `/well-known`                         | POST   | Insert a well-known (trusted) entity   |
| [`/scan`](/ingest/scan)               | POST   | Schedule a scan for an IP or hostname  |
| `/definitions`                        | GET    | Retrieve entity definitions            |

For detailed information about each endpoint, please refer to the specific documentation pages.
