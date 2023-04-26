---
layout: default
title: Email
parent: Authentication
nav_order: 2
---

# Email
<nav>
Table of Content:
  <ul>
    <li><a href="#createEmail">Create Email</a></li>
    <li><a href="#deleteEmail">Delete Email</a></li>
    <li><a href="#getEmails">Get Emails</a></li>
    <li><a href="#verifyEmail">Verify Email</a></li>
    <li><a href="#newCodeEmail">New Verification Code</a></li>
    <li><a href="#setPreferredEmail">Set Preferred Email</a></li>
    </ul>
</nav>

## Create Email {#createEmail}
This API endpoint creates an unverified Email and sends a verification code.

**EndPoint:** https://intelligence.threatwinds.com/api/auth/v2/email

### Parameters
<dl>
    <dt><b>Authorization header</b>(<i>string</i>)</dt>
    <dd>
      This authorization header can be obtained from an active session of the
      account. Please ensure that the session is active before attempting to
      retrieve the authorization header for the request.
    </dd>
    <dt><b>Message</b> (<i>JSON</i>)</dt>
    <dd>
        <dl>
        <dt><b>address</b> (<i>string</i>)</dt>
        <dd>
        The email parameter represents a user's
        email address that is going to be associated with their account. This
        email address is used for communication with the user, and may also be
        used as a way to reset their password or verify their account. _e.g.: john@doe.net_
        </dd>
        </dl>
    </dd>
</dl>

To create a email, use a <b class="label label-green">POST</b> request, for example:

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

<h3> <b class="label label-green">Code 202</b> Accepted</h3>

**Return a verification code as Response**

We get this code when the email was created successfully. It returns a verificationCodeID. Remember that you need to verify this email before you can use it.

### Fields of the response

<dl>
  <dt><b> verificationCodeID </b> (<i>string</i>)</dt>
  <dd>
    The verification code id that is used in the Email Verification Endpoint to
    ensure that the email is valid.
    <i>e.g.: 5f35d2c4-5633-4b16-bbf0-5ca22ef8ea2e</i>
  </dd>
</dl>

### Errors

### Fields of the response

<dl>
  <dt><b> uuid </b> (<i>string</i>)</dt>
  <dd>
    The id of the error. <i>e.g.: 6070686d-917d-44bb-ad11-02345a7f1939</i>
  </dd>
  <dt><b> error </b> (<i>string</i>)</dt>
  <dd>
    The description of the error. <i>e.g.: "duplicated key not allowed"</i>
  </dd>
</dl>

<h3><b class="label label-red">Code 400</b>Bad request</h3>

The HTTP status code 400, also known as a "Bad Request" error, is typically returned when the server is unable to process the client's request due to issues with the request parameters. This could happen when the authorization header parameter provided in the request is not valid.

e.g.:

```json
{
  "uuid": "85f0c8a9-d361-49e9-a9d1-826c770fcffb",
  "error": "incorrect authorization header"
}
```
<h3><b class="label label-red">Code 401</b>Authentication required</h3>

The 401 error code indicates that you need authentication to do this request.

<h3><b class="label label-red">Code 404</b>Not found</h3>

The HTTP status code 404, also known as a "Not Found" error occurs when the requested resource, in this case, the user, cannot be found.

There are several reasons why this may occur, such as the user not existing in the system or the session that is being used for authentication no longer being active. It's important to double-check the authorization header before attempting to use it.

For example:

```json
{
  "uuid": "7ae253bc-b1ca-4e8b-a5ca-268645e76f60",
  "error": "record not found"
}
```

<h3><b class="label label-yellow">Code 500</b>Internal server error</h3>
We get this code when an internal server error occurs. This is not user related.

For example:

```json
{
  "uuid": "5e67b7a2-ee10-493d-83da-15ba3cf66bd2",
  "error": "Internal Server Error"
}
```

## Delete Email {#deleteEmail}

This API endpoint deletes an Email.

**EndPoint:** https://intelligence.threatwinds.com/api/auth/v2/email

### Parameters

