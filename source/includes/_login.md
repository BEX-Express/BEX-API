# Login
## Overview
The *login* service serves as the starting point from which a valid session token will be generated. You call this endpoint to authenticate yourself using your BEX credentials.

The BEX platform will validate your request and if successful, generate and return in the response object a token contained in the `value` attribute.

<aside class="notice">
  This token is unique to your integration user identity and is to be **kept safe and secure** at all times.

  Should your token become compromised, please reach out to us as soon as possible.
</aside>

## Endpoint
The authentication endpoint is located at `https://api.bex.co.za/api/service/login`.

> Ensure special characters are URL encoded.
```json
{
  "username": "your_bex_username", 
  "password": "your_bex_password"
}
```

```javascript
<!DOCTYPE html>
<html xmlns="http://www.w3.org/1999/xhtml">
<head>
<title>BEX API Login Example</title>
<script src="http://code.jquery.com/jquery-latest.min.js" type="text/javascript"></script>
</head>
<body>
<script type="text/javascript">
    var user = encodeURIComponent('username'); //encode the username
    var pass = encodeURIComponent('password'); //encode the password
    jQuery.support.cors = true; //this will enable cross domain access for all services

    //call the service using jQuery ajax and passing the parameters
    $.ajax({
      url: 'https://api.bex.co.za/api/service/login?userName=' + user + '&password=' + pass, 
      type: 'POST', 
      async: false, //No need fora sync
      success: function (d) {
        if (d.ex) {
          alert('An error has occurred. ' + d.ex);
          return;
        }

        alert('Your token is ' + d.value);
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
preferAlternateToken | Boolean | No | Set to _true_ if so required by the documentation.

## Response

> Sample json response.

```json
{
    "id": 19696,
 // Here's the token -------------------------------------------------------
    "value": "dOdw98ycsih298749bd6MbZre8o7tt3r3nhfowo8dnBDpEJvkzcQG1vb44sy",
 // ------------------------------------------------------------------------
    "isStrongPassword": "True",
    "isEmailVerified": "True",
    "email": "your.email@company.com",
    "signalREnabled": "False",
    "allowApiLogin": "False"
}
```

The response body is structured as follows:

Attribute | Type | Description
--------- | ---- | -----------
id | Int | An internal BEX ID used to identify your user.
value | String | Your unique and private token.
isStrongPassword | String | Confirms whether your password meets our complexity requirements.
isEmailVerified	| String | Your email address is used to recover forgotten passwords or lost tokens and needs to be verified before transactions are possible.
email | String | The recovery email address against which this token is registered.
signalRenabled | Boolean | For internal use only.
allowApiLogin | Boolean | For internal use only.

With your token now generated you can proceed to call our security restricted API&#39;s.

<aside class="notice">
  Include your token in your request header. `token={your_token}`
</aside>
