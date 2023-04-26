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

## Message
  The body parameter that is going to contain the OpenSearch-like query search and aggregations.

### Parameters

<dl>
  <dt><b>query</b> (<i>json</i>)</dt>
  <dd>"The 'query' parameter allows users to specify their search query. For more information on how to construct valid query, please refer to the documentation on the 'QUERY' parameter available at <a href='./queryandagg/QUERY'>Query Page</a>.</dd>
  <dt><b>agg</b> (<i>json</i>)</dt>
  <dd>The 'aggregations' parameter allows users to specify one or more aggregations to perform on the search results. Aggregations are used to group and summarize search results according to specific criteria, such as the number of occurrences of a particular term or the average value of a particular field. For more information on how to construct valid aggregation strings, please refer to the documentation on the 'AGGREGATION' parameter available at <a href='./queryandagg/AGGREGATIONS'>Aggregation Page</a></dd>
  <dt><b>collapse</b> (<i>json</i>)</dt>
  <dd>The 'collapse' parameter is an optional parameter that allows users to collapse search results based on a specified field value. When a collapse field is specified, only the first document within each collapsed group is returned. <b>It's important to note that when specifying a field for collapsing, you should use the ".keyword" suffix</b></dd>
 <dt><b>source</b> (<i>json</i>)</dt>
  <dd>The 'source' parameter allows users to specify the fields that should be returned in the search results using the 'includes' option. By default, all fields are returned. Additionally, users can exclude specific fields from the search results using the 'excludes' option.
  <dl>
  <dt><b>includes</b> (<i>string list</i>)</dt>
  <dd>The 'includes' parameter is used to specify a list of fields that should be returned in the search results.</dd>
  <dt><b>excludes</b> (<i>string list</i>)</dt>
  <dd>The 'excludes' parameter is used to specify a list of fields that should not be returned in the search results.</dd>
  </dl>
  </dd>
  <dt><b>search_after</b> (<i>int</i>)</dt>
  <dd>The 'search_after' parameter is used to retrieve elements after the maximum number of elements returned in a single search request, which is typically limited to 10,000 by default. This parameter allows users to iterate over the search results beyond this limit by using the 'order' field as an identifier for the last element in the previous search request. By specifying the 'search_after' parameter with the value of the 'order' field of the last element in the previous search request, users can retrieve the next set of elements in the search results. This can be useful for applications that need to retrieve large sets of search results that cannot be returned in a single search request.</dd>
</dl>

For Example:

```json
{
  "query": {
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

```
In the previous example, we are retrieving records that show every time a win Trojan Agent malware was inserted into a relation with another entity. These records allow us to track the occurrence of this type of malware and its relationships with other entities over time.

{: .note }
"To retrieve all records sorted by their most recent occurrence, simply make a request with an empty JSON object. The API will return the records in descending order, from the most recent to the oldest.

To perform a history search, use a <b class="label label-green">POST</b> request, for example:

```bash
curl -X 'POST' \
  'https://intelligence.threatwinds.com/api/search/v1/relations/history' \
  -H 'accept: application/json' \
  -H 'Content-Type: application/json' \
  -d '{
  "query": {
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
}'
```

### Returns

<h3> <b class="label label-green">Code 202</b> Accepted</h3>

**Retrieving a Specified Page of Results and Aggregations in the Response**

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

  <dt><b>score</b> (<i>float</i>)</dt>
  <dd>The 'score' represent the relevance of the object to the query.</dd>

  <dt><b>sort</b> (<i>int</i>)</dt>
  <dd>Value used as an id for the search_after parameter.</dd>

  <dt><b>version</b> (<i>int</i>)</dt>
  <dd>The 'version' retrieve the version number of a object. When a document is indexed or updated, a version number is generated and stored with the document. </dd>
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
      "@timestamp": "2023-04-13T10:44:35.318946758Z",
      "entity": {
        "@timestamp": "2023-04-13T10:44:35.292224449Z",
        "attributes": {
          "malware": "win trojan agent",
          "malware-family": "win",
          "malware-type": "trojan"
        },
        "entityID": "malware-98bfacefdfb602a0bce99248894cb4b74f0f674336c000677709a8a3393581e8",
        "reputation": -3,
        "tags": null,
        "type": "malware",
        "userID": "2e4ad29c-bf32-47b0-ae0d-67ec870b1677",
        "visibleBy": [
          "quantfall",
          "public"
        ]
      },
      "id": "6afac091-960f-4dd0-80d1-c897f0b02f7a",
      "mode": "association",
      "relation": {
        "@timestamp": "2023-04-13T10:44:35.278150134Z",
        "attributes": {
          "md5": "cea05f8cce8b0b6a4ae642ffbd9261cd"
        },
        "entityID": "md5-7bca9168d534560b239a79cf366735c7c24e4ddea16243b6016a8080a0d5ef79",
        "reputation": -3,
        "tags": [
          "malware",
          "system-file"
        ],
        "type": "md5",
        "userID": "2e4ad29c-bf32-47b0-ae0d-67ec870b1677",
        "visibleBy": [
          "quantfall",
          "public"
        ]
      },
      "relationID": "ba92e58cc24ccd7a46384119ff2cd9e77933fed59cf2eba1a2c2ab65610cd03f",
      "score": null,
      "sort": [
        1681382675318
      ],
      "userID": "2e4ad29c-bf32-47b0-ae0d-67ec870b1677",
      "version": 1
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