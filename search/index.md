---
layout: default
title: Search API
nav_order: 8
has_children: true
---

# Search API

The Search API provides powerful search capabilities for threat intelligence entities in the ThreatWinds platform. It allows you to search for entities using simple or advanced queries, retrieve entity details, and explore relationships between entities.

## Overview

ThreatWinds Search API allows you to:

| Feature                                    | Description                                                  |
|--------------------------------------------|--------------------------------------------------------------|
| [Simple Search](/search/simple-search)     | Search for threat intelligence entities using simple queries |
| [Advanced Search](/search/advanced-search) | Perform complex searches with filters and aggregations       |
| [History Search](/search/history-search)   | Search in the entity history                                 |
| [Entity Lookup](/search/entity-lookup)     | Retrieve detailed information about specific entities        |
| [Entity Relations](/search/entity)         | View relationships between entities                          |
| [Comments](/search/comments)               | Access comments associated with entities                     |
| [Entity Types](/search/entity-types)       | Browse a comprehensive list of entity types                  |

## Authentication

Like all ThreatWinds APIs, the Search API requires authentication. You can authenticate using either:

| Authentication Method    | Description                                            |
|--------------------------|--------------------------------------------------------|
| **Authorization Header** | Include a bearer token in the Authorization header     |
| **API Key and Secret**   | Include your API key and secret in the request headers |

For more details on authentication, see the [Authentication](/auth) section.

## API Endpoints

The base URL for the Search API is:

```
https://intelligence.threatwinds.com/api/search/v1
```

For detailed information about each endpoint, please refer to the specific documentation pages.
