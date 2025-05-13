---
layout: default
title: Entity Mapping
parent: Search API
nav_order: 8
permalink: /search/entity-mapping
---

# Entity Mapping

This page provides information about the entity mapping structure used by the ThreatWinds Search API. Understanding this mapping is essential for developers who need to work with entity data returned by the API.

## Overview

The ThreatWinds platform uses OpenSearch to store and index threat intelligence entities. The mapping defines the structure of these entities, including their attributes, data types, and indexing behavior.

## Entity Structure

All entities in the ThreatWinds platform share a common structure:

* **id** - Unique identifier of the entity
* **type** - Type of the entity (e.g., "ip", "domain", "hash")
* **@timestamp** - Timestamp when the entity was last updated
* **reputation** - Current reputation score
* **bestReputation** - Best historical reputation score
* **worstReputation** - Worst historical reputation score
* **accuracy** - Accuracy score
* **lastSeen** - Timestamp when the entity was last seen
* **tags** - Array of tags associated with the entity
* **visibleBy** - Array of visibility settings
* **attributes** - Object containing entity-specific attributes

## Entity Types and Attributes

The Search API supports a wide range of entity types, each with its own set of attributes. Here are some of the most common entity types and their attributes:

### IP Address (`ip`)

* **ip** - The IP address value
* **asn** - Autonomous System Number
* **aso** - Autonomous System Organization
* **country** - Country where the IP is located
* **city** - City where the IP is located
* **latitude** - Geographical latitude
* **longitude** - Geographical longitude

### Domain (`domain`)

* **domain** - The domain name
* **whois-registrar** - Domain registrar information

### URL (`url`)

* **url** - The URL value
* **domain** - Associated domain name

### Malware (`malware`)

* **malware** - Malware name
* **malware-family** - Malware family
* **malware-type** - Type of malware

## OpenSearch mapping

