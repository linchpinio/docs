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
## Send to a public endpoint
## Send batch

# Search API
## Introduction

The main operation of the search API is of course to execute a query. LinchPin queries are defined using a JSON object that adheres to a JSON Schema. If you're not familiar with the JSON schema spec you can read more about it on this link: (http://json-schema.org/).

## Search Schema

You can download the latest version of the schema [clicking here](http://search.linchpin.io/schema).

Over the next sections we'll explore in more detail every available option on the API.

## Running a query

The simplest query we can build specifies only the Event-Type that we want to search.

Try running the examples with one of your EventTypes using curl. You can use the commands to the right.

> This is a simple query

```json
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

> JSON response should look like this:

```json
{
  "results": [],
  "count": 12275
}
```

## Result size and pagination

First thing we'll add to our query is a property cleverly named "size", which specifies the number of events the will return from the Api. Currently the maximum is 50.

Let's try that.

> Query with size 5

```json
{
  "type": ["your-event-type"],
  "size": 5
}
```

```shell
# With curl, just replace your-api-key with your key.
curl -XPOST "https://search.linchpin.io/search" -H "Content-type: application/json" -H "Authorization: Bearer your-api-key" -d'{
    "type":["your-event-type"],
    "size": 5
  }'
```

Depending on your-event-type your results will have different events, but they should look like the respose to the right.

> JSON response should look like this:

```json
{
  "results": [
    {
      "CustomerID": "583290",
      "Price": 67.552,
      "Qty": 3,
      "Sku": "SKU0g"
    },
    {
      "CustomerID": "583290",
      "Price": 67.552,
      "Qty": 3,
      "Sku": "SKU0g"
    },
    {
      "CustomerID": "583290",
      "Price": 67.552,
      "Qty": 3,
      "Sku": "SKU0g"
    },
    {
      "CustomerID": "583290",
      "Price": 67.552,
      "Qty": 3,
      "Sku": "SKU0g"
    },
    {
      "CustomerID": "322780",
      "Price": 77.36,
      "Qty": 9,
      "Sku": "SKU5b"
    }
  ],
  "count": 116
}
```

The search API always returns the total number of events in your series. If you need to paginate, you can do so, using the "offset" property.

Offset is an `integer` number, and refers to the number of pages that you want to skip. The default value is 0, so you always get the first page.

```json
{
  "type": ["your-event-type"],
  "size": 5,
  "offset":1
}
```

```shell
# With curl, just replace your-api-key with your key.
curl -XPOST "https://search.linchpin.io/search" -H "Content-type: application/json" -H "Authorization: Bearer your-api-key" -d'{
  "type": ["your-event-type"],
  "size": 5,
  "offset":1
}'
```

## Event properties

Sometimes you don't want to explore or return your entire events, specially if they're big objects. To request only specific properties, you can select them using the `event-properties` property on your query.

For example, if your EventType looks like the json object to the right:

```json
  {
    "CustomerID": "583290",
    "Price": 67.552,
    "Qty": 3,
    "Sku": "SKU0g"
  }
```

And you only want to see our events `Qty` and `Sku` properties, then we can build our query like this:

```json
{
    "type":["1xxfqa"],
    "size": 5,
    "event-properties":{
       "include":["Qty","Sku"] 
    }
}
```

Results will look like:

```json
{
  "results": [
    {
      "Qty": 3,
      "Sku": "SKU0g"
    },
    {
      "Qty": 3,
      "Sku": "SKU0g"
    },
    {
      "Qty": 3,
      "Sku": "SKU0g"
    },
    {
      "Qty": 3,
      "Sku": "SKU0g"
    },
    {
      "Qty": 9,
      "Sku": "SKU5b"
    }
  ],
  "count": 116
}
```

## Timeframe

Next, we'll learn how to query events restricted to a specific timeframe. There are two main options to this: use relative time, or use a specific timeframe.

Relative time means we can select events coming in during the last 'X' seconds, minutes, hours, days, weeks, months or years.

To do that, we specifiy a `timeframe` property inside our query object with a string value in the format: `'last_X_seconds'` where X can be replaced by an integer.

You can also replace `'seconds'` by one of the options above like hours or weeks.

You can see some examples on the right.

```shell
curl -XPOST "https://search.linchpin.io/search" -H "Content-type: application/json" -H "Authorization: Bearer your-api-key" -d'
{
    "type":["1xxfqa"],
    "size": 5,
    "timeframe":"last_3_days"
}
'

