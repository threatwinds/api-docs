---
layout: default
title: Entity
parent: Search
nav_order: 2
---

# Get Entity by type and value {#entityTypeValue}
This API endpoint retrieves the last entity that matches the given type and value. It's useful for fast searching when you need to determine if an entity exists or not.
<br><br>

**EndPoint:** https://intelligence.threatwinds.com/api/search/v1/entity

### Parameters
  
<dl>
  <dt><b>Autentication</b></dt>
  <dd>Authentication can be via Authorization header or Key-Pair. See the See <a href="/QUICKSTART#auth">Quick Start Auth</a> for a short explanation.<dl>
  <dt><b>Authorization</b> (<i>string</i>)</dt>
  <dd>The authorization header of the session if it is your authentication method.<br>
      <code>e.g.: Bearer fq6JoEFTsxiXAl1cVxPDnK4emIQCwaUBfq6JoEFTsxiXAl1cVxPDnK4emIQCwaUB</code></dd>
  <dt><b>api-key</b> (<i>string</i>)</dt>
  <dd>It needs to be combined with the api-secret. You can get it from the <a href="./auth/KeyPair">Key Pair Endpoints</a>.</dd>
  <dt><b>api-secret</b> (<i>string</i>)</dt>
  <dd>It needs to be combined with the api-key. You can get it from the <a href="./auth/KeyPair">Key Pair Endpoints</a>.</dd>
  </dl>
  </dd>
<dt><b>Message</b>(<i>JSON</i>)</dt>
  <dd>
  <dl>
  <dt><b>type</b> (<i>string</i>)</dt>
  <dd>The type of entity that you are looking for. _e.g.: ip, malware, file, md5_. (To see all the types of entities, check the <a href="../DEFINITIONS">Definition Page</a></dd>.
  <dt><b>value</b> (<i>string</i>)</dt>
  <dd>The value of the entity that you are looking for. <code>e.g.: "138.219.198.146", "Doc Dropper Agent", "a7ffc6f8bf1ed76651c14756a061d662f580ff4de43b49fa82d80a4b80f8434a", "161f923323b9c24f8ffc4db9b2f0d4d3".</code></dd>
  </dl>
  </dd>
</dl>


To get an entity by type and value, use a <b class="label label-green">POST</b> request, for example:

```bash
curl -X 'POST' \
  'https://intelligence.threatwinds.com/api/search/v1/entity' \
  -H 'accept: application/json' \
  -H 'api-key: fq6JoEFTsxiXAl1cVxPDnK4emIQCwaUB' \
  -H 'api-secret: fq6JoEFTsxiXAl1cVxPDnK4emIQCwaUBfq6JoEFTsxiXAl1cVxPDnK4emIQCwaUB' \
  -H 'Content-Type: application/json' \
  -d '{
  "type": "malware",
  "value": "Doc Dropper Agent"
}'
```

### Returns

<h3> <b class="label label-green">Code 202</b> Accepted</h3>

**Return an Entity as Respons**

The API returns this code when an entity is found successfully. It returns the entire entity.

### Fields of the response:

<dl>
  <dt><b>id</b> (<i>string</i>)</dt>
  <dd>The ID of the returned entity.</dd>
  <dt><b>type</b> (<i>string</i>)</dt>
  <dd>The type of the entity.</dd>
  <dt><b>reputation</b> (<i>int</i>)</dt>
  <dd>A number that represents the reputation of the entity on a scale of -3 to 3. The values are:
    <ul>
      <li>3 excellent</li>
      <li>2 good</li>
      <li>1 normal</li>
      <li>0 neutral</li>
      <li>-1 poor</li>
      <li>-2 bad</li>
      <li>-3 extremely bad</li>
    </ul>
  </dd>
  <dt><b>accuracy</b> (<i>string</i>)</dt>
  <dd>The accuracy of the match.</dd>
  <dt><b>attributes</b> (<i>JSON</i>)</dt>
  <dd>A list of attributes of the entity. It depends on the type of entity. To see the list of attributes, check the <a href="../DEFINITIONS">Definition Page</a>.</dd>
</dl>

_e.g.:_
```json
{
  "@timestamp": "2023-04-13T06:27:19.357776778Z",
  "accuracy": 1,
  "attributes": {
    "malware": "doc dropper agent",
    "malware-family": "doc",
    "malware-type": "dropper"
  },
  "id": "malware-354f478ccf22fdb49d2c4fd1e112bd05960bd9976f9709cbb46a7b04297d2b5a",
  "reputation": -3,
  "tags": null,
  "type": "malware"
}
```

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
  "uuid": "cf096e86-347f-4ddf-af1c-0c673a004211",
  "error": "value cannot be empty"
}
```
<h3><b class="label label-red">Code 401</b>Authentication required</h3>

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

The HTTP status code 404, also known as a "Not Found" error, occurs when the search could not find a match for the given parameters.

For example:

```json
{
  "uuid": "e23611e9-2974-4cd7-a08b-47fe7d35fa9a",
  "error": "entity not found"
}
```
<h3><b class="label label-yellow">Code 500</b>Internal server error</h3>
We get this code when an internal server error occurs. This is not session related.

## Get Entity by id {#entityById}
This API endpoint retrieves the entity that matches the given id.
<br><br>

**EndPoint:** <https://intelligence.threatwinds.com/api/search/v1/entity/{id}>

### Parameters
  
<dl>
  <dt><b>Autentication</b></dt>
  <dd>Authentication can be via Authorization header or Key-Pair. See the See <a href="/QUICKSTART#auth">Quick Start Auth</a> for a short explanation.<dl>
  <dt><b>Authorization</b> (<i>string</i>)</dt>
  <dd>The authorization header of the session if it is your authentication method.<br>
      <code>e.g.: Bearer fq6JoEFTsxiXAl1cVxPDnK4emIQCwaUBfq6JoEFTsxiXAl1cVxPDnK4emIQCwaUB</code></dd>
  <dt><b>api-key</b> (<i>string</i>)</dt>
  <dd>It needs to be combined with the api-secret. You can get it from the <a href="./auth/KeyPair">Key Pair Endpoints</a>.</dd>
  <dt><b>api-secret</b> (<i>string</i>)</dt>
  <dd>It needs to be combined with the api-key. You can get it from the <a href="./auth/KeyPair">Key Pair Endpoints</a>.</dd>
  </dl>
  </dd>
  <dt><b>entityID</b> (<i>string</i>)</dt>
  <dd>The ID of the entity for which you want to retrieve information.</dd>
</dl>




To create a session, use a <b class="label label-blue">GET</b> request, for example:

```bash
curl -X 'POST' \
  'https://intelligence.threatwinds.com/api/search/v1/entity/malware-784136848fb31fe5fadf1f5ed8af40737720be2a85124583c18495d6bcf188e3' \
  -H 'accept: application/json' \
  -H 'api-key: fq6JoEFTsxiXAl1cVxPDnK4emIQCwaUB' \
  -H 'api-secret: fq6JoEFTsxiXAl1cVxPDnK4emIQCwaUBfq6JoEFTsxiXAl1cVxPDnK4emIQCwaUB' 
