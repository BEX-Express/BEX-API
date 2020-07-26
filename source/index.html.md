---
title: BEX Webservice Integrations

language_tabs: # must be one of https://git.io/vQNgJ
  - javascript

toc_footers:
  - <a href='#'>Sign Up for a Developer Key</a>
  - <a href='https://github.com/acme'>Client libraries</a>
  - <a href='https://github.com/slatedocs/slate'>Documentation Powered by Slate</a>

includes:
  - waybill
  - test2
  - errors

search: true
---

# Introduction

BEX Express provides a comprehensive range of Domestic, Cross Border and International
 courier services

# Login

## Overview

The service accepts user credentials. After validation, it returns a valid session token which is
used to access additional services. This token string will be submitted via a cookie called “token” or
a header attribute with the same name.

## URL

Service Relative URL: `api/service/login`

## Parameters

Parameter | FieldType | Case| Required | Note
--------- | --------- | ---- | -------- |----
Username | String | No | Yes | Obtain from your account administrator
Password | String | Yes | Yes |Obtain from your account administrator
AccountNumber | String | No | No | Required unless encrypted password used
PartnerID | INT | No | No |Required unless encrypted password used

<aside class="notice">
    Encrypted passwords use the Rijndael algorithm. To enable this, a
cipher must be agreed upon and saved against your account. A .NET example can 
be supplied for encryption if required.

</aside>

<aside class="notice">
    For the multi-company support which BEX has per database, PartnerId is required

</aside>


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

Example Service Call using the URL Parameters

* URL: `http://insight.bex.co.za/api/service/login?username=user123&password=pass1234`

>Please note that all fields must be URL encoded if special characters are used


<aside class="notice">
    All URL-based field values must be URL encoded if special characters are used
</aside


Example Service Call using JSON message body
URL: `http://insight.bex.co.za/api/service/login`
BODY: `{“username”:”user123”,”password”:”pass1234”}`

