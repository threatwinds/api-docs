---
layout: default
title: Session
parent: Authentication
nav_order: 2
---

# Session

<nav>
Table of Content:
  <ul>
    <li><a href="#createSession">Create Session</a></li>
    <li><a href="#closeSession">Close Session</a></li>
    <li><a href="#getSessions">Get Sessions</a></li>
    <li><a href="#verifySession">Verify Session</a></li>
    <li><a href="#extendSession">Extend Session</a></li>
    <li><a href="#checkSession">Check Session</a></li>
  </ul>
</nav>

## Create Session {#createSession}
This API endpoint creates an unverified session.

**EndPoint:** https://intelligence.threatwinds.com/api/auth/v2/session

### Parameters
 <dl> 
<dt><b>Message</b> (<i>JSON</i>)</dt>
    <dd>
        <dl>
        <dt><b>email</b> (<i>string</i>)</dt>
        <dd>
       The email parameter represents the session's email address that is associated with the account. <i>e.g.: john@doe.net</i>
        </dd>
        </dl>
    </dd>
</dl>

To create a session, use a <b class="label label-green">POST</b> request, for example:

```bash
curl -X 'POST' \
  'https://intelligence.threatwinds.com/api/auth/v2/session' \
  -H 'accept: application/json' \
  -H 'Content-Type: application/json' \
  -d '{
  "email": "john@doe.net",
}'
```

### Returns

<h3> <b class="label label-green">Code 202</b> Accepted</h3>

**Return a Session as Response**

We get this code when the session was created successfully. It returns a Session. Remember that you need to verify the session before you can use it.

### Fields of the response:

<dl>
  <dt><b>bearer</b> (<i>string</i>)</dt>
  <dd>The bearer (refers to as Authentication Header) is used as a means of authentication for the APIs and must be included in the header of all requests that require authentication. See <a href="/QUICKSTART#auth">Quick Start Auth</a> for a short explanation. The authentication header must be verified using the verificationCodeID in the <a href="./auth/SESSION#verifySession">Session Verification Endpoint</a> before it can be used to ensure that it is valid.
  <i>e.g.: fq6JoEFTsxiXAl1cVxPDnK4emIQCwaUBfq6JoEFTsxiXAl1cVxPDnK4emIQCwaUB
</i>
</dd>
  <dt><b>verificationCodeID</b> (<i>string</i>)</dt>
  <dd>The verification code id that is used in the Session Verification Endpoint to ensure that the session is valid. <i>e.g.: 5f35d2c4-5633-4b16-bbf0-5ca22ef8ea2e</i></dd>
  
  <dt><b>expireAt</b> (<i>integer</i>)</dt>
  <dd>A timestamp that represents the expiration datetime of the session. If you want to extend it, use the Extend Session Endpoint. <i>e.g.:1674492894</i></dd>
  
  <dt><b>ip</b>(<i>string</i>)</dt>
  <dd>The IP from the account and session was created. <i>e.g.: 1.1.1.1</i></dd>
  
  <dt><b>sessionID</b>(<i>string</i>)</dt>
  <dd>The ID of the session that was created. <i>e.g.: 5f35d2c4-5633-4b16-bbf0-5ca22ef8ea2e</i></dd>
  
  <dt><b>sessionAgent</b>(<i>string</i>)</dt>
  <dd>The sessionAgent used to create the session. <i>e.g.: Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/51.0.2704.103 Safari/537.36</i></dd>
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

The HTTP status code 400, also known as a "Bad Request" error, is typically returned when the server is unable to process the client's request due to issues with the request parameters. This could happen when the email parameter provided in the request is not valid.

e.g.:

```
{
  "uuid": "6049f96e-4b32-4e1c-a6a3-7967f463d66d",
  "error": "mail: missing '@' or angle-addr"
}
```

<h3><b class="label label-red">Code 403</b>Forbidden</h3>
A 403 error code, also known as a Forbidden error, indicates that the server understood the client's request, but it is refusing to fulfill it. The server can return a 403 error if it suspects that the client is making too many requests, or if the client is using an IP address that is banned or blocked by the server. If the error persists you should contact support for further assistance.

<h3><b class="label label-red">Code 404</b>Not found</h3>

The HTTP status code 404, also known as a "Not Found" error occurs when the requested resource, in this case, the user, cannot be found.

There are several reasons why this may occur, such as the user not existing in the system or the session is not longer active. It's important to double-check the authorization header before attempting to delete them.

For example:

```
{
  "uuid": "7ae253bc-b1ca-4e8b-a5ca-268645e76f60",
  "error": "not found"
}
```

