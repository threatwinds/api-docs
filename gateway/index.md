---
layout: default
title: Gateway API
nav_order: 6
has_children: true
---

# Gateway API

The Gateway API serves as the central entry point for all ThreatWinds microservices. It handles routing, authentication, and authorization for requests to the various API endpoints.

## Overview

ThreatWinds Gateway API allows you to:

- Route requests to the appropriate microservices
- Manage access control through public/private routes and role-based permissions
- Configure routing rules for different API endpoints

## Authentication

The Gateway works in conjunction with the Auth API to authenticate and authorize requests. When a request comes in, the Gateway:

1. Checks if the route is public or requires authentication
2. For protected routes, validates the authentication credentials (bearer token or API key/secret)
3. Verifies that the authenticated user has the necessary roles to access the requested resource
4. Routes the request to the appropriate microservice if authentication and authorization are successful

For more details on authentication, see the [Authentication](/auth) section.

## API Endpoints

The base URL for the Gateway API management endpoints is:

```
https://intelligence.threatwinds.com/api/gateway/v1
```

These endpoints allow administrators to configure the Gateway's routing rules. For detailed information about each endpoint, please refer to the specific documentation pages or the [Swagger UI](https://intelligence.threatwinds.com/api/gateway/v1/swagger/index.html).

## Microservices

The Gateway provides access to the following microservices:

- [Authentication API](/auth) - User authentication and management
- [Search API](/search) - Search for threat intelligence entities
- [Analytics API](/analytics) - Analyze threat intelligence data
- [Ingest API](/ingest) - Contribute threat intelligence data
- [Feeds API](/feeds) - Access threat intelligence feeds

Each microservice has its own base URL and endpoints, but all requests should go through the Gateway to ensure proper authentication and authorization.