```

### Returns

> <h5>Code 200</h5>

**Return an Entity as Response**

The API returns this code when an entity is found successfully. It returns the entire entity.

### Fields of the response:

<dl>
  <dt><b>id</b> (<i>string</i>)</dt>
  <dd>The ID of the returned entity.</dd>
  <dt><b>type</b> (<i>string</i>)</dt>
  <dd>The type of the entity.</dd>
  <dt><b>reputation</b> (<i>int</i>)</dt>
  <dd>A number that represents the reputation of the entity on a scale of -3 to 3. The values are:
    <ul>
      <li>3 excellent</li>
      <li>2 good</li>
      <li>1 normal</li>
      <li>0 neutral</li>
      <li>-1 poor</li>
      <li>-2 bad</li>
      <li>-3 extremely bad</li>
    </ul>
  </dd>
  <dt><b>accuracy</b> (<i>string</i>)</dt>
  <dd>The accuracy of the match.</dd>
  <dt><b>attributes</b> (<i>JSON</i>)</dt>
  <dd>A list of attributes of the entity. It depends on the type of entity. To see the list of attributes, check the <a href="./AUTENTICATIONAPI.md">Definition Page</a>.</dd>
</dl>

_e.g.:_
```json
{
  "@timestamp": "2023-04-13T09:52:33.262967801Z",
  "accuracy": 1,
  "attributes": {
    "malware": "win malware agent",
    "malware-family": "win",
    "malware-type": "malware"
  },
  "id": "malware-784136848fb31fe5fadf1f5ed8af40737720be2a85124583c18495d6bcf188e3",
  "reputation": -3,
  "tags": null,
  "type": "malware"
}
```

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
  "uuid": "cf096e86-347f-4ddf-af1c-0c673a004211",
  "error": "value cannot be empty"
}
```
<h3><b class="label label-red">Code 401</b>Authentication required</h3>

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

The HTTP status code 404, also known as a "Not Found" error, occurs when the search could not find a match for the given parameters.

For example:

```json
{
  "uuid": "9ca21f08-c3a3-4eeb-8179-49669d7fe1fa",
  "error": "record not found"
}
```
<h3><b class="label label-yellow">Code 500</b>Internal server error</h3>
We get this code when an internal server error occurs. This is not session related.