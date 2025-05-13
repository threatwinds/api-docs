---
layout: default
title: Feeds API
nav_order: 5
has_children: true
---

# Feeds API

The Feeds API provides access to ThreatWinds threat intelligence feeds. These feeds contain comprehensive information on cybersecurity threats that can be used to enhance your security posture.

## Overview

ThreatWinds Feeds API allows you to:

| Feature                           | Description                                                 |
|-----------------------------------|-------------------------------------------------------------|
| [List Feeds](/feeds/list)         | Get a list of all available feeds                           |
| [Download Feeds](/feeds/download) | Download feed files based on accuracy level, type, and name |
| Checksums                         | Access feed checksums for verification                      |

The feeds are categorized by:

| Category           | Values                                |
|--------------------|---------------------------------------|
| **Accuracy Level** | level1, level2, level3                |
| **Type**           | accumulative, daily                   |
| **Name**           | various indicators like ip, md5, etc. |

## Authentication

Like all ThreatWinds APIs, the Feeds API requires authentication. You can authenticate using either:

| Authentication Method    | Description                                            |
|--------------------------|--------------------------------------------------------|
| **Authorization Header** | Include a bearer token in the Authorization header     |
| **API Key and Secret**   | Include your API key and secret in the request headers |

For more details on authentication, see the [Authentication](/auth) section.

## API Endpoints

The base URL for the Feeds API is:

```
https://intelligence.threatwinds.com/api/feeds/v1
```

For detailed information about each endpoint, please refer to the specific documentation pages.
