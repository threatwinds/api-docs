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
* **email** body _string_  
The email address for the new user account. This email will be used for authentication and communication. _e.g.: "john@doe.net"_

* **fullName** body _string_  
The full name of the user. _e.g.: "John Doe"_

* **alias** body _string_  
A username or alias for the user. _e.g.: "johny"_
   
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

> **Note**: After creating a user, you need to verify the session using the verification code sent to the provided email address. See the [Verify session](#verifySession) section in the [Sessions](/auth/session) documentation.

> To get information about responses and error codes please refer to the [Restful API definition](https://intelligence.threatwinds.com/api/auth/v2/swagger/index.html).

## Get user by ID {#getUserById}

This API endpoint retrieves information about a user by their ID.

**Endpoint:** https://intelligence.threatwinds.com/api/auth/v2/user/id/{id}

### Parameters

* **id** path _string_  
The unique identifier of the user. e.g.: 5f35d2c4-5633-4b16-bbf0-5ca22ef8ea2e

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

> To get information about responses and error codes please refer to the [Restful API definition](https://intelligence.threatwinds.com/api/auth/v2/swagger/index.html).