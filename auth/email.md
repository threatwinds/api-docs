---
layout: default
title: Email
parent: Authentication
nav_order: 2
permalink: /auth/email
---

# Email

Table of Content:
  
[Create Email](#createEmail)

[Delete Email](#deleteEmail)

[Get Emails](#getEmails)

[Verify Email](#verifyEmail)

[Set Preferred Email](#setPreferredEmail)
  
## Create email
This API endpoint creates an unverified Email and sends a verification code.

**Endpoint:** https://intelligence.threatwinds.com/api/auth/v2/email

### Parameters
* **Authorization** header _string_  
This Authorization header can be obtained from an active session of the account. Please ensure that the session is active before attempting to retrieve the authorization header for the request.
    
* **Address** body _string_  
The email parameter represents a user's email address that is going to be associated with their account. This email address is used for communication with the user, and may also be used as a way to reset their password or verify their account. _e.g.: john@doe.net_
   
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

> To get information about responses and error codes please refer to the [Restful API definition](https://intelligence.threatwinds.com/api/auth/v2/swagger/index.html).

## Delete email

This API endpoint deletes an Email.

**Endpoint:** https://intelligence.threatwinds.com/api/auth/v2/email/:id

### Parameters

* **Authorization** header _string_  
This authorization header can be obtained from an active session of the account.

* **ID** path _uuid_  
The id of the email that you want to delete. e.g.: 5f35d2c4-5633-4b16-bbf0-5ca22ef8ea2e

To delete an email, use a **DELETE** request, for example:

```bash
curl -X 'DELETE' \
  'https://intelligence.threatwinds.com/api/auth/v2/email/5f35d2c4-5633-4b16-bbf0-5ca22ef8ea2e' \
  -H 'accept: application/json' \
  -H 'Authorization: Bearer 5f35d2c4-5633-4b16-bxf0-5ca32ef8ea2e'
```

### Returns

> To get information about responses and error codes please refer to the [Restful API definition](https://intelligence.threatwinds.com/api/auth/v2/swagger/index.html).

## Get emails

This API endpoint gets the user's emails.

**Endpoint:** https://intelligence.threatwinds.com/api/auth/v2/emails

### Parameters

* **Authorization** header _string_
  This authorization header can be obtained from an active session of the account.

To get the currents emails, use a **GET** request, for example:

```bash
curl -X 'GET' \
  'https://intelligence.threatwinds.com/api/auth/v2/emails' \
  -H 'accept: application/json' \
  -H 'Authorization: Bearer 63VD8JoautQNqRLcqNOlJid02R7CDbWK'
```

### Returns

> To get information about responses and error codes please refer to the [Restful API definition](https://intelligence.threatwinds.com/api/auth/v2/swagger/index.html).

## Verify email

This API endpoint verifies the email using code sent by email.

**Endpoint:** https://intelligence.threatwinds.com/api/auth/v2/email/verification

### Parameters

* **Authorization** header _string_  
This authorization header can be obtained from an active session of the account.

* **verificationCodeID** body _uuid_  
You can obtain it from the email that you wish to verify at the time of its creation. e.g.: "1c233e4a-27e7-4b77-8b0d-9a35cf212afe"

* **code** body _integer_  
This code is sent to your email when it is created. e.g.: "757564"

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

> To get information about responses and error codes please refer to the [Restful API definition](https://intelligence.threatwinds.com/api/auth/v2/swagger/index.html).

## Set email as preferred

This API endpoint sets an email as preferred

**Endpoint:** https://intelligence.threatwinds.com/api/auth/v2/email/preferred

### Parameters

* **Authorization** header _string_  
  This authorization header can be obtained from an active session of the account.

* **emailID** body _uuid_  
The id of the email that would like to set as preferred. e.g.: 5f35d2c4-5633-4b16-bbf0-5ca22ef8ea2e

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

> To get information about responses and error codes please refer to the [Restful API definition](https://intelligence.threatwinds.com/api/auth/v2/swagger/index.html).