<dl>
  <dt><b>Authorization header</b>(<i>string</i>)</dt>
  <dd>
    This authorization header can be obtained from an active session of the
    account. Please ensure that the Email is active before attempting to
    retrieve the authorization header for email deletion.
    <i>
      e.g.: Bearer
      fq6JoEFTsxiXAl1cVxPDnK4emIQCwaUBfq6JoEFTsxiXAl1cVxPDnK4emIQCwaUB
    </i>
  </dd>
  <dt><b>Email ID</b>(<i>string</i>)</dt>
  <dd>The id of the Email that you want to delete. <i>e.g.: john@doe.net</i></dd>
</dl>

To delete an email, use a <b class="label label-red">DELETE</b> request, for example:

```bash
curl -X 'DELETE' \
  'https://intelligence.threatwinds.com/api/auth/v2/email/5f35d2c4-5633-4b16-bbf0-5ca22ef8ea2e' \
  -H 'accept: application/json' \
  -H 'Authorization: Bearer 5f35d2c4-5633-4b16-bxf0-5ca32ef8ea2e'
```

### Returns

<h3> <b class="label label-green">Code 202</b> Accepted</h3>

**Returns the message "acknowledged"**

When an email is successfully deleted, the code returns a JSON object with a 'message' field whose value is 'acknowledged'.

### Fields of the response:

<dl>
  <dt><b> message </b> (<i>string</i>)</dt>
  <dd>
    A message that describes the result of the operation. _e.g.:<i>"acknowledged"</i>
  </dd>
</dl>

### Errors

### Fields of the response

<dl>
  <dt><b> uuid </b> (<i>string</i>)</dt>
  <dd>
    The id of the error. <i>e.g.: 6070686d-917d-44bb-ad11-02345a7f1939</i>
  </dd>
  <dt><b> error </b> (<i>string</i>)</dt>
  <dd>
    The description of the error. <i>e.g.: "duplicated key not allowed"</i>
  </dd>
</dl>


<h3><b class="label label-red">Code 400</b>Bad request</h3>

The HTTP status code 404, also known as a "Not Found" error occurs when the requested resource, in this case, the email or the session, cannot be found.

There are several reasons why this may occur, such as:
* the session or the email has expired.
* the session has not been verified.
*  the email does not exist in the system.
<br><br>
  
It's important to double-check the parameters before attempting to delete the email.

For example:

```json
{
  "uuid": "7ae253bc-b1ca-4e8b-a5ca-268645e76f60",
  "error": "record not found"
}
```

<h3><b class="label label-red">Code 400</b>Bad request</h3>

The HTTP status code 400, also known as a "Bad Request" error, is typically returned when the server is unable to process the client's request due to issues with the request parameters.

For example:

```json
{
  "uuid": "85f0c8a9-d361-49e9-a9d1-826c770fcffb",
  "error": "incorrect authorization header"
}
```

<h3><b class="label label-yellow">Code 500</b>Internal server error</h3>
We get this code when an internal server error occurs. This is not email related.

For example:

```json
{
  "uuid": "5e67b7a2-ee10-493d-83da-15ba3cf66bd2",
  "error": "Internal Server Error"
}
```

## Get Emails {#getEmails}

This API endpoint gets the user's emails.<br><br>

**EndPoint:** https://intelligence.threatwinds.com/api/auth/v2/emails

### Parameters

<dl>
  <dt><b>Authorization header</b> (<i>string</i>)</dt>
  <dd>
    This authorization header can be obtained from an active session of the
    account. Please ensure that the session is active before attempting to
    use it.
    <code>
      e.g.: Bearer
      fq6JoEFTsxiXAl1cVxPDnK4emIQCwaUBfq6JoEFTsxiXAl1cVxPDnK4emIQCwaUB
    </code>
  </dd>
</dl>

To get the currents emails, use a <b class="label label-blue">GET</b> request, for example:

```bash
curl -X 'GET' \
  'https://intelligence.threatwinds.com/api/auth/v2/emails' \
  -H 'accept: application/json' \
  -H 'Authorization: Bearer 63VD8JoautQNqRLcqNOlJid02R7CDbWK'
```

