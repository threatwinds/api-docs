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

## Error Response Headers

For responses with status codes other than 200 and 202, the following headers are included:

| Header        | Description                                                |
|---------------|------------------------------------------------------------|
| **x-error**   | Contains a description of the error that occurred          |
| **x-error-id**| Contains a unique identifier for the error for support     |

## Error Codes

| Status Code | Description           | Possible Cause                                          |
|-------------|-----------------------|---------------------------------------------------------|
| **400**     | Bad Request           | Invalid request parameters or malformed JSON            |
| **401**     | Unauthorized          | Missing or invalid authentication credentials           |
| **403**     | Forbidden             | Authenticated user lacks permission for this operation  |
| **404**     | Not Found             | The requested resource does not exist                   |
| **500**     | Internal Server Error | Server-side error; please contact support if persistent |