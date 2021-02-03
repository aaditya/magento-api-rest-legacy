# Magento API REST (Legacy)

A Node.js client wrapper to work with the Magento 1.X REST API.

[![npm version](https://badge.fury.io/js/magento-api-rest-legacy.svg)](https://www.npmjs.com/package/magento-api-rest-legacy)
[![dependencies Status](https://david-dm.org/aaditya/magento-api-rest-legacy/status.svg)](https://david-dm.org/aaditya/magento-api-rest-legacy)

## Installation

```
npm i magento-api-rest-legacy
```

## Getting started

Generate API credentials by following [these instructions](https://devdocs.magento.com/guides/m1x/api/rest/introduction.html).

Make sure to check the resource access is as per your requirements to prevent misuse of the API Keys.

* This library is compatible only with Magento 1.X REST Endpoints for Magento 2.X based stores, use the [sister package](https://www.npmjs.com/package/magento-api-rest).

## Setup

Setup for the Magento REST API integration:

```js
const MagentoAPI = require('magento-api-rest-legacy');

const client = new MagentoAPI({
    'url': 'http://www.your-store.dev',
    'consumerKey': '<OAuth 1.0a consumer key>',
    'consumerSecret': '<OAuth 1.0a consumer secret>',
    'accessToken': '<OAuth 1.0a access token>',
    'tokenSecret': '<OAuth 1.0a access token secret>'
});
```

### Options

| Option              | Type      | Required | Description                                                |
|---------------------|-----------|----------| -----------------------------------------------------------|
| `url`               | `String`  | yes      | Your Store URL                                             |
| `consumerKey`       | `String`  | yes      | Your API consumer key                                      |
| `consumerSecret`    | `String`  | yes      | Your API consumer secret                                   |
| `accessToken`       | `String`  | yes      | Your API Access Token                                      |
| `tokenSecret`       | `String`  | yes      | Your API Access Token Secret                               |
| `timeout`           | `Number`  | no       | Request Timeout                                            |
| `axiosConfig`       | `Object`  | no       | [Reference](https://github.com/axios/axios#request-config) |

## Methods

### GET

- `.get(endpoint)`
- `.get(endpoint, params)`

| Params     | Type     | Description                                                   |
|------------|----------|---------------------------------------------------------------|
| `endpoint` | `String` | Magento API endpoint, example: `orders`                       |
| `params`   | `Object` | JSON object to be sent as params.                             |

* Note: In case no params are specified or required, you can leave params as empty. 
That will result in "?searchCriteria=all" in the URL. 

### POST

- `.post(endpoint, data)`

| Params     | Type     | Description                                                 |
|------------|----------|-------------------------------------------------------------|
| `endpoint` | `String` | Magento API endpoint, example: `shipments`                  |
| `data`     | `Object` | JSON object to be sent as body.                             |

### PUT

- `.put(endpoint, data)`

| Params     | Type     | Description                                                 |
|------------|----------|-------------------------------------------------------------|
| `endpoint` | `String` | Magento API endpoint, example: `shipments/12`               |
| `data`     | `Object` | JSON object to be sent as body.                             |

### DELETE

- `.delete(endpoint, data)`

| Params     | Type     | Description                                                     |
|------------|----------|-----------------------------------------------------------------|
| `endpoint` | `String` | Magento API endpoint, example: `orders/12`                      |
| `data`     | `Object` | JSON object to be sent as body.                                 |

### API

Requests are made with [Axios library](https://github.com/axios/axios) with [support to promises](https://github.com/axios/axios#promises).

```js
let params = {
    "filter": [
        {
            "attribute": "entity_id",
            "neq": 3,
            "in": [1,2,3],
            "nin": [1,2,3],
            "gt": 3,
            "lt": 3,
            "from": "Date",
            "to": "Date"
        }
    ],
    "page": 1,
    "order": "name",
    "dir": "dsc", // Or asc
    "limit": 100
}
```

If you want to use the above object in a request,
```js
async function getOrders () {
    try {
        let response = await client.get('orders', params);
        // Response Handling
    } catch (error) {
        // Error Handling
    }
}
```