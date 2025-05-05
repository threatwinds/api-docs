---
layout: default
title: Authentication
nav_order: 4
has_children: true
---

# Authentication API

The Authentication API (Auth API) is responsible for user authentication, session management, and access control in the ThreatWinds platform. It works in conjunction with the Gateway to secure all API requests and ensure that only authorized users can access protected resources.

## Overview

ThreatWinds Auth API allows you to:

- Create and manage user accounts
- Authenticate users through email verification
- Manage user sessions
- Create and manage API keys for programmatic access
- Verify and validate authentication credentials

## Authentication Methods

ThreatWinds supports two primary authentication methods:

1. **Bearer Token Authentication** - Using an Authorization header with a bearer token
2. **API Key Authentication** - Using API key and API secret headers

For more details on the authentication flow, see the [Authentication Flow](/auth/flow) page.

## API Endpoints

The base URL for the Auth API is:

```
https://intelligence.threatwinds.com/api/auth/v2
```

For detailed information about each endpoint, please refer to the specific documentation pages or the [Swagger UI](https://intelligence.threatwinds.com/api/auth/v2/swagger/index.html).
