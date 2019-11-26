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

# Conversion API

## Convert to PDF

This is the main endpoint of PDFSwitch's API. Simply pass your raw HTML or URL to us and get a PDF in return.

### HTTP Request

`POST https://pdfswitch.com/api/v1/convert`

### BODY Parameters

All parameters should be passed as a JSON object. We grouped the parameters into sections by their main usage.

#### 1. Main Options

Default options to manipulate our PDF render engine.

| Parameter        | Type          | Default         | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
| ---------------- | ------------- | --------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| source           | string or URL | <b>required</b> | The target document to convert to PDF. Can be an URL or a String of raw HTML.<br><b>1. URL Format:</b> The URL of a web page you want to convert.<br><b>Example: </b>`https://example.com`<br><b>2. HTML Format:</b> Raw HTML string that you want to convert.<br><b>Example: </b>`<html><h1>Test</h1></html>`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| page_ranges      | string        | null            | Returns only specified pages of the PDF document. If not provided, it will return all pages by default.<br><b>Pattern: </b><code>1&#124;5-6</code>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
| page_orientation | string        | portrait        | Page orientation<br><b>Pattern: </b><code>portrait&#124;landscape</code><br>cf) <a href="https://en.wikipedia.org/wiki/Page_orientation#/media/File:800x518_smartphone_portrait_vs_landscape_orientation.png" target="_blank">Portrait vs Landscape</a>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
| page_format      | string        | A4              | Page size for converted pdf<br><b>Pattern: </b><code>Letter&#124;Legal&#124;Tabloid&#124;Ledger&#124;A0&#124;A1&#124;A2&#124;A3&#124;A4&#124;A5&#124;A6</code><br>cf) <a href="https://www.engineeringtoolbox.com/drawing-sheet-sizes-d_108.html" target="_blank">A0~A6 Sizes</a>, <a href="https://www.engineeringtoolbox.com/office-paper-sizes-d_213.html" target="_blank">Letter~Ledger Sizes</a>                                                                                                                                                                                                                                                                                                                                                                                                                                               |
| page_width       | string        | null            | Custom pdf page width. Use it together with custom `page_height`. <br>Unit can be one of <code>px&#124;in&#124;mm&#124;cm</code>.<br><b>Pattern: </b><code>NUM[px&#124;in&#124;mm&#124;cm]</code><br><b>Example: </b><code>600px</code><aside class="warning">Cannot be used together with `page_format`. Prefer to use `page_format` if you don't have an exact page size requirement.</aside>                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| page_height      | string        | null            | Custom pdf page height. Use it together with custom `page_width`. <br>Unit can be one of <code>px&#124;in&#124;mm&#124;cm</code>.<br><b>Pattern: </b><code>NUM[px&#124;in&#124;mm&#124;cm]</code><br><b>Example: </b><code>600px</code><aside class="warning">Cannot be used together with `page_format`. Prefer to use `page_format` if you don't have an exact page size requirement.</aside>                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| emulate_media    | string        | screen          | Force CSS media emulation for print or screen. The print option controls how your page looks when printed.<br><b>Pattern: </b><code>screen&#124;print</code>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| margins          | object        | null            | CSS style margin sizes. Use it to give space between the limits of the document and captured content.<br><ul><li>**top**[string]: Space between the top and the content<br>Pattern: <code>NUM[px&#124;in&#124;mm&#124;cm]</code></li><li>**right**[string]: Space between the right and the content<br>Pattern: <code>NUM[px&#124;in&#124;mm&#124;cm]</code></li><li>**bottom**[string]: Space between the bottom and the content<br>Pattern: <code>NUM[px&#124;in&#124;mm&#124;cm]</code></li><li>**left**[string]: Space between the left and the content<br>Pattern: <code>NUM[px&#124;in&#124;mm&#124;cm]</code></li></ul><b>Example: </b><code>{"top": "100px", "right": "0px", "bottom": "0px", "left": "300px" }</code><br><br>cf) _If a certain key`[top, right, bottom, left]` is not defined, the value for the key is `0px` by default._ |
| zoom             | number        | 1               | Allows you to increase the zoom in the document. It should be a **float value between 0.1 and 2.** Value larger than 1 means zooming in(larger), smaller than 1 means zooming out(smaller). For example, the default 1 means 100% zoom(original state) on the document, 2 means 200% zoom(zoom in), 0.1 means 10% zoom(zoom out) and so on.<br><b>Example: </b><code>1.5</code>                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |

