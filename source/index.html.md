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

Parameter | FieldType | Required | Case | Example | Description
--------- | --------- | -------- | ---- | ------- | -----------
AccountNumber | String | No (Unless an encrypted password is used) | No | 112233

Any valid account number for the user credentials can be used. This field is only required when a
password is encrypted.

Parameter | FieldType | Required | Case | Example | Description
--------- | --------- | -------- | ---- | ------- | -----------
PartnerID | INT | No (Unless an encrypted password is used) | No | 123

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

Example Service Call using JSON message body
URL: `http://insight.bex.co.za/api/service/login`
BODY: `{“username”:”user123”,”password”:”pass1234”}`

# Waybill Overview 

This service is used to create a waybill shipping entry into the BEX courier database. This
allows BEX customers to interface directly their shipping requests and helps to eliminate errors as
a result of manual data capture or misinterpretation of written content on shipping
documentation.
The service allows for the communication of a number of shipping options, such as:

* Full addressing of both the sending and receiving parties
* Additional shipping references against which the shipping entry is to be tagged against
* Multiple packages and their associated dimensions and weights
* Special handling requests such as after-hour deliveries, saturday service or public holidays

## URL

Service Relative URL: `/api/service/submitwaybillwia`

### Parameters

The parameter requirements for this api call have been broken down into three sub-sections,
namely:

* 1. Addresses - This sub-section contains parameters related to the construction of the Waybill and it's routing information
* 2. Dimensioning
* 3. Service Add-Ons

## 1 Addresses

Parameter | FieldType | Required | Case | Example
--------- | --------- | -------- | ---- | ------- 
AccountNumber | String | Yes | No | 1110123
Bex account number to which shipment costs will be allocated

Parameter | FieldType | Required | Case | Example
--------- | --------- | -------- | ---- | ------- 
SendersRef | String | No | No | Order123-456
Shippers reference linked to the database. This reference can be used for tracking, reporting and invoice requests

Parameter | FieldType | Required | Case | Example
--------- | --------- | -------- | ---- | ------- 
WaybillNumber | String | No(See below) | No | ABC123456
The waybill number is the BEX issued number printed on the respective waybill documentation
that is being completed for this shipping request. If you are integrating and make use of
alternative shipping stationary options such as thermal labels or A4 waybills, this number will be
auto-generated by BEX and communicated in the service response.

Parameter | FieldType | Required | Case | Example
--------- | --------- | -------- | ---- | ------- 
WaybillDate | Date | No(See Below) | No | 2020-07-17
The date on which the requested shipping entry is to be collected by BEX Express. It is formatted
as yyyy-mm-dd.

Parameter | FieldType | Required | Case | Example
--------- | --------- | -------- | ---- | ------- 
ServiceCode | String | Yes | No | NCA

The service delivery priority requested for this shipment. Typical service codes are as follows:

* ● SDX - Sameday Express
* ● DBD - Daybreak Delivery
* ● ONC- Overnight Courier
* ● NCA - Normal Cargo
* ● BUD - Budget Cargo

Parameter | FieldType | Required | Case | Example
--------- | --------- | -------- | ---- | ------- 
OriginCompany | String | No | No | BOEING AITCRAFT CORPORATION
The name of the company where the collection is to take place. Please note this is not necessarily
the company name of the person requesting the collection, but rather the name of the company
where BEX will go so as to collect the goods.

Parameter | FieldType | Required | Case | Example
--------- | --------- | -------- | ---- | ------- 
OriginFloorBuilding | String | Yes | No | Unit 6 The Broads Office Park
Information such as floor number, building and office park name can be entered into this field.

Parameter | FieldType | Required | Case | Example
--------- | --------- | -------- | ---- | ------- 
OriginStreet | String | Yes | No | 120 Loper Avenue
The place number and street of the sender of the shipment. Please note this is not the same as the
address of the account holder, but rather the shipments “departure address”

Parameter | FieldType | Required | Case | Example
--------- | --------- | -------- | ---- | ------- 
OriginLocationName | String | Yes | No | Rosebank
The suburb name of the senders address.

Parameter | FieldType | Required | Case | Example
--------- | --------- | -------- | ---- | ------- 
OriginPostCode | String | Yes | No | 2054
The postcode of the senders address.

