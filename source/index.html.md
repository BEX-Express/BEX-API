---
title: BEX REST API Documentation

language_tabs: # must be one of https://git.io/vQNgJ
  - javascript
  - json

toc_footers:
  - <a href='https://bex.co.za/signup'>Open a BEX account</a>
  - <a href='https://github.com/acme'>Client libraries</a>
  - <a href='https://github.com/slatedocs/slate'>Documentation Powered by Slate</a>

includes:
  - waybill


search: true
---

# Introduction

Here at BEX we frequently develop and publish new API’s to assist our customers who wish to gain deeper control and visibility into their courier processes.

These API’s allow for direct integration into our courier platform, allowing data to be read and created using requests in JSON format.
Couple this with our token based Authentication methods and standard HTTP verbs (which are understood by most HTTP clients) you have the capability to orchestrate and manage a number of processes within the courier environment.

We hope that you will build something great and benefit from integrating with our courier API’s.

# Requirements

To make use of our API integration we require the following:

1. A valid shipping account with BEX.
1. The creation of an integration user identity under which you will transact over the API’s
1. For security sensitive data requests, a valid token.

<aside class="notice">
Note: If you have multiple shipping accounts you are not required to transact under multiple integration identities (“tokens”). Our platform supports the assignment of multiple shipping accounts to a single token, making integration across multiple business units easier.
You may access the API over HTTP or HTTPS, but HTTPS us recommended where possible.
</aside>

# Request/Response Format
The default response format is JSON. Requests with a message-body use plain JSON to set resource attributes.

Successful requests will return a `200 OK` HTTP status.

<aside class="notice">
A word on success flags:
We return 200 OK status responses for ALL requests that make it to our server, even if we run into an error when processing your request. For API submissions that result in a processing error (read validation and business logic/”elegant” type errors) we include an attribute in the API response titled ex: where we communicate the reason for the processing failure. An example could be:
HTTP status code: 200 OK
API response body: ex: Account number is not registered for transactions with this token.
In instances where there is a technical breakdown in the processing of the API request, such as querying the wrong API name, you will receive the appropriate http status code such as a 503.x error.
</aside>

# Errors

You can find the errors specific to each API endpoint under its dedicated API topic.

Errors return both an appropriate HTTP status code and response object which contains an ex attribute.

# Parameters

Almost all endpoints accept optional parameters which can be passed as a HTTP query string parameter, e.g. GET /getcustomquicktracking_V3?ref=invoices
Parameters containing special characters that are passed in a URL request must have their data url encoded.

All parameters are documented along each endpoint.

# Libraries
We do not currently offer any libraries for our API implementation. We are in the process of creating .NET wrappers that you can use in the future. We will publish more information on this topic once it is ready for use.

# Tools
Some useful tools you can use to access the API include:
Some useful tools you can use to access the API include:
+ Insomnia - Cross-platform GraphQL and REST client, available for Mac, Windows, and Linux.
+ Postman - Cross-platform REST client, available for Mac, Windows, and Linux.
+ RequestBin - Allows you test webhooks.
+ Hookbin - Another tool to test webhooks.


# Authentication
Authentication against the BEX API ecosystem is a two-part process and is required to prevent access to confidential client data. The process involves the generation of your API security token which is described in the next section. This token is then included in the HTTP headers of your API calls and serves to identify and authenticate you on our platform.

API’s that serve non-sensitive client data such as our quick waybill tracking do not require your token to be present in the API call and can be called anonymously.


# Login

## Overview

The *login* service serves as the starting point from which a valid session token will be generated. You call this endpoint to authenticate your _username_ and _password_ user credentials.

The BEX platform will validate your request and if successful, will generate and return in the response object a token contained in the value attribute.

This token is unique to your integration user identity and is to be kept private as it is the key needed to unlock access to the account security restricted API’s.

<aside class="notice">
The token will not expire so once generated can be persisted securely on your side.
</aside>

## URL

Service Relative URL: `api/service/login`

> Make sure to replace `user123`and `pass123` with your API key.

```html
<!DOCTYPE html>
<html xmlns="http://www.w3.org/1999/xhtml">
<head>
<title>BEX API Login Example</title>
<script src="http://code.jquery.com/jquery-latest.min.js" type="text/javascript"></script>
</head>
<body>
<script type="text/javascript">
    var user = encodeURIComponent('test123'); //encode the username
    var pass = encodeURIComponent('pass1234'); //encode the password
    jQuery.support.cors = true; //this will enable cross domain access for all services
    //call the service using jQuery ajax and passing the parameters
    $.ajax({
    url: 'https://api.bex.co.za/api/service/login?username=' + user + '&password=' + pass,
    type: 'POST',
    async: false, //dont run asynchronously
    success: function (data) {
    if (data.ex) { //if the network call succeeds, it may still contain an error (ex)
    alert('The following error occured: ' + data.ex); //display the error
    return;
    };
    alert('Your token is: ' + data.value); //display the valid token
    },
    error: function (ex) { //this will fire for network related issues
        alert(ex.responseText);
        }
    });
</script>
</body>
</html>
```

## Parameters

Parameter | FieldType | Required | Description
--------- | --------- | -------- | -----------
username | String | Yes |Your integration account _username_.
password | String | Yes |Your integration account _password_ (case sensitive).

## Transportation

The service supports GET and POST requests. We recommend POST requests to avoid caching issues.
The service supports URL parameters or JSON parameters in the message body

Example Service Call using the URL Parameters

* URL: `http://insight.bex.co.za/api/service/login?username=user123&password=pass1234`

<aside class="notice">
    All URL-based field values must be URL encoded if special characters are used

</aside>

## Response

The response body is structured as follows:

Attribute | Type | Description
--------- | ---- | -----------
id | int | An internal BEX ID used to identify your user.
value | string | Your unique and private token.
isStrongPassword | string | Confirms whether your password meets our complexity requirements.
isEmailVerified	| string | Your email address is used to recover forgotten passwords or lost tokens and needs to be verified before transactions are possible.
email | string | The recovery email address against which this token is registered.
signalRenabled | string | A feature-flag used internally on our platform.
allowApiLogin | string | A feature-flag used internally to grant additional login rights to your user identity. This is not a requirement to use our API endpoints.

```json
{
    "id": 19696,
    "value": "dOdw98ycsih298749bd6MbZre8o7tt3r3nhfowo8dnBDpEJvkzcQG1vb44sy",
    "isStrongPassword": "True",
    "isEmailVerified": "True",
    "email": "your.email@company.com",
    "signalREnabled": "False",
    "allowApiLogin": "False"
}
```

With your token now generated you can proceed to call our security restricted API’s.

To do so, you *include it as a header attribute* in the HTTP headers of the call you are making to the respective API endpoints. The attribute is to be titled **token**
