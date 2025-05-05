---
layout: default
title: Routes Management
parent: Gateway API
nav_order: 1
permalink: /gateway/routes
---

# Routes Management

The Gateway API provides endpoints for managing routing rules. These endpoints allow administrators to create, update, delete, and retrieve routes that define how requests are forwarded to the appropriate microservices.

## Route Properties

A route in the Gateway has the following properties:

* **method** - The HTTP method (GET, POST, PUT, DELETE, etc.)
* **relativePath** - The path pattern to match (e.g., "/api/auth/v2/*any")
* **redirectTo** - The URL of the microservice to forward the request to
* **public** - Whether the route is accessible without authentication
* **roles** - The roles required to access the route (if not public)
* **enabled** - Whether the route is active
* **optional** - Whether the route is optional
* **description** - A description of the route
* **region** - The region for the route

## List Routes

**Endpoint:** https://intelligence.threatwinds.com/api/gateway/v1/routes

### Parameters

* **Authorization** header _string_ (required)  
  This authorization header can be obtained from an active session of the account.

* **api-key** header _string_ (optional)  
  Your API key.

* **api-secret** header _string_ (optional)  
  Your API secret.

> Note: You must use either the Authorization header OR the API key and secret combination.

### Request

To get a list of all routes, use a **GET** request, for example:

```bash
curl -X 'GET' \
  'https://intelligence.threatwinds.com/api/gateway/v1/routes' \
  -H 'accept: application/json' \
  -H 'Authorization: Bearer fq6JoEFTsxiXAl1cVdPDnK4emIQCwaUBfq9JoEFTsxhXAl1cVxPDnK4emIQCwaUB'
```

### Response

A successful response will return a JSON object containing an array of routes:

```json
{
  "routes": [
    {
      "id": 846677924910268400,
      "method": "GET",
      "relativePath": "/api/auth/v2/*any",
      "redirectTo": "https://auth-2zmeoijpja-uc.a.run.app",
      "public": true,
      "roles": [],
      "enabled": true,
      "optional": true,
      "description": "GET any Auth API endpoint",
      "region": "region",
      "createdAt": 1678455996,
      "updatedAt": 1678457013,
      "createdBy": "2e4ad29c-bf32-47b0-ae0d-67ec870b1677"
    }
  ]
}
```

## Create Route

**Endpoint:** https://intelligence.threatwinds.com/api/gateway/v1/route

### Parameters

* **Authorization** header _string_ (required)  
  This authorization header can be obtained from an active session of the account.

* **api-key** header _string_ (optional)  
  Your API key.

* **api-secret** header _string_ (optional)  
  Your API secret.

* **Route** body _object_ (required)  
  The route configuration to create.

### Request

To create a new route, use a **POST** request, for example:

```bash
curl -X 'POST' \
  'https://intelligence.threatwinds.com/api/gateway/v1/route' \
  -H 'accept: application/json' \
  -H 'Authorization: Bearer fq6JoEFTsxiXAl1cVdPDnK4emIQCwaUBfq9JoEFTsxhXAl1cVxPDnK4emIQCwaUB' \
  -H 'Content-Type: application/json' \
  -d '{
  "method": "GET",
  "relativePath": "/api/feeds/v1/*any",
  "redirectTo": "https://feeds-2zmeoijpja-uc.a.run.app",
  "public": false,
  "roles": ["user", "admin"],
  "enabled": true,
  "optional": false,
  "description": "GET any Feeds API endpoint",
  "region": "us-central1"
}'
```

### Response

A successful response will return a JSON object with the created route details, including the assigned ID.

## Update Route

**Endpoint:** https://intelligence.threatwinds.com/api/gateway/v1/route/{id}

### Parameters

* **Authorization** header _string_ (required)  
  This authorization header can be obtained from an active session of the account.

* **api-key** header _string_ (optional)  
  Your API key.

* **api-secret** header _string_ (optional)  
  Your API secret.

* **id** path _integer_ (required)  
  The ID of the route to update.

* **Route** body _object_ (required)  
  The updated route configuration.

### Request

To update an existing route, use a **PUT** request, for example:

```bash
curl -X 'PUT' \
  'https://intelligence.threatwinds.com/api/gateway/v1/route/846677924910268400' \
  -H 'accept: application/json' \
  -H 'Authorization: Bearer fq6JoEFTsxiXAl1cVdPDnK4emIQCwaUBfq9JoEFTsxhXAl1cVxPDnK4emIQCwaUB' \
  -H 'Content-Type: application/json' \
  -d '{
  "method": "GET",
  "relativePath": "/api/feeds/v1/*any",
  "redirectTo": "https://feeds-2zmeoijpja-uc.a.run.app",
  "public": true,
  "roles": [],
  "enabled": true,
  "optional": false,
  "description": "GET any Feeds API endpoint (public)",
  "region": "us-central1"
}'
```

## Delete Route

**Endpoint:** https://intelligence.threatwinds.com/api/gateway/v1/route/{id}

### Parameters

* **Authorization** header _string_ (required)  
  This authorization header can be obtained from an active session of the account.

* **api-key** header _string_ (optional)  
  Your API key.

* **api-secret** header _string_ (optional)  
  Your API secret.

* **id** path _integer_ (required)  
  The ID of the route to delete.

### Request

To delete a route, use a **DELETE** request, for example:

```bash
curl -X 'DELETE' \
  'https://intelligence.threatwinds.com/api/gateway/v1/route/846677924910268400' \
  -H 'accept: application/json' \
  -H 'Authorization: Bearer fq6JoEFTsxiXAl1cVdPDnK4emIQCwaUBfq9JoEFTsxhXAl1cVxPDnK4emIQCwaUB'
```

### Response

A successful response will return a JSON object with a success message:

```json
{
  "message": "acknowledged"
}
```

## Error Codes

* **400** - Bad request
* **401** - Unauthorized
* **403** - Forbidden
* **404** - Route not found
* **500** - Internal server error

> For more detailed information about responses and error codes, please refer to the [Restful API definition](https://intelligence.threatwinds.com/api/gateway/v1/swagger/index.html).