curl -XPOST "https://search.linchpin.io/search" -H "Content-type: application/json" -H "Authorization: Bearer your-api-key" -d'
{
    "type":["1xxfqa"],
    "size": 5,
    "timeframe":"last_1_weeks"
}
'
```

Another way of expressing a relative time query, is to use a predefined set of keywords like: 

- today
- yesterday
- this_week
- last_week
- this_month
- last_month
- this_quarter
- last_quarter
- this_year
- last_year

This options get a new value at each moment the query gets executed.

```shell
curl -XPOST "https://search.linchpin.io/search" -H "Content-type: application/json" -H "Authorization: Bearer your-api-key" -d'
{
    "type":["1xxfqa"],
    "size": 5,
    "timeframe":"this_year"
}
'
```

Another way of expressing a timeframe is to use specific `start` and `end` values and forming an object. Our start and end values should always be expressed in UTC and using the `ISO 8601` format, so they look like this: `2014-09-08T08:02:17.323Z`.

For example a query with start and end timeframe values will looks like: 

```shell
curl -XPOST "https://search.linchpin.io/search" -H "Content-type: application/json" -H "Authorization: Bearer your-api-key" -d'
{
    "type":["1xxfqa"],
    "size": 5,
    "timeframe":{
      "start":"2015-10-08T00:00:00.000Z",
      "end":"2015-11-08T00:00:00.000Z"
    }
}
'
```

To facilitate creating querys where you need a fixed date in the past, and get all events until this moment, then you can use the string `now` as the value for end, like the example on the right.

```shell
curl -XPOST "https://search.linchpin.io/search" -H "Content-type: application/json" -H "Authorization: Bearer your-api-key" -d'
{
    "type":["1xxfqa"],
    "size": 5,
    "timeframe":{
      "start":"2015-01-08T00:00:00.000Z",
      "end":"now"
    }
}
'
```

## Filters

Filters allow you to select from your events based on a specific property. For example, let's say you're storing sales events and they looked like :

```json
    {
      "CustomerID": "583290",
      "Price": 67.552,
      "Qty": 3,
      "Sku": "SKU0g"
    }
```

You can write a query to filter and show only the events that match a specific `CustomerID` or a certain `Sku`.

```shell
curl -XPOST "https://search.linchpin.io/search" -H "Content-type: application/json" -H "Authorization: Bearer your-api-key" -d'
{
    "type":["1xxfqa"],
    "size": 5,
    "filter": {
        "property": "Sku",
        "operator": "match",
        "value": "SKU5b",
        "type": "and"
    }
}
'
```

You're not limited to a single Filter on a query, in fact, you can build an array of filters.

Filters have a property named `type` that can have 3 possible values:

- must
- should
- must_not

They allow you to specify the boolean logic that the filters will be applied with. It's important to note, that order within the filters array is not important, as every filter is evaluated independently. `must` type filters are conditions that are always met, even if you have several of them. They are the equivalent of an `and` logic clause. If you have two or more `should` type filters, then every event in the results will meet at least one of them. Several `should` filters act like `or` clauses. Finally `must_not` filters are also enforced with a negated logic. 


```shell
curl -XPOST "https://search.linchpin.io/search" -H "Content-type: application/json" -H "Authorization: Bearer your-api-key" -d'
{
    "type":["1xxfqa"],
    "size": 5,
    "filter": [{
        "property": "Sku",
        "operator": "match",
        "value": "SKU5b",
        "type": "should"
    },
    {
        "property": "Sku",
        "operator": "match",
        "value": "SKU0g",
        "type": "should"
    }
    ]
}
'

curl -XPOST "https://search.linchpin.io/search" -H "Content-type: application/json" -H "Authorization: Bearer your-api-key" -d'
{
    "type":["1xxfqa"],
    "size": 5,
        "filter": [{
        "property": "Sku",
        "operator": "match",
        "value": "SKU0g",
        "type": "must_not"
    }]
}
'

curl -XPOST "https://search.linchpin.io/search" -H "Content-type: application/json" -H "Authorization: Bearer your-api-key" -d'
{
    "type":["1xxfqa"],
    "size": 5,
        "filter": [{
        "property": "Qty",
        "operator": "gt",
        "value": 5,
        "type": "must"
    }]
}
'

```
## Behaviors

Behaviors are special flags that you can include with your query object, and allow you to enhance or alter the structure of your results.

### Search By

`search-by` behavior is just a shortcut to sort your results by either CreatedTime or LastUpdateTime. `search-by` takes a string value and only the following keywords are allowed:

- created
- updated

### include-linchpin-object

`include-linchpin-object` adds the normally "hidden" internal LinchPin object to your results. It is a `boolean` value, so just set it to true to enable it.

### compare-to-previous

`compare-to-previous` is a behavior that allows you query the prior period if you include a timeframe. It's also a `boolean` and it will not affect results if timeframe is not selected.

## Facets

### Interval or Date Histogram

The interval facet returns a date-histogram. In its simplest form, it takes only a string declaring the time interval for the buckets.

```shell
curl -XPOST "https://search.linchpin.io/search" -H "Content-type: application/json" -H "Authorization: Bearer your-api-key" -d'
{
	"type": ["sales"],
    "facet": {
        "interval":"day"
    }
}
'
```

> Results would look like:

```json
{
  "results": [],
  "count": 116,
  "buckets": [
    {
      "count": 36,
      "time": "2015-09-29T00:00:00.000"
    },
    {
      "count": 7,
      "time": "2015-10-04T00:00:00.000"
    },
    {
      "count": 41,
      "time": "2015-10-11T00:00:00.000"
    },
    {
      "count": 32,
      "time": "2015-10-18T00:00:00.000"
    }
  ]
}
````

