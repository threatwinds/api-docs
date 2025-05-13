---
layout: default
title: Key Pairs
parent: Authentication
nav_order: 3
permalink: /auth/keypair
---

# Key Pairs

Table of Content:

* [Create key pair](#createKeyPair)
* [Check key pair](#checkKeyPair)
* [Delete key pair](#deleteKeyPair)
* [Get key pairs](#getKeyPairs)
* [Verify key pair](#verifyKeyPair)

## Create a key pair {#createKeyPair}
This API endpoint creates a new API key pair and returns the key, secret, and verification code ID.

**Endpoint:** https://intelligence.threatwinds.com/api/auth/v2/keypair

### Parameters
* **Authorization** header _string_  
This Authorization header can be obtained from an active session of the account. Please ensure that the session is active before attempting to retrieve the authorization header for the request.

* **name** body _string_  
A descriptive name for the key pair to help you identify it later. _e.g.: "My Application Key"_

* **days** body _integer_  
The number of days until the key pair expires. _e.g.: 365_

To create a key pair, use a **POST** request, for example:

```bash
curl -X 'POST' \
  'https://intelligence.threatwinds.com/api/auth/v2/keypair' \
  -H 'accept: application/json' \
  -H 'Authorization: Bearer fq6JoEFTsxiXAl1cVdPDnK4emIQCwaUBfq9JoEFTsxhXAl1cVxPDnK4emIQCwaUB' \
  -H 'Content-Type: application/json' \
  -d '{
  "name": "Example",
  "days": 365
}'
```

### Returns

A successful response will return a JSON object containing the API key, API secret, key ID, and verification code ID:

```json
{
  "apiKey": "fq6JoEFTsxiXAl1cVxPDnK4emIQCwaUB",
  "apiSecret": "fq6JoEFTsxiXAl1cVxPDnK4emIQCwaUBfq6JoEFTsxiXAl1cVxPDnK4emIQCwaUB",
  "keyID": "5f35d2c4-5633-4b16-bbf0-5ca22ef8ea2e",
  "keyName": "Example",
  "expireAt": 1674492894,
  "verified": false,
  "verificationCodeID": "5f35d2c4-5633-4b16-bbf0-5ca22ef8ea2e"
}
```

> **Important**: Store the API secret securely. It will not be displayed again.

## Check a key pair {#checkKeyPair}

This API endpoint checks a key pair and returns its privileges.

**Endpoint:** https://intelligence.threatwinds.com/api/auth/v2/keypair

### Parameters

* **api-key** header _string_  
Your API key.

* **api-secret** header _string_  
Your API secret.

To check a key pair, use a **GET** request, for example:

```bash
curl -X 'GET' \
  'https://intelligence.threatwinds.com/api/auth/v2/keypair' \
  -H 'accept: application/json' \
  -H 'api-key: fq6JoEFTsxiXAl1cVxPDnK4emIQCwaUB' \
  -H 'api-secret: fq6JoEFTsxiXAl1cVxPDnK4emIQCwaUBfq6JoEFTsxiXAl1cVxPDnK4emIQCwaUB'
```

### Returns

A successful response will return a JSON object containing information about the key pair and its associated privileges:

```json
{
  "apiKey": "fq6JoEFTsxiXAl1cVxPDnK4emIQCwaUB",
  "keyID": "5f35d2c4-5633-4b16-bbf0-5ca22ef8ea2e",
  "keyName": "Example",
  "expireAt": 1674492894,
  "verified": true,
  "userID": "5f35d2c4-5633-4b16-bbf0-5ca22ef8ea2e",
  "userName": "John Doe",
  "userAlias": "johny",
  "roles": ["user", "admin"],
  "groups": ["public"]
}
```

## Delete a key pair {#deleteKeyPair}

This API endpoint deletes a key pair.

**Endpoint:** https://intelligence.threatwinds.com/api/auth/v2/keypair/{id}

### Parameters

* **Authorization** header _string_  
This authorization header can be obtained from an active session of the account.

* **id** path _string_  
The ID of the key pair to delete. e.g.: 5f35d2c4-5633-4b16-bbf0-5ca22ef8ea2e

To delete a key pair, use a **DELETE** request, for example:

```bash
curl -X 'DELETE' \
  'https://intelligence.threatwinds.com/api/auth/v2/keypair/5f35d2c4-5633-4b16-bbf0-5ca22ef8ea2e' \
  -H 'accept: application/json' \
  -H 'Authorization: Bearer fq6JoEFTsxiXAl1cVdPDnK4emIQCwaUBfq9JoEFTsxhXAl1cVxPDnK4emIQCwaUB'
```

### Returns

A successful response will return a JSON object with a success message:

```json
{
  "message": "acknowledged"
}
```

## Get key pairs {#getKeyPairs}

This API endpoint gets all key pairs for the current user.

**Endpoint:** https://intelligence.threatwinds.com/api/auth/v2/keypairs

### Parameters

* **Authorization** header _string_  
This authorization header can be obtained from an active session of the account.

To get all key pairs, use a **GET** request, for example:

```bash
curl -X 'GET' \
  'https://intelligence.threatwinds.com/api/auth/v2/keypairs' \
  -H 'accept: application/json' \
  -H 'Authorization: Bearer fq6JoEFTsxiXAl1cVdPDnK4emIQCwaUBfq9JoEFTsxhXAl1cVxPDnK4emIQCwaUB'
```

### Returns

A successful response will return a JSON object containing an array of key pairs:

```json
{
  "keys": [
    {
      "apiKey": "fq6JoEFTsxiXAl1cVxPDnK4emIQCwaUB",
      "keyID": "5f35d2c4-5633-4b16-bbf0-5ca22ef8ea2e",
      "keyName": "Example",
      "expireAt": 1674492894,
      "verified": true
    },
    {
      "apiKey": "aB7cD8eFgH9iJ0kL1mN2oP3qR4sT5uV6",
      "keyID": "6a2b4c5d-6e7f-8g9h-0i1j-2k3l4m5n6o7p",
      "keyName": "Another Key",
      "expireAt": 1689492894,
      "verified": false
    }
  ]
}
```

## Verify key pair {#verifyKeyPair}

This API endpoint verifies a key pair using a verification code.

**Endpoint:** https://intelligence.threatwinds.com/api/auth/v2/keypair/verification

### Parameters

* **Authorization** header _string_  
This authorization header can be obtained from an active session of the account.

* **verificationCodeID** body _string_  
The verification code ID received when creating the key pair. e.g.: "5f35d2c4-5633-4b16-bbf0-5ca22ef8ea2e"

* **code** body _string_  
The verification code sent to your email. e.g.: "654321"

To verify a key pair, use a **PUT** request, for example:

```bash
curl -X 'PUT' \
  'https://intelligence.threatwinds.com/api/auth/v2/keypair/verification' \
  -H 'accept: application/json' \
  -H 'Authorization: Bearer fq6JoEFTsxiXAl1cVdPDnK4emIQCwaUBfq9JoEFTsxhXAl1cVxPDnK4emIQCwaUB' \
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
