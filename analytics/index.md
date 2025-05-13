---
layout: default
title: Analytics API
nav_order: 7
has_children: true
---

# Analytics API

The Analytics API provides detailed information and analysis of threat intelligence entities in the ThreatWinds platform. It allows you to retrieve entity details, including attributes, metadata, geolocations, and relationships with other entities.

## Overview

ThreatWinds Analytics API allows you to:

| Feature                                         | Description                                                 |
|-------------------------------------------------|-------------------------------------------------------------|
| [Entity Details](/analytics/entity-details)     | Get detailed information about threat intelligence entities |
| [Entity Relations](/analytics/entity-relations) | View relationships between entities in a graph format       |
| Metadata Access                                 | Access extended metadata and geolocation information        |

## Authentication

Like all ThreatWinds APIs, the Analytics API requires authentication. You can authenticate using either:

| Authentication Method    | Description                                            |
|--------------------------|--------------------------------------------------------|
| **Authorization Header** | Include a bearer token in the Authorization header     |
| **API Key and Secret**   | Include your API key and secret in the request headers |

For more details on authentication, see the [Authentication](/auth) section.

## API Endpoints

The base URL for the Analytics API is:

```
https://intelligence.threatwinds.com/api/analytics/v1
```

For detailed information about each endpoint, please refer to the specific documentation pages.
