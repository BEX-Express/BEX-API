# Waybill 

## Overview
This service is used to create a waybill shipping entry into the BEX courier database. 
This allows BEX customers to directly interface their shipping requests and helps to 
eliminate errors as a result of manual data capture or misinterpretation of written content 
on shipping documentation.
The service allows for the communication of a number of shipping options, such as:

* Full addressing of both the sending and receiving parties
* Additional shipping references against which the shipping entry is to be tagged
* Multiple packages and their associated dimensions and weights
* Special handling requests such as after-hour, Saturday or public holiday deliveries

## Endpoint
The waybill API endpoint is located at `https://api.bex.co.za/api/service/submitwaybillv4`.

The parameter requirements for this API call have been broken down into five sub-sections,
namely:

* Basic Details
* Addresses - Contains parameters for the construction and routing information of the Waybill 
* Dimensioning
* Additional References
* Printing Options

> Sample json message body.

```json
{
    "waybillNumber": "", //Leave empty to auto generate OR enter a custom waybill number.
    "autoGenerateWaybillNumber": true, //Set this to _false_ if you are providing a custom waybill number.
    "waybillDate": "YYYY-MM-DD",
    "accountNumber": "your_bex_account_number",
    "serviceCode": "ONC", //or whichever service you may require
    "sendersRef": "a reference for the sender", //(optional)
    "instructions": "special instructions for the courier", //(optional)
    "insuranceRequired": false, //Set to _true_ if you require insurance
    "insuranceValue": 0, //(optional) Specify a value only if _insuranceRequired_ is set to _true_.
    "senderCompany": "Doe Digital Works", //(optional)
    "senderFloorBuilding": "17 Cheltham House, Floor 3", //(optional)
    "senderStreet": "123 John Doe Avenue",
    "senderSuburb": "Ross Kent South",
    "senderCity": "Odendaalsrus", 
    "senderPostCode": "9800", 
    "senderProvince": "Free State", //(optional)
    "senderContactName": "John Doe", //(optional)
    "senderMobile": "1234567890", //(optional)
    "senderEmail": "john@domain.com", //(optional) 
    "senderTel": "1234567890", //(optional)
    "senderTelHome": "1234567890", //(optional) 
    "senderLat": sender_latitude, //(optional) Although, we encourage the use of coordinates!
    "senderLon": sender_longitude, //(optional) Although, we encourage the use of coordinates!
    "receiverCompany": "Jane Flower Works", //(optional) 
    "receiverFloorBuilding": "Floor B-4", //(optional) 
    "receiverStreet": "166 Wringham Drive", 
    "receiverSuburb": "Table View", 
    "receiverCity": "Cape Town", 
    "receiverPostCode": "7441",
    "receiverProvince": "Western Cape", //(optional) 
    "receiverContactName": "Jane Doe", //(optional) 
    "receiverMobile": "1234567890", //(optional) If a number is provided, delivery notifications will be sent to this number.
    "receiverEmail": "jane@domain.com", //(optional) If an email is provided, delivery notifications will be sent to this address. 
    "receiverTel": "1234567890", //(optional) 
    "receiverTelHome": "1234567890", //(optional) 
    "receiverLat": receiver_latitude, //(optional) Although, we encourage the use of coordinates!
    "receiverLon": receiver_longitude, //(optional) Although, we encourage the use of coordinates!
    "returnParcelPrintingText": false, //(optional) Returns EPL thermal label data as part of the response for you to send to the thermal printer.
    "printParcels": false, //(optional) If _true_, parcels are printed to the thermal label printer after submission using the URL specified.
    "printUrl": "", //(optional) Specify the thermal label printer URL to print to IF _printParcels=true_.
    "printParcelsPrintNode": false, //(optional) Specify _true_ to use PrintNode to print the labels if _printParcels=true_.
    "dimensions": [{
        "dimension1": length_cm, 
        "dimension2": breadth_cm, 
        "dimension3": height_cm, 
        "weight": parcel_weight_kg, 
        "reference": "" //(optional) A parcel description
    }],
    "additionalReferences": [{
        "value": "reference_1"
    }, {
        "value": "reference_2"
    }]
}
```