<h3><b class="label label-yellow">Code 500</b>Internal server error</h3>
We get this code when an internal server error occurs. This is not session related.

For example:

```
{
  "uuid": "5e67b7a2-ee10-493d-83da-15ba3cf66bd2",
  "error": "Internal Server Error"
}
```

## Close session {#closeSession}

This API endpoint closes a session.

**EndPoint:** https://intelligence.threatwinds.com/api/auth/v2/session

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
  <dt><b>Session ID</b>(<i>string</i>)</dt>
  <dd>The id of the session that you want to close. <i>e.g.: 5f35d2c4-5633-4b16-bbf0-5ca22ef8ea2et</i></dd>
</dl>
  
To close a session, use a <b class="label label-red">DELETE</b> request, for example:


```bash
curl -X 'DELETE' \
  'https://intelligence.threatwinds.com/api/auth/v2/session/5f35d2c4-5633-4b16-bbf0-5ca22ef8ea2e' \
  -H 'accept: application/json' \
  -H 'Authorization: Bearer fq6JoEFTsxiXAl1cVxPDnK4emIQCwaUBfq6JoEFTsxiXAl1cVxPDnK4emIQCwaUB'
```

### Returns

<h3> <b class="label label-green">Code 202</b> Accepted</h3>

<h5>Returns the message "acknowledged" </h5>

When a session is successfully closed, the code returns a JSON object with a 'message' field whose value is 'acknowledged'.

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

The HTTP status code 404, also known as a "Not Found" error occurs when the requested resource, in this case, the session, cannot be found.

There are several reasons why this may occur, such as the session not existing in the system or is no longer active. It's important to double-check the parameters before attempting to delete the session.

For example:

```json
{
  "uuid": "7ae253bc-b1ca-4e8b-a5ca-268645e76f60",
  "error": "not found"
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
We get this code when an internal server error occurs. This is not session related.

For example:

```json
{
  "uuid": "5e67b7a2-ee10-493d-83da-15ba3cf66bd2",
  "error": "Internal Server Error"
}
```

## Get sessions {#getSessions}

This API endpoint to get the user sessions.

**EndPoint:** https://intelligence.threatwinds.com/api/auth/v2/sessions

### Parameters

<dl>
  <dt><b>Authorization header</b> (<i>string</i>)</dt>
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

To get the currents sessions, use a <b class="label label-blue">GET</b> request, for example:

```
curl -X 'GET' \
  'https://intelligence.threatwinds.com/api/auth/v2/sessions' \
  -H 'accept: application/json' \
  -H 'Authorization: Bearer fq6JoEFTsxiXAl1cVxPDnK4emIQCwaUBfq6JoEFTsxiXAl1cVxPDnK4emIQCwaUB'
