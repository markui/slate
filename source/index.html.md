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

If you have further questions or feedback you would like to share with us, always feel free to contact us at <support@pdfswitch.io>. We would love to hear from you :)

> API Endpoint

```
https://pdfswitch.com/api/v1/switch
```

## Getting an API Key

PDFSwitch uses API keys to allow access to the API. You can find your API keys on your [Dashboad](https://pdfswitch.io/dashboard), which you can access by [logging in](https://pdfswitch.io/login) or [signing up](https://pdfswitch.io/signup).

## Authentication

PDFSwitch expects the API key to be included in all API requests to the server via [HTTP Bearer Auth Scheme](https://tools.ietf.org/html/rfc6750). Provide your API key in the `Authorization` header with the `Bearer` authentication scheme.

`Authorization: Bearer YOUR_PDFSWITCH_API_KEY`

<aside class="notice">
You must replace <code>YOUR_PDFSWITCH_API_KEY</code> with your personal API key.
</aside>

## Errors

# Conversion API

## Convert to PDF

This is the main endpoint of PDFSwitch's API. Simply pass your raw HTML or URL to us and get a PDF in return.

### HTTP Request

`POST https://pdfswitch.com/api/v1/convert`

### BODY Parameters

All parameters should be passed as a JSON object. We grouped the parameters into sections by their main usage.

#### 1. Main Options

Default options to manipulate our PDF render engine.

| Parameter        | Type                    | Default         | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
| ---------------- | ----------------------- | --------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| source           | string(raw HTML or URL) | <b>required</b> | The target document to convert to PDF.<br><b>1. URL Format:</b> The URL of a web page you want to convert.<br><b>Example: </b>`https://example.com`<br><b>2. HTML Format:</b> Raw HTML string of a page that you want to convert.<br><b>Example: </b>`<html><h1>Test</h1></html>`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
| page_ranges      | string                  | null            | Returns only specified pages of the PDF document. If not provided, it will return all pages by default.<br><b>Pattern: </b><code>1&#124;5-6</code>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
| page_orientation | string                  | portrait        | Page orientation<br><b>Pattern: </b><code>portrait&#124;landscape</code><br>cf) <a href="https://en.wikipedia.org/wiki/Page_orientation#/media/File:800x518_smartphone_portrait_vs_landscape_orientation.png" target="_blank">Portrait vs Landscape</a>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
| page_format      | string                  | A4              | Page size for converted pdf<br><b>Pattern: </b><code>Letter&#124;Legal&#124;Tabloid&#124;Ledger&#124;A0&#124;A1&#124;A2&#124;A3&#124;A4&#124;A5&#124;A6</code><br>cf) <a href="https://www.engineeringtoolbox.com/drawing-sheet-sizes-d_108.html" target="_blank">A0~A6 Sizes</a>, <a href="https://www.engineeringtoolbox.com/office-paper-sizes-d_213.html" target="_blank">Letter~Ledger Sizes</a>                                                                                                                                                                                                                                                                                                                                                                                                                                               |
| page_width       | string                  | null            | Custom pdf page width. Use it together with custom `page_height`. <br>Unit can be one of <code>px&#124;in&#124;mm&#124;cm</code>.<br><b>Pattern: </b><code>NUM[px&#124;in&#124;mm&#124;cm]</code><br><b>Example: </b><code>600px</code><aside class="warning">Cannot be used together with `page_format`. Prefer to use `page_format` if you don't have an exact page size requirement.</aside>                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| page_height      | string                  | null            | Custom pdf page height. Use it together with custom `page_width`. <br>Unit can be one of <code>px&#124;in&#124;mm&#124;cm</code>.<br><b>Pattern: </b><code>NUM[px&#124;in&#124;mm&#124;cm]</code><br><b>Example: </b><code>600px</code><aside class="warning">Cannot be used together with `page_format`. Prefer to use `page_format` if you don't have an exact page size requirement.</aside>                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| emulate_media    | string                  | screen          | Force CSS media emulation for print or screen. The print option controls how your page looks when printed.<br><b>Pattern: </b><code>screen&#124;print</code>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| margins          | object                  | null            | CSS style margin sizes. Use it to give space between the limits of the document and captured content.<br><ul><li>**top**[string]: Space between the top and the content<br>Pattern: <code>NUM[px&#124;in&#124;mm&#124;cm]</code></li><li>**right**[string]: Space between the right and the content<br>Pattern: <code>NUM[px&#124;in&#124;mm&#124;cm]</code></li><li>**bottom**[string]: Space between the bottom and the content<br>Pattern: <code>NUM[px&#124;in&#124;mm&#124;cm]</code></li><li>**left**[string]: Space between the left and the content<br>Pattern: <code>NUM[px&#124;in&#124;mm&#124;cm]</code></li></ul><b>Example: </b><code>{"top": "100px", "right": "0px", "bottom": "0px", "left": "300px" }</code><br><br>cf) _If a certain key`[top, right, bottom, left]` is not defined, the value for the key is `0px` by default._ |
| zoom             | number                  | 1               | Allows you to increase the zoom in the document. It should be a **float value between 0.1 and 2.** Value larger than 1 means zooming in(larger), smaller than 1 means zooming out(smaller).<br>**For example,** the default 1 means 100% zoom(original state) on the document, 2 means 200% zoom(zoom in), 0.1 means 10% zoom(zoom out) and so on.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |

#### 2. Injection Options

Options to inject elements on PDF render.

| Parameter     | Type                    | Default         | Description                                                                                                                                                                                                                                                             |
| ------------- | ----------------------- | --------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| css           | string(raw HTML or URL) | <b>required</b> | The target document to convert to PDF.<br>1. URL Format: The URL of a web page you want to convert.<br><b>Example: </b>`https://example.com`<br>2. Raw HTML Format: Raw HTML string of a page that you want to convert.<br><b>Example: </b>`<html><h1>Test</h1></html>` |
| javascript    | string                  | null            | Returns only specified pages of the PDF document. If not provided, it will return all pages.<br><b>Pattern: </b><code>1&#124;5-6</code>                                                                                                                                 |
| custom_header | string                  | null            | Returns only specified pages of the PDF document. If not provided, it will return all pages.<br><b>Pattern: </b><code>1&#124;5-6</code>                                                                                                                                 |
| custom_footer | string                  | null            | Returns only specified pages of the PDF document. If not provided, it will return all pages.<br><b>Pattern: </b><code>1&#124;5-6</code>                                                                                                                                 |

#### 3. Disable Options

Options to disable elements on PDF render.

| Parameter           | Type                    | Default         | Description                                                                                                                                                                                                                                                             |
| ------------------- | ----------------------- | --------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| disable_images      | string(raw HTML or URL) | <b>required</b> | The target document to convert to PDF.<br>1. URL Format: The URL of a web page you want to convert.<br><b>Example: </b>`https://example.com`<br>2. Raw HTML Format: Raw HTML string of a page that you want to convert.<br><b>Example: </b>`<html><h1>Test</h1></html>` |
| disable_javascript  | string                  | null            | Returns only specified pages of the PDF document. If not provided, it will return all pages.<br><b>Pattern: </b><code>1&#124;5-6</code>                                                                                                                                 |
| disable_links       | string                  | null            | Returns only specified pages of the PDF document. If not provided, it will return all pages.<br><b>Pattern: </b><code>1&#124;5-6</code>                                                                                                                                 |
| disable_backgrounds | string                  | null            | Returns only specified pages of the PDF document. If not provided, it will return all pages.<br><b>Pattern: </b><code>1&#124;5-6</code>                                                                                                                                 |

#### 4. Delay Options

Options to delay on PDF render.

| Parameter         | Type                    | Default         | Description                                                                                                                                                                                                                                                             |
| ----------------- | ----------------------- | --------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| delay             | string(raw HTML or URL) | <b>required</b> | The target document to convert to PDF.<br>1. URL Format: The URL of a web page you want to convert.<br><b>Example: </b>`https://example.com`<br>2. Raw HTML Format: Raw HTML string of a page that you want to convert.<br><b>Example: </b>`<html><h1>Test</h1></html>` |
| wait_for_selector | string                  | null            | Returns only specified pages of the PDF document. If not provided, it will return all pages.<br><b>Pattern: </b><code>1&#124;5-6</code>                                                                                                                                 |
| wait_for_function | string                  | null            | Returns only specified pages of the PDF document. If not provided, it will return all pages.<br><b>Pattern: </b><code>1&#124;5-6</code>                                                                                                                                 |

#### 5. Request Options

Options on manipulating requests.

| Parameter    | Type                    | Default         | Description                                                                                                                                                                                                                                                             |
| ------------ | ----------------------- | --------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| http_headers | string(raw HTML or URL) | <b>required</b> | The target document to convert to PDF.<br>1. URL Format: The URL of a web page you want to convert.<br><b>Example: </b>`https://example.com`<br>2. Raw HTML Format: Raw HTML string of a page that you want to convert.<br><b>Example: </b>`<html><h1>Test</h1></html>` |

#### 6. Storage Options

Options for utilizing 3rd Party Storage(AWS S3) for rendered PDFs

| Parameter | Type                    | Default         | Description                                                                                                                                                                                                                                                             |
| --------- | ----------------------- | --------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| filename  | string(raw HTML or URL) | <b>required</b> | The target document to convert to PDF.<br>1. URL Format: The URL of a web page you want to convert.<br><b>Example: </b>`https://example.com`<br>2. Raw HTML Format: Raw HTML string of a page that you want to convert.<br><b>Example: </b>`<html><h1>Test</h1></html>` |

### Custom Headers/Footers

# Examples

We have provided examples of how to perform PDF conversions using our API in various use cases.<br>Please report problems or request new examples via <support@pdfswitch.io>.

## Example 1

## Example 2

## Example 3

# Credits

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