> jQuery Waybill Submission Example.

```javascript
<!DOCTYPE html>
<html xmlns="http://www.w3.org/1999/xhtml">
<head>
<title>BEX waybill API example</title>
<script src="http://code.jquery.com/jquerylatest.min.js" type="text/javascript"></script>
</head>
<body>
<script type="text/javascript">
//********** Login Service Call ********************
var tokenStr = '';
var user = encodeURIComponent('someusername'); //encode the username
var pass = encodeURIComponent('somepassword'); //encode the password
var host = 'https://api.bex.co.za';
//********** Submit Waybill Service Call ***********
var postData = {};

//BASIC
postData.waybillNumber = 'IT2701152404'; //or leave empty and set [autoGenerateWaybillNumber=true]
postData.autoGenerateWaybillNumber = false; //set to [false] if you're supplying a waybill number.
postData.waybillDate = '20150127';
postData.accountNumber = '111983';
postData.serviceCode = 'NCA';
postData.sendersRef = 'WE HAVE A REFERENCE';
postData.instructions = 'DO NOT INVERT COMPONENT';

//INSURANCE OPTIONS
postData.insuranceRequired = false; //Set to true if you need insurance.
postData.insuranceValue = 0; //Set the waybill value here if [insuranceRequired=true].

//SENDER DETAILS
//--Address
postData.senderCompany = 'THE BOEING COMPANY';
postData.senderFloorBuilding = 'UNIT 6 ASSEMBLY BUILDING';
postData.senderStreet = '21 AIRCRAFT LANE';
postData.senderSuburb = 'MARLBORO';
postData.senderCity = 'SANDTON';
postData.senderPostCode = '2050';
postData.senderProvince = 'GAUTENG'; //optional
postData.senderLat = null; //optional BUT WE ENCOURAGE the use of COORDINATES. Enter the LATITUDE here, e.g. -26.123496410189606
postData.senderLon = null; //optional BUT WE ENCOURAGE the use of COORDINATES. Enter the LONGITUDE here, e.g. 28.205670867327754

//--Contact
postData.senderContactName = 'JONATHAN'; //optional
postData.senderMobile = '0821234321'; //optional
postData.senderEmail = 'JONATHAN@BOEING.COM'; //optional
postData.senderTel = '0119299700'; //optional
postData.senderTelHome = '0119299700'; //optional

//RECEIVER DETAILS
//--Address
postData.receiverCompany = 'THE AIRLINER CO';
postData.receiverFloorBuilding = '1ST FLOOR HEAD OFFICE';
postData.receiverStreet = '21 TULP STREET';
postData.receiverSuburb = 'MILNERTON';
postData.receiverCity = 'CAPE TOWN';
postData.receiverPostCode = '7401';
postData.receiverProvince = 'WESTERN CAPE'; //optional
postData.receiverLat = null; //optional BUT WE ENCOURAGE the use of COORDINATES. Enter the LATITUDE here, e.g. -26.123496410189606
postData.receiverLon = null; //optional BUT WE ENCOURAGE the use of COORDINATES. Enter the LONGITUDE here, e.g. 28.205670867327754

//--Contact
postData.receiverContactName = 'DEREK';
postData.receiverMobile = '0833211234';
postData.receiverEmail = 'DEREK@THEAIRLINER.COM';
postData.receiverTel = '0218641202';
postData.receiverTelHome = '0218641202';

//PARCELS AND DIMENSIONS
var dimensions = [];
var parcel1 = {};
parcel1.dimension1 = 40; //Length (cm)
parcel1.dimension2 = 30; //Breadth (cm) 
parcel1.dimension3 = 1; //Height (cm)
parcel1.weight = 6; //Parcel Weight (nearest whole kg)
parcel1.reference = 'AIRFRAME COMPONENT'; //(optional)A reference for this parcel.
dims.push(parcel1);

//-->...continue adding the rest of the parcels

postData.dims = dims;

//ADDITIONAL REFERENCES []
var additionalRefs = [];
var ref1 = {};
ref1.value = "ORDER 22341";
additionalRefs.push(ref1);

//-->...continue adding the rest of your references.

postData.additionalReferences = additionalRefs;

//THERMAL LABEL PRINTING
postData.returnParcelPrintingText = false; //(optional) Returns EPL thermal label data as part of the response for you to send to the thermal printer.
postData.printParcels = false; //(Optional) When set to [true], parcels are printed to the thermal label printer after submission using the [printUrl] specified.
postData.printUrl = ''; //(optional) Specify the thermal label printer URL to print to IF [printParcels=true]
postData.printParcelsPrintNode = false; //(optional) Specify [true] to use PrintNode to print the labels if [printParcels=true].

//----- SUBMIT TO BEX -----//
//NOTE: If your call fails, it will still be communicated as 200 OK. Inspect the [ex] property in the response for error information.
jQuery.support.cors = true; //Enables cross-domain access to our services using jQuery ajax.

//--Step 1: Get your token.
$.ajax({
  url: host + '/api/service/login?userName=' + user + '&password=' + pass, 
  type: 'POST', 
  async: false, //No need for async here.
  success: function (d) {
    if (d.ex) {
      alert('We couldnt complete the request. ' + d.ex);
      return;
    }

    tokenStr = data.value;
  }
});

//--Step 2: Submit your waybill request.
$.ajax({
  url: host + '/api/service/submitwaybillv4', 
  type: 'POST', 
  data: JSON.stringify(postData), 
  dataType: 'json', 
  contentType: 'application/json; charset=UTF8', 
  beforeSend: function (xhr) {
    //Add your token to the header
    xhr.setRequestHeader('token', tokenStr);
  }, 
  success: function (d) {
    if (d.ex) {
        document.write('We failed to submit your waybill. ' + d.ex);
        return;
    }

    document.write('Your waybill was created successfully.');
  }
});
</script>
</body>
</html>
```