```

### Returns

<h3> <b class="label label-green">Code 202</b> Accepted</h3>

<h5>Returns a list of sessions</h5>

It returns the list of the current user Sessions.

### Fields of the response:

<dl>
  <dt><b>sessionID</b> <i>(string)</i></dt>
  <dd>The id of the session that was created. <i>e.g.: 5f35d2c4-5633-4b16-bbf0-5ca22ef8ea2e</i></dd>
  
  <dt><b>ip</b> <i>(string)</i>:</dt>
  <dd>The IP from the account and session was created. <i>e.g.: 1.1.1.1</i></dd>
  
  <dt><b>userAgent</b> <i>(string)</i></dt>
  <dd>The userAgent used to create the account and session. <i>e.g.: Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/51.0.2704.103 Safari/537.36</i></dd>
  
  <dt><b>expireAt</b> <i>(integer)</i></dt>
  <dd>A timestamp that represents the expiration datetime of the session. If you want to extend it, use the Extend Session Endpoint. <i>e.g.:1674492894</i></dd>
  
  <dt><b>current</b> <i>(boolean)</i></dt>
  <dd>A boolean value that represents the current session in which you are performing the operations.</dd>
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
Authentication required</h5>

The 401 error code indicates that you need authentication to do this request.

<h3><b class="label label-red">Code 403</b>Forbidden</h3>
A 403 error code, also known as a Forbidden error, indicates that the server understood the client's request, but it is refusing to fulfill it. The server can return a 403 error if it suspects that the client is making too many requests, or if the client is using an IP address that is banned or blocked by the server. If the error persists you should contact support for further assistance.

<h3><b class="label label-red">Code 404</b>Not found</h3>

The HTTP status code 404, also known as a "Not Found" error occurs when the requested resource, in this case the session, cannot be found.

There are several reasons why this may occur, such as the session not existing in the system. It's important to double-check the authorization header.

For example:

```json
{
  "uuid": "9ca21f08-c3a3-4eeb-8179-49669d7fe1fa",
  "error": "record not found"
}
```
<h3><b class="label label-yellow">Code 500</b>Internal server error</h3>
We get this code when an internal server error occurs. This is not session related.

## Verify Session {#verifySession}

This API endpoint verifies the Sign-in of a session using code sent by email.
**EndPoint:** <https://intelligence.threatwinds.com/api/auth/v2/session/verification>

### Parameters

<dl>
  <dt><b>verificationCodeID</b> <i>(string)</i>:</dt>
  <dd>You can obtain it from the email that you wish to verify at the time of its creation. <i>e.g.: "1c233e4a-27e7-4b77-8b0d-9a35cf212afe"</i></dd>
  <dt><b>code</b> <i>(string)</i>:</dt>
  <dd>A code sent to your preferred email when an email is created in the system. <i>e.g.: "757564"</i></dd>
</dl>
  
To verify a session, use a<b class="label label-yellow">PUT</b>, for example:

```bash
curl -X 'PUT' \
  'https://intelligence.threatwinds.com/api/auth/v2/session/verification' \
  -H 'accept: application/json' \
  -H 'Content-Type: application/json' \
  -d '{
  "code": "757564",
  "verificationCodeID": "1c233e4a-27e7-4b77-8b0d-9a35cf212afe"
}'
```

### Returns

<h3> <b class="label label-green">Code 202</b> Accepted</h3>

<h5>Returns the message "acknowledged" </h5>

When a session is successfully verified, the code returns a JSON object with a 'message' field whose value is 'acknowledged'.

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

The HTTP status code 404, also known as a "Not Found" error occurs when the requested resource, in this case, the verificationCodeID , cannot be found.

There are several reasons why this may occur, such as the session not existing in the system or is no longer active.

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
  "uuid": "4b990113-7215-4b4c-940a-c31ddbc4ccba",
  "error": "invalid UUID length: 35"
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
We get this code when an internal server error occurs. This is not session related.

For example:

```json
{
  "uuid": "5e67b7a2-ee10-493d-83da-15ba3cf66bd2",
  "error": "Internal Server Error"
}
```

## Extend session {#extendSession}

This API endpoint extends the life of the current user sessions.

**EndPoint:** https://intelligence.threatwinds.com/api/auth/v2/session/extend

### Parameters

<dl>
  <dt>Authorization Header:</dt>
  <dd>The authorization header of the session that you want to extend. Please ensure that the session is active before attempting to retrieve the authorization header for extending <i>e.g.: Bearer fq6JoEFTsxiXAl1cVxPDnK4emIQCwaUBfq6JoEFTsxiXAl1cVxPDnK4emIQCwaUB</i>
  </dd>
</dl>

To get the currents sessions, use a <b class="label label-yellow">PUT</b> request, for example:

```
curl -X 'PUT' \
  'https://intelligence.threatwinds.com/api/auth/v2/session/extend' \
  -H 'accept: application/json' \
  -H 'Authorization: Bearer fq6JoEFTsxiXAl1cVxPDnK4emIQCwaUBfq6JoEFTsxiXAl1cVxPDnK4emIQCwaUB'
```

### Returns

<h3> <b class="label label-green">Code 202</b> Accepted</h3>
<h5>Returns the message "acknowledged" </h5>

When a session is successfully verified, the code returns a JSON object with a 'message' field whose value is 'acknowledged'.

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
Authentication required</h5>

The 401 error code indicates that you need authentication to do this request.

<h3><b class="label label-red">Code 403</b>Forbidden</h3>
A 403 error code, also known as a Forbidden error, indicates that the server understood the client's request, but it is refusing to fulfill it. The server can return a 403 error if it suspects that the client is making too many requests, or if the client is using an IP address that is banned or blocked by the server. If the error persists you should contact support for further assistance.

<h3><b class="label label-red">Code 404</b>Not found</h3>

The HTTP status code 404, also known as a "Not Found" error occurs when the requested resource, in this case the session, cannot be found.

There are several reasons why this may occur, such as the session not existing in the system or this has expired. It's important to double-check the authorization header.

For example:

```json
{
  "uuid": "9ca21f08-c3a3-4eeb-8179-49669d7fe1fa",
  "error": "record not found"
}
```
<h3><b class="label label-yellow">Code 500</b>Internal server error</h3>
We get this code when an internal server error occurs. This is not session related.

## Check session {#checkSession}

This API endpoint checks the user's session and returns their privileges.

**EndPoint:** https://intelligence.threatwinds.com/api/auth/v2/session

### Parameters

<dl>
  <dt><b>Authorization header</b> (<i>string</i>)</dt>
  <dd>
The authorization header of the session that you want to check. Please ensure that the session is active before attempting to check it._e.g.:
 <code>
      e.g.: Bearer
      fq6JoEFTsxiXAl1cVxPDnK4emIQCwaUBfq6JoEFTsxiXAl1cVxPDnK4emIQCwaUB
    </code>
  </dd>
</dl>

To get the currents sessions, use a <b class="label label-blue">GET</b> request, for example:

```bash
curl -X 'GET' \
  'https://intelligence.threatwinds.com/api/auth/v2/session' \
  -H 'accept: application/json' \
  -H 'Authorization: Bearer fq6JoEFTsxiXAl1cVxPDnK4emIQCwaUBfq6JoEFTsxiXAl1cVxPDnK4emIQCwaUB'
