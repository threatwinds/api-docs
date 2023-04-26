---
layout: default
title: KeyPair
parent: Authentication
nav_order: 2
---

<h2>KeyPair</h2>

<nav>
Table of Content:
  <ul>
    <li><a href="#createKeyPair">Create KeyPair</a></li>
    <li><a href="#deleteKeyPair">Delete KeyPair</a></li>
    <li><a href="#getKeyPairs">Get KeyPairs</a></li>
    <li><a href="#verifyKeyPair">Verify KeyPair</a></li>
    <li><a href="#checkKeyPair">Check KeyPair</a></li>
    </ul>
</nav>

## Create KeyPair {#createKeyPair}
This API endpoint creates an unverified KeyPair.<br><br>

**EndPoint:** https://intelligence.threatwinds.com/api/auth/v2/keypair

### Parameters
  
<dl>
  <dt><b>Authorization header</b> <i>(string)</i></dt>
  <dd>This authorization header can be obtained from an active session of the account. Please ensure that the session is active before attempting to retrieve the authorization header for Keypair creation. <i>e.g.: </i></dd>
  
  <dt><b>Message</b> <i>(JSON)</i>:</dt>
  <dd>
    <dl>
      <dt><b>days</b> <i>(integer)</i></dt>
      <dd>The 'days' parameter specifies the length of time, in days, that the key pair will remain valid. <i>e.g.: 365</i></dd>
      
      <dt><b>name</b> <i>(string)</i></dt>
      <dd>The 'name' parameter is used to specify a name or identifier for the key pair. <i>e.g.: Example</i></dd>
    </dl>
  </dd>
</dl>


To create a KeyPair, use a <b class="label label-green">POST</b> request, for example:

```bash
curl -X 'POST' \
  'https://intelligence.threatwinds.com/api/auth/v2/keypair' \
  -H 'accept: application/json' \
  -H 'Authorization: Bearer fq6JoEFTsxiXAl1cVdPDnK4emIQCwaUBfq9JoEFTsxhXAl1cVxPDnK4emIQCwaUB' \
  -H 'Content-Type: application/json' \
  -d '{
  "days": 365,
  "name": "Example"
}'
```

### Returns

<h3> <b class="label label-green">Code 202</b> Accepted</h3>

**Return a KeyPair as Response**

We get this code when the KeyPair was created successfully. It returns a KeyPair(apiKey and apiSecret). Remember that you need to verify this KeyPair before you can use it.

### Fields of the response

<dl>
  <dt><b>apiKey</b> <i>(string)</i>:</dt>
  <dd>The apiKey is a unique identifier that, when combined with the apiSecret, serves as a method of authentication for accessing to the endpoints that require authentication. It acts as a public key that identifies the authorized user or system making the API request.</dd>
  
  <dt><b>apiSecret</b> <i>(string)</i></dt>
  <dd>The apiSecret is a piece of confidential information that, when combined with the apiKey, serves as a method of authentication for accessing to the endpoints that require it. Ensure to save the apiSecret in a safe place, this will not be shown again.</dd>
  
  <dt><b>verificationCodeID</b> <i>(string)</i></dt>
  <dd>The verification code id that is used in the <a href="./auth/KeyPair#verifyKeyPair">KeyPair Verification Endpoint</a> to ensure that the KeyPair is valid. <i>e.g.: 5f35d2c4-5633-4b16-bbf0-5ca22ef8ea2e</i></dd>
  
  <dt><b>expireAt</b> <i>(integer)</i></dt>
  <dd>A timestamp that represents the expiration datetime of the KeyPair. <i>e.g.: 1674492894</i></dd>
  
  <dt><b>keyID</b> <i>(string)</i></dt>
  <dd>The id of the KeyPair that was created. <i>e.g.: 5f35d2c4-5633-4b16-bbf0-5ca22ef8ea2e</i></dd>
  
  <dt><b>keyName</b> <i>(string)</i></dt>
  <dd>The name of the KeyPair that was created. <i>e.g.: 5f35d2c4-5633-4b16-bbf0-5ca22ef8ea2e</i></dd>
  
  <dt><b>verified</b> <i>(boolean)</i></dt>
  <dd>A boolean that represents if the KeyPair is verified or not.</dd>
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