### Basic Details
Parameter | FieldType | Case | Required | Note
--------- | --------- | ---- | ---------| ---- 
waybillNumber | String | No* | No | A unique waybill number. This field will be _required_ if _AutoGenerateWaybillNumber_ is set to _false_.
autoGenerateWaybillNumber | Boolean | N/A | No* | If set to _true_, the BEX system will automatically generate a waybill number on your behalf.
waybillDate | Date | No | No | Date of collection by BEX. Format yyyy-mm-dd
accountNumber | String | No | Yes | 
sendersRef | String | No | No | For tracking, reporting and invoice requests
instructions | String | No | No | Communicate additional info

*`waybillNumber` is auto-generated by BEX and communicated in the service response when making use of alternative shipping stationery options such as thermal labels or A4 waybills

Parameter | FieldType | Case | Required | Note
--------- | --------- | ---- | -------- | ---- 
serviceCode | String | No | Yes | The service delivery priority code

Service delivery code values are:

* SDX - Sameday Express
* DBD - Daybreak Delivery
* ONC - Overnight Courier
* NCA - Normal Cargo
* BUD - Budget Cargo

### Sender Address

Parameter | FieldType | Case | Required | Note
--------- | --------- | ---- | -------- | ------- 
senderCompany | String | No | No | Company from which to collect
senderFloorBuilding | String | No | Yes | Floor, building, office park
senderStreet | String | No | Yes | Street for shipment collection
senderSuburb | String | No | Yes | Sender suburb
senderCity | String | No | Yes | Sender town or city
senderPostCode | String | No | Yes | Sender postcode
senderProvince | String | No | No | Sender province.
senderLat | Double | N/A | No | Sender coordinate latitude value (we encourage the use of these)
senderLon | Double | N/A | No | Sender coordintae longitude value (we encourage the use of these)
senderContactName | String | No | No | Contact person for collection/dispatch
senderMobile | String | No | No | Contact person mobile number
senderTel | String | No | No | Contact person telephone number
senderTelHome | String | No | No | Contact person telephone number (home or alternate)
senderEmail | String | No | No | Contact person email address

<aside class="notice">
  senderMobile and senderEmail are useful for shipment progress updates.
</aside>