### Returns

<h3> <b class="label label-green">Code 202</b> Accepted</h3>

**Returns a list of Emails**

It returns the list of the current user Emails.

### Fields of the response:
<dl>
  <dt><b>address</b> (<em>string</em>):</dt>
  <dd>The address of the email.</dd>
  <dt><b>emailID</b> (<em>string</em>):</dt>
  <dd>The id of the email that was created. _e.g.: 5f35d2c4-5633-4b16-bbf0-5ca22ef8ea2e_</dd>
  <dt><b>preferred</b> (<em>boolean</em>):</dt>
  <dd>A boolean that represents if the email is set as preferred or not.</dd>
  <dt><b>verified</b> (<em>boolean</em>):</dt>
  <dd>A boolean that represents if the email is verified or not.</dd>
</dl>

### Errors

### Fields of the response

<dl>
  <dt><b> uuid </b> (<i>string</i>)</dt>
  <dd>
    The id of the error. <i>e.g.: 6070686d-917d-44bb-ad11-02345a7f1939</i>
  </dd>
  <dt><b> error </b> (<i>string</i>)</dt>
  <dd>
    The description of the error. <i>e.g.: "duplicated key not allowed"</i>
  </dd>
</dl>


<h3><b class="label label-red">Code 400</b>Bad request</h3>

The HTTP status code 400, also known as a "Bad Request" error, is typically returned when the server is unable to process the client's request due to issues with the request parameters.

For example:

```json
{
  "uuid": "85f0c8a9-d361-49e9-a9d1-826c770fcffb",
  "error": "incorrect authorization header"
}
```

<h3><b class="label label-red">Code 401</b>Authentication required</h3>


The 401 error code indicates that you need authentication to do this request.

<h3><b class="label label-red">Code 404</b> Not found</h3>

The HTTP status code 404, also known as a "Not Found" error occurs when the requested resource, in this case the user, cannot be found.

There are several reasons why this may occur, such as the session has expired or has not been verified. It's important to double-check the parameters before attempting to make the request.


For example:

```json
{
  "uuid": "9ca21f08-c3a3-4eeb-8179-49669d7fe1fa",
  "error": "record not found"
}
```
<h3><b class="label label-yellow">Code 500</b> Internal server error</h3>
We get this code when an internal server error occurs. This is not user related.

## Verify Email {#verifyEmail} 

This API endpoint verifies the Email using code sent by email.<br><br>

**EndPoint:** https://intelligence.threatwinds.com/api/auth/v2/email/verification

### Parameters

<dl>
  <dt><b>verificationCodeID</b> <i>(string)</i>:</dt>
  <dd>You can obtain it from the email that you wish to verify at the time of its creation or with the New Verification Code endpoint. <i>e.g.: "1c233e4a-27e7-4b77-8b0d-9a35cf212afe"</i></dd>
  <dt><b>code</b> <i>(string)</i>:</dt>
  <dd>A code is sent to your preferred email when an email is created in the system or with the New Verification Code endpoint.. <i>e.g.: "757564"</i></dd>
</dl>To verify an email, use a<b class="label label-yellow">PUT</b>request, for example:

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

<h3> <b class="label label-green">Code 202</b> Accepted</h3>

**Returns the message "acknowledged"*When an email is successfully verified, the code returns a JSON object with a 'message' field whose value is 'acknowledged'.

### Fields of the response:

<dl>
  <dt><b> message </b> (<i>string</i>)</dt>
  <dd>
    A message that describes the result of the operation. _e.g.:<i>"acknowledged"</i>
  </dd>
</dl>

### Errors

### Fields of the response

<dl>
  <dt><b> uuid </b> (<i>string</i>)</dt>
  <dd>
    The id of the error. <i>e.g.: 6070686d-917d-44bb-ad11-02345a7f1939</i>
  </dd>
  <dt><b> error </b> (<i>string</i>)</dt>
  <dd>
    The description of the error. <i>e.g.: "duplicated key not allowed"</i>
  </dd>
</dl>

<h3><b class="label label-red">Code 404</b>Not found</h3>

