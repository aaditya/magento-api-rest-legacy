# Magento API REST

A Node.js client wrapper to work with the Magento REST API.

[![npm version](https://badge.fury.io/js/magento-api-rest.svg)](https://www.npmjs.com/package/magento-api-rest)
[![dependencies Status](https://david-dm.org/aadityachakravarty/magento-api-rest/status.svg)](https://david-dm.org/aadityachakravarty/magento-api-rest)

## Installation

```
npm i magento-api-rest
```

## Getting started

Generate API credentials by following these instructions <https://devdocs.magento.com/guides/v2.3/get-started/create-integration.html>.

Make sure to check the resource access is as per your requirements to prevent misuse of the API Keys.

Check out the Magento 2 API endpoints and data that can be manipulated in <https://devdocs.magento.com/redoc/2.3/index.html>.

Or, if you are using Magento 1.X, refer to [these](https://devdocs.magento.com/guides/m1x/api/rest/introduction.html) documentation.

* This library is compatible with Magento 2 as well as Magento 1.X REST Endpoints.

## Setup

Setup for the Magento REST API integration:

```js
const MagentoAPI = require('magento-api-rest');

const client = new MagentoAPI({
    'url': 'http://www.test.com',
    'consumerKey': '<OAuth 1.0a consumer key>',
    'consumerSecret': '<OAuth 1.0a consumer secret>',
    'accessToken': '<OAuth 1.0a access token>',
    'tokenSecret': '<OAuth 1.0a access token secret>',
    'version': '2'
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
| `version`           | `Number`  | no       | Magento Store Version, default is 2                        |
| `type`              | `String`  | no       | Magento endpoint type, default is 'V1'                     |

* If you want to use the [Asynchronous Endpoints](https://devdocs.magento.com/guides/v2.3/rest/asynchronous-web-endpoints.html) set `type` to `async/V1`.

* If you want to use the [Bulk Endpoints](https://devdocs.magento.com/guides/v2.3/rest/bulk-endpoints.html) set `type` to `async/bulk/V1`.

* *Type* parameter is only valid for Magento Versions 2 and above.

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

For Magento 2 REST API:

```js
let params = {
    "filterGroups": [
        {
            "filters": [
                {
                    "field": "created_at",
                    "value": "2019-08-03 11:22:47",
                    "condition_type": "from"
                }
            ],
            "filters": [
                {   
                    "field": "created_at",
                    "value": "2020-08-03 11:22:47",
                    "condition_type": "to"
                }
            ]
        }
    ],
    "sortOrders": [
        {
            "field": "created_at",
            "direction": "desc"
        }
    ],
    "pageSize": 200,
    "currentPage": 1
}
```
Or, you can use the inbuilt parser to write the above query as:
```js
let params = {
    $or: [
        { $from: "2019-08-03 11:22:47" },
        { $to: "2020-08-03 11:22:47" }
    ],
    $sort: {
        "created_at": "desc"
    },
    $perPage: 200,
    $page: 1
}
```
For Magento 1 REST API:

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

* Note: You cannot use both the param writing styles together.

* Parser works for Magento Versions 2 and above only.

#### Parser Operators

| Operator | Description |
|---|---|
| $or | Execute OR queries. |
| $from | Starting point of order search via ISO date. Requires $to. |
| $to | Starting point of order search via ISO date. |
| $after | Search after a specific ISO date. |
| $before | Search before a specific ISO date. |
| $sort | Sort the orders, see docs for more. |
| $perPage | Specifies the per page orders. |
| $page | Specifies the current page. |

* By default { key: value } translates to an "eq" operation where key = value.

To get more information as to how to form queries natively, use the following reference,
<https://devdocs.magento.com/guides/v2.3/rest/performing-searches.html>.

If you want to use the above object in a request,
```js
function getOrders () {
   client.get('orders', params).then((response) => {
    //  Response Handling
   }).catch((error) => {
    //  Error Handling
   })
}
```
or by async await,

```js
async function getOrders () {
    try {
        let response = await client.get('orders', params);
        // Response Handling
    } catch (e) {
        // Error Handling
    }
}
```

## To Do

* Add test cases.
