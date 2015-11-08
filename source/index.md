---
title: API Reference

language_tabs:
  - shell: cURL
  - javascript: JavaScript
  - php: PHP

toc_footers:
  - <a href='http://github.com/tripit/slate'>Documentation Powered by Slate</a>

includes:
  - errors

search: false
---

# Introduction

Welcome to the LinchPin API docs! 

You can use our API to send and query all types of events happening around your company. You can later use our app or HTML5 sdk to build dashboards and display the most relevant performance indicators of your business.

We have examples in cURL, JavaScript, and Php to get you started. You can view code examples in the dark area to the right, and you can switch the programming language of the examples with the tabs in the top right.

# Authentication

> To authorize, use this code:

```javascript
some javascript code
```

```php
// some php code
```

```shell
# With shell, you can just pass the correct header with each request
curl "api_endpoint_here"
  -H "Authorization: meowmeowmeow"
```

> Make sure to replace `meowmeowmeow` with your API key.

LinchPin uses API keys to allow access to the API. You can register a new Kittn API key at our [developer portal](http://example.com/developers).

Kittn expects for the API key to be included in all API requests to the server in a header that looks like the following:

`Authorization: meowmeowmeow`

<aside class="notice">
You must replace <code>meowmeowmeow</code> with your personal API key.
</aside>

# Events API
## Send an Event
## Send batch

# Search API
## Introduction

The main operation of the search API is of course to execute a query. LinchPin queries are defined using a JSON object that adheres to a JSON Schema. If you're not familiar with the JSON schema spec you can read more about it [here](http://json-schema.org/).

## Search Schema

You can download the latest version of the schema [clicking here](http://search.linchpin.io/schema).

Over the next sections we'll explore in more detail every available option on the API.

## Running a query

The simplest query we can build specifies only the Event-Type that we want to search.

Try running the examples with one of your EventTypes using curl. You can use the commands to the right.

```
{
  "type": ["your-event-type"]
}
```

```shell
# With curl, just replace your-api-key with your key.
curl -XPOST "https://search.linchpin.io/search" -H "Content-type: application/json" -H "Authorization: Bearer your-api-key" -d'{
    "type":["your-event-type"]
  }'
```





## Get Last Event from EventType
## Get Event Count for EventType

# Kittens

## Get All Kittens

```ruby
require 'kittn'

api = Kittn::APIClient.authorize!('meowmeowmeow')
api.kittens.get
```

```python
import kittn

api = kittn.authorize('meowmeowmeow')
api.kittens.get()
```

```shell
curl "http://example.com/api/kittens"
  -H "Authorization: meowmeowmeow"
```

> The above command returns JSON structured like this:

```json
[
  {
    "id": 1,
    "name": "Fluffums",
    "breed": "calico",
    "fluffiness": 6,
    "cuteness": 7
  },
  {
    "id": 2,
    "name": "Isis",
    "breed": "unknown",
    "fluffiness": 5,
    "cuteness": 10
  }
]
```

This endpoint retrieves all kittens.

### HTTP Request

`GET http://example.com/api/kittens`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
include_cats | false | If set to true, the result will also include cats.
available | true | If set to false, the result will include kittens that have already been adopted.

<aside class="success">
Remember — a happy kitten is an authenticated kitten!
</aside>

## Get a Specific Kitten

```ruby
require 'kittn'

api = Kittn::APIClient.authorize!('meowmeowmeow')
api.kittens.get(2)
```

```python
import kittn

api = kittn.authorize('meowmeowmeow')
api.kittens.get(2)
```

```shell
curl "http://example.com/api/kittens/2"
  -H "Authorization: meowmeowmeow"
```

> The above command returns JSON structured like this:

```json
{
  "id": 2,
  "name": "Isis",
  "breed": "unknown",
  "fluffiness": 5,
  "cuteness": 10
}
```

This endpoint retrieves a specific kitten.

<aside class="warning">If you're not using an administrator API key, note that some kittens will return 403 Forbidden if they are hidden for admins only.</aside>

### HTTP Request

`GET http://example.com/kittens/<ID>`

### URL Parameters

Parameter | Description
--------- | -----------
ID | The ID of the kitten to retrieve

