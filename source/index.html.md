---
title: BEX REST API Documentation

language_tabs: # must be one of https://git.io/vQNgJ
  - json
  - javascript

toc_footers:
  - <a href='https://bex.co.za/signup'>Open a BEX account</a>
  - <a href='https://bex.co.za/quick-track'>Track a shipment</a>
  - <a href='https://bex.co.za/about'>About us</a>


includes:
  - login
  - tracking
  - quoting
  - waybill
  - waybilldownload
  - webhooks

search: true
---

# Welcome to our DOCS site

Here at BEX we frequently develop and publish new API’s to assist our customers who wish to gain deeper control and visibility into their courier processes.

These API’s allow for direct integration into our courier platform, allowing data to be read and created using requests in JSON format.
Couple this with our token based Authentication methods and standard HTTP verbs (which are understood by most HTTP clients) you have the capability to orchestrate and manage a number of processes within the courier environment.

We hope that you will build something great and benefit from integrating with our courier API’s.

We want your courier journey to be pain-free - If you run into any difficulties, or perhaps you have some suggestions for us, please do not hesitate to <a href="mailto:it@bex.co.za?subject=Please%20help%20me%20to%20integrate%20with%20you">reach out</a> and we will gladly work with you.

# Requirements

To make use of our API integration we require the following:

1. A valid <a href="https://bex.co.za/signup">shipping account</a> with BEX.
1. The creation of an integration user identity under which you will transact over the API’s
1. For security sensitive data requests, a valid <a href="#login">token</a>.

**Note:** If you have multiple shipping accounts you are not required to transact under multiple integration identities (“tokens”). Our platform supports the assignment of multiple shipping accounts to a _single_ token, making integration across multiple business units easier.

<aside class="notice">
Our API endpoints should be accessed through HTTPS.
</aside>

# Request/Response Format
Our API endpoints will respond with **JSON** content accompanied by a `200 OK` HTTP status. This includes any possible processing errors. Please see the Errors section.

# Errors
You can find the errors specific to each API endpoint under its dedicated API topic.

API errors are communicated through the `ex` (short for _exception_) property inside the response body. Please inspect this property for any possible errors that may have occurred.

A possible problematic response could look as follows:

* HTTP status code: `200 OK`
* API response: `"ex": "Account number is not registered for transactions with this token."`

![200 OK with error ex:](200OK-response-with-ex-error.jpg)
_Above: Note the **200 OK** status whilst receiving an error **"ex":** response_

In instances where there is a technical breakdown in the processing of the API request, such as querying the wrong endpoint address, you will receive the appropriate http status code such as a `404`.

# Parameters
All parameters as required by the relevant API endpoint may be provided in either query string format (e.g. `POST /getcustomquicktracking_v3?ref=test123`) OR as part of the body in `JSON` format.

<aside class="notice">
  Parameter values containing special characters should be URL encoded as needed.
</aside>

The parameters the relevant API endpoint requires are discussed in its relevant section.

# Tools
You are free to use any IDE or tool that satisfies your needs. We do recommend the use of <a href="https://www.postman.com" target="_blank">**POSTMAN**</a> to test your requests to our server.

[<img src="https://run.pstmn.io/button.svg" alt="Run In Postman" style="width: 128px; height: 32px;">](https://god.gw.postman.com/run-collection/37289786-9f57b8f8-96c9-4898-bda5-4514aca2ec85?action=collection%2Ffork&source=rip_markdown&collection-url=entityId%3D37289786-9f57b8f8-96c9-4898-bda5-4514aca2ec85%26entityType%3Dcollection%26workspaceId%3D4cf8191c-8bd5-4790-ab64-f1296609276c)

Other tools you could also look at are:

* <a href="https://insomnia.rest" target="_blank">Insomnia</a> - Cross-platform GraphQL and REST client, available for Mac, Windows, and Linux.
* <a href="https://getpostman.com" target="_blank">Postman</a> - Cross-platform REST client, available for Mac, Windows, and Linux.
* <a href="https://requestbin.com" target="_blank">RequestBin</a> - Allows you test webhooks.
* <a href="https://hookbin.com" target="_blank">Hookbin</a> - Another tool to test webhooks.

# Authentication
Authentication against the BEX API ecosystem is a two-part process and is required to prevent access to confidential client data. The process involves the generation of your API security token which is described in the next section. This token is then included in the HTTP headers of your API calls and serves to identify and authenticate you on our platform.

API&#39;s that serve non-sensitive client data such as our quick waybill <a href="tracking">tracking</a> do not require your token to be present in the API call and can be called anonymously.
