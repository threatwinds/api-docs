---
layout: default
title: Negative Reputation Domains list
parent: Use Cases
nav_order: 2
---

# Negative Reputation Domains list

A developer is tasked with developing a DNS filtering module for an enterprise network to block access to malicious websites. To achieve this, the developer needs to programmatically identify all domains with negative reputation and block access to them.

To accomplish this, the developer can use the Advanced Search Endpoint to search for all entities of domain type with negative reputation and only return the domain field.

**STEP 1**

To retrieve domains with negative reputation, we can use the <a href="../Search/ENTITY(ADVANCED)">Advanced Entity Search</a> endpoint available in the Search API. The following query can be used:

```json
{
  "query": {
    "filter": [
      {
        "term": {
          "type": {
            "value": "domain"
          }
        }
      }
    ],
    "must": [
      {
        "range": {
          "reputation": {
            "gte": -3,
            "lte": -1
          }
        }
      }
    ]
  },
  "source": {
    "includes": ["attributes.domain"]
  }
}
```

The response would contain the domain entities, as shown below:

```json
{
  "pages": 1000,
  "items": 10000,
  "results": [
    {
      "attributes": {
        "domain": "www.garrett.kz"
      },
      "id": "domain-a26df02ae06d0e07e598d8ef97bf67b394a84d0c0f9a0dfc7620bd385d2d0b7e",
      "score": null,
      "sort": [
        1681487678898
      ],
      "version": 7
    },
    {
      "attributes": {
        "domain": "griincom.co.ke"
      },
      "id": "domain-ea06fc39487657a3d07f68e409d0c8c7722acb990bdcd264a436df8b982d5266",
      "score": null,
      "sort": [
        1681487643354
      ],
      "version": 7
    },...
    ]
}
```

After retrieving the domains with negative reputation through the Advanced Search endpoint query, we can iterate over the resulting entities and extract the domain field from each entity.

Here is the final list of domains that were obtained:

```txt
departmentstores-onlineshopping.com
bellezane.info
pbmz.net
brokenbeside.net
rainingcats.net
inrgroups.com
gtc3595.com
tnjdkfi7wvqrc7tfa.com
tnbtvrirc.info
armanfze.com
gotnewedgein.info
...
```