#### 2. Injection Options

Options to inject on PDF render.

| Parameter     | Type          | Default | Description                                                                                                                                                                                                                                                                                                                                                |
| ------------- | ------------- | ------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| css           | string or URL | null    | Additional CSS to be injected into the page before render. Can be an URL or String of CSS Rules.<br><b>1. URL:</b> The URL to a file of CSS Rules<br><b>Example: </b>`https://path-to-css-rules`<br><b>2. String:</b> String of CSS Rules.<br><b>Example: </b>`body{background-color: red;} h1{background-color: blue;}`                                   |  |
| javascript    | string or URL | null    | Additional Javascript to be injected into the page before render. Can be an URL or a String of Javascript code.<br><b>1. URL:</b> The URL to a file of Javascript code<br><b>Example: </b>`https://path-to-javascript-code`<br><b>2. String:</b> String of Javascript code.<br><b>Example: </b>`document.getElementById("some-id").style.display = "none"` |
| custom_header | string        | null    | HTML template for custom page header. See <a href="#custom-header-footer">Custom Header/Footer</a> section for more details.<br><b>Example: </b>`<header style="font-size:16px;">Page <span class="pageNumber"></span> of <span class="totalPages"></span></header>`                                                                                       |
| custom_footer | string        | null    | HTML template for custom page footer. See <a href="#custom-header-footer">Custom Header/Footer</a> section for more details.<br><b>Example: </b>`<footer style="font-size:16px;">Date: <span class="date"></span></footer>`                                                                                                                                |

#### 3. Disable Options

Options to disable on PDF render.

| Parameter           | Type    | Default | Description                                                              |
| ------------------- | ------- | ------- | ------------------------------------------------------------------------ |
| disable_images      | boolean | false   | Images will be excluded in the rendered PDF.                             |
| disable_javascript  | boolean | false   | Will not execute any javascript in the document on render.               |
| disable_links       | boolean | false   | The links(`<a><a/>` tags) in the rendered PDF will not link to anywhere. |
| disable_backgrounds | boolean | false   | The rendered PDF will not have any background images.                    |

#### 4. Delay Options

Options to delay on PDF render.

| Parameter         | Type    | Default | Description                                                                                                                                                                 |
| ----------------- | ------- | ------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| delay             | integer | 0       | Time in milliseconds to delay render after page load.<br><b>Max: </b>`10000`                                                                                                |
| wait_for_selector | string  | null    | CSS selector. Waits until a DOM element matching the provided CSS selector becomes present on the page. Times out after 10 seconds.<br><b>Example: </b>`span.someClassName` |
| wait_for_function | string  | null    | Name of a globally available function(assigned in the `window` scope). Waits until the function returns a truthy value(boolean true, 1, etc). Times out after 10 seconds.   |

#### 5. Request Options

Options on manipulating requests before render.

| Parameter    | Type   | Default | Description                                                                                                                                                                       |
| ------------ | ------ | ------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| http_headers | object | null    | An object containing additional HTTP headers to be sent on a request before render. All header values must be strings.<br><b>Example: </b>`{"Accept-Language": "en-US,en;q=0.5"}` |

#### 6. Storage Options

Options for using 3rd Party Storage(AWS S3) for rendered PDFs.

| Parameter | Type   | Default | Description                                                                                                                                                                                                                                                                          |
| --------- | ------ | ------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| filename  | string | null    | If a filename is specified, PDFSwitch will not return a binary PDF but store the PDF in a AWS S3 storage and return the storage URL. See <a href="#saving-the-document-to-amazon-s3">Saving the document to Amazon S3</a> section for more details. <br><b>Example: </b>`result.pdf` |

