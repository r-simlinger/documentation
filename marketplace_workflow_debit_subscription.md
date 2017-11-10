# Get Token

At first you need to get a session token for the further request.

## Request

```
POST /oauth/token HTTP/1.1
Host: connect-testing.secupay-ag.de
Content-Type: application/x-www-form-urlencoded
Cache-Control: no-cache

grant_type=client_credentials&client_id=09ae83af7c37121b2de929b211bad944&client_secret=9c5f250b69f6436cb38fd780349bc00810d8d5051d3dcf821e428f65a32724bd
```

## Response

```
{
    "access_token": "gm3594cd1vvqffokcf1lc8t735",
    "expires_in": 1200,
    "token_type": "bearer",
    "scope": "https://scope.secucard.com/e/api"
}
```

# Create Project/Merchant

With a valid token you can start to create a new project (merchant).

From the response you need to save the field "contract->id" (f.e. PCR_20NP4X8NK2MG5BAD875XUKDW5QYVAB).

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
   "project":"project_name #XYZ",
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
        "id": "PCR_20NP4X8NK2MG5BAD875XUKDW5QYVAB",
        "parent": {
            "object": "payment.contracts",
            "id": "PCR_2NSCASA2N2MF75F5875XUDD87M8UA6"
        },
        "demo": true,
        "allow_cloning": false,
        "sepa_mandate_inform": "never",
        "created": "2017-11-10T08:51:54+01:00"
    },
    "apikey": "40a80e5a1526e031213ac42c11312911da8b58fa",
    "payin_account": null
}
```

# Save the customer (payer) data

To create payment transactions you need to transmit the customer (payer) of the customer first.

From the response you need to save the field "id" (f.e. PCU_3RGVMQ5GR2MG4KUZ875XUNP5YQYVAA).

## Request

```
POST /api/v2/Payment/Customers HTTP/1.1
Host: connect-testing.secupay-ag.de
Accept: application/json
Content-Type: application/json
Accept-Charset: utf-8
Authorization: Bearer gm3594cd1vvqffokcf1lc8t735
Cache-Control: no-cache

