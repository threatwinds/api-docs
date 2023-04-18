---
layout: default
title: Relations
parent: Search
nav_order: 2
---
# Get Relations by entity id{#entityById}
This endpoint retrieves all the entities associated with a given entity based on their relationship. This API can provide valuable information for identifying relationships and patterns between entities, such as malware, domains, IP addresses, etc.
<br><br>

**EndPoint:** <https://intelligence.threatwinds.com/api/search/v1/entity/{id}/relations>

### Parameters
  
<dl>
  <dt><b>Autentication</b></dt>
  <dd>Authentication can be via Authorization header or Key-Pair. See the See <a href="../QUICKSTART#auth">Quick Start Auth</a> for a short explanation.<dl>
  <dt><b>Authorization</b> (<i>string</i>)</dt>
  <dd>The authorization header of the session if it is your authentication method.<br>
      <code>e.g.: Bearer fq6JoEFTsxiXAl1cVxPDnK4emIQCwaUBfq6JoEFTsxiXAl1cVxPDnK4emIQCwaUB</code></dd>
  <dt><b>api-key</b> (<i>string</i>)</dt>
  <dd>It needs to be combined with the api-secret. You can get it from the <a href="../auth/KeyPair">Key Pair Endpoints</a>.</dd>
  <dt><b>api-secret</b> (<i>string</i>)</dt>
  <dd>It needs to be combined with the api-key. You can get it from the <a href="../auth/KeyPair">Key Pair Endpoints</a>.</dd>
  </dl>
  </dd>
  <dt><b>groups</b> (<i>string</i>)</dt>
  <dd>This parameter specifies your access level or certain information. Default <i>global</i></dd>
  <dt><b>limit</b> (<i>int</i>)</dt>
  <dd>This parameter specifies the maximum number of results you wish to retrieve per page. (Default is 10)</dd>
  <dt><b>page</b> (<i>int</i>)</dt>
  <dd>This parameter specifies the page of results you want to retrieve, where each page contains a specified number of objects based on the value of the "limit" parameter. (Default is 0)</dd>
  <dt><b>sort</b> (<i>string</i>)</dt>
  <dd>The sort parameter is a query parameter that allows you to sort the results of your search based on a specific field.<br>
      <code>e.g.: reputation</code></dd>
  <dt><b>order</b> ("asc" or "desc")</dt>
  <dd>The order parameter specifies the order in which the results should be sorted based on the specified "sort" parameter. It can take two values: "asc" for ascending order and "desc" for descending order. (Default is asc)</dd>
  <dt><b>entityID</b> (<i>string</i>)</dt>
  <dd>The ID of the entity for which you want to retrieve the associations.</dd>
</dl>

To get the relations of one entity, use a <b class="label label-blue">GET</b>request, for example:

```bash
curl -X 'GET' \
  'https://intelligence.threatwinds.com/api/search/v1/entity/domain-59f5e994104053f590acdd1d2d72071669c839a60fc68291110facfa2fd16396/relations' \
  -H 'accept: application/json'\
  -H 'api-key: fq6JoEFDsxiXAl1cVxPDnK4emIQCwaUB' \
  -H 'api-secret: fq6JoEFTsxiXAl1cVxPDnK4emIQSwaUBfq6JoEFTsxiXAl1cVxPDnK4emIQCwaUB' 
```

### Returns

<h3> <b class="label label-green">Code 202</b> Accepted</h3>

**Return a list of Entities as Response**

It returns a list of associated entities or an empty list if no match is found.

### Fields of the response:

<dl>
  <dt><b>pages</b> (<i>string</i>)</dt>
  <dd>The total number of pages in the result set.</dd>
  <dt><b>items</b> (<i>string</i>)</dt>
  <dd>The total number of entities in the result set.</dd>
  <dt><b>result</b> (<i>entity list</i>)</dt>
  <dd>A list of hits that correspond to the entities that matched the search criteria.
  </dd>
</dl>

_e.g.:_
```json
{
  "pages": 1,
  "items": 1,
  "results": [
    {
      "@timestamp": "2023-04-13T06:49:43.218143362Z",
      "accuracy": 1,
      "attributes": {
        "threat": "expansion on 123@123.com"
      },
      "id": "threat-d8257cec79ed8b20c32decd305c1692391c185ab13d52b3495ea486df5385b27",
      "reputation": -2,
      "tags": null,
      "type": "threat"
    }
  ]
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