```
{
  "uuid": "7ae253bc-b1ca-4e8b-a5ca-268645e76f60",
  "error": "record not found"
}
```

<h3><b class="label label-yellow">Code 500</b>Internal server error</h3>
We get this code when an internal server error occurs. This is not user related.

For example:

```
{
  "uuid": "5e67b7a2-ee10-493d-83da-15ba3cf66bd2",
  "error": "Internal Server Error"
}
```

## Delete KeyPair {#deleteKeyPair}

This API endpoint delte a KeyPair.

**EndPoint:** https://intelligence.threatwinds.com/api/auth/v2/keypair

### Parameters

<dl>
  <dt><b>Authorization header</b>(<i>string</i>)</dt>
  <dd>
    This authorization header can be obtained from an active session of the
    account. Please ensure that the session is active before attempting to
    retrieve the authorization header for KeyPair deletion.
    <code>
      e.g.: Bearer
      fq6JoEFTsxiXAl1cVxPDnK4emIQCwaUBfq6JoEFTsxiXAl1cVxPDnK4emIQCwaUB
    </code>
  </dd>
  <dt><b>Email ID</b>(<i>string</i>)</dt>
  <dd>The id of the Email that you want to delete. <i>e.g.: 5f35d2c4-5633-4b16-bbf0-5ca22ef8ea2e</i></dd>
</dl>
  
To delete a KeyPair, use a <b class="label label-red">DELETE</b> request, for example:

```bash
curl -X 'DELETE' \
  'https://intelligence.threatwinds.com/api/auth/v2/keypair/5f35d2c4-5633-4b16-bbf0-5ca22ef8ea2e' \
  -H 'accept: application/json' \
  -H 'Authorization: Bearer 5f35d2c4-5633-4b16-bxf0-5ca32ef8ea2e'
```

### Returns

<h3> <b class="label label-green">Code 202</b> Accepted</h3>

**Returns the message "acknowledged"**

When a KeyPair is successfully deleted, the code returns a JSON object with a 'message' field whose value is 'acknowledged'.

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
    The description of the error. <i>e.g.: "not found"</i>
  </dd>
</dl>

<h3><b class="label label-red">Code 404</b>Not found</h3>

The HTTP status code 404, also known as a "Not Found" error occurs when the requested resource, in this case, the KeyPair or the session, cannot be found.

There are several reasons why this may occur, such as:
* the session or the keypair has expired.
* the session has not been verified.
*  the keypair does not exist in the system.
<br><br>
  
It's important to double-check the parameters before attempting to delete the KeyPair.

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
We get this code when an internal server error occurs. This is not user related.

For example:

```json
{
  "uuid": "5e67b7a2-ee10-493d-83da-15ba3cf66bd2",
  "error": "Internal Server Error"
}
```

## Get KeyPairs {#getKeyPairs}

This API endpoint to get the user KeyPairs.<br><br>

**EndPoint:** https://intelligence.threatwinds.com/api/auth/v2/keypairs

### Parameters

<dl>
  <dt><b>Authorization header</b>(<i>string</i>)</dt>
  <dd>
    This authorization header can be obtained from an active session of the
    account. Please ensure that the session is active before attempting to
    retrieve the authorization header for the request.
    <code>
      e.g.: Bearer
      fq6JoEFTsxiXAl1cVxPDnK4emIQCwaUBfq6JoEFTsxiXAl1cVxPDnK4emIQCwaUB
    </code>
  </dd>
</dl>

To get the currents KeyPairs, use a <b class="label label-blue">GET</b> request, for example:

