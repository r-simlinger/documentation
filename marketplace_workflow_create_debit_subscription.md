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

# Save the customer (payer) data

After you got a vlid token you can start with saving the customer data. From the response you need to save the customer->id (f.e. PCU_3RGVMQ5GR2MG4KUZ875XUNP5YQYVAA).

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

For each bank account of the customer, you need to save the data in a payment container. From the response you need to save the container->id (f.e. PCT_26VH44YJM2MG4Y6WX75XUVH65QYVAB).

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

If the bank account already exists for the customer you will get the following error as response:

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

After you have saved the customer data and the bank account, you can start with the creation of the first payment. This payment can be reused (as subscription) for further (f.e monthly) payment transactions.

## Request

```
{
    "container": "PCT_26VH44YJM2MG4Y6WX75XUVH65QYVAB",
    "customer": "PCU_3577GD8KT2MG4Y5UX75XUVH65QYVAA",
    "contract": "PCR_W30TEBYFV2MG4K03875XUNP35QYVAB",
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
    		"contract_id": "PCR_KR40B0A0T2MF75EN875XUBW87M8UA6",
    		"total": "490"
    	}
	]
}
```

## Response

```
TODO
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
