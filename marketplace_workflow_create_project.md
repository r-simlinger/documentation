# Get Token

At first you need to get a session token for the further request.

## Request

```
POST /oauth/token HTTP/1.1
Host: connect-testing.secupay-ag.de
Content-Type: application/x-www-form-urlencoded
Cache-Control: no-cache

grant_type=client_credentials&client_id=61c4e67fca435d864436fc174a0c309d&client_secret=0ecfb081e6af8c5bae90923ec8fa384b83bb7097c21ee1457fad0ab994a81617
```

## Response

```js
{
    "access_token": "biu3647lorggqhhoh46qsou202",
    "expires_in": 1200,
    "token_type": "bearer",
    "scope": "https://scope.secucard.com/e/api"
}
```

# Create Project/Merchant
With a valid token you can start to create a new project (merchant). From the response you need to store the contract->id (f.e. PCR_W30TEBYFV2MG4K03875XUNP35QYVAB).

## Request
```
POST /api/v2/Payment/Contracts/me/RequestId HTTP/1.1
Host: connect-testing.secupay-ag.de
Accept: application/json
Content-Type: application/json
Accept-Charset: utf-8
Authorization: Bearer biu3647lorggqhhoh46qsou202
Cache-Control: no-cache

{
   "contact":{
      "salutation":"Mr.",
      "title":"Dr.",
      "forename":"John",
      "surname":"Doe",
      "companyname":"Example Inc.",
      "gender":"m",
      "dob":"1901-02-03",
      "url_website":"example.com",
      "birthplace":"New Examplecity",
      "nationality":"german",
      "address":{
         "type":"invoice",
         "street":"Example Street",
         "street_number":"6a",
         "postal_code":"01234",
         "city":"Examplecity",
         "country":"DE"
      },
      "email":[
         {
            "type":"invoice",
            "email":"example@example.com"
         }
      ],
      "phone":[
         {
            "type":"invoice",
            "phone":"0049-123-456789"
         }
      ]
   },
   "project":"project_name #XYZ1",
   "payout_account":{
      "iban":"DE89370400440532013000",
      "bic":"COBADEFFXXX",
      "owner":"John Doe"
   },
   "iframe_opts":{
      "show_basket":true,
      "basket_title":"Projext XY unterstützen",
      "submit_button_title":"Zahlungspflichtig unterstützen"
   },
   "payin_account":false
}
```

## Response

```
{
    "contract": {
        "object": "payment.contracts",
        "id": "PCR_W30TEBYFV2MG4K03875XUNP35QYVAB",
        "parent": {
            "object": "payment.contracts",
            "id": "PCR_KR40B0A0T2MF75EN875XUBW87M8UA6"
        },
        "demo": true,
        "allow_cloning": false,
        "sepa_mandate_inform": "never",
        "created": "2017-11-09T19:35:50+01:00"
    },
    "apikey": "0efa58d5becfc95879f68ef4f57055ceb8514839",
    "payin_account": null
}
```