Parameter | FieldType | Required | Case | Example
--------- | --------- | -------- | ---- | ------- 
SenderContactName | String | No | No | JONATHAN
The name of the contact person where the collection/dispatch of the goods will take place.

Parameter | FieldType | Required | Case | Example
--------- | --------- | -------- | ---- | ------- 
SenderTelephone | String | No | No | +27119292345
The telephone number of the contact person where the collection/dispatch will take place.

Parameter | FieldType | Required | Case | Example
--------- | --------- | -------- | ---- | ------- 
SenderMobile | String | No | No | 0829091234
The mobile number of the contact person where the collection/dispatch will take place. This field
is beneficial as the sender will be kept abreast of up to the minute news concerning the progress
of the shipment as it routes through the BEX network.

Parameter | FieldType | Required | Case | Example
--------- | --------- | -------- | ---- | ------- 
SenderEmail | String | No | No | JONATHAN@AIRLINER.COM
The email address of the contact person where the collection/dispatch will take place. Same as the
SenderMobile field, the email address is beneficial as the sender will be kept abreast of up to the
minute new concerning the progress of the collection request.

Parameter | FieldType | Required | Case | Example
--------- | --------- | -------- | ---- | ------- 
DestinationCompany | String | No | No | GENERAL ELECTRIC
The company name of the receiving party. Should the receiver be a private individual, their name
and surname details can be specified.

Parameter | FieldType | Required | Case | Example
--------- | --------- | -------- | ---- | ------- 
DestinationFloorBuilding | String | No | No | BLOCK B 1ST Floor
The floor and building address information of the receiver. Similar to the receiver company
name, this can only be optionally supplied if there is a single receiver for the goods to be
collected.

Parameter | FieldType | Required | Case | Example
--------- | --------- | -------- | ---- | ------- 
DestinationStreet | String | No | No | 16 COMMISIONER STREET
The street number and name of the receiver. It follows the same logic as the OriginStreet field.

Parameter | FieldType | Required | Case | Example
--------- | --------- | -------- | ---- | ------- 
DestinationLocationName | String | Yes | No | MILNERTON
The name of the suburb where delivery will take place.

Parameter | FieldType | Required | Case | Example
--------- | --------- | -------- | ---- | ------- 
DestinationPostCode | String | Yes | No | 7435
The postcode of the suburb specified in the DestinationLocationName parameter.

Parameter | FieldType | Required | Case | Example
--------- | --------- | -------- | ---- | ------- 
ReceiverContactName | String | No | No | Neil
The name of the contact person who is to receive the goods at delivery time.

Parameter | FieldType | Required | Case | Example
--------- | --------- | -------- | ---- | ------- 
ReceiverTelephone | String | No | No | 0114523454
The telephone number of the contact person who is to receive the goods at delivery time

Parameter | FieldType | Required | Case | Example
--------- | --------- | -------- | ---- | ------- 
ReceiverMobile | String | No | No | 0823241234
The mobile number of the contact person who is to receive the goods at delivery time. This
information is beneficial as the receiver will receive status updates as to the progress of the goods
whilst they are being routed through the BEX network.

Parameter | FieldType | Required | Case | Example
--------- | --------- | -------- | ---- | ------- 
ReceiverEmail | String | No | No | DEREK@BOEING.COM
The mobile number of the contact person who is to receive the goods at delivery time. This
information is beneficial as the receiver will receive status updates as to the progress of the goods
whilst they are being routed through the BEX network.

Parameter | FieldType | Required | Case | Example
--------- | --------- | -------- | ---- | ------- 
InsuranceRequired | BOOLEAN | No | No | TRUE
A Boolean flag that can be set should the requestor request insurance for the routing of the
packages once they have been collected by BEX. Should this Boolean field be set then the waybills
created for the shipping of the goods will carry an insurance surcharge that covers the premium
payable so as to have insurance cover during the shipping of the goods

Parameter | FieldType | Required | Case | Example
--------- | --------- | -------- | ---- | ------- 
InsuranceValue | FLOAT/DOUBLE | No | No | 1500
The insured value, in ZAR, of the goods at which the insurance cover will cater for.