### Receiver Address
Parameter | FieldType | Case | Required | Note
--------- | --------- | ---- | -------- | ------- 
receiverCompany | String | No | No | Receiver individual/company name
receiverFloorBuilding | String | No | No | Floor and building address.
receiverStreet | String | No | Yes | Receiver street number and name
receiverSuburb | String | No | Yes | Receiver suburb.
receiverCity | String | No | Yes | Receiver city or town.
receiverPostCode | String | Yes | No | Receiver postcode
receiverProvince | String | No | No | Receiver province.
receiverLat | Double | N/A | No | Receiver coordinate latitude value (we encourage the use of these)
receiverLon | Double | N/A | No | Receiver coordinate longitude value (we encourage the use of these)
receiverContactName | String | No | No | Receiver name.
receiverMobile | String | No | No | Useful for status updates
receiverEmail | String | No | No | Useful for status updates
receiverTel | String | No | No | Receiver telephone number
receiverTelHome | String | No | No | Receiver telephone number (home or alternate).

If you would like to add insurance cover on your waybill, complete these two fields:

Parameter | FieldType | Case | Required | Note
--------- | --------- | ---- | -------- | ------- 
insuranceRequired | Boolean | No | No | _true_ if insurance is required for this shipment.
insuranceValue | Float/Double | No | Yes | Insured value, in ZAR. Omit this field if you _do not_ require insurance.

### Dimensioning

For communication of parcel specific information for each package that comprises this waybill shipping request.

Parameter | FieldType | Case | Required
--------- | --------- | ---- | -------- 
dimensions | Array | No | Yes | An array of parcels for this shipment.

<aside class="notice">
  Should you not have the individual weights of each package to be shipped on the waybill
  you can state the total weight of all of the parcels in one of the dimension array&#39;s weight field,
  followed by zeroes for the remaining weights in the dimensions array.
</aside>

**The `dimensions` array comprises:**

FieldName | FieldType | Req | Description
--------- | --------- | --- | -----------
dimension1 | Int | Yes | Length of the package, rounded up to the nearest whole centimeter
dimension2 | Int | Yes | Width of the package, rounded up to the nearest whole centimeter
dimension3 | Int | Yes | Height of the package, rounded up to the nearest whole centimeter
weight | Float | Yes | Actual weight of package in kilograms
reference | String | No | Tag this package with parcel specific information.


### Additional References

You can include additional references to the waybill for ease of reference and search.

Parameter | FieldType | Case | Required
--------- | --------- | ---- | -------- 
additionalReferences | Array | No | No | An array of additional references.

**The `additionalReferences` array comprises:**

FieldName | FieldType | Req | Description
--------- | --------- | --- | -----------
value | String | Yes | The reference number you want to add.

<aside class="notice">
  Ensure that special characters are URL encoded.
</aside>

### Printing Options

These options should only be used if so advised by your sales representative or a BEX staff member. You may need additional information and/or setup before you may use these features.

Parameter | FieldType | Req | Required
--------- | --------- | ---- | -------- 
returnParcelPrintingText | Boolean | No | Optional. If `true`, returns EPL thermal label data as part of the response for you to send to the thermal printer.
printParcels | Boolean | No | Optional. If `true`, parcels are printed to the thermal label printer after submission using the `printUrl` specified.
printUrl | String | No | Optional. Specify the thermal printer URL to print to IF `printParcels=true`.
printParcelsPrintNode | Boolean | No | Optional. Specify `true` to use PrintNode to print the labels.

## Response
The base object will contain a property called `items` which consists of an array holding a single object with the following properties:
 
Property | Description
-------- | -----------
waybillNumber | String value reflecting the unique BEX waybill number for this successful shipping request.
baseRateTotal | Price of the rate card billing component for shipment. 
surchargesTotal | Price of the surcharges for additional service add-ons
subTotal | The subtotal, excluding VAT, for the shipment routing requested.
vat | The VAT portion of the pricing for the delivery.
totalCharges | The grand total price for the waybill.
volRatio | The volumetric ratio used during the volumetric weight calculation in the billing engine. This ratio is directly dependent on the service delivery priority requested.

## Submission Failures
Should your request fail, please inspect the `ex` property as mentioned earlier in this document. 

<aside class="notice">
  These errors often will need to be addressed before you can re-submit them to ensure successful submission.
</aside>