```bash
curl -X 'GET' \
  'https://intelligence.threatwinds.com/api/auth/v2/keypairs' \
  -H 'accept: application/json' \
  -H 'Authorization: Bearer 63VD8JoautQNqRLcqNOlJid02R7CDbWK'
```

### Returns

<h3> <b class="label label-green">Code 202</b> Accepted</h3>

**Returns a list of KeyPairs**

It returns the list of the current user KeyPairs.

### Fields of the response:
<dl>
  <dt><b>apiKey</b> <i>(string)</i>:</dt>
  <dd>The apiKey is a unique identifier that, when combined with the apiSecret, serves as a method of authentication for accessing to the endpoints that require authentication. It acts as a public key that identifies the authorized user or system making the API request.</dd>
  <dt><b>expireAt</b> <i>(integer)</i>:</dt>
  <dd>A timestamp that represents the expiration datetime of the KeyPair. <i>e.g.: 1674492894</i></dd>
  <dt><b>keyID</b> <i>(string)</i>:</dt>
  <dd>The id of the KeyPair that was created. <i>e.g.: 5f35d2c4-5633-4b16-bbf0-5ca22ef8ea2e</i></dd>
  <dt><b>keyName</b> <i>(string)</i>:</dt>
  <dd>The name of the KeyPair that was created. <i>e.g.: 5f35d2c4-5633-4b16-bbf0-5ca22ef8ea2e</i></dd>
  <dt><b>verified</b> <i>(boolean)</i>:</dt>
  <dd>A boolean that represents if the KeyPair is verified or not.</dd>
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

<h3><b class="label label-red">Code 404</b>Not found</h3>

The HTTP status code 404, also known as a "Not Found" error occurs when the requested resource, in this case the user, cannot be found.

There are several reasons why this may occur, such as the session has expired or has not been verified. It's important to double-check the parameters before attempting to make the request.


For example:

```json
{
  "uuid": "9ca21f08-c3a3-4eeb-8179-49669d7fe1fa",
  "error": "record not found"
}
```
<h3><b class="label label-yellow">Code 500</b>Internal server error</h3>
We get this code when an internal server error occurs. This is not user related.

## Verify KeyPair {#verifyKeyPair}

This API endpoint verifies the KeyPair using code sent by email.

**EndPoint:** https://intelligence.threatwinds.com/api/auth/v2/KeyPair/verification

### Parameters

<dl>
  <dt><b>verificationCodeID</b> <i>(string)</i>:</dt>
  <dd>You can obtain it from the KeyPair that you wish to verify at the time of its creation or with the New Verification Code endpoint. <i>e.g.: "1c233e4a-27e7-4b77-8b0d-9a35cf212afe"</i></dd>
  <dt><b>code</b> <i>(string)</i>:</dt>
  <dd>A code sent to your preferred email when an email is created in the system. <i>e.g.: "757564"</i></dd>
</dl>

To verify a KeyPair, use a<b class="label label-yellow">PUT</b>request, for example:

```bash
curl -X 'PUT' \
  'https://intelligence.threatwinds.com/api/auth/v2/KeyPair/verification' \
  -H 'accept: application/json' \
  -H 'Content-Type: application/json' \
  -d '{
  "code": "757564",
  "verificationCodeID": "1c233e4a-27e7-4b77-8b0d-9a35cf212afe"
}'
```

### Returns

<h3> <b class="label label-green">Code 202</b> Accepted</h3>

**Returns the message "acknowledged"**

When a KeyPair is successfully verified, the code returns a JSON object with a 'message' field whose value is 'acknowledged'.

### Fields of the response:

<dl>
  <dt><b> message </b> (<i>string</i>)</dt>
  <dd>
    A message that describes the result of the operation.<i>e.g.:"acknowledged"</i>
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
    The description of the error. <i>e.g.: "not found"</i>
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

## Check KeyPair {#checkKeyPair}

This API endpoint check a KeyPair and returns privileges

**EndPoint:** https://intelligence.threatwinds.com/api/auth/v2/KeyPair

### Parameters

