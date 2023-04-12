---
layout: default
title: Relations (Advanced)
parent: Search
nav_order: 2
---
# Relations (Advanced)
This API endpoint offers advanced search capabilities for entity relationships through an OpenSearch-like query search feature, which allows for fast and efficient searching of a vast number of entities. Furthermore, it provides aggregation functionality that enables intricate data analysis on search results, offering deep insights into entity relations.


**EndPoint:** https://intelligence.threatwinds.com/api/search/v1/relations

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
  <dt><b>limit</b> (<i>int</i>)</dt>
  <dd>This parameter specifies the maximum number of results you wish to retrieve per page. (Default is 10)</dd>
  <dt><b>page</b> (<i>int</i>)</dt>
  <dd>This parameter specifies the page of results you want to retrieve, where each page contains a specified number of objects based on the value of the "limit" parameter. (Default is 0)</dd>
  <dt><b>sort</b> (<i>string</i>)</dt>
  <dd>The sort parameter is a query parameter that allows you to sort the results of your search based on a specific field.<br>
      <code>e.g.: reputation</code></dd>
  <dt><b>order</b> ("asc" or "desc")</dt>
  <dd>The order parameter specifies the order in which the results should be sorted based on the specified "sort" parameter. It can take two values: "asc" for ascending order and "desc" for descending order. (Default is asc)</dd>
</dl>
  
### Message
  The body parameter that is going to contain the OpenSearch-like query search and aggregations.

  For Example:

```json
{
    "query": {
        "bool": {
            "filter": [
                {
                    "term": {
                        "entity.entityID.keyword": {
                            "value": "md5-68522c19344fadfb8c2c5d0c966f837ad2bfe75ebde38f9512b9d9457a873a85"
                        }
                    }
                },
                {
                    "term": {
                        "relation.type.keyword": {
                            "value": "malware"
                        }
                    }
                }
            ]
        }
    }
}
```

In the previous example, we filtered all entities of the type 'malware' related to a specific MD5 file.

We get a response like:

```json
{
  "pages": 1,
  "items": 1,
  "results": [
    {
      "@timestamp": "2023-04-12T15:00:11.899073932Z",
      "entity": {
        "@timestamp": "2023-04-12T15:00:10.816097836Z",
        "accuracy": 1,
        "attributes": {
          "md5": "0486254644cef6f085ff3a754b301ff3"
        },
        "entityID": "md5-68522c19344fadfb8c2c5d0c966f837ad2bfe75ebde38f9512b9d9457a873a85",
        "reputation": -3,
        "tags": [
          "malware",
          "system-file"
        ],
        "type": "md5"
      },
      "id": "c8cb9af272e3486cf27841b7dfd942b3f19467559feb461255c4b93f8ce14472",
      "mode": "association",
      "relation": {
        "@timestamp": "2023-04-12T15:00:10.740711619Z",
        "accuracy": 1,
        "attributes": {
          "malware": "win trojan sirefef",
          "malware-family": "win",
          "malware-type": "trojan"
        },
        "entityID": "malware-3bc164dcb2816c523af114432e3109d5dad55bd67c705f7eebdf2c68ed84d626",
        "reputation": -3,
        "tags": null,
        "type": "malware"
      }
    }
  ]
}
```
If you have not worked with Opensearch or elastic search before you can go to <a id="./queryandagg/QUERY" >Query</a> and <a id="./queryandagg/AGGREGATIONS.md">Agregations Page</a> to learn more about performing advanced searches, if you did, you can do a fast review about the queries we have available.

{: .note }
"To retrieve all relations sorted by their most recent occurrence, simply make a request with an empty JSON object. The API will return the relations in descending order, from the most recent to the oldest.

To perform a search, use a <b class="label label-green">POST</b> request, for example:

```bash
curl -X 'POST' \
  'https://intelligence.threatwinds.com/api/search/v1/relations' \
  -H 'accept: application/json' \
  -H 'Content-Type: application/json' \
  -d '   {
  "query": {
    "bool": {
      "filter": [
        {
          "term": {
            "entity.entityID.keyword": {
              "value": "md5-68522c19344fadfb8c2c5d0c966f837ad2bfe75ebde38f9512b9d9457a873a85"}
          }
        },{
"term": {
            "relation.type.keyword": {
              "value": "malware"}
          }
        }
      ]
    }
  }
  }'
```

### Returns


<h3> <b class="label label-green">Code 202</b> Accepted</h3>

**Retrieving a Specified Page of Results and Aggregations in the Response**

When a search query is executed in the API, the response object contains a list of hits that correspond to the entities associated with the given entity and that matched the search criteria, along with the corresponding aggregations.

The following fields are included in the response:

<dl>
  <dt><b>pages</b> (<i>string</i>)</dt>
  <dd>The total number of pages in the result set.</dd>
  <dt><b>items</b> (<i>string</i>)</dt>
  <dd>The total number of entities in the result set.</dd>
  <dt><b>result</b> (<i>list of relations</i>)</dt>
  <dd>A list of hits that correspond to the relations that matched the search criteria.
    <dl>
      <dt><b>id</b> (<i>string</i>)</dt>
      <dd>The id of the relation.</dd>
      <dt><b>mode</b> (<i>string</i>)</dt>
  <dd>The type of the relation, it can be <b>description</b> or <b>association</b>.</dd>
  
  <dt><b>entity</b> (<i>entity</i>)</dt>
  <dd>The 'center entity' that determines the main entity of interest for the search criteria, while the related entity is specified in the "relation" field.</dd>
  
  <dt><b>relation</b> (<i>entity</i>)</dt>
  <dd>The related entity.</dd>
</dl>
  </dd>
  <dt><b>aggregations</b> (<i>JSON</i>)</dt>
  <dd>A list of metrics based on the criteria specified in the search request.</dd>
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
  "uuid": "60791adb-e0f3-4df0-a45a-02a1718d75f5",
  "error": "... error: {\"error\":{\"root_cause\":[{\"type\":\"illegal_argument_exception\",\"reason\":\"query malformed, empty clause found at [1:131]\"}],\"type\":\"illegal_argument_exception\",\"reason\":\"query malformed, empty clause found at [1:131]\"},\"status\":400}"
}
```
<h3><b class="label label-red">Code 401</b>Authentication required</h3>

The 401 error code indicates that you need authentication to do this request.

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

<h3><b class="label label-yellow">Code 500</b>Internal server error</h3>
We get this code when an internal server error occurs. This is not session related.