{
    "contact": {
        "salutation": "Mr.",
        "title": "Dr.",
        "forename": "John",
        "surname": "Doe",
        "companyname": "Example Inc.",
        "dob": "1901-02-03",
        "email": "example@example.com",
        "phone": "0049-123-456789",
        "mobile": "0049-987-654321",
        "address": {
            "street": "Example Street",
            "street_number": "6a",
            "postal_code": "01234",
            "city": "Examplecity",
            "country": "DE"
        }
    }
}
```

## Response

```
{
    "object": "payment.customers",
    "id": "PCU_3577GD8KT2MG4Y5UX75XUVH65QYVAA",
    "contract": {
        "object": "payment.contracts",
        "id": "PCR_2NSCASA2N2MF75F5875XUDD87M8UA6"
    },
    "contact": {
        "forename": "John",
        "surname": "Doe",
        "companyname": "Example Inc.",
        "name": "John Doe",
        "salutation": "Mr.",
        "title": "Dr.",
        "address": {
            "street": "Example Street",
            "street_number": "6a",
            "postal_code": "01234",
            "city": "Examplecity",
            "country": "DE"
        },
        "email": "example@example.com",
        "mobile": "0049-987-654321",
        "phone": "0049-123-456789",
        "dob": "1901-02-03T00:00:00+01:00"
    },
    "created": "2017-11-09T20:16:13+01:00"
}
```

# Save the debit payment instrument

For each payment instrument (bank account) of the customer, you need to save the data in a payment container.

From the response you need to save the field "id" (f.e. PCT_26VH44YJM2MG4Y6WX75XUVH65QYVAB).

## Request

```
{
    "type": "bank_account",
    "customer": "PCU_3RGVMQ5GR2MG4KUZ875XUNP5YQYVAA",
    "private": {
        "owner": "John Doe",
        "iban":"DE89370400440532013000",
        "bic":"COBADEFFXXX"
    }
}
```

## Response

```
{
    "object": "payment.containers",
    "id": "PCT_26VH44YJM2MG4Y6WX75XUVH65QYVAB",
    "contract": {
        "object": "payment.contracts",
        "id": "PCR_2NSCASA2N2MF75F5875XUDD87M8UA6"
    },
    "customer": {
        "object": "payment.customers",
        "id": "PCU_3577GD8KT2MG4Y5UX75XUVH65QYVAA"
    },
    "private": {
        "owner": "John Doe",
        "iban": "DE89370400440532013000",
        "bic": "COBADEFFXXX",
        "bankname": "Commerzbank"
    },
    "public": {
        "owner": "John Doe",
        "iban": "DE89370400440532013000",
        "bic": "COBADEFFXXX",
        "bankname": "Commerzbank"
    },
    "type": "bank_account",
    "created": "2017-11-09T20:16:19+01:00"
}
```

## Error

If the bank account already exists for the customer you will get the following error:

```
{
    "status": "error",
    "error": "ProductDuplicateException",
    "error_details": "Container with IBAN DE89370400440532013000 for customer already exists",
    "error_user": "Es ist ein unbekannter Fehler aufgetreten",
    "code": 0,
    "supportId": "9c9a230e5b3f1408e92c520cd2a5636c"
}
```

# Create the first payment transaction

After you have transmitted the customer data and the bank account, you can start with the creation of the first payment.

The following example descrripte the way to create a subscription. So this payment can be reused for further (f.e monthly) payment transactions, after the first payment transaction completed successfully.

To define for which project (merchant) the payment should be performed, you need to transmit the field "contract" (with the id from the examples above).

To create a subscription you need to transmit the field "subscription->purpose". In the response you will receive the field "subscription->id". With this id you can then start new payment transactions without enter the complete payment details again.

In the field "basket" you should transmit the items which the customer wants to pay for.

And if you want to define some plattform fee (so the project will not get the full amount) you need to add a basket position with the "item_type" = "stakeholder_payment" and the contract id of the plattform (in general this is the parent id of the created project - see the example above).

In the response there is also a field "redirect_url->iframe_url" (f.e. https://api-testing.secupay-ag.de/payment/qbhjffjfalar2408801 ). To this url you need to redirect the customer, so that he can complete the payment process.

## Request

```
{
    "container": "PCT_26VH44YJM2MG4Y6WX75XUVH65QYVAB",
    "customer": "PCU_3577GD8KT2MG4Y5UX75XUVH65QYVAA",
    "contract": "PCR_20NP4X8NK2MG5BAD875XUKDW5QYVAB",
    "amount": "9490",
    "currency": "EUR",
    "purpose": "for what text",
    "order_id": "2017418454",
    "subscription": {
    	"purpose": "for what text"
    },
    "basket": [
    	{
    		"item_type": "article",
    		"name": "Super fancy product - 45,00 EUR",
    		"price": "4500",
    		"quantity": "2",
    		"tax": 19,
    		"total": "9000"
    	},
    	{
    		"item_type": "shipping",
    		"name": "DHL Paket National - 4,90 EUR",
    		"price": "490",
    		"quantity": "1",
    		"tax": 19,
    		"total": "490"
    	},
    	{
    		"item_type": "stakeholder_payment",
    		"name": "platform fee",
    		"price": "475",
    		"quantity": "1",
    		"contract_id": "PCR_2NSCASA2N2MF75F5875XUDD87M8UA6",
    		"total": "490"
    	}
	]
}
```

## Response

```
{
    "object": "payment.secupaydebits",
    "id": "qbhjffjfalar2408801",
    "trans_id": "10899396",
    "status": "internal_server_status",
    "amount": "9490",
    "currency": "EUR",
    "purpose": "for what text",
    "order_id": "2017418454",
    "transaction_status": "1",
    "basket": [
        {
            "item_type": "article",
            "name": "Super fancy product - 45,00 EUR",
            "price": 4500,
            "quantity": 2,
            "tax": 19,
            "total": 9000
        },
        {
            "item_type": "shipping",
            "name": "DHL Paket National - 4,90 EUR",
            "price": 490,
            "quantity": 1,
            "tax": 19,
            "total": 490
        },
        {
            "apikey": "37373c132df0299c5bdcf7c7638dd47aa41a2fe2",
            "item_type": "stakeholder_payment",
            "name": "platform fee",
            "price": 475,
            "quantity": 1,
            "total": 490
        }
    ],
    "payment_action": "sale",
    "customer": {
        "object": "payment.customers",
        "id": "PCU_3577GD8KT2MG4Y5UX75XUVH65QYVAA",
        "contract": {
            "object": "payment.contracts",
            "id": "PCR_2NSCASA2N2MF75F5875XUDD87M8UA6"
        },
        "contact": {
            "forename": "John",
            "surname": "Doe",
            "companyname": "Example Inc.",
            "name": "John Doe",
            "salutation": "Mr.",
            "title": "Dr.",
            "address": {
                "street": "Example Street",
                "street_number": "6a",
                "postal_code": "01234",
                "city": "Examplecity",
                "country": "DE"
            },
            "email": "example@example.com",
            "mobile": "0049-987-654321",
            "phone": "0049-123-456789",
            "dob": "1901-02-03T00:00:00+01:00"
        },
        "created": "2017-11-09T20:16:13+01:00"
    },
    "used_payment_instrument": {
        "type": "bank_account",
        "data": {
            "owner": "",
            "iban": "DE89 XXXX XXXX XXXX XX30 00",
            "bic": "COBADEFFXXX",
            "bankname": "Commerzbank"
        }
    },
    "redirect_url": {
        "iframe_url": "https://api-testing.secupay-ag.de/payment/qbhjffjfalar2408801",
        "url_success": "http://example.com",
        "url_failure": "http://example.com"
    },
    "container": {
        "object": "payment.containers",
        "id": "PCT_26VH44YJM2MG4Y6WX75XUVH65QYVAB",
        "contract": {
            "object": "payment.contracts",
            "id": "PCR_2NSCASA2N2MF75F5875XUDD87M8UA6"
        },
        "customer": {
            "object": "payment.customers",
            "id": "PCU_3577GD8KT2MG4Y5UX75XUVH65QYVAA",
            "contract": {
                "object": "payment.contracts",
                "id": "PCR_2NSCASA2N2MF75F5875XUDD87M8UA6"
            },
            "contact": {
                "forename": "John",
                "surname": "Doe",
                "companyname": "Example Inc.",
                "name": "John Doe",
                "salutation": "Mr.",
                "title": "Dr.",
                "address": {
                    "street": "Example Street",
                    "street_number": "6a",
                    "postal_code": "01234",
                    "city": "Examplecity",
                    "country": "DE"
                },
                "email": "example@example.com",
                "mobile": "0049-987-654321",
                "phone": "0049-123-456789",
                "dob": "1901-02-03T00:00:00+01:00"
            },
            "created": "2017-11-09T20:16:13+01:00"
        },
        "private": {
            "owner": "John Doe",
            "iban": "DE89370400440532013000",
            "bic": "COBADEFFXXX",
            "bankname": "Commerzbank"
        },
        "public": {
            "owner": "John Doe",
            "iban": "DE89370400440532013000",
            "bic": "COBADEFFXXX",
            "bankname": "Commerzbank"
        },
        "type": "bank_account",
        "created": "2017-11-09T20:16:19+01:00"
    },
    "iframe_url": "https://api-testing.secupay-ag.de/payment/qbhjffjfalar2408801",
    "subscription": {
        "id": 930,
        "purpose": "for what text"
    }
}
```

## Error

```
{
    "status": "error",
    "error": "ProductNotAllowedException",
    "error_details": "Referenced contract access denied PCR_W30TEBYFV2MG4K03875XUNP35QYVAB",
    "error_user": "Es ist ein unbekannter Fehler aufgetreten",
    "code": 0,
    "supportId": "6113d143e8f640daec79f3cfb48ebaf0"
}
```