The HTTP status code 404, also known as a "Not Found" error occurs when the requested resource, in this case, the verificationCodeID, cannot be found.

There are several reasons why this may occur, such as the session not existing in the system or is not longer active.

For example:

```json
{
  "uuid": "7ae253bc-b1ca-4e8b-a5ca-268645e76f60",
  "error": "record not found"
}
```

<h3><b class="label label-red">Code 400</b>Bad request</h3>

The HTTP status code 400, also known as a "Bad Request" error, is typically returned when the server is unable to process the client's request due to issues with the request parameters.

For example:

```json
{
  "uuid": "52dbba7a-99fd-4e87-999f-d9e1df38b4f4",
  "error": "incorrect authorization header"
}
```
<h3><b class="label label-red">Code 401</b>Unauthorized</h3>

We get this code when the code sent is incorrect.

For example:

```json
{
  "uuid": "39908d33-7953-440f-895c-69423c6f7c71",
  "error": "verification code not found"
}
```
<h3><b class="label label-red">Code 403</b>Forbidden</h3>
A 403 error code, also known as a Forbidden error, indicates that the server understood the client's request, but it is refusing to fulfill it. The server can return a 403 error if it suspects that the client is making too many requests, or if the client is using an IP address that is banned or blocked by the server. If the error persists you should contact support for further assistance.

<h3><b class="label label-yellow">Code 500</b>Internal server error</h3>
We get this code when an internal server error occurs. This is not user related.

For example:

```json
{
  "uuid": "5e67b7a2-ee10-493d-83da-15ba3cf66bd2",
  "error": "Internal Server Error"
}
```

## New Verification Code {#newCodeEmail}

This API endpoint sends a new verification code.

**EndPoint:** https://intelligence.threatwinds.com/api/auth/v2/email/verification

### Parameters
<dl>
  <dt><b>Authorization header</b>(<i>string</i>)</dt>
  <dd>
    This authorization header can be obtained from an active session of the
    account. Please ensure that the session is active before attempting to
    use it.
  </dd>
  <dt><b>Message</b> (<i>JSON</i>)</dt>
  <dd>
      A JSON with the following parameter:<br>
      <b>address</b> (<i>string</i>): The email parameter is the email address that you want to verify and you want to send a new verification code. <i>e.g.: john@doe.net</i>
    </div>
  </dd>
  <br />
</dl>

To send a new verification code, use a <b class="label label-green">POST</b> request, for example:

```bash
curl -X 'POST' \
  'https://intelligence.threatwinds.com/api/auth/v2/email/verification' \
  -H 'accept: application/json' \
  -H 'Authorization: Jlore138gST9TnQKRWZZzLW4NfxCo0q8' \
  -H 'Content-Type: application/json' \
  -d '{
  "address": "john@doe.com"
}'
```

### Returns 

<h3> <b class="label label-green">Code 202</b> Accepted</h3>

**Returns a verificationCodeID.**

Returns a new verificationCodeID and a code is sent to the email. Then you can use both in the Verification Endpoint to verify your email.

### Fields of the response

<dl>
  <dt><b> verificationCodeID </b> (<i>string</i>)</dt>
  <dd>
    The verification code id that is used in the Email Verification Endpoint to
    ensure that the email is valid.
    <i>e.g.: 5f35d2c4-5633-4b16-bbf0-5ca22ef8ea2e</i>
  </dd>
</dl>

### Errors

### Fields of the response

<dl>
  <dt><b> uuid </b> (<i>string</i>)</dt>
  <dd>
    The id of the error. <i>e.g.: 6070686d-917d-44bb-ad11-02345a7f1939</i>
  </dd>
  <dt><b> error </b> (<i>string</i>)</dt>
  <dd>
    The description of the error. <i>e.g.: "duplicated key not allowed"</i>
  </dd>
</dl>

<h3><b class="label label-red">Code 400</b>Bad request</h3>

The HTTP status code 400, also known as a "Bad Request" error, is typically returned when the server is unable to process the client's request due to issues with the request parameters.

For example:

```json
{
  "uuid": "85f0c8a9-d361-49e9-a9d1-826c770fcffb",
  "error": "incorrect authorization header"
}
```
<h3><b class="label label-red">Code 401</b>Authentication required</h3>


The 401 error code indicates that you need authentication to do this request.

<h3><b class="label label-red">Code 403</b>Forbidden</h3>
A 403 error code, also known as a Forbidden error, indicates that the server understood the client's request, but it is refusing to fulfill it. The server can return a 403 error if it suspects that the client is making too many requests, or if the client is using an IP address that is banned or blocked by the server. If the error persists you should contact support for further assistance.

<h3><b class="label label-red">Code 404</b>Not found</h3>

The HTTP status code 404, also known as a "Not Found" error occurs when the requested resource, in this case the email, cannot be found.

There are several reasons why this may occur, such as the email not existing in the system or this has expired. It's important to double-check the authorization header.

For example:

```json
{
  "uuid": "9ca21f08-c3a3-4eeb-8179-49669d7fe1fa",
  "error": "record not found"
}
```
<h3><b class="label label-yellow">Code 500</b>Internal server error</h3>
We get this code when an internal server error occurs. This is not user related.

## Set Preferred Email {#setPreferredEmail}

This API endpoint an email as preferred

**EndPoint:** https://intelligence.threatwinds.com/api/auth/v2/email/preferred

### Parameters

   <dl>
  <dt><b>Authorization header</b> <i>(string)</i></dt>
  <dd>This authorization header can be obtained from an active session of the account. Please ensure that the session is active before attempting to retrieve the authorization header for the request. <i>e.g.:</i></dd>
  <dt><b>Message</b> <i>(Json)</i>:</dt>
  <dd>
    <dl>
      <dt><b>emailID</b> <i>(string)</i></dt>
      <dd>The id of the email that was created. <i>e.g.: 5f35d2c4-5633-4b16-bbf0-5ca22ef8ea2e</i></dd>
    </dl>
  </dd>
</dl>

To set a preferred email, use a <b class="label label-yellow">PUT</b> request, for example:

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

<h3> <b class="label label-green">Code 202</b> Accepted</h3>
**Returns the message "acknowledged"**

When an email is successfully set as preferred, the code returns a JSON object with a 'message' field whose value is 'acknowledged'.


### Errors

### Fields of the response

<dl>
  <dt><b> uuid </b> (<i>string</i>)</dt>
  <dd>
    The id of the error. <i>e.g.: 6070686d-917d-44bb-ad11-02345a7f1939</i>
  </dd>
  <dt><b> error </b> (<i>string</i>)</dt>
  <dd>
    The description of the error. <i>e.g.: "duplicated key not allowed"</i>
  </dd>
</dl>

<h3><b class="label label-red">Code 400</b>Bad request</h3>

The HTTP status code 400, also known as a "Bad Request" error, is typically returned when the server is unable to process the client's request due to issues with the request parameters.

For example:

```json
{
  "uuid": "85f0c8a9-d361-49e9-a9d1-826c770fcffb",
  "error": "incorrect apyKey"
}
```
<h3><b class="label label-red">Code 401</b>Authentication required</h3>


The 401 error code indicates that you need authentication to do this request.

<h3><b class="label label-red">Code 403</b>Forbidden</h3>
A 403 error code, also known as a Forbidden error, indicates that the server understood the client's request, but it is refusing to fulfill it. The server can return a 403 error if it suspects that the client is making too many requests, or if the client is using an IP address that is banned or blocked by the server. If the error persists you should contact support for further assistance.

<h3><b class="label label-red">Code 404</b>Not found</h3>

The HTTP status code 404, also known as a "Not Found" error occurs when the requested resource, in this case, the email, cannot be found.

There are several reasons why this may occur, such as the email not existing in the system or it has expired. It's important to double-check the parameters.

For example:

```json
{
  "uuid": "9ca21f08-c3a3-4eeb-8179-49669d7fe1fa",
  "error": "record not found"
}
```
<h3><b class="label label-yellow">Code 500</b>Internal server error</h3>
We get this code when an internal server error occurs. This is not user related.