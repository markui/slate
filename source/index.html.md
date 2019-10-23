---
title: Documentation

language_tabs: # must be one of https://git.io/vQNgJ
  - shell
  - javascript
  - python
  - ruby

toc_footers:
  - <a href='https://pdfswitch.io/signup'>Signup for PDFSwitch</a>
  - <support@pdfswitch.io>

includes:
  - errors

search: true
---

# Getting Started

## Overview

Welcome to PDFSwitch!
PDFSwitch provides a simple and robust REST-ful API to integrate HTML to PDF conversion technology into your existing applications and services.

The PDFSwitch API enables the following operations:

- Conversion of raw HTML/URL to PDF documents
- Get result PDF in binary format OR saved in AWS S3 storage
- Customization of your PDF with css, javascript, headers/footers and much more

If you have further questions or feedback you would like to share, always feel free to contact us at <support@pdfswitch.io>. We would love to hear from you :)

## Getting an API Key

PDFSwitch uses API keys to allow access to the API. You can find your API keys on your [Dashboad](https://pdfswitch.io/dashboard), which you can access by [logging in](https://pdfswitch.io/login) or [signing up](https://pdfswitch.io/signup).

## Authentication

PDFSwitch expects the API key to be included in all API requests to the server via [HTTP Bearer Auth Scheme](https://tools.ietf.org/html/rfc6750). Provide your API key in the `Authorization` header with the `Bearer` authentication scheme.

`Authorization: Bearer YOUR_PDFSWITCH_API_KEY`

<aside class="notice">
You must replace <code>YOUR_PDFSWITCH_API_KEY</code> with your personal API key.
</aside>

# Conversion

## Convert to PDF

This is the main endpoint of PDFSwitch's API. Simply pass your raw HTML OR URL to us and get a PDF in return.

### HTTP Request

`POST https://pdfswitch.com/api/v1/convert`

### BODY Parameters

| Parameter        | Type                    | Default         | Description                                                                                                                                                                                                                                                                                                                                                                                     |
| ---------------- | ----------------------- | --------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| source           | string(raw HTML or URL) | <b>required</b> | The target document to convert to PDF.<br>1. URL Format: The URL of a web page you want to convert.<br><b>Example: </b>`https://example.com`<br>2. Raw HTML Format: Raw HTML string of a page that you want to convert.<br><b>Example: </b>`<html><h1>Test</h1></html>`                                                                                                                         |
| page_ranges      | string                  | null            | Returns only specified pages of the PDF document. If not provided, it will return all pages.<br><b>Pattern: </b><code>1&#124;5-6</code>                                                                                                                                                                                                                                                         |
| page_orientation | string                  | portrait        | Page Orientation<br><b>Pattern: </b><code>portrait&#124;landscape</code>                                                                                                                                                                                                                                                                                                                        |
| page_format      | string                  | A4              | Page size for converted pdf<br><b>Pattern: </b><code>Letter&#124;Legal&#124;Tabloid&#124;Ledger&#124;A0&#124;A1&#124;A2&#124;A3&#124;A4&#124;A5&#124;A6</code>                                                                                                                                                                                                                                  |
| page_width       | string                  | null            | Custom pdf page width. Use it together with custom `page_height`. <br>Unit can be one of <code>px&#124;in&#124;mm&#124;cm</code>.<br><b>Pattern: </b><code>NUM[px&#124;in&#124;mm&#124;cm]</code><br><b>Example: </b><code>600px</code><aside class="warning">Cannot be used together with `page_format`. Prefer to use `page_format` if you don't have an exact page size requirement.</aside> |
| page_height      | string                  | null            | Custom pdf page width. Use it together with custom `page_width`. <br>Unit can be one of <code>px&#124;in&#124;mm&#124;cm</code>.<br><b>Pattern: </b><code>NUM[px&#124;in&#124;mm&#124;cm]</code><br><b>Example: </b><code>600px</code><aside class="warning">Cannot be used together with `page_format`. Prefer to use `page_format` if you don't have an exact page size requirement.</aside>  |
| emulate_media    | string                  | screen          | Force CSS media emulation for print or screen.<br><b>Pattern: </b><code>screen&#124;print</code>                                                                                                                                                                                                                                                                                                |
| margins          | string                  | true            | If set to false, the result will include kittens that have already been adopted.                                                                                                                                                                                                                                                                                                                |
| zoom             | number                  | 1               | Allows you to increase the zoom in the document. **It should be a float value between 0.1 and 2.** <br>For example, the default 1 means 100% zoom(original state) on the document, 2 means 200% zoom, 0.1 means 10% zoom and so on.                                                                                                                                                             |

| Parameter    | Type  | Default                                                                          | Description |
| ------------ | ----- | -------------------------------------------------------------------------------- | ----------- |
| include_cats | false | If set to true, the result will also include cats.                               |
| available    | true  | If set to false, the result will include kittens that have already been adopted. |

# Credits

# Examples

## Example

> To authorize, use this code:

```ruby
require 'kittn'

api = Kittn::APIClient.authorize!('meowmeowmeow')
```

```python
import kittn

api = kittn.authorize('meowmeowmeow')
```

```shell
# With shell, you can just pass the correct header with each request
curl "api_endpoint_here"
  -H "Authorization: meowmeowmeow"
```

```javascript
const kittn = require("kittn");

let api = kittn.authorize("meowmeowmeow");
```

> Make sure to replace `meowmeowmeow` with your API key.

# Kittens

## Get All Kittens

Kittn expects for the API key to be included in all API requests to the server in a header that looks like the following:

`Authorization: meowmeowmeow`

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

```javascript
const kittn = require("kittn");

let api = kittn.authorize("meowmeowmeow");
let kittens = api.kittens.get();
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
    "name": "Max",
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

| Parameter    | Default | Description                                                                      |
| ------------ | ------- | -------------------------------------------------------------------------------- |
| include_cats | false   | If set to true, the result will also include cats.                               |
| available    | true    | If set to false, the result will include kittens that have already been adopted. |

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

```javascript
const kittn = require("kittn");

let api = kittn.authorize("meowmeowmeow");
let max = api.kittens.get(2);
```

> The above command returns JSON structured like this:

```json
{
  "id": 2,
  "name": "Max",
  "breed": "unknown",
  "fluffiness": 5,
  "cuteness": 10
}
```

This endpoint retrieves a specific kitten.

<aside class="warning">Inside HTML code blocks like this one, you can't use Markdown, so use <code>&lt;code&gt;</code> blocks to denote code.</aside>

### HTTP Request

`GET http://example.com/kittens/<ID>`

### URL Parameters

| Parameter | Description                      |
| --------- | -------------------------------- |
| ID        | The ID of the kitten to retrieve |

## Delete a Specific Kitten

```ruby
require 'kittn'

api = Kittn::APIClient.authorize!('meowmeowmeow')
api.kittens.delete(2)
```

```python
import kittn

api = kittn.authorize('meowmeowmeow')
api.kittens.delete(2)
```

```shell
curl "http://example.com/api/kittens/2"
  -X DELETE
  -H "Authorization: meowmeowmeow"
```

```javascript
const kittn = require("kittn");

let api = kittn.authorize("meowmeowmeow");
let max = api.kittens.delete(2);
```

> The above command returns JSON structured like this:

```json
{
  "id": 2,
  "deleted": ":("
}
```

This endpoint deletes a specific kitten.

### HTTP Request

`DELETE http://example.com/kittens/<ID>`

### URL Parameters

| Parameter | Description                    |
| --------- | ------------------------------ |
| ID        | The ID of the kitten to delete |
