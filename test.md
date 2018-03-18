# Managing Customers

The customer is the customer of the merchant. There is an object created with the payment transaction. So in most cases "Customer" is the person who will pay the created payment transaction.

Customer is a collection of personal data and address.

Every time a new customer (payer) registered on the platform and wants to make a payment for the first time, it is necessary to transfer the customer data to the secuconnect-API. The created Customer-ID can then be reused for each future payment transaction payment

Possible actions:
* create a new customer
* read the customers data
* update an existing customer
* delete a customer
* read a list of all existing customers.

Every API user who is able to access the payment service can create a new customer, read his data, update it, delete it and get a list of all existing customers.

You find more information in the different sections below.

## Data models

### PaymentCustomersProductModel

### PaymentCustomerDTOContact

### PaymentCustomersDTO

## Supported actions

### Create a new customer

To create new Customer `PaymentCustomersDTO` need to be passed to **`paymentCustomersPost`** method from `PaymentCustomersApi` class. With `PaymentCustomersDTO` prepared we can pass it to `paymentCustomersPost`.

#### Example

(In this code example there is no Authentication part, it is explained TODO.)

##### Request

First we create address.

```php
// Build request objects
$contactAddress = new Address();
$contactAddress
  ->setStreet('Frankfurter Str.')
  ->setStreetNumber('125a')
  ->setPostalCode('60326')
  ->setCity('Frankfurt am Main')
  ->setCountry('Germany');
```

Then we provide other contact informations.

```php
$customerContact = new PaymentCustomersDTOContact();
$customerContact
  ->setSalutation('Mr.')
  ->setTitle('Dr.')
  ->setForename('John')
  ->setSurname('Doe')
  ->setCompanyname('My Company Inc.')
  ->setDob('1905-03-03')
  ->setEmail('mr-john-doe@mail.com')
  ->setPhone('0049-123-456789')
  ->setMobile('0049-987-654321')
  ->setAddress($contactAddress);
```

Then we provide other contact informations.

```php
$customer = new PaymentCustomersDTO();
$customer->setContact($customerContact);
```

Customer DTO only contains contact information.

```php
// Make request
try {
  $api = new PaymentCustomersApi();
  $response = $api->paymentCustomersPost($customer);
  // Success. $response contain PaymentCustomersProductModel
  $customerId = $response->getId();
 
  echo 'Created new payment customer product with ID: ' . $customerId . PHP_EOL;
  echo 'Payment customer product data after create:' . PHP_EOL . $response . PHP_EOL;
} catch (\Secuconnect\Client\ApiException $e) {
  // Failure. $e->getResponseBody() contains ProductExceptionPayload
  echo $e->getResponseBody() . PHP_EOL;
  throw $e;
}
```

It's very straightforward code example. Most important is:

* build request objects to line **21** (of course it's just a simple example in real life it will look differently) - this request object contains information about customer address and his personal and contact data like: surname, email, phone, etc.
* making a request to our API using PHP SDK on line **25** we create new object of PaymentCustomersApi and then sending the request using paymentCustomersPost method to create new User

##### Response

###### Success

If creation succeeded `$response` will contain instance of `PaymentCustomersProductModel`.

###### Error

In case of error `$e->getResponseBody()` will contain `ProductExceptionPayload` with 2 possible specific types:

| Error | Possible reasons |
| --- | --- |
| `ProductFormatException` | `no payload` <br/> `invalid dob format` |

###### Example of successful response

After successful creating new payment customer we get a response like below:

```json
{
    "object": "payment.customers",
    "id": "PCU_2...7",
    "contract": {
        "object": "payment.contracts",
        "id": "PCR_2...6"
    },
    "contact": {
        "forename": "John",
        "surname": "Doe",
        "companyname": "My Company Inc.",
        "salutation": "Mr.",
        "title": "Dr.",
        "email": "mr-john-doe@mail.com",
        "phone": "0049-123-456789",
        "mobile": "0049-987-654321",
        "dob": "1905-03-03T00:00:00+01:00",
        "address": {
            "street": "Frankfurter Str.",
            "street_number": "125a",
            "city": "Frankfurt am Main",
            "postal_code": "60326",
            "country": "Germany"
        }
    },
    "created": "2017-12-22T14:59:32+01:00"
}
```

### Update an existing customer

To update customer it's ID is required and `PaymentCustomersDTO` must be updated with new values of properties.

#### Example

(In this code example there is no Authentication part, it is explained TODO.)

##### Request

Let's reuse `$customer` object from previous section, and assume that `$customerId` contains customer ID, then we can update it by following code:

We are creating customer address.

```php
// Build request objects
$contactAddress = new Address();
$contactAddress
  ->setStreet('Braslauer Str.')
  ->setStreetNumber('41c')
  ->setPostalCode('01129')
  ->setCity('Dresden')
  ->setCountry('Germany');
```

We are setting updated contact informations.

```php
$customerContact = new PaymentCustomersDTOContact();
$customerContact
  ->setSalutation('Mr.')
  ->setTitle('Dr.')
  ->setForename('John')
  ->setSurname('Doe')
  ->setCompanyname('My Company Inc.')
  ->setDob('1905-03-03')
  ->setEmail('mr-john-doe@mail.com')
  ->setPhone('0049-123-456789')
  ->setMobile('0049-987-654321')
  ->setAddress($this->contactAddress);
```

We are setting contact on customer DTO.

```php
$customerId = 'PCU_2...7';
```

We also need ID of existing customer. It's the same value as `PaymentCustomerProductModel::getId()`.

```php
// Make request
try {
  $api = new PaymentCustomersApi();
  $response = $api->paymentCustomersIdPut($customerId, $customer);
  // Sucess. $response contain PaymentCustomersProductModel
  echo 'Payment customer product data after update:' . PHP_EOL . $response . PHP_EOL;
} catch (\Secuconnect\Client\ApiException $e) {
  // Failure. $e->getResponseBody() contains ProductExceptionPayload
  echo $e->getResponseBody() . PHP_EOL;
  throw $e;
}
```

It's very similar to creating a new customer. Most important things are:

* build request objects to line **26** (of course it's just a simple example in real life it will look differently) very important is to fill the customerId that we want to update and information about customer address, his personal and contact data like: surname, email, phone, etc. **NOTE:** We need to enter the same data as when a new customer is creating, because when we don't enter those data, then existing data will be overwritten by empty values.
* making a request to our api using PHP SDK on line **29** we create new object of PaymentCustomersApi and then sending the request using paymentCustomersIdPut method to update existing user

##### Response

###### Success

If update succeeded `$response` will contain instance of `PaymentCustomersProductModel`.

###### Error

In case of error `$e->getResponseBody()` will contain `ProductExceptionPayload` with 2 possible specific types:

| Error | Possible reasons |
| --- | --- |
| `ProductFormatException` | `no payload` <br/> `invalid dob format` |

###### Example successful response

After successful updating existing payment customer we get a response like below:

### Delete an existing customer

To delete customer, it's ID is required and should be passed to `paymentCustomersIdDelete method from `PaymentCustomersApi class.

#### Example

(In this code example there is no Authentication part, it is explained TODO.)

##### Request

phpDeleting existing customertruepaymentCustomersIdDelete($customerId); // Sucess. $response contain PaymentCustomersProductModel echo 'Data of payment customer product, which deleted:' . PHP\_EOL . $response . PHP\_EOL; } catch (\\Secuconnect\\Client\\ApiException $e) { // Failure. $e->getResponseBody() contains ProductExceptionPayload echo $e->getResponseBody() . PHP_EOL; throw $e; }\]\]>

To delete a customer only thing we have to do is:

* to fill customerId that we want to delete and then call paymentCustomersIdDelete method of PaymentCustomersApi (line 6)

##### Response

###### Success

If delete succeeded `$response` will contain instance of `PaymentCustomersProductModel`, which was deleted.

###### Error

In case of error `$e→getResponseBody()` will contain `ProductExceptionPayload` with 1 possible specific types:

| Error | Possible reasons |
| --- | --- |
| `ProductNotAllowedException` | `incorrect customer id` |

###### Example successful response

After successful deleting existing payment customer we get a response like below:

### Search for an existing Customer

Existing customers can be search by `id`, `PaymentCustomersAPI->paymentCustomersGetById` should be used for it:

#### Example

(In this code example there is no Authentication part, it is explained TODO.)

##### Request

phpSearching for existing CustomertruepaymentCustomersGetById($customerId); if(empty($response)) { // Not found. } else { // Sucess. $response contain PaymentCustomersProductModel } } catch (\\Exception $e) { // Failure. E.g. network issues. }\]\]>

To search a customer only thing we have to do is:

* to fill customerId that we want to search and then call paymentCustomersIdGetById method of PaymentCustomersApi (line **3**)

##### Response

###### Success

If search succeeded and customer exists `$response` will contain instance of `PaymentCustomersProductModel`, which was found. 

###### Error

If no matching customer will be found, empty object will be returned.

### Obtain list of Customers

Existing customers can be listed using `PaymentCustomersAPI->paymentCustomersGet` method.

#### Example

(In this code example there is no Authentication part, it is explained TODO.)

##### Request

phpListing customerstruepaymentCustomersGet(); if(empty($response)) { // No customers found. } else { // Sucess. $response contain list of PaymentCustomersProductModel $response->getCount(); foreach($response->getData() as $customer) {...} } // Sucess. $response contain list of PaymentCustomersProductModels } catch (\\Exception $e) { // Failure. E.g. network issues. }\]\]>

To obtain list of customers we must:

* call paymentCustomersGet method of PaymentCustomersApi (line **3**)

##### Response

###### Success

If obtain succeeded and any customers exist `$response` will contain object with count of existing customers and data, where data is a list of PaymentCustomersProductModel.
