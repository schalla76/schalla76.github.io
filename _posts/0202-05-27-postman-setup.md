---
layout: post
title: "Postman Setup"
date: 2020-05-27
categories: js, postman
---

## Setup postman for SDx

### Add the below enrinomental variables to the collection

[Inspired from this article](https://nicolaswidart.com/blog/automatically-set-authentication-tokens-in-postman-requests)

```json
{
  "variable": [
    {
      "id": "37a5f584-e318-4487-933e-04f473b0ca10",
      "key": "clientID",
      "value": "TestClient",
      "type": "string"
    },
    {
      "id": "3d2537ec-7d73-415c-8a5b-764e13a57b1e",
      "key": "clientSecret",
      "value": "XYZ12345",
      "type": "string"
    },
    {
      "id": "70a56db6-eb0c-466e-9b86-e8149ad31f1f",
      "key": "resource",
      "value": "XXXXXXX-XXXX-XXX-XXXX-XXXXXXXXX",
      "type": "string"
    },
    {
      "id": "00934b0a-52cf-4097-8df8-c2d276860cac",
      "key": "password",
      "value": "1",
      "type": "string"
    },
    {
      "id": "25dce16b-19ac-4df8-ab0d-16b0af932713",
      "key": "username",
      "value": "superuser",
      "type": "string"
    },
    {
      "id": "8f8da666-68fb-4968-8a66-0688d7a3ada2",
      "key": "jwtToken",
      "value": "Test",
      "type": "string"
    },
    {
      "id": "b9643306-171c-46ac-b7f2-1b04740589f2",
      "key": "odataURL",
      "value": "http://XXXXXX/XXXXXXServer/api/v2/SDA",
      "type": "string"
    },
    {
      "id": "8bc9ad78-ceb1-490f-8d4a-3f1634d7da27",
      "key": "oauthURL",
      "value": "http://XXXXXX/XXXXXXConfigSvc/SPFAuthentication/oauth",
      "type": "string"
    }
  ]
}
```

### Add the below code to the collection script

```js
var clientID = pm.collectionVariables.get('clientID');
var clientSecret = pm.collectionVariables.get('clientSecret');
var resource = pm.collectionVariables.get('resource');
var username = pm.collectionVariables.get('username');
var password = pm.collectionVariables.get('password');
var odataURL = pm.collectionVariables.get('odataURL');
var jwtToken = pm.collectionVariables.get('jwtToken');
var oauthURL = pm.collectionVariables.get('oauthURL');


var sdk = require('postman-collection');

var isValidTokenRequest = new sdk.Request({
    url: odataURL + "/IsFTRAvailable",
    method: 'POST',
    header: [
        new sdk.Header({
            key: 'content-type',
            value: 'application/json',
        }),
        new sdk.Header({
            key: 'accept',
            value: 'application/json',
        }),
        new sdk.Header({
            key: 'Authorization',
            value: 'Bearer ' + pm.collectionVariables.get("jwtToken"),
        }),
    ]
});

pm.sendRequest(isValidTokenRequest, function (err, response) {
    if (response.code === 401) {
        refreshToken();
    }
});

function refreshToken() {

    var tokenRequest = new sdk.Request({
    url: oauthURL + '/connect/token',
    method: 'POST',
    header: [
        new sdk.Header({
            key: 'Content-Type',
            value: 'application/x-www-form-urlencoded'
        }),
        new sdk.Header({
            key: 'Accept',
            value: '*/*'
        }),
    ],
    body: {
        mode: 'urlencoded',
        urlencoded: [
          {key: "grant_type", value: "password", disabled: false},
          {key: "username", value: username, disabled: false},
          {key: "password", value: password, disabled: false},
          {key: "client_id", value: clientID, disabled: false},
          {key: "client_secret", value: clientSecret, disabled: false},
          {key: "scope", value: 'ingr.api', disabled: false},
          {key: "resource", value: resource, disabled: false},
      ]
    }
  });

  pm.sendRequest(tokenRequest, function (err, response) {
      if (err) {
          throw err;
      }
      
      if (response.code !== 200) {
          throw new Error('Could not log in.');
      }
      
      pm.collectionVariables.set("jwtToken", response.json().access_token);
      console.log(`New token has been set: ${response.json().access_token}`);
  });
}
```