<dl>
  <dt><b>Authorization header</b> (<i>string</i>)</dt>
  <dd>This authorization header can be obtained from an active session of the account. Please ensure that the session is active before attempting to retrieve the authorization header for the request. <br> <code>e.g.: Bearer fq6JoEFTsxiXAl1cVxPDnK4emIQCwaUBfq6JoEFTsxiXAl1cVxPDnK4emIQCwaUB</code></dd>
  <dt><b>Message</b> (<i>Json</i>)</dt>
  <dd>
    <dl>
      <dt><b>apiKey</b> (<i>string</i>)</dt>
      <dd>The apiKey is a unique identifier that, when combined with the apiSecret, serves as a method of authentication for accessing to the endpoints that require authentication. You can get it at the time of its creation or when you return the list of yours keyPairs.</dd>
      <dt><b>apiSecret</b> (<i>string</i>)</dt>
      <dd>The apiSecret is a piece of confidential information that, when combined with the apiKey, serves as a method of authentication for accessing to the endpoints that require it. This is only obtained at the time of its creation.</dd>
    </dl>
  </dd>
</dl>

To get the currents KeyPairs, use a <b class="label label-blue">GET</b> request, for example:

```bash
curl -X 'GET' \
  'https://intelligence.threatwinds.com/api/auth/v2/keypair' \
  -H 'accept: application/json' \
  -H 'api-key: 5i3w1FwOAGehi5Ce' \
  -H 'api-secret: HAciZAv0IODXAd2fXYEdZMAZHiOKb3En'
```

### Returns

<h3> <b class="label label-green">Code 202</b> Accepted</h3>
<h5>Returns the message "acknowledged" </h5>

When a KeyPair is successfully retrieved, the code returns a JSON object with info about the KeyPair and the user.

### Fields of the response:
<dl>
  <dt><b>userID</b> <i>(string)</i></dt>
  <dd>The user id owner of the KeyPair. <i>e.g.: "5f35d2c4-5633-4b16-bbf0-5ca22ef8ea2e"</i></dd>
  
  <dt><b>userAlias</b> <i>(string)</i></dt>
  <dd>The user's alias on the platform. <i>e.g.: johny</i></dd>
  
  <dt><b>userName</b> <i>(string)</i></dt>
  <dd>User's full name.</dd>
  
  <dt><b>apiKey</b> <i>(string)</i></dt>
  <dd>The apiKey is a unique identifier that, when combined with the apiSecret, serves as a method of authentication for accessing to the endpoints that require authentication. It acts as a public key that identifies the authorized user or system making the API request.</dd>

  <dt><b>expireAt</b> <i>(integer)</i></dt>
  <dd>A timestamp that represents the expiration datetime of the KeyPair. <i>e.g.: 1674492894</i></dd>
  
  <dt><b>keyID</b> <i>(string)</i></dt>
  <dd>The id of the KeyPair that was created. <i>e.g.: 5f35d2c4-5633-4b16-bbf0-5ca22ef8ea2e</i></dd>
  
  <dt><b>keyName</b> <i>(string)</i></dt>
  <dd>The name of the KeyPair that was created. <i>e.g.: 5f35d2c4-5633-4b16-bbf0-5ca22ef8ea2e</i></dd>
  
  <dt><b>verified</b> <i>(boolean)</i></dt>
  <dd>A boolean that represents if the KeyPair is verified or not.</dd>
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
    The description of the error. <i>e.g.: "not found"</i>
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

The HTTP status code 404, also known as a "Not Found" error occurs when the requested resource, in this case the KeyPair, cannot be found.

There are several reasons why this may occur, such as the KeyPair not existing in the system or this has expired. It's important to double-check the parameters.

For example:

```json
{
  "uuid": "9ca21f08-c3a3-4eeb-8179-49669d7fe1fa",
  "error": "record not found"
}
```
<h3><b class="label label-yellow">Code 500</b>Internal server error</h3>
We get this code when an internal server error occurs. This is not user related.