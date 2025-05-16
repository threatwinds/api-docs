---
layout: default
title: Users
parent: Authentication
nav_order: 5
permalink: /auth/user
---

# Users

Table of Content:

* [Create user](#createUser)
* [Get user by ID](#getUserById)

## Create user {#createUser}
This API endpoint creates a new user account and sends a verification code.

**Endpoint:** https://intelligence.threatwinds.com/api/auth/v2/user

### Parameters

| Parameter    | Location | Type   | Required | Description                                                                                               | Example        |
|--------------|----------|--------|----------|-----------------------------------------------------------------------------------------------------------|----------------|
| **email**    | body     | string | Yes      | The email address for the new user account. This email will be used for authentication and communication. | "john@doe.net" |
| **fullName** | body     | string | Yes      | The full name of the user.                                                                                | "John Doe"     |
| **alias**    | body     | string | Yes      | A username or alias for the user.                                                                         | "johny"        |

To create a user, use a **POST** request, for example:

```bash
curl -X 'POST' \
  'https://intelligence.threatwinds.com/api/auth/v2/user' \
  -H 'accept: application/json' \
  -H 'Content-Type: application/json' \
  -d '{
  "email": "john@doe.net",
  "fullName": "John Doe",
  "alias": "johny"
}'
```

### Returns

A successful response will return a JSON object containing session information and a verification code ID, similar to creating a session:

```json
{
  "bearer": "fq6JoEFTsxiXAl1cVdPDnK4emIQCwaUBfq6JoEFTsxiXAl1cVxPDnK4emIQCwaUB",
  "sessionID": "5f35d2c4-5633-4b16-bbf0-5ca22ef8ea2e",
  "expireAt": 1674492894,
  "ip": "1.1.1.1",
  "userAgent": "Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/51.0.2704.103 Safari/537.36",
  "verificationCodeID": "5f35d2c4-5633-4b16-bbf0-5ca22ef8ea2e"
}
```

> **Note:** After creating a user, you need to verify the session using the verification code sent to the provided email address. See the [Verify session](#verifySession) section in the [Sessions](/auth/session) documentation.

## Get user by ID {#getUserById}

This API endpoint retrieves information about a user by their ID.

**Endpoint:** https://intelligence.threatwinds.com/api/auth/v2/user/id/{id}

### Parameters

| Parameter | Location | Type   | Required | Description                        | Example                              |
|-----------|----------|--------|----------|------------------------------------|--------------------------------------|
| **id**    | path     | string | Yes      | The unique identifier of the user. | 5f35d2c4-5633-4b16-bbf0-5ca22ef8ea2e |

To get a user by ID, use a **GET** request, for example:

```bash
curl -X 'GET' \
  'https://intelligence.threatwinds.com/api/auth/v2/user/id/5f35d2c4-5633-4b16-bbf0-5ca22ef8ea2e' \
  -H 'accept: application/json'
```

### Returns

A successful response will return a JSON object containing basic information about the user:

```json
{
  "alias": "johny"
}
```

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