```

### Returns

<h3> <b class="label label-green">Code 202</b> Accepted</h3>
<h5>Returns the message "acknowledged" </h5>

When a session is successfully retrieved, the code returns a JSON object with info about the session.

>Fields of the response:
<dl>
  <dt><b>alias</b> <i>(string)</i></dt>
  <dd>The user's alias on the platform. <i>e.g.: johny</i></dd>
  
  <dt><b>fullName</b> <i>(string)</i></dt>
  <dd>The user's full name.</dd>
  
  <dt><b>sessionID</b> <i>(string)</i></dt>
  <dd>The id of the session that was created. <i>e.g.: 5f35d2c4-5633-4b16-bbf0-5ca22ef8ea2e</i></dd>
  
  <dt><b>verify</b> <i>(boolean)</i></dt>
  <dd>A boolean that represents if the user is verified or not.</dd>
  
  <dt><b>roles</b> <i>(list [string])</i></dt>
  <dd>A list of the user's roles. <i>e.g.: ["user", "admin"]</i></dd>
  
  <dt><b>userID</b> <i>(string)</i></dt>
  <dd>The user id. <i>e.g.: "5f35d2c4-5633-4b16-bbf0-5ca22ef8ea2e"</i></dd>
  
  <dt><b>userAgent</b> <i>(string)</i></dt>
  <dd>The userAgent used to create the account and session. <i>e.g.: Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/51.0.2704.103 Safari/537.36</i></dd>
  
  <dt><b>ip</b> <i>(string)</i></dt>
  <dd>The ip from the account and session was created. <i>e.g.: 1.1.1.1</i></dd>
  
  <dt><b>expireAt</b> <i>(integer)</i></dt>
  <dd>A timestamp that represents the expiration datetime of the session. If you want to extend it, use the <a href="./AUTENTICATIONAPI.md">Extend Session Endpoint</a>. <i>e.g.:1674492894</i></dd>
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
  "error": "incorrect authorization header"
}
```
<h3><b class="label label-red">Code 401</b>Authentication required</h3>
Authentication required</h5>

The 401 error code indicates that you need authentication to do this request. See <a> Authentication API Page</a> for more details.

For example:

```json
{
  "uuid": "4b2561eb-2b4b-43ee-a272-643df36e2c5b",
  "error": "authentication required"
}
```

<h3><b class="label label-red">Code 403</b>Forbidden</h3>
A 403 error code, also known as a Forbidden error, indicates that the server understood the client's request, but it is refusing to fulfill it. The server can return a 403 error if you do not have enough permissions to make this operation, it suspects that the client is making too many requests, or if the client is using an IP address that is banned or blocked by the server. If the error persists you should contact support for further assistance.

For example:
```json
{
  "uuid": "75827622-bb05-46b5-80a9-344a6df0e001",
  "error": "unauthorized"
}
```

<h3><b class="label label-red">Code 404</b>Not found</h3>

The HTTP status code 404, also known as a "Not Found" error occurs when the requested resource, in this case the session, cannot be found.

There are several reasons why this may occur, such as the session not existing in the system or this has expired. It's important to double-check the authorization header.

For example:

```json
{
  "uuid": "9ca21f08-c3a3-4eeb-8179-49669d7fe1fa",
  "error": "record not found"
}
```
<h3><b class="label label-yellow">Code 500</b>Internal server error</h3>
We get this code when an internal server error occurs. This is not session related.