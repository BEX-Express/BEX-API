# Waybill Downloads
## Overview
As part of our service offering we allow you to download an _e-waybill_ and any parcel labels associated with the waybill. The _e-waybill_ is in A4 format and can be printed using any printer at your disposal.

## Authentication
The stationery download service requires a different token than the one you have been using to access the other endpoints. 

You will need to re-complete the authentication procedure but you are required to set the `preferAlternateToken` to `true`. This will result in a completely different token being issued for your request. You **must use this new token** for the stationery download service **only**.

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
A successful response will be the resulting PDF document. If you are accessing it through a browser, the download will happen automatically. 

If you decide to integrate using your own system or code, you will need to process the response accordingly.

<aside class="notice">
  An error response will still contain the relevant error message in the `ex` parameter of the response. 
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
A successful response will be the resulting PDF document. If you are accessing it through a browser, the download will happen automatically. 

If you decide to integrate using your own system or code, you will need to process the response accordingly.

<aside class="notice">
  An error response will still contain the relevant error message in the `ex` parameter of the response. 
</aside>

### Label Size
The downloadable label is sized at 10.08cm x 14.98cm and may be printed on any printer you have at your disposal.
