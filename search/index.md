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

- Search for threat intelligence entities using [simple queries](/search/simple-search)
- Perform [advanced searches](/search/advanced-search) with complex filters and aggregations
- Search in the [entity history](/search/history-search)
- Retrieve detailed information about [specific entities](/search/entity-lookup)
- View relationships between [entities](/search/entity)
- Access [comments](/search/comments) associated with entities
- Browse a comprehensive list of [entity types](/search/entity-types)

## Authentication

Like all ThreatWinds APIs, the Search API requires authentication. You can authenticate using either:

1. **Authorization Header:** include a bearer token in the Authorization header
2. **API Key and Secret:** include your API key and secret in the request headers

For more details on authentication, see the [Authentication](/auth) section.

## API Endpoints

The base URL for the Search API is:

```
https://intelligence.threatwinds.com/api/search/v1
```

For detailed information about each endpoint, please refer to the specific documentation pages.
