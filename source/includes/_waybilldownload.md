# Waybill Downloads
## Overview
As part of our service offering we allow you to download an _e-waybill_ and any parcel labels associated with the waybill. The _e-waybill_ is in A4 format and can be printed using any printer at your disposal.

<aside class="notice">
  The parcel labels are sized at 10.08cm x 14.98cm. You can print these using any printer at your disposal too.
</aside>

## Authentication
The stationery downloads service requires a different token than the one you have been using to access the other endpoints. 

<aside class="notice">
  To access the stationery download service, please re-authenticate by adding/setting the `preferAlternateToken` to `true`, for example `https://api.bex.co.za/api/service/login?userName=your_username&password=your_password&preferAlternateToken=true`, and then use the new token in the response to download your stationery.
</aside>

## eWaybill Download
You can download the e-waybill from `https://api.bex.co.za/api/service/downloadewaybill`.

<aside class="notice">
  Only one e-waybill can be requested in a single call.
</aside>

> Supply only 1 waybill number per call.
```json
{
  "token": "`alternate_token`", 
  "waybillNumber": "waybill_number"
}
```

### Parameters
Parameter | FieldType | Required | Description
--------- | --------- | -------- | -----------
token | String | Yes | The `alternate_token` received through the `login` service.
waybillNumber | String | Yes | A single waybill number to lookup and produce a PDF for.

### Response
An error response will still contain the relevant error message in the `ex` parameter of the response. 

**However**, a successful response will be the resulting PDF document. If you are accessing the service endpoint using query parameters inside a browser, the document will automatically download to your computer. 

<aside class="notice">
  If you are accessing this service through code, you will need to process the result and save it to disk so that you can access the file.
</aside>

## Labels Downloads
You can download "thermal" labels from `https://api.bex.co.za/api/service/downloadparcellabels`. 

> Example JSON body.
```json
{
  "token": "`alternate_token`", 
  "waybillNumbers": "waybill_number_1,waybill_number_2,waybill_number_3,..."
}
```

### Parameters
Parameter | FieldType | Required | Description
--------- | --------- | -------- | -----------
token | String | Yes | The `alternate_token` received through the `login` service.
waybillNumbers | String | Yes | A comma-seperated list of waybill numbers or parcel numbers to generate PDF labels for.

### Response
An error response will still contain the relevant error message in the `ex` parameter of the response. 

**However**, a successful response will be the resulting PDF document. If you are accessing the service endpoint using query parameters inside a browser, the document will automatically download to your computer. 

<aside class="notice">
  If you are accessing this service through code, you will need to process the result and save it to disk so that you can access the file.
</aside>

If you have requested multiple parcels and/or waybills in this request, the document will be multi-paged.
