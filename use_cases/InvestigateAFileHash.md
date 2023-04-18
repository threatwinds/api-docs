---
layout: default
title: Investigate a file Hash
parent: Use Cases
nav_order: 1
---

# Investigate a file Hash

A researcher needs to investigate a file hash. Specifically, they want to search for the file hash and discover if it is related with any malware.

To accomplish this, the researcher can use the Advanced Search Endpoint to search for the md5 file hash and retrieve the related md5 entity. Once they have the md5 entity, they can use the Relations Endpoint to search for any relation between the file and any known malware.

_**md5**: "2ed56a78d957dcde2db76e830f5f0741"_

**STEP 1**

To retrieve the md5 entity associated with the given hash, we can use the <a href="../Search/ENTITY(ADVANCED)">Advanced Entity Search</a> endpoint available in the Search API. Here is an example query:

```json
{
  "query": {
    "must": [
      {
        "term": {
          "attributes.md5": {
            "value": "2ed56a78d957dcde2db76e830f5f0741"
          }
        }
      }
    ]
  }
}
```

The response would contain the md5 entity, as shown below:

```json
{
  "pages": 1,
  "items": 1,
  "results": [
    {
      "@timestamp": "2023-04-12T14:31:07.99960127Z",
      "accuracy": 1,
      "attributes": {
        "md5": "2ed56a78d957dcde2db76e830f5f0741"
      },
      "id": "md5-2bef618643db4b0f1b8631b1567abf2262dc4161e94f671c5a9197d3b22a2c52",
      "reputation": -3,
      "score": null,
      "sort": [
        1681309867999
      ],
      "tags": [
        "malware",
        "common-file"
      ],
      "type": "md5",
      "version": 1
    }
  ]
}
```

To determine the reputation of a file, we can check the "reputation" attribute in the response. The scale for reputation ranges from -3 (extremely bad) to 3 (excellent). In this case, the file has a negative reputation with a value of -3, which is the worst possible rating on the scale.

**STEP 2** Now that we have retrieved the ID of the md5 file associated with the given hash, we can proceed to the next step, which is to use the <a href="../Search/RELATIONS(ADVANCED)">Advanced Relations</a> endpoint to retrieve the malware associated with that file.

```json
{
  "query": {
      "filter": [
        {
          "term": {
            "entity.entityID.keyword": {
              "value": "md5-2bef618643db4b0f1b8631b1567abf2262dc4161e94f671c5a9197d3b22a2c52"
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
```

The response would contain a list of relations between the given MD5 and malware entities, as shown below:

```json
{
  "pages": 1,
  "items": 1,
  "results": [
    {
      "@timestamp": "2023-04-12T14:31:09.07498005Z",
      "entity": {
        "@timestamp": "2023-04-12T14:31:07.99960127Z",
        "accuracy": 1,
        "attributes": {
          "md5": "2ed56a78d957dcde2db76e830f5f0741"
        },
        "entityID": "md5-2bef618643db4b0f1b8631b1567abf2262dc4161e94f671c5a9197d3b22a2c52",
        "reputation": -3,
        "tags": [
          "malware",
          "common-file"
        ],
        "type": "md5",
        "visibleBy": [
          "quantfall",
          "public"
        ]
      },
      "id": "84a7a37a89a3e444caa9ffed78363cbcc5092db455fb938ee39799dfd4bad49d",
      "mode": "association",
      "relation": {
        "@timestamp": "2023-04-12T14:31:08.019737077Z",
        "accuracy": 1,
        "attributes": {
          "malware": "win exploit cve_2023_21823",
          "malware-family": "win",
          "malware-type": "exploit"
        },
        "entityID": "malware-efefabf74562def7d4fba5e384896ef59a08ba45b54c98fb98a3772f96a6d3cb",
        "reputation": -3,
        "tags": [],
        "type": "malware",
        "visibleBy": [
          "quantfall",
          "public"
        ]
      },
      "score": null,
      "sort": [
        1681309869074
      ],
      "version": 1
    }
  ]
}
```

The provided information is about a malware named "win exploit cve_2023_21823". An exploit is a type of malware that takes advantage of vulnerabilities in software or systems to execute malicious code or commands. In this case, the exploit targets a specific vulnerability identified as "cve_2023_21823".

It is important to note that exploits are often used as a means to deliver other types of malware such as viruses, worms, or trojans. The reputation score of this specific malware is -3, which indicates a high risk for the user. It is recommended to take immediate action to remove the malware from the affected system.
