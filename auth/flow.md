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

| Authentication Method           | Description                                       |
|---------------------------------|---------------------------------------------------|
| **Bearer Token Authentication** | Using an Authorization header with a bearer token |
| **API Key Authentication**      | Using API key and API secret headers              |

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

## Session Management

When using bearer token authentication:

| Aspect                     | Description                                                                                |
|----------------------------|--------------------------------------------------------------------------------------------|
| **Session Lifetime**       | Sessions have a limited lifetime                                                           |
| **Refresh Responsibility** | The client is responsible for refreshing the session before it expires                     |
| **Management Endpoints**   | The Auth API provides endpoints for session management (create, refresh, validate, revoke) |

## Best Practices

| Application Type                   | Recommended Authentication  | Security Practices                                                                                  |
|------------------------------------|-----------------------------|-----------------------------------------------------------------------------------------------------|
| **Web Applications**               | Bearer token authentication | • Store the token securely (for example, in an HTTP-only cookie)<br>• Implement token refresh logic |
| **Server-to-Server Communication** | API key authentication      | • Store the API key and secret securely                                                             |
| **Mobile Applications**            | Bearer token authentication | • Store the token securely in the device's secure storage<br>• Implement token refresh logic        |

## Related Documentation

| Documentation                        | Description                                 |
|--------------------------------------|---------------------------------------------|
| [Email Authentication](/auth/email)  | Managing email addresses for authentication |
| [Session Management](/auth/session)  | Creating and managing user sessions         |
| [Key Pair Management](/auth/keypair) | Creating and managing API keys              |
| [User Management](/auth/user)        | Creating and managing user accounts         |