### Custom Header/Footer

Rendered PDFs do not contain any header/footer by default. If you want to add a custom header/footer to your documents, provide an HTML template and PDFSwitch injects it into your document at time of render.

<aside class="notice">
Custom header and footer are <b>independent</b> from the original document(<code>source</code>) you are trying to convert. Therefore, your original document's CSS styles will not apply to your custom header and footer.<br><br> Below is the recommended ways to style your custom header and footer.<br><b>1) Set inline CSS styles</b><br>Example: <code>&lt;header style="color:red;font-size:20px"&gt;Header&lt;/header&gt;</code><br><b>2) Set CSS styles using &lt;style&gt; tags before your header</br></b>Example: <code>&lt;style&gt;header{color:red;font-size:20px}&lt;/style&gt;&lt;header&gt;Header&lt;/header&gt;</code>

</aside>

We refer to the html strings you pass as **HTML Templates** because you can use the below predefined template variables in your html strings. For example, if you want to display the current page number in your PDF pages, you can simply use a placeholder element like `<span class="pageNumber"></span>`, and our PDF render engine automatically injects the current page number in the `pageNumber` class element.

#### Header/Footer Template Classnames

> Custom Header/Footer

```python
import requests

response = requests.post(
    'https://pdfswitch.io/v1/convert',
    headers={'Authorization': YOUR_API_KEY }
    json={'source': 'https://www.wikipedia.org/',
    # use margins with custom header/footer
    'margins': {'top': '50px', 'bottom': '50px'},
    'custom_header': '<header style="width:100%;text-align:center;font-size:16px;">Page <span class="pageNumber"></span> of <span class="totalPages"></span></header>',
    'custom_footer': '<footer style="width:100%;text-align:center;font-size:16px;">Date: <span class="date"></span></footer>'},
    stream=True
)

response.raise_for_status()

with open('result.pdf', 'wb') as output:
    for chunck in response.iter_content(chunk_size=1024):
        output.write(chunk)
```

> The above command returns a PDF in binary format.

These are the predefined classnames that you can use for dynamic data.

| Classname  | Description                           |
| ---------- | ------------------------------------- |
| date       | formatted date of render in UTF       |
| title      | document title                        |
| url        | document location                     |
| pageNumber | current page number                   |
| totalPages | total number of pages in the document |

Note that custom header/footer are automatically added to all pages. Also, you **must use `margins` together with `custom_header|custom_footer`**, to provide top and bottom margins to your page so there is space for the custom header/footer to show up.

# Examples

We have provided examples of how to perform PDF conversions using our API in various use cases.<br>Please report problems or request new examples via <support@pdfswitch.io>.

## Saving the document to Amazon S3

```python
import requests

response = requests.post(
    'https://pdfswitch.io/v1/convert',
    headers={'Authorization': YOUR_API_KEY }
    json={
      'source': 'https://www.wikipedia.org/',
      'filename': 'wiki.pdf'
    }
)

response.raise_for_status()

json_response = response.json()
# the filesize unit is MB.
# You can use this to figure out what pricing plan suits you best.
```

> The above command returns JSON structured like this:

```json
{
  "url": "https://pdfswitch.s3.us-west-1.amazonaws.com/pdfswitch/d/2/2019-11/46301658ad7e4ace8f97903ac53d88e6/wiki.pdf",
  "filesize": 0.797048
}
```

[Amazon Simple Storage Service(Amazon S3)](https://aws.amazon.com/s3/) is an object storage service to store data. By passing the `filename` parameter, PDFSwitch won't return a binary PDF, but upload the PDF to AWS S3 and return an URL to the storage location.

#### When is this useful?

This feature is useful when you don't want to hold a large binary PDF in memory in your server but to just serve the storage URL to users for download.

<aside class="warning">The PDF document will be stored for 2 days in AWS S3 before being automatically deleted.</aside>

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