### Terms Histogram

Sometimes you need to create a histogram but instead of grouping events by a numeric range, you want buckets of distinct string values of a property. 

For instance, in our `test_sales` events, you might want to understand the number of sales per `Sku` or get the top 3 selling `Sku`'s.

```shell
curl -XPOST "https://search.linchpin.io/search" -H "Content-type: application/json" -H "Authorization: Bearer your-api-key" -d'
{
	"type": ["1xxfqa"],
    "facet": {
        "terms":{
           "property":"Sku",
           "size":3
        }
    }
}
'
```

```json
{
  "results": [],
  "count": 116,
  "buckets": [
    {
      "count": 34,
      "term": "SKU0g"
    },
    {
      "count": 40,
      "term": "SKU5b"
    },
    {
      "count": 42,
      "term": "7WYVRT"
    }
  ]
}
```

Results include a `count` property with the total number of events, in this case, 116.

Results also return a buckets array with two properties each: `count` and `term`. `count` is the number of events in that bucket, and `term` is the property value for that bucket. In this case, "SKU0g" is the value of 34 `test_sales` events.

With this information you can build a pareto or a pie chart.

You may have noticed that `count` in the previous query refers to the number of `test_sales` events and not the quantity of items sold that is specified in the `Qty` property of our events.

If you wanted to get the sum of the total no. of items sold, you will need to build your query as follows:

```json
{
    "type": ["1xxfqa"],
    "facet": {
        "terms":{
           "property":"Sku",
           "size":3,
           "aggregation":{
	           "property":"Qty",
	           "aggregation":"sum"
           }
        }
    }
}

```

By adding an `aggregation` object inside our terms facet, we can alter our results to include a `value` property inside each bucket, that specifies the `sum` of `Qty` property values for each bucket. 

Instead of a `sum`, other possible values for aggregation are:
- avg
- min
- max

```shell
curl -XPOST "https://search.linchpin.io/search" -H "Content-type: application/json" -H "Authorization: Bearer your-api-key" -d'
{
    "type": ["1xxfqa"],
    "facet": {
        "terms":{
           "property":"Sku",
           "size":3,
           "aggregation":{
	           "property":"Qty",
	           "aggregation":"sum"
           }
        }
    }
}
'
```

> Result

```json
{
  "results": [],
  "count": 116,
  "buckets": [
    {
      "count": 42,
      "value": 42,
      "term": "7WYVRT"
    },
    {
      "count": 34,
      "value": 102,
      "term": "SKU0g"
    },
    {
      "count": 40,
      "value": 360,
      "term": "SKU5b"
    }
  ]
}
```

In this case, our store sold 360 items of `Sku` "SKU5b" in 40 different `test_sale` events.

### Numeric Histogram

Numeric histograms provide a good shortcut to create a histogram on numeric values just by specifying an integer interval.

For example, if we wanted to a histogram breaking down our `test_sales` event by the `Qty` of items sold in each we would do:

```shell
curl -XPOST "https://search.linchpin.io/search" -H "Content-type: application/json" -H "Authorization: Bearer 79dd2563905af91fa8011486cd911b25f7af151b" -d'
{
    "type": ["1xxfqa"],
    "timeframe": "this_year",
    "facet": {
        "histogram": {
            "property":"Qty",
            "interval":2
        }
    }
}
'
```

The results will automatically create buckets with an incremenet of 2 between them.

> Results:
```json
{
  "results": [],
  "count": 116,
  "buckets": [
    {
      "count": 42,
      "bucket": 0
    },
    {
      "count": 34,
      "bucket": 2
    },
    {
      "count": 0,
      "bucket": 4
    },
    {
      "count": 0,
      "bucket": 6
    },
    {
      "count": 40,
      "bucket": 8
    }
  ]
}
```

Bucket values represent a range. For instance bucket:0, the range 0-2, bucket:2, the range 2-4, and so on.

If you wanted to have more control over the bucket size, then read on to the next section on Ranges.

### Ranges Facet

As stated above, ranges allow you to control the bucketing for a histogram on numeric values.


### Stats Facet

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

