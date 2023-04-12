---
layout: default
title: Relations History
parent: Search
nav_order: 2
---
# Get Relations History

Our API provides efficient analysis of entities' relationship records over time through a robust search functionality and an OpenSearch-like query search option. In addition, we offer powerful aggregation capabilities that enable users to perform complex data analysis on their search results, giving them deeper insights into the relationships between entities.

**EndPoint:** https://intelligence.threatwinds.com/api/search/v1/relations/history

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
                            "entity.attributes.malware.keyword": {
                                "value": "win trojan agent"
                            }
                        }
                    }
                ]
            }
        }
    }
  ```
"In the previous example, we are retrieving records that show every time a win Trojan Agent malware was inserted into a relation with another entity. These records allow us to track the occurrence of this type of malware and its relationships with other entities over time.

{: .note }
"To retrieve all records sorted by their most recent occurrence, simply make a request with an empty JSON object. The API will return the records in descending order, from the most recent to the oldest.

To perform a history search, use a **POST**</span> request, for example:

```bash
curl -X 'POST' \
  'https://intelligence.threatwinds.com/api/search/v1/relations/history' \
  -H 'accept: application/json' \
  -H 'Content-Type: application/json' \
  -d '    {
        "query": {
            "bool": {
                "filter": [
                    {
                        "term": {
                            "entity.attributes.malware.keyword": {
                                "value": "win trojan agent"
                            }
                        }
                    }
                ]
            }
        }
    }'
```

### Returns

> <h5>Code 200</h5>

<h5>Retrieving a Specified Page of Results and Aggregations in the Response</h5>

When a search query is executed in the API, the response object contains a list of hits that correspond to the relationship records that matched the search criteria, along with the corresponding aggregation.  

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


For Example:
```json
{
  "pages": 1000,
  "items": 10000,
  "results": [
    {
      "@timestamp": "2023-04-12T16:19:59.353450277Z",
      "entity": {
        "@timestamp": "2023-04-12T16:19:59.317622426Z",
        "attributes": {
          "malware": "win trojan agent",
          "malware-family": "win",
          "malware-type": "trojan"
        },
        "entityID": "malware-98bfacefdfb602a0bce99248894cb4b74f0f674336c000677709a8a3393581e8",
        "reputation": -3,
        "tags": null,
        "type": "malware",
        "userID": "2e4ad29c-bf32-47b0-ae0d-67ec870b1677"
      },
      "id": "4cf7949a-4851-40f8-a922-e0d6e470bf0b",
      "mode": "association",
      "relation": {
        "@timestamp": "2023-04-12T16:19:59.301897516Z",
        "attributes": {
          "md5": "65fef19db66256e8fa0d2743af72b3d4"
        },
        "entityID": "md5-8cdb267e21155f666172257c0da40222e800be68d01014b026a5af38c2b36131",
        "reputation": -3,
        "tags": [
          "malware",
          "system-file"
        ],
        "type": "md5",
        "userID": "2e4ad29c-bf32-47b0-ae0d-67ec870b1677"
      },
      "relationID": "0eacf1628e7237821b937fe4e8fdfa02b260a577461e95d210547052b426328b",
      "userID": "2e4ad29c-bf32-47b0-ae0d-67ec870b1677"
    },...]
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

<h3><b class="label label-yellow">Code 500</b>Internal server error</h3>
We get this code when an internal server error occurs. This is not session related.