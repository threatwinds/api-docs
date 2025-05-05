---
layout: default
title: Entity Types
parent: Search API
nav_order: 7
permalink: /search/entity-types
---

# Entity Types

This page provides a comprehensive list of all possible entity types and their attributes in the ThreatWinds platform. These types can be used when searching for entities or interpreting entity data returned by the API.

## Entity Types

| Type | Description | Data Type |
|------|-------------|-----------|
| file | An object identifying a file, the value can be a UUID or a SHA3-256 or MD5 checksum | UUID\|MD5\|SHA3-256 |
| payload | A SHA3-256 of a message sent in a network packet | SHA3-256 |
| file-data | A file or attachment URL | URL |
| adversary | An object identifying a threat actor | Adversary |
| aso | An autonomous System Organization | String |
| asn | An autonomous System Organization Number | Integer |
| malware | A software intentionally designed to harm a computer, server, network, or user | String |
| aba-rtn | An ABA routing transit number | Integer |
| latitude | A GPS latitude | Float |
| longitude | A GPS longitude | Float |
| country | A country name | Country |
| city | A city name | City |
| cookie | An HTTP cookie as often stored on the user web client | Case-sensitive string |
| text | A case insensitive text value | String |
| value | A case sensitive text value | Case-sensitive string |
| password | A password | Case-sensitive string |
| airport-name | An airport name | String |
| profile-photo | A profile photo URL | URL |
| authentihash | An authenticode executable signature hash | Hexadecimal |
| bank-account-nr | A bank account number without any routing number | Integer |
| bic | A bank identifier code also known as SWIFT-BIC, SWIFT code or ISO 9362 code | String |
| bin | A bank identification number | Integer |
| btc | A bitcoin address | Case-sensitive string |
| cc-number | A credit card number | Integer |
| issuer | An issuer name | String |
| issuing-country | An issuing country name | Country |
| cdhash | An Apple Code Directory Hash, identifying a code-signed Mach-O executable file | Hexadecimal |
| certificate-fingerprint | A fingerprint of a SSL/TLS certificate | Hexadecimal |
| chrome-extension-id | A Chrome extension ID | Case-sensitive string |
| cidr | A public network segment | CIDR |
| cpe | A standardized label used to identify applications, operating systems, and hardware devices | String |
| cve | A standardized identifier for a known cybersecurity vulnerability | String |
| dash | A Dash address | Case-sensitive string |
| dkim | A public key used for email authentication | Case-sensitive string |
| dkim-signature | An email authentication method | Case-sensitive string |
| domain | A human-readable address that points to a specific website or server on the internet | FQDN |
| email | An email message ID | Case-sensitive string |
| email-address | An email sender address | Email |
| email-body | An email message body | String |
| email-display-name | An email sender display name | String |
| email-header | An email header | Case-sensitive string |
| email-mime-boundary | A strings of 7-bit US-ASCII text that define the boundaries between message parts | Case-sensitive string |
| email-subject | An email subject | String |
| email-thread-index | An email thread index | BASE64 |
| email-x-mailer | An email x-mailer header | String |
| eppn | A NetId of a person for the purposes of inter-institutional authentication | Email |
| facebook-profile | A Facebook profile | URL |
| ffn | A frequent flyer number of a passenger | Case-sensitive string |
| filename | A filename or email attachment name | String |
| size-in-bytes | A size in bytes of an element | Float |
| filename-pattern | A pattern in the name of a file | Regex |
| flight | A flight number | Case-sensitive string |
| github-organization | A Github organization | URL |
| github-repository | A Github repository | URL |
| github-user | A Github profile | URL |
| link | An external link for reference | URL |
| datetime | A time with nanoseconds in the format 2006-01-02T15:04:05.999999999Z07:00 | Datetime |
| last-analysis | A time of last analysis. Format 2006-01-02T15:04:05.999999999Z | Datetime |
| date | A date in format 2006-01-02 | Date |
| date-of-issue | A date in format 2006-01-02 | Date |
| expiration-date | A date in format 2006-01-02 | Date |
| malware-sample | A malware sample URL | URL |
| group | A group of adversaries like APTnn or Anonymous | Adversary |
| hex | A value in hexadecimal | Hexadecimal |
| base64 | A value in BASE64 format | BASE64 |
| hostname | A human-readable name that identifies that specific device | FQDN |
| iban | An international bank account number | String |
| id-number | It can be an ID card, residence permit, etc. | Case-sensitive string |
| ip | An unique numerical identifier assigned to every device connected to a network | IP |
| ja3-fingerprint | An SSL/TLS client fingerprints | MD5 |
| jabber-id | A Jabber ID | Email |
| jarm-fingerprint | An SSL/TLS server fingerprints | Hexadecimal |
| mac-address | A network interface hardware address | MAC |
| malware-family | A malware family | String |
| malware-type | A malware type | String |
| md5 | A 128-bit fingerprint | MD5 |
| mime-type | A two-part identifier | MIME type |
| mobile-app-id | An ID of a mobile application | Case-sensitive string |
| passport | A passport number | Case-sensitive string |
| path | A path to a file, folder or process, also an HTTP request path | Path |
| pattern-in-file | A pattern inside a file | Regex |
| pattern-in-memory | A pattern in memory | Regex |
| pattern-in-traffic | A pattern in traffic | Regex |
| pgp-private-key | A private key of a popular encryption system | Case-sensitive string |
| pgp-public-key | A public key of a popular encryption system | Case-sensitive string |
| phone | A phone number | Phone |
| pnr | A Passenger Name Record Locator | Case-sensitive string |
| process | A running process | String |
| process-state | A state of a process | String |
| prtn | A premium-rate phone number | String |
| redress-number | A Redress Control Number | Case-sensitive string |
| regkey | A registry key | String |
| sha1 | A 160-bit fingerprint | SHA-1 |
| sha224 | A 224-bit fingerprint | SHA-224 |
| sha256 | A 256-bit fingerprint | SHA-256 |
| sha384 | A 384-bit fingerprint | SHA-384 |
| sha512 | A 512-bit fingerprint | SHA-512 |
| sha3-224 | A 224-bit fingerprint | SHA3-224 |
| sha3-256 | A 256-bit fingerprint | SHA3-256 |
| sha3-384 | A 384-bit fingerprint | SHA3-384 |
| sha3-512 | A 512-bit fingerprint | SHA3-512 |
| sha512-224 | A 224-bit fingerprint | SHA512-224 |
| sha512-256 | A 256-bit fingerprint | SHA512-256 |
| ssh-fingerprint | A fingerprint of SSH key material | Case-sensitive string |
| ssh-banner | An SSH Hello Banner | Case-sensitive string |
| ssr | A Special Service Request | Case-sensitive string |
| category | A label to classify things that share certain characteristics or qualities | String |
| threat | A circumstance or event that has the potential to harm a computer system | String |
| tiktok-profile | A TikTok user profile | URL |
| twitter-profile | A Twitter/X user profile | URL |
| url | An address of a specific resource on the internet | URL |
| username | An unique identifier that can be used to log in to a website | String |
| visa | A visa number | Case-sensitive string |
| whois-registrant | A person or organization who has registered a domain name | String |
| whois-registrar | A company or organization accredited by ICANN | String |
| windows-scheduled-task | A Windows scheduled task | String |
| windows-service-displayname | A Windows Service's display name | String |
| windows-service-name | A Windows Service's name | String |
| xmr | A Monero address | Case-sensitive string |
| breach | A security breach that resulted in a leak of PII | UUID |
| breach-date | Date of breach occurrence | Date |
| breach-count | Number of items leaked in the breach | Integer |
| breach-description | A detailed description of the breach | Case-sensitive string |
| postal-address | A postal address | String |
| zip-code | A number that identifies a specific geographic region also known as Postal Code | Case-sensitive string |
| port | A network ports used in combination with IP addresses | Port |
| os | A core software that manages a computer's hardware, software resources | String |
| command | A specific instruction you type within a CLI to tell the computer to perform a task | Case-sensitive string |

## Using Entity Types

When using the Search API, you can specify the entity type in your queries to search for specific types of entities. For example, when using the [Entity Lookup](/search/entity-lookup) endpoint, you can specify the entity type in the request body:

```json
{
  "type": "ip",
  "value": "8.8.8.8"
}
```

Similarly, when interpreting the results of a search query, the `type` field in the response indicates the type of entity, and the `attributes` field contains entity-specific attributes based on the entity type.

For more information on how to search for entities, see the [Simple Search](/search/simple-search) and [Advanced Search](/search/advanced-search) documentation.