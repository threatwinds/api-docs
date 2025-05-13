---
layout: default
title: Email
parent: Authentication
nav_order: 2
permalink: /auth/email
---

# Email

Table of Content:

* [Create email](#createEmail)
* [Delete email](#deleteEmail)
* [Get emails](#getEmails)
* [Verify email](#verifyEmail)
* [Set email as preferred](#setPreferredEmail)

## Create email {#createEmail}
This API endpoint creates an unverified email and sends a verification code.

**Endpoint:** https://intelligence.threatwinds.com/api/auth/v2/email

### Parameters

| Parameter | Location | Type | Required | Description | Example |
|-----------|----------|------|----------|-------------|---------|
| **Authorization** | header | string | Yes | This Authorization header can be obtained from an active session of the account. Please ensure that the session is active before attempting to retrieve the authorization header for the request. | |
| **address** | body | string | Yes | The email parameter represents a user's email address that's going to be associated with their account. This email address is used for communication with the user and may also be used as a way to reset their password or verify their account. | "john@doe.net" |

To create an email, use a **POST** request, for example:

```bash
curl -X 'POST' \
  'https://intelligence.threatwinds.com/api/auth/v2/email' \
  -H 'accept: application/json' \
  -H 'Authorization: Bearer fq6JoEFTsxiXAl1cVdPDnK4emIQCwaUBfq9JoEFTsxhXAl1cVxPDnK4emIQCwaUB' \
  -H 'Content-Type: application/json' \
  -d '{
  "address": "john@doe.com"
}'
```

### Returns

## Delete email {#deleteEmail}

This API endpoint deletes an email.

**Endpoint:** https://intelligence.threatwinds.com/api/auth/v2/email/:id

### Parameters

| Parameter | Location | Type | Required | Description | Example |
|-----------|----------|------|----------|-------------|---------|
| **Authorization** | header | string | Yes | This authorization header can be obtained from an active session of the account. | |
| **id** | path | uuid | Yes | The id of the email that you want to delete. | 5f35d2c4-5633-4b16-bbf0-5ca22ef8ea2e |

To delete an email, use a **DELETE** request, for example:

```bash
curl -X 'DELETE' \
  'https://intelligence.threatwinds.com/api/auth/v2/email/5f35d2c4-5633-4b16-bbf0-5ca22ef8ea2e' \
  -H 'accept: application/json' \
  -H 'Authorization: Bearer 5f35d2c4-5633-4b16-bxf0-5ca32ef8ea2e'
```

### Returns

## Get emails {#getEmails}

This API endpoint gets the user's emails.

**Endpoint:** https://intelligence.threatwinds.com/api/auth/v2/emails

### Parameters

| Parameter | Location | Type | Required | Description |
|-----------|----------|------|----------|-------------|
| **Authorization** | header | string | Yes | This authorization header can be obtained from an active session of the account. |

To get the current emails, use a **GET** request, for example:

```bash
curl -X 'GET' \
  'https://intelligence.threatwinds.com/api/auth/v2/emails' \
  -H 'accept: application/json' \
  -H 'Authorization: Bearer 63VD8JoautQNqRLcqNOlJid02R7CDbWK'
```

### Returns

## Verify email {#verifyEmail}

This API endpoint verifies the email using code sent by email.

**Endpoint:** https://intelligence.threatwinds.com/api/auth/v2/email/verification

### Parameters

| Parameter              | Location | Type    | Required | Description                                                                        | Example                                |
|------------------------|----------|---------|----------|------------------------------------------------------------------------------------|----------------------------------------|
| **Authorization**      | header   | string  | Yes      | This authorization header can be obtained from an active session of the account.   |                                        |
| **verificationCodeID** | body     | uuid    | Yes      | You can get it from the email that you wish to verify at the time of its creation. | "1c233e4a-27e7-4b77-8b0d-9a35cf212afe" |
| **code**               | body     | integer | Yes      | This code is sent to your email when it is created.                                | "757564"                               |

To verify an email, use a **PUT** request, for example:

```bash
curl -X 'PUT' \
  'https://intelligence.threatwinds.com/api/auth/v2/email/verification' \
  -H 'accept: application/json' \
  -H 'Content-Type: application/json' \
  -d '{
  "code": "757564",
  "verificationCodeID": "1c233e4a-27e7-4b77-8b0d-9a35cf212afe"
}'
```

### Returns

## Set email as preferred {#setPreferredEmail}

This API endpoint sets an email as preferred

**Endpoint:** https://intelligence.threatwinds.com/api/auth/v2/email/preferred

### Parameters

| Parameter         | Location | Type   | Required | Description                                                                      | Example                              |
|-------------------|----------|--------|----------|----------------------------------------------------------------------------------|--------------------------------------|
| **Authorization** | header   | string | Yes      | This authorization header can be obtained from an active session of the account. |                                      |
| **emailID**       | body     | uuid   | Yes      | The id of the email that you'd like to set as preferred.                         | 5f35d2c4-5633-4b16-bbf0-5ca22ef8ea2e |

To set a preferred email, use a **PUT** request, for example:

```bash
curl -X 'PUT' \
  'https://intelligence.threatwinds.com/api/auth/v2/email/preferred' \
  -H 'accept: application/json' \
  -H 'Authorization: Jlore138gST9TnQKRWZZzLW4NfxCo0q8' \
  -H 'Content-Type: application/json' \
  -d '{
  "emailID": "5f35d2c4-5633-4b16-bbf0-5ca22ef8ea2e"
}'
```

### Returns

A successful response returns a JSON object with a success message:

```json
{
  "message": "acknowledged"
}
```
