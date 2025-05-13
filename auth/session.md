---
layout: default
title: Sessions
parent: Authentication
nav_order: 4
permalink: /auth/session
---

# Sessions

Table of Content:

* [Create session](#createSession)
* [Check session](#checkSession)
* [Close session](#closeSession)
* [Extend session](#extendSession)
* [Verify session](#verifySession)
* [Get sessions](#getSessions)

## Create session {#createSession}
This API endpoint starts a new session and sends a one-time password for verification.

**Endpoint:** https://intelligence.threatwinds.com/api/auth/v2/session

### Parameters

| Parameter | Location | Type   | Required | Description                                                                                            | Example        |
|-----------|----------|--------|----------|--------------------------------------------------------------------------------------------------------|----------------|
| **email** | body     | string | Yes      | The email address associated with your account. This email will be used to send the verification code. | "john@doe.net" |

To create a session, use a **POST** request, for example:

```bash
curl -X 'POST' \
  'https://intelligence.threatwinds.com/api/auth/v2/session' \
  -H 'accept: application/json' \
  -H 'Content-Type: application/json' \
  -d '{
  "email": "john@doe.net"
}'
```

### Returns

A successful response will return a JSON object containing session information and a verification code ID:

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

> **Note:** The session is not fully active until it is verified with the verification code sent to your email.

## Check session {#checkSession}

This API endpoint checks a user session and returns privileges.

**Endpoint:** https://intelligence.threatwinds.com/api/auth/v2/session

### Parameters

| Parameter         | Location | Type   | Required | Description                                                        |
|-------------------|----------|--------|----------|--------------------------------------------------------------------|
| **Authorization** | header   | string | Yes      | The bearer token received when creating and verifying the session. |

To check a session, use a **GET** request, for example:

```bash
curl -X 'GET' \
  'https://intelligence.threatwinds.com/api/auth/v2/session' \
  -H 'accept: application/json' \
  -H 'Authorization: Bearer fq6JoEFTsxiXAl1cVdPDnK4emIQCwaUBfq6JoEFTsxiXAl1cVxPDnK4emIQCwaUB'
```

### Returns

A successful response will return a JSON object containing information about the session and user privileges:

```json
{
  "sessionID": "5f35d2c4-5633-4b16-bbf0-5ca22ef8ea2e",
  "userID": "5f35d2c4-5633-4b16-bbf0-5ca22ef8ea2e",
  "alias": "johny",
  "fullName": "John Doe",
  "expireAt": 1674492894,
  "ip": "1.1.1.1",
  "userAgent": "Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/51.0.2704.103 Safari/537.36",
  "verified": true,
  "roles": ["user", "admin"],
  "groups": ["public"]
}
```

## Close session {#closeSession}

This API endpoint closes a session.

**Endpoint:** https://intelligence.threatwinds.com/api/auth/v2/session/{id}

### Parameters

| Parameter         | Location | Type   | Required | Description                             | Example                              |
|-------------------|----------|--------|----------|-----------------------------------------|--------------------------------------|
| **Authorization** | header   | string | Yes      | The bearer token for an active session. |                                      |
| **id**            | path     | string | Yes      | The ID of the session to close.         | 5f35d2c4-5633-4b16-bbf0-5ca22ef8ea2e |

To close a session, use a **DELETE** request, for example:

```bash
curl -X 'DELETE' \
  'https://intelligence.threatwinds.com/api/auth/v2/session/5f35d2c4-5633-4b16-bbf0-5ca22ef8ea2e' \
  -H 'accept: application/json' \
  -H 'Authorization: Bearer fq6JoEFTsxiXAl1cVdPDnK4emIQCwaUBfq6JoEFTsxiXAl1cVxPDnK4emIQCwaUB'
```

### Returns

A successful response will return a JSON object with a success message:

```json
{
  "message": "acknowledged"
}
```

## Extend session {#extendSession}

This API endpoint extends the current session's expiration time.

**Endpoint:** https://intelligence.threatwinds.com/api/auth/v2/session/extend

### Parameters

| Parameter         | Location | Type   | Required | Description                                          |
|-------------------|----------|--------|----------|------------------------------------------------------|
| **Authorization** | header   | string | Yes      | The bearer token for the session you want to extend. |

To extend a session, use a **PUT** request, for example:

```bash
curl -X 'PUT' \
  'https://intelligence.threatwinds.com/api/auth/v2/session/extend' \
  -H 'accept: application/json' \
  -H 'Authorization: Bearer fq6JoEFTsxiXAl1cVdPDnK4emIQCwaUBfq6JoEFTsxiXAl1cVxPDnK4emIQCwaUB'
```

### Returns

A successful response will return a JSON object with a success message:

```json
{
  "message": "acknowledged"
}
```

## Verify session {#verifySession}

This API endpoint verifies a session using the verification code sent during session creation.

**Endpoint:** https://intelligence.threatwinds.com/api/auth/v2/session/verification

### Parameters

| Parameter              | Location | Type   | Required | Description                                                  | Example                                |
|------------------------|----------|--------|----------|--------------------------------------------------------------|----------------------------------------|
| **verificationCodeID** | body     | string | Yes      | The verification code ID received when creating the session. | "5f35d2c4-5633-4b16-bbf0-5ca22ef8ea2e" |
| **code**               | body     | string | Yes      | The verification code sent to your email.                    | "654321"                               |

To verify a session, use a **PUT** request, for example:

```bash
curl -X 'PUT' \
  'https://intelligence.threatwinds.com/api/auth/v2/session/verification' \
  -H 'accept: application/json' \
  -H 'Content-Type: application/json' \
  -d '{
  "verificationCodeID": "5f35d2c4-5633-4b16-bbf0-5ca22ef8ea2e",
  "code": "654321"
}'
```

### Returns

A successful response will return a JSON object with a success message:

```json
{
  "message": "acknowledged"
}
```

## Get sessions {#getSessions}

This API endpoint gets all active sessions for the current user.

**Endpoint:** https://intelligence.threatwinds.com/api/auth/v2/sessions

### Parameters

| Parameter | Location | Type | Required | Description |
|-----------|----------|------|----------|-------------|
| **Authorization** | header | string | Yes | The bearer token for an active session. |

To get all sessions, use a **GET** request, for example:

```bash
curl -X 'GET' \
  'https://intelligence.threatwinds.com/api/auth/v2/sessions' \
  -H 'accept: application/json' \
  -H 'Authorization: Bearer fq6JoEFTsxiXAl1cVdPDnK4emIQCwaUBfq6JoEFTsxiXAl1cVxPDnK4emIQCwaUB'
```

### Returns

A successful response will return a JSON object containing an array of active sessions:

```json
{
  "sessions": [
    {
      "sessionID": "5f35d2c4-5633-4b16-bbf0-5ca22ef8ea2e",
      "ip": "1.1.1.1",
      "userAgent": "Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/51.0.2704.103 Safari/537.36",
      "expireAt": 1674492894,
      "current": true
    },
    {
      "sessionID": "6a2b4c5d-6e7f-8g9h-0i1j-2k3l4m5n6o7p",
      "ip": "2.2.2.2",
      "userAgent": "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/91.0.4472.114 Safari/537.36",
      "expireAt": 1674492894,
      "current": false
    }
  ]
}
```
