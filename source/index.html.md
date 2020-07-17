---
title: BEX Webservice Integrations

language_tabs: # must be one of https://git.io/vQNgJ
  - javascript

toc_footers:
  - <a href='#'>Sign Up for a Developer Key</a>
  - <a href='https://github.com/acme'>Client libraries</a>
  - <a href='https://github.com/slatedocs/slate'>Documentation Powered by Slate</a>

includes:
  - test
  - errors

search: true
---

# Introduction

BEX Express provides a comprehensive range of Domestic, Cross Border and International
 courier services

# Login

## Overview

The service accepts user credentials. After validation, it then returns a valid session token which can be
used to access additional services. This token string will then be submitted via a cookie called “token” or
a header attribute with the same name.

## URL

Service Relative URL: `api/service/login`

## Parameters

Parameter | FieldType | Required | Case | Example | Description
--------- | --------- | -------- | ---- | ------- | -----------
Username | String | Yes | No | user123 | Requires a valid username obtained from your account administrator
Password | String |Yes |Yes |pass1234 | Requires a valid password obtained from your account administrator

Passwords can be sent in plain-text or encrypted using the Rijndael algorithm. For this to be enabled, a
cipher will need to be agreed upon and saved against your account for decryption purposes.We are also
able to supply a .NET example for encryption if needed.

AccountNumber

Field Type:|STRING
Required:|No (Unless an encrypted password is used)
Case Sensitive:|No
Example:|112233

Any valid account number for the user credentials can be used. This field is only required when a
password is encrypted.

PartnerID

Field Type:|INT
Required:|No (Unless an encrypted password is used)
Case Sensitive:|No
Example:|123

The PartnerID is a unique identifier that can be provided by your administrator. This is required due to
the multi-company support which BEX has per database. This field is only required when a password is
encrypted

## Transportation

> Make sure to replace `user123`and `pass123` with your API key.

```javascript
<!DOCTYPE html>
<html xmlns="http://www.w3.org/1999/xhtml">
<head>
<title>Login Example</title>
<script src="http://code.jquery.com/jquery-latest.min.js" type="text/javascript"></script>
</head>
<body>
<script type="text/javascript">
var user = encodeURIComponent('test123'); //encode the username
var pass = encodeURIComponent('pass1234'); //encode the password
jQuery.support.cors = true; //this will enable cross domain access for all services
//call the service using jQuery ajax and passing the parameters
$.ajax({
url: 'http://insight.bex.co.za/api/service/login?username=' + user + '&password=' + pass,
type: 'POST',
async: false, //dont run asynchronously
success: function (data) {
if (data.ex) { //if the network call succeeds, it may still contain an error (ex)
alert('The following error occured: ' + data.ex); //display the error
return;
}a
lert('Your token is: ' + data.value); //display the valid token
},
error: function (ex) { //this will fire for network related issues
alert(ex.responseText);
}
});
</script>
</body>
</html>
```
The service supports GET and POST requests.We recommend POST requests to avoid caching issues.
The service supports URL parameters or JSON parameters in the message body
All URL-based field values must be URL encoded if special characters are used

Example Service Call using the URL Parameters
URL: `http://insight.bex.co.za/api/service/login?username=user123&password=pass1234`

>Please note that all fields must be URL encoded if special characters are used




# Waybill Overview -- Next section

This service is used to create a waybill shipping entry into the BEX courier database. This
allows BEX customers to interface directly their shipping requests and helps to eliminate errors as
a result of manual data capture or misinterpretation of written content on shipping
documentation.
The service allows for the communication of a number of shipping options, such as:

* Full addressing of both the sending and receiving parties
* Additional shipping references against which the shipping entry is to be tagged against
* Multiple packages and their associated dimensions and weights
* Special handling requests such as after-hour deliveries, saturday service or public holidays

# URL

Service Relative URL: `/api/service/submitwaybillwia`

# Parameters

The parameter requirements for this api call have been broken down into three sub-sections,
namely:
1. Addresses
    This sub-section contains parameters related to the construction of the Waybill and it's routing information
2. Dimensioning
3. Service Add-Ons

## 1 Addresses

Parameter | FieldType | Required | Case | Example | Description
--------- | --------- | -------- | ---- | ------- | -----------
Account No | String | Yes | No | 1110123 | Bex account number to which shipment costs will be allocated
Senders Ref | String | No | No | Order123-456 | Shippers reference linked to the database. This reference can be used for tracking, reporting and invoice requests


This example API documentation page was created with [Slate](https://github.com/slatedocs/slate). Feel free to edit it and use it as a base for your own API's documentation.

# Authentication

> To authorize, use this code:

```javascript
const kittn = require('kittn');

let api = kittn.authorize('meowmeowmeow');
```

> Make sure to replace `meowmeowmeow` with your API key.

Kittn uses API keys to allow access to the API. You can register a new Kittn API key at our [developer portal](http://example.com/developers).

Kittn expects for the API key to be included in all API requests to the server in a header that looks like the following:

`Authorization: meowmeowmeow`

<aside class="notice">
You must replace <code>meowmeowmeow</code> with your personal API key.
</aside>

# Kittens

## Get All Kittens

```ruby
require 'kittn'


```javascript
const kittn = require('kittn');

let api = kittn.authorize('meowmeowmeow');
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

```javascript
const kittn = require('kittn');

let api = kittn.authorize('meowmeowmeow');
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

Parameter | Description
--------- | -----------
ID | The ID of the kitten to retrieve

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
const kittn = require('kittn');

let api = kittn.authorize('meowmeowmeow');
let max = api.kittens.delete(2);
```

> The above command returns JSON structured like this:

```json
{
  "id": 2,
  "deleted" : ":("
}
```

This endpoint deletes a specific kitten.

### HTTP Request

`DELETE http://example.com/kittens/<ID>`

### URL Parameters

Parameter | Description
--------- | -----------
ID | The ID of the kitten to delete

