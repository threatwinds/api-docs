---
layout: default
title: User
parent: Authentication
nav_order: 2
---

# User

## Create User
This API endpoint creates a new user and an unverified session.

**EndPoint:** https://intelligence.threatwinds.com/api/auth/v2/user

### Parameters

<tl>
  <tr>
    <td><b>alias</b> (<i>string</i>) <i>unique</i></td>
    <dd>The alias parameter represents the user's alias on the platform. The alias can contain letters, numbers, and special characters. <i>e.g.: johny</i></dd>
  </tr>
  <tr>
    <td><b>email</b> (<i>string</i>) <i>unique</i></td>
    <dd>The email parameter represents the user's email address that is going to be associated with their account. This email address is used for communication with the user, and may also be used as a way to reset their password or verify their account. <i>e.g.: john@doe.net</i></dd>
  </tr>
  <tr>
    <td><b>fullName</b> (<i>string</i>)</td>
    <dd>The user's full name. This name may be used as a way to verify the user. <i>e.g.: John Doe</i></dd>
  </tr>
</tl>


To create a user, use a <b class="label label-green">POST</b> request, for example:

```bash
curl -X 'POST' \
  'https://intelligence.threatwinds.com/api/auth/v2/user' \
  -H 'accept: application/json' \
  -H 'Content-Type: application/json' \
  -d '{
  "alias": "johny",
  "email": "john@doe.net",
  "fullName": "John Doe"
}'
```

### Returns

<h3> <b class="label label-green">Code 202</b> Accepted</h3>

**Return a Session as Response**

We get this code when the user was created successfully. It returns a <a href="./SESSION">Session</a>.

### Fields of the response:

<dl>
  <dt><b>bearer</b> (<i>string</i>):</dt>
  <dd>The bearer (Refers from now on as Authentication Header) is used as a means of authentication for the APIs, and must be included in the header of all requests that require authentication. The authentication header must be verified using the <i>verificationCodeID</i> in the <a href="./SESSION#verifySession">Session Verification Endpoint</a> before it can be used to ensure that it is valid. <br>
      <code>e.g.: fq6JoEFTsxiXAl1cVxPDnK4emIQCwaUBfq6JoEFTsxiXAl1cVxPDnK4emIQCwaUB</code>
  </dd>
  
  <dt><b>verificationCodeID</b> (<i>string</i>):</dt>
  <dd>The verification code id that is used in the <a href="./SESSION#verifySession">Session Verification Endpoint</a> to ensure that the session is valid. <br>
      <code>e.g.: 5f35d2c4-5633-4b16-bbf0-5ca22ef8ea2e</code>
  </dd>
  
  <dt><b>expireAt</b> (<i>integer</i>):</dt>
  <dd>A timestamp that represents the expiration datetime of the session. If you want to extend it, use the <a href="./SESSION#extendSession">Extend Session Endpoint</a>. <br>
      <code>e.g.: 1674492894</code>
  </dd>
  
  <dt><b>ip</b> (<i>string</i>):</dt>
  <dd>The ip from the account and session was created. <br>
      <code>e.g.: 1.1.1.1</code>
  </dd>
  
  <dt><b>sessionID</b> (<i>string</i>):</dt>
  <dd>The id of the session that was created. <br>
      <code>e.g.: 5f35d2c4-5633-4b16-bbf0-5ca22ef8ea2e</code>
  </dd>
  
  <dt><b>userAgent</b> (<i>string</i>):</dt>
  <dd>The userAgent used to create the account and session. <br>
      <code>e.g.: Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/51.0.2704.103 Safari/537.36</code>
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

The HTTP status code 400, also known as a "Bad Request" error, is typically returned when the server is unable to process the client's request due to issues with the request parameters. This could happen when a parameter that must be unique, such as an email or an alias, already exists in the system. Another reason for this error could be that the email or alias provided in the request are not valid.

e.g.:

```
{
  "uuid": "5e67b7a2-ee10-493d-83da-15ba3cf66bd2",
  "error": "duplicated key not allowed"
}
```

> <h5> Code 409 - Conflict</h5>
A 409 error, also known as a "Conflict" error, is an HTTP status code that indicates that the client's request conflicts with the current state of the server. In general, a 409 error occurs when there is a conflict between the current state of the resource and the changes that the client is attempting to make.

<h3><b class="label label-yellow">Code 500</b>Internal server error</h3>
We get this code when an internal server error occurs. This is not user related.

For example:

```
{
  "uuid": "5e67b7a2-ee10-493d-83da-15ba3cf66bd2",
  "error": "Internal Server Error"
}
```

<h3>Get user by id</h3>

This API endpoint retrieves a user with a given id.<br><br>

**EndPoint:** https://intelligence.threatwinds.com/api/auth/v2/user/id/{id}

### Parameters

<dl>
  <dt><b>User id</b>(<i>string</i>)</dt>
  <dd>
    The unique identifier of the user for which to retrieve information.
  </dd>
</dl>
  
To get an user, use a <b class="label label-blue">GET</b> request, for example:

```bash
curl -X 'GET' \
  'https://intelligence.threatwinds.com/api/auth/v2/user/id/2e4ac29c-bf32-47b0-de0d-67ec870b1277' \
  -H 'accept: application/json'
```

### Returns

<h3> <b class="label label-green">Code 202</b> Accepted</h3>

**The API will respond with a JSON object containing the user's full name and alias.**

We get this code when the user was found successfully.

### Fields of the response:


<tl>
  <tr>
    <td><b>alias</b> (<i>string</i>)</td>
    <dd>The alias parameter represents the user's alias on the platform. The alias can contain letters, numbers, and special characters. <i>e.g.: johny</i></dd>
  </tr>
  <tr>
    <td><b>fullName</b> (<i>string</i>)</td>
    <dd>The user's full name. This name may be used as a way to verify the user. <i>e.g.: John Doe</i></dd>
  </tr>
</tl>

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

The HTTP status code 404, also known as a "Not Found" error occurs when the requested resource, in this case the user, cannot be found.

There are several reasons why this may occur, such as the user not existing in the system or a the session is not longer active. It's important to double-check the authorization header before attempting to delete them.

For example:

```
{
  "uuid": "7ae253bc-b1ca-4e8b-a5ca-268645e76f60",
  "error": "not found"
}
```

<h3><b class="label label-red">Code 400</b>Bad request</h3>

The HTTP status code 400, also known as a "Bad Request" error, is typically returned when the server is unable to process the client's request due to issues with the request parameters.

For example:

```
{
  "uuid": "85f0c8a9-d361-49e9-a9d1-826c770fcffb",
  "error": "incorrect authorization header"
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
