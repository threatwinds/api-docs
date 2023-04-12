---
layout: default
title: Example 3
parent: Use Cases
nav_order: 3
---

# Website Reputation Chrome Extension

A developer is designing a chrome extension to provide users with real-time updates about the reputation and related information of websites they visit. The extension will leverage a script that extracts the domain name of the current website the user is on, and then utilize our service to retrieve any available information about the website's reputation and any associated malware.


**STEP 1** 

The first step is to check whether the domain name, hostname, and URL already exist in our dataset, in that order. Before we can search for the information, we must extract the necessary data from the browser every time the URL changes in the active tab of the Chrome browser.

```js
chrome.tabs.onUpdated.addListener(function(tabId, changeInfo, tab) {
  if (changeInfo.url) {
    // Initialize an empty dictionary
    pageData = {};
    // Extract the hostname, domain, and URL and save it in the dictionary
    pageData['url'] = new URL(changeInfo.url);
    pageData['hostname'] = url.hostname;
    pageData['domain'] = url.hostname.split('.').slice(-2).join('.');
  }
});

```

To determine if any of these elements are in our dataset, we can use the [Get Entity by Type and Value](./search/ENTITY#entityTypeValue) endpoint for a quick check.

For example, suppose a user enters tho this url: `http://support.clz.kr/soft_hair/thisIsAVirus.exe`. In this case, we need to check if:

* The domain is in ThreatWinds and has a negative reputation.
* The hostname is in ThreatWinds and has a negative reputation.
* The URL is in ThreatWinds and has a negative reputation.

{: .note}
We can perform a more advanced search by checking for relations of any of these elements in the system and looking for negative reputation, repeating this process for multiple levels.

The code should looks like:

```js
// Define the API endpoint and headers
const apiUrl = 'https://intelligence.threatwinds.com/api/search/v1/entity';
const headers = {
  'Accept': 'application/json',
  'Content-Type': 'application/json'
};

// Iterate over the dictionary keys and values
for (const [key, value] of Object.entries(pageData)) {
  // Make a POST request to the API for each key-value pair
  const requestBody = {
    'type': key,
    'value': value
  };

  fetch(apiUrl, {
    method: 'POST',
    headers: headers,
    body: JSON.stringify(requestBody)
  })
  .then(response => response.json())
  .then(data => {
    // Do something with the API response
  })
  .catch(error => {
    console.error(`Error calling API for ${key} '${value}':`, error);
  });
}

```

The API response will contain the entity if it is found, or a 404 error if it is not found. For more information about the definition of a specific entity, you can check the [Definition page](../DEFINITIONS).

Continuing with the previous example, suppose we didn't find any information about the domain or the hostname, but we did find information about the URL.

An example of the information we might receive from the API could look like this:

```json
{
  "@timestamp": "2023-04-12T13:49:30.178544578Z",
  "accuracy": 1,
  "attributes": {
    "url": "http://support.clz.kr/soft_hair/thisIsAVirus.exe"
  },
  "id": "url-abe8ccb7784c8fabd6d25a6784ab21319abd4c55ba17f3288d0946dfa04cc1e9",
  "reputation": -1,
  "tags": [],
  "type": "url"
}
```


**STEP 2** Now, you can create an alert for the user, informing them that they have a considerable risk on this website based on its negative reputation:

```js
// remember the API response is stored in a variable called 'data'

// check if the reputation is negative
if (response.reputation < 0) {
  // create a div element to show the message
  const alertDiv = document.createElement("div");
  alertDiv.style = `
    position: fixed;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    background-color: #ff6666;
    padding: 20px;
    border-radius: 10px;
    box-shadow: 0 0 10px rgba(0, 0, 0, 0.5);
    font-family: Arial, sans-serif;
    font-size: 16px;
    text-align: center;
  `;
  // create a message to show to the user
  const message = `Warning! The following URL has a negative reputation: ${response.attributes.url}`;
  const messageText = document.createTextNode(message);
  alertDiv.appendChild(messageText);
  // add the div element to the document
  document.body.appendChild(alertDiv);
}
```