| Field Path                               | OpenSearch Type   | Supports Full-text Search | Supports Term-level Search | How to Perform Term Search & `.keyword` Usage                                                                                               |
|------------------------------------------|-------------------|---------------------------|----------------------------|---------------------------------------------------------------------------------------------------------------------------------------------|
| @timestamp                               | date              | No                        | Yes                        | Query `@timestamp` directly for exact matches or range queries. Not analyzed. No `.keyword` needed.                                         |
| accuracy                                 | long              | No                        | Yes                        | Query `accuracy` directly for exact matches or range queries. Not analyzed. No `.keyword` needed.                                           |
| attributes                               | object            | N/A                       | N/A                        | This is an object field. Searchability depends on its sub-fields.                                                                           |
| attributes.adversary                     | text              | Yes (analyzed)            | Yes (via `.keyword`)       | For full-text search, query `attributes.adversary`. For exact term matches, sorting, or aggregations, query `attributes.adversary.keyword`.     |
| attributes.asn                           | long              | No                        | Yes                        | Query `attributes.asn` directly for exact matches or range queries. Not analyzed. No `.keyword` needed.                                      |
| attributes.aso                           | text              | Yes (analyzed)            | Yes (via `.keyword`)       | For full-text search, query `attributes.aso`. For exact term matches, sorting, or aggregations, query `attributes.aso.keyword`.               |
| attributes.breach                        | text              | Yes (analyzed)            | Yes (via `.keyword`)       | For full-text search, query `attributes.breach`. For exact term matches, sorting, or aggregations, query `attributes.breach.keyword`.         |
| attributes.breach-count                  | long              | No                        | Yes                        | Query `attributes.breach-count` directly for exact matches or range queries. Not analyzed. No `.keyword` needed.                            |
| attributes.breach-date                   | date              | No                        | Yes                        | Query `attributes.breach-date` directly for exact matches or range queries. Not analyzed. No `.keyword` needed.                              |
| attributes.breach-description            | text              | Yes (analyzed)            | Yes (via `.keyword`)       | For full-text search, query `attributes.breach-description`. For exact term matches, sorting, or aggregations, query `attributes.breach-description.keyword`. |
| attributes.btc                           | text              | Yes (analyzed)            | Yes (via `.keyword`)       | For full-text search, query `attributes.btc`. For exact term matches, sorting, or aggregations, query `attributes.btc.keyword`.               |
| attributes.certificate-fingerprint       | text              | Yes (analyzed)            | Yes (via `.keyword`)       | For full-text search, query `attributes.certificate-fingerprint`. For exact term matches, sorting, or aggregations, query `attributes.certificate-fingerprint.keyword`.|
| attributes.cidr                          | ip_range          | No                        | Yes                        | Query `attributes.cidr` directly for exact matches or range queries. Not analyzed. No `.keyword` needed.                                    |
| attributes.city                          | text              | Yes (analyzed)            | Yes (via `.keyword`)       | For full-text search, query `attributes.city`. For exact term matches, sorting, or aggregations, query `attributes.city.keyword`.             |
| attributes.cookie                        | text              | Yes (analyzed)            | Yes (via `.keyword`)       | For full-text search, query `attributes.cookie`. For exact term matches, sorting, or aggregations, query `attributes.cookie.keyword`.         |
| attributes.country                       | text              | Yes (analyzed)            | Yes (via `.keyword`)       | For full-text search, query `attributes.country`. For exact term matches, sorting, or aggregations, query `attributes.country.keyword`.       |
| attributes.cpe                           | text              | Yes (analyzed)            | Yes (via `.keyword`)       | For full-text search, query `attributes.cpe`. For exact term matches, sorting, or aggregations, query `attributes.cpe.keyword`.               |
| attributes.cve                           | text              | Yes (analyzed)            | Yes (via `.keyword`)       | For full-text search, query `attributes.cve`. For exact term matches, sorting, or aggregations, query `attributes.cve.keyword`.               |
| attributes.datetime                      | date              | No                        | Yes                        | Query `attributes.datetime` directly for exact matches or range queries. Not analyzed. No `.keyword` needed.                                |
| attributes.descriptor                    | text              | Yes (analyzed)            | Yes (via `.keyword`)       | For full-text search, query `attributes.descriptor`. For exact term matches, sorting, or aggregations, query `attributes.descriptor.keyword`. |
| attributes.domain                        | text              | Yes (analyzed)            | Yes (via `.keyword`)       | For full-text search, query `attributes.domain`. For exact term matches, sorting, or aggregations, query `attributes.domain.keyword`.         |
| attributes.email                         | text              | Yes (analyzed)            | Yes (via `.keyword`)       | For full-text search, query `attributes.email`. For exact term matches, sorting, or aggregations, query `attributes.email.keyword`.           |
| attributes.email-address                 | text              | Yes (analyzed)            | Yes (via `.keyword`)       | For full-text search, query `attributes.email-address`. For exact term matches, sorting, or aggregations, query `attributes.email-address.keyword`. |
| attributes.email-body                    | text              | Yes (analyzed)            | Yes (via `.keyword`)       | For full-text search, query `attributes.email-body`. For exact term matches, sorting, or aggregations, query `attributes.email-body.keyword`. |
| attributes.email-display-name            | text              | Yes (analyzed)            | Yes (via `.keyword`)       | For full-text search, query `attributes.email-display-name`. For exact term matches, sorting, or aggregations, query `attributes.email-display-name.keyword`. |
| attributes.email-header                  | text              | Yes (analyzed)            | Yes (via `.keyword`)       | For full-text search, query `attributes.email-header`. For exact term matches, sorting, or aggregations, query `attributes.email-header.keyword`. |
| attributes.email-subject                 | text              | Yes (analyzed)            | Yes (via `.keyword`)       | For full-text search, query `attributes.email-subject`. For exact term matches, sorting, or aggregations, query `attributes.email-subject.keyword`. |
| attributes.file                          | text              | Yes (analyzed)            | Yes (via `.keyword`)       | For full-text search, query `attributes.file`. For exact term matches, sorting, or aggregations, query `attributes.file.keyword`.             |
| attributes.filename                      | text              | Yes (analyzed)            | Yes (via `.keyword`)       | For full-text search, query `attributes.filename`. For exact term matches, sorting, or aggregations, query `attributes.filename.keyword`.     |
| attributes.filename-pattern              | text              | Yes (analyzed)            | Yes (via `.keyword`)       | For full-text search, query `attributes.filename-pattern`. For exact term matches, sorting, or aggregations, query `attributes.filename-pattern.keyword`. |
| attributes.github-repository             | text              | Yes (analyzed)            | Yes (via `.keyword`)       | For full-text search, query `attributes.github-repository`. For exact term matches, sorting, or aggregations, query `attributes.github-repository.keyword`. |
| attributes.hex                           | text              | Yes (analyzed)            | Yes (via `.keyword`)       | For full-text search, query `attributes.hex`. For exact term matches, sorting, or aggregations, query `attributes.hex.keyword`.               |
| attributes.hostname                      | text              | Yes (analyzed)            | Yes (via `.keyword`)       | For full-text search, query `attributes.hostname`. For exact term matches, sorting, or aggregations, query `attributes.hostname.keyword`.     |
| attributes.iban                          | text              | Yes (analyzed)            | Yes (via `.keyword`)       | For full-text search, query `attributes.iban`. For exact term matches, sorting, or aggregations, query `attributes.iban.keyword`.             |
| attributes.ip                            | ip                | No                        | Yes                        | Query `attributes.ip` directly for exact matches or range queries. Not analyzed. No `.keyword` needed.                                      |
| attributes.ja3-fingerprint               | text              | Yes (analyzed)            | Yes (via `.keyword`)       | For full-text search, query `attributes.ja3-fingerprint`. For exact term matches, sorting, or aggregations, query `attributes.ja3-fingerprint.keyword`.|
| attributes.ja3-fingerprint-md5           | text              | Yes (analyzed)            | Yes (via `.keyword`)       | For full-text search, query `attributes.ja3-fingerprint-md5`. For exact term matches, sorting, or aggregations, query `attributes.ja3-fingerprint-md5.keyword`. |
| attributes.jabber-id                     | text              | Yes (analyzed)            | Yes (via `.keyword`)       | For full-text search, query `attributes.jabber-id`. For exact term matches, sorting, or aggregations, query `attributes.jabber-id.keyword`.   |
| attributes.jarm-fingerprint              | text              | Yes (analyzed)            | Yes (via `.keyword`)       | For full-text search, query `attributes.jarm-fingerprint`. For exact term matches, sorting, or aggregations, query `attributes.jarm-fingerprint.keyword`. |
| attributes.latitude                      | float             | No                        | Yes                        | Query `attributes.latitude` directly for exact matches or range queries. Not analyzed. No `.keyword` needed.                              |
| attributes.link                          | text              | Yes (analyzed)            | Yes (via `.keyword`)       | For full-text search, query `attributes.link`. For exact term matches, sorting, or aggregations, query `attributes.link.keyword`.             |
| attributes.longitude                     | float             | No                        | Yes                        | Query `attributes.longitude` directly for exact matches or range queries. Not analyzed. No `.keyword` needed.                             |
| attributes.malware                       | text              | Yes (analyzed)            | Yes (via `.keyword`)       | For full-text search, query `attributes.malware`. For exact term matches, sorting, or aggregations, query `attributes.malware.keyword`.       |
| attributes.malware-family                | text              | Yes (analyzed)            | Yes (via `.keyword`)       | For full-text search, query `attributes.malware-family`. For exact term matches, sorting, or aggregations, query `attributes.malware-family.keyword`. |
| attributes.malware-type                  | text              | Yes (analyzed)            | Yes (via `.keyword`)       | For full-text search, query `attributes.malware-type`. For exact term matches, sorting, or aggregations, query `attributes.malware-type.keyword`. |
| attributes.md5                           | text              | Yes (analyzed)            | Yes (via `.keyword`)       | For full-text search, query `attributes.md5`. For exact term matches, sorting, or aggregations, query `attributes.md5.keyword`.               |
| attributes.mime-type                     | text              | Yes (analyzed)            | Yes (via `.keyword`)       | For full-text search, query `attributes.mime-type`. For exact term matches, sorting, or aggregations, query `attributes.mime-type.keyword`.   |
| attributes.mobile-app-id                 | text              | Yes (analyzed)            | Yes (via `.keyword`)       | For full-text search, query `attributes.mobile-app-id`. For exact term matches, sorting, or aggregations, query `attributes.mobile-app-id.keyword`. |
| attributes.object                        | text              | Yes (analyzed)            | Yes (via `.keyword`)       | For full-text search, query `attributes.object`. For exact term matches, sorting, or aggregations, query `attributes.object.keyword`.         |
| attributes.path                          | text              | Yes (analyzed)            | Yes (via `.keyword`)       | For full-text search, query `attributes.path`. For exact term matches, sorting, or aggregations, query `attributes.path.keyword`.             |
| attributes.pattern-in-file               | text              | Yes (analyzed)            | Yes (via `.keyword`)       | For full-text search, query `attributes.pattern-in-file`. For exact term matches, sorting, or aggregations, query `attributes.pattern-in-file.keyword`. |
| attributes.pattern-in-memory             | text              | Yes (analyzed)            | Yes (via `.keyword`)       | For full-text search, query `attributes.pattern-in-memory`. For exact term matches, sorting, or aggregations, query `attributes.pattern-in-memory.keyword`. |
| attributes.pattern-in-traffic            | text              | Yes (analyzed)            | Yes (via `.keyword`)       | For full-text search, query `attributes.pattern-in-traffic`. For exact term matches, sorting, or aggregations, query `attributes.pattern-in-traffic.keyword`. |
| attributes.phone                         | text              | Yes (analyzed)            | Yes (via `.keyword`)       | For full-text search, query `attributes.phone`. For exact term matches, sorting, or aggregations, query `attributes.phone.keyword`.           |
| attributes.port                          | text              | Yes (analyzed)            | Yes (via `.keyword`)       | For full-text search, query `attributes.port`. For exact term matches, sorting, or aggregations, query `attributes.port.keyword`.|
| attributes.prtn                          | text              | Yes (analyzed)            | Yes (via `.keyword`)       | For full-text search, query `attributes.prtn`. For exact term matches, sorting, or aggregations, query `attributes.prtn.keyword`.             |
| attributes.regkey                        | text              | Yes (analyzed)            | Yes (via `.keyword`)       | For full-text search, query `attributes.regkey`. For exact term matches, sorting, or aggregations, query `attributes.regkey.keyword`.         |
| attributes.sha1                          | text              | Yes (analyzed)            | Yes (via `.keyword`)       | For full-text search, query `attributes.sha1`. For exact term matches, sorting, or aggregations, query `attributes.sha1.keyword`.             |
| attributes.sha224                        | text              | Yes (analyzed)            | Yes (via `.keyword`)       | For full-text search, query `attributes.sha224`. For exact term matches, sorting, or aggregations, query `attributes.sha224.keyword`.         |
| attributes.sha256                        | text              | Yes (analyzed)            | Yes (via `.keyword`)       | For full-text search, query `attributes.sha256`. For exact term matches, sorting, or aggregations, query `attributes.sha256.keyword`.         |
| attributes.sha3-256                      | text              | Yes (analyzed)            | Yes (via `.keyword`)       | For full-text search, query `attributes.sha3-256`. For exact term matches, sorting, or aggregations, query `attributes.sha3-256.keyword`.    |
| attributes.sha384                        | text              | Yes (analyzed)            | Yes (via `.keyword`)       | For full-text search, query `attributes.sha384`. For exact term matches, sorting, or aggregations, query `attributes.sha384.keyword`.         |
| attributes.sha512                        | text              | Yes (analyzed)            | Yes (via `.keyword`)       | For full-text search, query `attributes.sha512`. For exact term matches, sorting, or aggregations, query `attributes.sha512.keyword`.         |
| attributes.snort-rule                    | text              | Yes (analyzed)            | Yes (via `.keyword`)       | For full-text search, query `attributes.snort-rule`. For exact term matches, sorting, or aggregations, query `attributes.snort-rule.keyword`. |
| attributes.ssh-banner                    | text              | Yes (analyzed)            | Yes (via `.keyword`)       | For full-text search, query `attributes.ssh-banner`. For exact term matches, sorting, or aggregations, query `attributes.ssh-banner.keyword`.|
| attributes.ssh-fingerprint               | text              | Yes (analyzed)            | Yes (via `.keyword`)       | For full-text search, query `attributes.ssh-fingerprint`. For exact term matches, sorting, or aggregations, query `attributes.ssh-fingerprint.keyword`.|
| attributes.suricata-rule                 | text              | Yes (analyzed)            | Yes (via `.keyword`)       | For full-text search, query `attributes.suricata-rule`. For exact term matches, sorting, or aggregations, query `attributes.suricata-rule.keyword`. |
| attributes.text                          | text              | Yes (analyzed)            | Yes (via `.keyword`)       | For full-text search, query `attributes.text`. For exact term matches, sorting, or aggregations, query `attributes.text.keyword`.             |
| attributes.threat                        | text              | Yes (analyzed)            | Yes (via `.keyword`)       | For full-text search, query `attributes.threat`. For exact term matches, sorting, or aggregations, query `attributes.threat.keyword`.         |
| attributes.url                           | text              | Yes (analyzed)            | Yes (via `.keyword`)       | For full-text search, query `attributes.url`. For exact term matches, sorting, or aggregations, query `attributes.url.keyword`.               |
| attributes.value                         | text              | Yes (analyzed)            | Yes (via `.keyword`)       | For full-text search, query `attributes.value`. For exact term matches, sorting, or aggregations, query `attributes.value.keyword`.           |
| attributes.virustotal-report             | text              | Yes (analyzed)            | Yes (via `.keyword`)       | For full-text search, query `attributes.virustotal-report`. For exact term matches, sorting, or aggregations, query `attributes.virustotal-report.keyword`. |
| attributes.whois-registrar               | text              | Yes (analyzed)            | Yes (via `.keyword`)       | For full-text search, query `attributes.whois-registrar`. For exact term matches, sorting, or aggregations, query `attributes.whois-registrar.keyword`. |
| attributes.windows-scheduled-task        | text              | Yes (analyzed)            | Yes (via `.keyword`)       | For full-text search, query `attributes.windows-scheduled-task`. For exact term matches, sorting, or aggregations, query `attributes.windows-scheduled-task.keyword`. |
| attributes.windows-service-name          | text              | Yes (analyzed)            | Yes (via `.keyword`)       | For full-text search, query `attributes.windows-service-name`. For exact term matches, sorting, or aggregations, query `attributes.windows-service-name.keyword`. |
| attributes.yara-rule                     | text              | Yes (analyzed)            | Yes (via `.keyword`)       | For full-text search, query `attributes.yara-rule`. For exact term matches, sorting, or aggregations, query `attributes.yara-rule.keyword`.   |
| bestReputation                           | long              | No                        | Yes                        | Query `bestReputation` directly for exact matches or range queries. Not analyzed. No `.keyword` needed.                                     |
| lastSeen                                 | date              | No                        | Yes                        | Query `lastSeen` directly for exact matches or range queries. Not analyzed. No `.keyword` needed.                                           |
| reputation                               | long              | No                        | Yes                        | Query `reputation` directly for exact matches or range queries. Not analyzed. No `.keyword` needed.                                         |
| tags                                     | text              | Yes (analyzed)            | Yes (via `.keyword`)       | For full-text search, query `tags`. For exact term matches, sorting, or aggregations, query `tags.keyword`.                                 |
| type                                     | text              | Yes (analyzed)            | Yes (via `.keyword`)       | For full-text search, query `type`. For exact term matches, sorting, or aggregations, query `type.keyword`.                                  |
| visibleBy                                | text              | Yes (analyzed)            | Yes (via `.keyword`)       | For full-text search, query `visibleBy`. For exact term matches, sorting, or aggregations, query `visibleBy.keyword`.                        |
| wellKnown                                | boolean           | No                        | Yes                        | Query `wellKnown` directly for `true` or `false` values. Not analyzed. No `.keyword` needed.                                                |
| worstReputation                          | long              | No                        | Yes                        | Query `worstReputation` directly for exact matches or range queries. Not analyzed. No `.keyword` needed.                                    |

