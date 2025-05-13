---
layout: default
title: Authentication Flow
parent: Authentication
nav_order: 1
permalink: /auth/flow
---

# Authentication Flow

This document explains how authentication works across the ThreatWinds microservices architecture, focusing on the interaction between the Auth API and the Gateway.

## Overview

ThreatWinds uses a microservices architecture where all API requests pass through a central Gateway. The Gateway works in conjunction with the Auth API to authenticate and authorize requests before forwarding them to the appropriate microservice.

## Authentication Methods

ThreatWinds supports two primary authentication methods:

1. **Bearer Token Authentication**: using an Authorization header with a bearer token
2. **API Key Authentication**: using API key and API secret headers

## Authentication Flow

### Bearer Token Authentication

1. **User Login**:
   - The client sends the session request to the Auth API
   - The Auth API creates a session, returning a Bearer token, verification ID and sending a verification code through email
   - The client validates the session through the Auth API

2. **Authenticated Request**:
   - The client includes the bearer token in the Authorization header of later requests
   - The Gateway receives the request and extracts the bearer token
   - The Gateway validates the token with the Auth API
   - If valid, the Gateway checks if the user has the required roles for the requested resource
   - If authorized, the Gateway forwards the request to the appropriate microservice

### API Key Authentication

1. **API Key Creation**:
   - The client creates an API key and secret through the Auth API
   - The Auth API returns the key and secret to the client with a verification ID and sends an email with the verification code
   - The client verifies the creation of the key through the Auth API

2. **Authenticated Request**:
   - The client includes the API key and secret in the request headers
   - The Gateway receives the request and extracts the API key and secret
   - The Gateway validates the key and secret with the Auth API
   - If valid, the Gateway checks if the API key has the required roles for the requested resource
   - If authorized, the Gateway forwards the request to the appropriate microservice

## Gateway Route Configuration

The Gateway uses route configurations to determine:

1. Whether a route is public or requires authentication
2. Required roles to access a protected route
3. Microservice to forward the request to

Example route configuration:

```json
{
  "method": "GET",
  "relativePath": "/api/search/v1/entities/simple",
  "redirectTo": "https://search-2zmeoijpja-uc.a.run.app",
  "public": false,
  "roles": ["user", "admin"],
  "enabled": true,
  "optional": false,
  "description": "Simple entity search endpoint",
  "region": "us-central1"
}
```

In this example:
- The route isn't public, so authentication is required
- The user must have either the "user" or "admin" role to access it
- The request is forwarded to the Search API microservice

## Public vs. Protected Routes

Some routes in the ThreatWinds API are public and don't require authentication, such as:
- Login endpoints
- Password reset endpoints
- Some documentation endpoints

All other routes require authentication and appropriate roles.

## Session Management

When using bearer token authentication:
- Sessions have a limited lifetime
- The client is responsible for refreshing the session before it expires
- The Auth API provides endpoints for session management (create, refresh, validate, revoke)

## Best Practices

1. **For Web Applications**:
   - Use bearer token authentication
   - Store the token securely (for example, in an HTTP-only cookie)
   - Implement token refresh logic

2. **For Server-to-Server Communication**:
   - Use API key authentication
   - Store the API key and secret securely

3. **For Mobile Applications**:
   - Use bearer token authentication
   - Store the token securely in the device's secure storage
   - Implement token refresh logic

## Related Documentation

- [Email Authentication](/auth/email) - Managing email addresses for authentication