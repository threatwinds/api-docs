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

| Feature                   | Description                                        | Documentation                     |
|---------------------------|----------------------------------------------------|-----------------------------------|
| User Management           | Create and manage user accounts                    | [User API](/auth/user)            |
| Email Authentication      | Authenticate users through email verification      | [Email API](/auth/email)          |
| Session Management        | Create and manage user sessions                    | [Session API](/auth/session)      |
| API Key Management        | Create and manage API keys for programmatic access | [Key Pair API](/auth/keypair)     |
| Authentication Validation | Verify and validate authentication credentials     | [Authentication Flow](/auth/flow) |

## Authentication Methods

ThreatWinds supports two primary authentication methods:

| Authentication Method           | Description                                      | Best For                                       |
|---------------------------------|--------------------------------------------------|------------------------------------------------|
| **Bearer Token Authentication** | Uses an Authorization header with a bearer token | Web applications and interactive sessions      |
| **API Key Authentication**      | Uses API key and API secret headers              | Third-party integrations and automated systems |

For more details on the authentication flow, see the [Authentication Flow](/auth/flow) page.

## API Endpoints

The base URL for the Auth API is:

```
https://intelligence.threatwinds.com/api/auth/v2
```

For detailed information about each endpoint, please refer to the specific documentation pages.