**Explanation of Terms:**

* **Full-text Search:** This type of search analyzes the text content, breaking it down into individual terms (tokens) after processing (e.g., lowercasing, removing punctuation, stemming). It's suitable for finding words or phrases within larger blocks of text, and relevance scoring plays a significant role. Fields of type `text` are analyzed and support full-text search.
* **Term-level Search (Keyword Search):** This type of search looks for exact matches of terms. It does not analyze the query string in the same way full-text search does. It's used for filtering by exact values, as well as for sorting and aggregations. Fields of type `keyword`, numeric types, dates, booleans, and IP addresses support term-level searches directly. For `text` fields, a `.keyword` multi-field is typically used for term-level operations.
* **`.keyword` Suffix:** For fields mapped as `text`, OpenSearch often creates a multi-field (sub-field) typically named `<field_name>.keyword`.
  * The main `text` field (e.g., `attributes.city`) is analyzed and used for full-text searches.
  * The `.keyword` field (e.g., `attributes.city.keyword`) is *not* analyzed and stores the original string as a single term. This is essential for exact matching, sorting, and aggregations on that text.
* **Analyzed:** Indicates that the field's content is processed by an analyzer before indexing. For example, "Quick Brown Fox" might be indexed as "quick", "brown", "fox".
* **N/A (Not Applicable):** Used for object fields which are containers, or when the concept doesn't directly apply.

## Related Documentation

* [Entity Retrieval](/search/entity) - How to retrieve entity details
* [Entity Types](/search/entity-types) - List of all entity types
* [Simple Search](/search/simple-search) - How to perform simple searches
* [Advanced Search](/search/advanced-search) - How to perform advanced searches
