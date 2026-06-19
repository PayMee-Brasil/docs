---
title: API PayMee 1.1 v1.0.0
language_tabs:
  - shell: Shell
  - http: HTTP
  - javascript: JavaScript
  - ruby: Ruby
  - python: Python
  - php: PHP
  - java: Java
  - go: Go
toc_footers: []
includes: []
search: true
highlight_theme: darkula
headingLevel: 2

---

<!-- Generator: Widdershins v4.0.1 -->

<h1 id="api-paymee-1-1">API PayMee 1.1 v1.0.0</h1>

> Scroll down for code samples, example requests and responses. Select a language for code samples from the tabs above or the mobile navigation menu.

# Overview

The purpose of this documentation is to guide the developer on how to integrate with PayMee, describing the features and methods to be used, listing information to be sent and received as well as providing examples.

The integration mechanism with PayMee is simple, so only intermediate knowledge in Web programming language, HTTP/HTTPS requests and JSON file manipulation are required to successfully deploy the PayMee solution.

In this manual you will find reference to all operations available on the API REST of the PayMee API. These operations must be performed using specific keys (**x-api-key** and **x-api-token**) on the respective environment endpoints:

Production Environment

[<b>https://api.paymee.com.br/</b>](https://api.paymee.com.br/)

Sandbox Environment

[<b>https://apisandbox.paymee.com.br/</b>](https://apisandbox.paymee.com.br/)

To perform an operation, combine the base URL of the environment with the resource (URI) of the desired operation and send it using the HTTP verb as described in the operation.

# Solution features

The API solution of the PayMee platform was developed with REST technology, which is the market standard and also independent from the technology used by other parties. This way, it is possible to integrate it through the great majority of programming languages, such as: ASP, ASP. Net, Java, PHP, Ruby, Python, etc.

Amongst other features, the attributes that stand out the most in the PayMee platform are:

**No proprietary apps**: it is not necessary to install any applications in the virtual shop environment, under no circumstances.

**Simplicity**: the protocol used is purely HTTPS.

**Ease of testing**: the PayMee platform offers a publicly accessible Sandbox environment, which allows the developer to create a [test account](https://sandbox.paymee.com.br/register) making it easier and faster to start integration.

**Credentials**: handling of the customer’s credentials (**x-api-key** and **x-api-token**) traffics in the header of the HTTP request of the message.

**Safety**: the information exchange always takes place between the store's Server and PayMee's Server, that is, without the buyer’s browser in between.

**Multiplatform**: the integration is performed through the REST Web Service.

# Architecture

Integration is performed through Web Services. The model employed is quite simple: There are two URLs (endpoints) available: a specific one for production (real time operation), and a specific one for testing, called sandbox.

These URLs receive the HTTP messages through the **POST**, **GET** or **PUT** methods. Each message type must be sent to a resource identified through the path.

| **METHOD** | **DESCRIPTION** |
| --- | --- |
| POST | The **POST** HTTP method is used in the creation of resources or sending information that will be processed. For example, creation of a transaction. |
| PUT | The **PUT** HTTP method is used to update an already existing resource. For example, refunding or cancelation of a previous transaction. |
| GET | The **GET** HTTP method is used for querying already existing resources. For example, transaction query. |

## Supported Payment Methdos

The current version of PayMee Webservice supports the following payment methods:

| BANK | WIRE TRANSFER | CASH | CREDIT CARD |
| --- | --- | --- | --- |
| 001 - BANCO DO BRASIL | YES | YES | NO |
| 237 - BANCO BRADESCO | YES | NO | NO |
| 104 - BANCO CAIXA ECONOMICA FEDERAL | YES | NO | NO |
| 341 - BANCO ITAÚ-UNIBANCO | YES | YES | NO |
| 033 - BANCO SANTANDER BRASIL | YES | YES | NO |
| 212 - BANCO ORIGINAL | YES | NO | NO |
| 077 - BANCO INTER | YES | NO | NO |
| 218 - BANCO BS2 | YES | NO | NO |
| PIX | YES | NO | NO |
| CREDIT CARD | NO | NO | YES |

# Sandbox and Tools

## About Sandbox

To facilitate testing during integration, PayMee offers a Sandbox environment, which is composed by two areas:

- Test account register
- Transactional Endpoint
    - Request: [https://apisandbox.paymee.com.br](https://apisandbox.paymee.com.br)

Advantages of using the Sandbox  
No affiliation is required to use PayMee's Sandbox. You just have to access the Sandbox Registration, create an account and, with it, receive a **x-api-key** and a **x-api-token**, which are the credentials required for the API methods.

You can create your sandbox account here: [<b>https://sandbox.paymee.com.br/register</b>](https://sandbox.paymee.com.br/register)

# Webhook

## Notification Post

The Notification Post is sent based on a selection of events to be made in the PayMee registry.

The events that can be notified are:

| EVENT TYPE | EVENT |
| --- | --- |
| SALE | PAID |
| REVERSAL | CREATE/PAID |

PayMee's send the notifications to the URL that you have configured using the HTTP protocol, by the POST method.

**Authentication**

Basic http auth.  
The username is your **x-api-key** and password is your **x-api-token**

**Base64 Basic Auth reference:**

**x-api-key** = af38b751-30d7-4261-a9fb-ea30f6ece609

**x-api-token** = 28331f43-e2b3-4078-9502-5f656fb66cdf

### TRANSFER/WALK-IN NOTIFICATION (PAYMENT CONFIRMATION)

Here is an example of a notification sent by PayMee (the lines have been broken for ease of reading):

```
POST /callback HTTP/1.1
Authorization: Basic YWYzOGI3NTEtMzBkNy00MjYxLWE5ZmItZWEzMGY2ZWNlNjA5OjI4MzMxZjQzLWUyYjMtNDA3OC05NTAyLTVmNjU2ZmI2NmNkZg==
Content-Length:272
Content-Type:application/json
{
  "saleToken": "d59b39ce-bffd-3f6d-80c4-c376a242afd1",
  "referenceCode":"0000000000014",
   "currency": "BRL",
   "amount":100.00,
   "shopper":{
    "id": "12911",
    "firstName":"Teste",
    "lastName":"Silva",
    "email":"teste@teste.com.br",
    "agency": "1234",
    "account": "123456-0"
   },
  "date":"2017-07-28 10:48:56",
  "newStatus": "PAID"
}

 ```

A Payment Status URL must be registered, so that the notification POST is executed.

The callback URL should return the HTTP status 200. Otherwise PayMee will try again in 60-seconds interval for a maximum of 5 (five) attempts.

## Response

| **PROPERTY** | **TYPE** | **DESCRIPTION** |
| --- | --- | --- |
| saleToken | string | transaction UUID |
| referenceCode | string | order identification |
| amount | number | order amount |
| shopper.firstName | string | shopper's first name |
| shopper.firstName | string | shopper's last name |
| shopper.email | string | shopper's email |
| shopper.agency | string | shopper's agency |
| shopper.account | string | shopper's account |
| date | string | payment date and time (yyyy-MM-dd HH:mm:ss) |
| newStatus | string | payment PAID status |

### REVERSAL NOTIFICATION

Here is an example of a notification sent by PayMee (the lines have been broken for ease of reading):

```
POST /callback?reversing=true&type=reversing HTTP/1.1
Authorization: Basic YWYzOGI3NTEtMzBkNy00MjYxLWE5ZmItZWEzMGY2ZWNlNjA5OjI4MzMxZjQzLWUyYjMtNDA3OC05NTAyLTVmNjU2ZmI2NmNkZg==
Content-Length:272
Content-Type:application/json
{
   "id": 611212,
   "uuid": "d19b39ce-bffd-3f6d-80c4-c376a242afd3",
   "currency": "BRL",
   "originalAmount":100.00,
   "reversedAmount":100.00,
   "status": "PENDING",
   "bankDetails":{
    "bank": "341 - BANCO ITAÚ-UNIBANCO S.A.",
    "branch":"0000",
    "account":"00000-0"
   },
   "sale": {
       "id": 611210,
       "uuid": "75e0b5e2-bc3f-4e75-9c52-24096f2e004d"
   }
   "creation": "2018-04-15 08:33:22",
   "receipt": null,
   "reason": "amount greater than total of sale"
}

 ```

A Reversal Status URL must be registered (just like the payment notification) and you **must enable** this feature on your [merchant panel](https://www2.paymee.com.br/merchants/API), so the notification POST is executed.

This notification occurs two moments:

- when we cannot approve or identify the sale - In this case, we send the status attribute as **PENDING**;
- when we revert the amount successfully to the customer - In this case, we send the status attribute as **PAID**
    

The callback URL should return the HTTP status 200. Otherwise PayMee will try again in 60-seconds interval for a maximum of 5 (five) attempts.

## Response

| **PROPERTY** | **TYPE** | **DESCRIPTION** |
| --- | --- | --- |
| id | number | transaction id |
| uuid | string | transaction uuid |
| currency | string | order currency |
| originalAmount | number | original order amount |
| reversedAmount | number | reversed amount |
| bankDetails.bank | string | credit bank |
| bankDetails.branch | string | credit branch |
| bankDetails.account | string | credit account |
| receipt | string | reversal receipt (under development) |
| status | string | reversal status (PENDING/PAID/CANCELLED) |
| reason | string | reversal reason |
| date | string | reversal date (yyyy-MM-dd HH:mm:ss) |

### REFUND NOTIFICATION

Here is an example of a notification sent by PayMee (the lines have been broken for easy reading):

```
POST /callback?refund=true&type=refund HTTP/1.1
Authorization: Basic YWYzOGI3NTEtMzBkNy00MjYxLWE5ZmItZWEzMGY2ZWNlNjA5OjI4MzMxZjQzLWUyYjMtNDA3OC05NTAyLTVmNjU2ZmI2NmNkZg==
Content-Length:272
Content-Type:application/json
{
   "saleToken": "d19b39ce-bffd-3f6d-80c4-c376a242afd3",
   "referenceCode":"0000000000014",
   "currency": "BRL",
   "originalAmount":150.00,
   "reversedAmount":50.00,
   "bankDetails":{
    "bank": "341 - BANCO ITAÚ-UNIBANCO S.A.",
    "branch":"0000",
    "account":"00000-0"
   },
   "receipt": "null",
   "status": "PAID",
   "refund": {
       "id": 611211,
       "uuid": fbebcd28-ae51-11e9-a2a3-2a2ae2dbcce4,
       "status": "PAID",
       "amount": 50.00,
       "creation": "2018-04-15 08:33:22",
       "creditDate": "2018-04-16 10:48:56"
   }
   "reason": "amount greater than total of sale",
   "date":"2018-04-16 10:48:56"
}

 ```

A Refund Status URL must be registered (The same as the payment notification) and you **must enable** this feature on your **merchant panel**, so the notification POST is executed.

This notification occurs when:

- Refund the amount successfully - In this case, we send the status attribute as **PAID**
    

The callback URL should return the HTTP status 200. Otherwise PayMee will try again in 60-seconds interval for a maximum of 5 (five) attempts.

## Response

| **PROPERTY** | **TYPE** | **DESCRIPTION** |
| --- | --- | --- |
| saleToken | string | transaction UUID |
| referenceCode | string | order identification |
| currency | string | order currency |
| originalAmount | number | original order amount |
| amountRefunded | number | refund amount |
| bankDetails.bank | string | credit bank |
| bankDetails.branch | string | credit branch |
| bankDetails.account | string | credit account |
| receipt | string | refund receipt (under development) |
| status | string | refund status (PENDING/PAID/CANCELLED) |
| reason | string | refund reason |
| date | string | refund date (yyyy-MM-dd HH:mm:ss) |

### PAYOUT NOTIFICATION

Here is a success example of a notification sent by PayMee (the lines have been broken for ease of reading):

```
POST /callback?type=payout HTTP/1.1
Authorization: Basic YWYzOGI3NTEtMzBkNy00MjYxLWE5ZmItZWEzMGY2ZWNlNjA5OjI4MzMxZjQzLWUyYjMtNDA3OC05NTAyLTVmNjU2ZmI2NmNkZg==
Content-Length:272
Content-Type:application/json
{
   "success": true,
   "message": "success"
   "id": 611212,
   "uuid": "d19b39ce-bffd-3f6d-80c4-c376a242afd3",
   "currency": "BRL",
   "amount":100.00,
   "status": "PAID",
   "referenceCode": "XPADOA",
   "bankDetails":{
    "bank": "341 - BANCO ITAÚ-UNIBANCO S.A.",
    "branch":"0000",
    "account":"00000-0"
   },
   "creation": "2018-04-15 08:33:22",
   "receipt": "https://receipts.paymee.com.br/transaction/23e49aa1f7f193395a7f30900f8aeb40a65a9d7dd85e8d05456847b800713546/d19b39ce-bffd-3f6d-80c4-c376a242afd3",
}

 ```

Here is a error example of a notification sent by PayMee (the lines have been broken for ease of reading):

```
POST /callback?payout=true HTTP/1.1
Authorization: Basic YWYzOGI3NTEtMzBkNy00MjYxLWE5ZmItZWEzMGY2ZWNlNjA5OjI4MzMxZjQzLWUyYjMtNDA3OC05NTAyLTVmNjU2ZmI2NmNkZg==
Content-Length:272
Content-Type:application/json
{
   "success": false,
   "message": "Os dados informados não são validos",
   "error_code": "PE0001"
   "id": 611212,
   "uuid": "d19b39ce-bffd-3f6d-80c4-c376a242afd3",
   "currency": "BRL",
   "amount":100.00,
   "status": "PENDING",
   "referenceCode": "XPADOA",
   "bankDetails":{
    "bank": "341 - BANCO ITAÚ-UNIBANCO S.A.",
    "branch":"0000",
    "account":"00000-0"
   },
   "creation": "2018-04-15 08:33:22",
   "receipt": "https://receipts.paymee.com.br/transaction/23e49aa1f7f193395a7f30900f8aeb40a65a9d7dd85e8d05456847b800713546/d19b39ce-bffd-3f6d-80c4-c376a242afd3",
}

 ```

A webhook URL must be registered (just like the payment notification) and you **must enable** this feature on your [merchant panel](https://www2.paymee.com.br/merchants/API), so the notification POST is executed.

## Response

| **PROPERTY** | **TYPE** | **DESCRIPTION** |
| --- | --- | --- |
| success | boolean | payments status flag |
| message | string | return message |
| error_code | string | response error code |
| id | number | PayMee's transaction id |
| uuid | string | PayMee's transaction uuid |
| currency | string | transaction currency |
| amount | number | transaction amount |
| status | string | refund status (PENDING/PAID/CANCELLED) |
| bankDetails.bank | string | credit bank |
| bankDetails.branch | string | credit branch |
| bankDetails.account | string | credit account |
| receipt | string | payment receipt |
| date | string | transaction date (yyyy-MM-dd HH:mm:ss) |
| receiptDate | string | receipt date (yyyy-MM-dd HH:mm:ss) |

## Error codes

| **PROPERTY** | **DESCRIPTION** |
| --- | --- |
| PE0001 | Os dados bancarios fornecidos não validos |
| PE0002 | A titularidade da conta não pertence ao documento informado |
| PE0003 | A conta nao aceita movimentacoes - (MOTIVOS) |
| PE0004 | OUTROS - (MOTIVOS) |

# Resources

Base URLs:

* <a href="https://api.paymee.com.br">https://api.paymee.com.br</a>

* <a href="https://apisandbox.paymee.com.br">https://apisandbox.paymee.com.br</a>

# Authentication

<h1 id="api-paymee-1-1-default">Default</h1>

## getAvailablePaymentMethods

<a id="opIdgetAvailablePaymentMethods"></a>

> Code samples

```shell
# You can also use wget
curl -X GET https://api.paymee.com.br/v1.1/available_payment_methods \
  -H 'Accept: application/json' \
  -H 'x-api-key: {{your-x-api-key}}' \
  -H 'x-api-token: {{your-x-api-token}}'

```

```http
GET https://api.paymee.com.br/v1.1/available_payment_methods HTTP/1.1
Host: api.paymee.com.br
Accept: application/json
x-api-key: {{your-x-api-key}}
x-api-token: {{your-x-api-token}}

```

```javascript

const headers = {
  'Accept':'application/json',
  'x-api-key':'{{your-x-api-key}}',
  'x-api-token':'{{your-x-api-token}}'
};

fetch('https://api.paymee.com.br/v1.1/available_payment_methods',
{
  method: 'GET',

  headers: headers
})
.then(function(res) {
    return res.json();
}).then(function(body) {
    console.log(body);
});

```

```ruby
require 'rest-client'
require 'json'

headers = {
  'Accept' => 'application/json',
  'x-api-key' => '{{your-x-api-key}}',
  'x-api-token' => '{{your-x-api-token}}'
}

result = RestClient.get 'https://api.paymee.com.br/v1.1/available_payment_methods',
  params: {
  }, headers: headers

p JSON.parse(result)

```

```python
import requests
headers = {
  'Accept': 'application/json',
  'x-api-key': '{{your-x-api-key}}',
  'x-api-token': '{{your-x-api-token}}'
}

r = requests.get('https://api.paymee.com.br/v1.1/available_payment_methods', headers = headers)

print(r.json())

```

```php
<?php

require 'vendor/autoload.php';

$headers = array(
    'Accept' => 'application/json',
    'x-api-key' => '{{your-x-api-key}}',
    'x-api-token' => '{{your-x-api-token}}',
);

$client = new \GuzzleHttp\Client();

// Define array of request body.
$request_body = array();

try {
    $response = $client->request('GET','https://api.paymee.com.br/v1.1/available_payment_methods', array(
        'headers' => $headers,
        'json' => $request_body,
       )
    );
    print_r($response->getBody()->getContents());
 }
 catch (\GuzzleHttp\Exception\BadResponseException $e) {
    // handle exception or api errors.
    print_r($e->getMessage());
 }

 // ...

```

```java
URL obj = new URL("https://api.paymee.com.br/v1.1/available_payment_methods");
HttpURLConnection con = (HttpURLConnection) obj.openConnection();
con.setRequestMethod("GET");
int responseCode = con.getResponseCode();
BufferedReader in = new BufferedReader(
    new InputStreamReader(con.getInputStream()));
String inputLine;
StringBuffer response = new StringBuffer();
while ((inputLine = in.readLine()) != null) {
    response.append(inputLine);
}
in.close();
System.out.println(response.toString());

```

```go
package main

import (
       "bytes"
       "net/http"
)

func main() {

    headers := map[string][]string{
        "Accept": []string{"application/json"},
        "x-api-key": []string{"{{your-x-api-key}}"},
        "x-api-token": []string{"{{your-x-api-token}}"},
    }

    data := bytes.NewBuffer([]byte{jsonReq})
    req, err := http.NewRequest("GET", "https://api.paymee.com.br/v1.1/available_payment_methods", data)
    req.Header = headers

    client := &http.Client{}
    resp, err := client.Do(req)
    // ...
}

```

`GET /v1.1/available_payment_methods`

*Get available payment methods*

Returns the list of payment methods currently enabled for the merchant.

<h3 id="getavailablepaymentmethods-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|x-api-key|header|string|true|none|
|x-api-token|header|string|true|none|

> Example responses

> success response

```json
{
  "status": 0,
  "message": "success",
  "payment_methods": [
    {
      "label": "Pagamento via PIX",
      "type": "pix",
      "value": "PIX"
    },
    {
      "label": "Pagamento por transferência via Banco do Brasil",
      "type": "wire_transfer",
      "value": "BB_TRANSFER"
    },
    {
      "label": "Pagamento por transferência via Bradesco",
      "type": "wire_transfer",
      "value": "BRADESCO_TRANSFER"
    },
    {
      "label": "Pagamento por transferência via Caixa",
      "type": "wire_transfer",
      "value": "CEF_TRANSFER"
    },
    {
      "label": "Pagamento por transferência via Santander Brasil",
      "type": "wire_transfer",
      "value": "SANTANDER_TRANSFER"
    },
    {
      "label": "Pagamento por transferência via Banco BS2",
      "type": "wire_transfer",
      "value": "BS2_TRANSFER"
    },
    {
      "label": "Pagamento por depósito bancário via Banco do Brasil",
      "type": "cash_deposit",
      "value": "BB_DI"
    },
    {
      "label": "Pagamento por depósito bancário via Santander Brasil",
      "type": "cash_deposit",
      "value": "SANTANDER_DI"
    }
  ]
}
```

> Unauthorized access

```json
{
  "status": -1,
  "message": "field validation failure",
  "errorCount": 1,
  "errors": [
    {
      "error": "Forbidden",
      "message": "Access Denied"
    }
  ]
}
```

<h3 id="getavailablepaymentmethods-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|success response|Inline|
|403|[Forbidden](https://tools.ietf.org/html/rfc7231#section-6.5.3)|Unauthorized access|Inline|

<h3 id="getavailablepaymentmethods-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» status|number|false|none|Response status code (0 = success)|
|» message|string|false|none|Response message|
|» payment_methods|[object]|false|none|List of available payment methods|
|»» type|string|false|none|Payment method type|
|»» value|string|false|none|Internal payment method value|
|»» label|string|false|none|Human‑readable label|
|»» bank_code|string¦null|false|none|COMPE code (wire transfer only)|
|»» payment_method_name|string¦null|false|none|Display name|
|»» approval_deadline|integer¦null|false|none|Deadline (seconds) for manual approval|

#### Enumerated Values

|Property|Value|
|---|---|
|type|pix|
|type|wire_transfer|
|type|cash_deposit|

Status Code **403**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» status|number|false|none|Response status code (-1 = unauthorized)|
|» message|string|false|none|Status message|
|» errorCount|number|false|none|Number of errors|
|» errors|[object]|false|none|List of errors|
|»» error|string|false|none|Error identifier|
|»» message|string|false|none|Error message|

### Response Headers

|Status|Header|Type|Format|Description|
|---|---|---|---|---|
|200|Date|string||none|
|200|Transfer-Encoding|string||none|
|403|Date|string||none|
|403|Transfer-Encoding|string||none|

<aside class="success">
This operation does not require authentication
</aside>

## getPixDataFromASale

<a id="opIdgetPixDataFromASale"></a>

> Code samples

```shell
# You can also use wget
curl -X GET https://api.paymee.com.br/v1.1/transactions/pix/{transactionUUID} \
  -H 'Accept: application/json' \
  -H 'x-api-key: your-x-api-key' \
  -H 'x-api-token: your-x-api-token'

```

```http
GET https://api.paymee.com.br/v1.1/transactions/pix/{transactionUUID} HTTP/1.1
Host: api.paymee.com.br
Accept: application/json
x-api-key: your-x-api-key
x-api-token: your-x-api-token

```

```javascript

const headers = {
  'Accept':'application/json',
  'x-api-key':'your-x-api-key',
  'x-api-token':'your-x-api-token'
};

fetch('https://api.paymee.com.br/v1.1/transactions/pix/{transactionUUID}',
{
  method: 'GET',

  headers: headers
})
.then(function(res) {
    return res.json();
}).then(function(body) {
    console.log(body);
});

```

```ruby
require 'rest-client'
require 'json'

headers = {
  'Accept' => 'application/json',
  'x-api-key' => 'your-x-api-key',
  'x-api-token' => 'your-x-api-token'
}

result = RestClient.get 'https://api.paymee.com.br/v1.1/transactions/pix/{transactionUUID}',
  params: {
  }, headers: headers

p JSON.parse(result)

```

```python
import requests
headers = {
  'Accept': 'application/json',
  'x-api-key': 'your-x-api-key',
  'x-api-token': 'your-x-api-token'
}

r = requests.get('https://api.paymee.com.br/v1.1/transactions/pix/{transactionUUID}', headers = headers)

print(r.json())

```

```php
<?php

require 'vendor/autoload.php';

$headers = array(
    'Accept' => 'application/json',
    'x-api-key' => 'your-x-api-key',
    'x-api-token' => 'your-x-api-token',
);

$client = new \GuzzleHttp\Client();

// Define array of request body.
$request_body = array();

try {
    $response = $client->request('GET','https://api.paymee.com.br/v1.1/transactions/pix/{transactionUUID}', array(
        'headers' => $headers,
        'json' => $request_body,
       )
    );
    print_r($response->getBody()->getContents());
 }
 catch (\GuzzleHttp\Exception\BadResponseException $e) {
    // handle exception or api errors.
    print_r($e->getMessage());
 }

 // ...

```

```java
URL obj = new URL("https://api.paymee.com.br/v1.1/transactions/pix/{transactionUUID}");
HttpURLConnection con = (HttpURLConnection) obj.openConnection();
con.setRequestMethod("GET");
int responseCode = con.getResponseCode();
BufferedReader in = new BufferedReader(
    new InputStreamReader(con.getInputStream()));
String inputLine;
StringBuffer response = new StringBuffer();
while ((inputLine = in.readLine()) != null) {
    response.append(inputLine);
}
in.close();
System.out.println(response.toString());

```

```go
package main

import (
       "bytes"
       "net/http"
)

func main() {

    headers := map[string][]string{
        "Accept": []string{"application/json"},
        "x-api-key": []string{"your-x-api-key"},
        "x-api-token": []string{"your-x-api-token"},
    }

    data := bytes.NewBuffer([]byte{jsonReq})
    req, err := http.NewRequest("GET", "https://api.paymee.com.br/v1.1/transactions/pix/{transactionUUID}", data)
    req.Header = headers

    client := &http.Client{}
    resp, err := client.Do(req)
    // ...
}

```

`GET /v1.1/transactions/pix/{transactionUUID}`

*Get pix data from a sale*

To query a transaction, do a GET as shown below.

<h3 id="getpixdatafromasale-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|transactionUUID|path|string|true|Transaction UUID|
|x-api-key|header|string|true|none|
|x-api-token|header|string|true|none|

> Example responses

> success response

```json
{
  "status": 0,
  "message": "success",
  "qrCode": {
    "plain": "00020126890014BR.GOV.BCB.PIX2567api-pix.bancobs2.com.br/spi/v2/d4dd5d08-d6ff-4b41-913c-4d9344e9a27c52040000530398654040.015802BR5906PAYMEE6014Belo Horizonte61083038040362070503***63040857",
    "base64": "iVBORw0KGgoAAAANSUhEUgAAAPoAAAD6CAIAAAAHjs1qAAAGJklEQVR42u3cUW6cQBBF…",
    "url": "https://api.paymee.com.br/resources/payments/pix/qrcode/4ccad173-6206-3b30-a7ce-4ea455040ba1"
  }
}
```

> Unauthorized access

```json
{
  "status": -1,
  "message": "field validation failure",
  "errorCount": 1,
  "errors": [
    {
      "error": "Forbidden",
      "message": "Access Denied"
    }
  ]
}
```

> transaction not found response

```json
{
  "status": 998,
  "message": "transaction not found",
  "errorCount": 1,
  "errors": [
    {
      "field": "saleToken",
      "message": "non-existent transaction"
    }
  ]
}
```

<h3 id="getpixdatafromasale-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|success response|Inline|
|403|[Forbidden](https://tools.ietf.org/html/rfc7231#section-6.5.3)|Unauthorized access|Inline|
|404|[Not Found](https://tools.ietf.org/html/rfc7231#section-6.5.4)|transaction not found response|Inline|

<h3 id="getpixdatafromasale-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» status|number|false|none|Response status code|
|» message|string|false|none|Response status message|
|» qrCode|object|false|none|PIX QR‑code data|
|»» plain|string|false|none|PIX QR‑code plain text|
|»» base64|string|false|none|PIX QR‑code PNG (Base64)|
|»» url|string|false|none|URL to the PNG image|

Status Code **403**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» status|number|false|none|Response status code (-1 = unauthorized)|
|» message|string|false|none|Response status message|
|» errorCount|number|false|none|Number of errors|
|» errors|[object]|false|none|List of errors|
|»» error|string|false|none|Error identifier|
|»» message|string|false|none|Human‑readable error message|

Status Code **404**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» status|number|false|none|Response status code (998 = not found)|
|» message|string|false|none|Response status message|
|» errorCount|number|false|none|Number of errors|
|» errors|[object]|false|none|List of errors|
|»» field|string|false|none|Field that failed|
|»» message|string|false|none|Error description|

### Response Headers

|Status|Header|Type|Format|Description|
|---|---|---|---|---|
|200|Access-Control-Allow-Headers|string||none|
|200|Access-Control-Allow-Methods|string||none|
|200|Access-Control-Allow-Origin|string||none|
|200|Date|string||none|
|200|Transfer-Encoding|string||none|
|403|Date|string||none|
|403|Transfer-Encoding|string||none|
|404|Date|string||none|
|404|Transfer-Encoding|string||none|

<aside class="success">
This operation does not require authentication
</aside>

## queryATransaction

<a id="opIdqueryATransaction"></a>

> Code samples

```shell
# You can also use wget
curl -X GET https://api.paymee.com.br/v1.1/transactions/{transactionUUID} \
  -H 'Accept: application/json' \
  -H 'x-api-key: your-x-api-key' \
  -H 'x-api-token: your-x-api-token'

```

```http
GET https://api.paymee.com.br/v1.1/transactions/{transactionUUID} HTTP/1.1
Host: api.paymee.com.br
Accept: application/json
x-api-key: your-x-api-key
x-api-token: your-x-api-token

```

```javascript

const headers = {
  'Accept':'application/json',
  'x-api-key':'your-x-api-key',
  'x-api-token':'your-x-api-token'
};

fetch('https://api.paymee.com.br/v1.1/transactions/{transactionUUID}',
{
  method: 'GET',

  headers: headers
})
.then(function(res) {
    return res.json();
}).then(function(body) {
    console.log(body);
});

```

```ruby
require 'rest-client'
require 'json'

headers = {
  'Accept' => 'application/json',
  'x-api-key' => 'your-x-api-key',
  'x-api-token' => 'your-x-api-token'
}

result = RestClient.get 'https://api.paymee.com.br/v1.1/transactions/{transactionUUID}',
  params: {
  }, headers: headers

p JSON.parse(result)

```

```python
import requests
headers = {
  'Accept': 'application/json',
  'x-api-key': 'your-x-api-key',
  'x-api-token': 'your-x-api-token'
}

r = requests.get('https://api.paymee.com.br/v1.1/transactions/{transactionUUID}', headers = headers)

print(r.json())

```

```php
<?php

require 'vendor/autoload.php';

$headers = array(
    'Accept' => 'application/json',
    'x-api-key' => 'your-x-api-key',
    'x-api-token' => 'your-x-api-token',
);

$client = new \GuzzleHttp\Client();

// Define array of request body.
$request_body = array();

try {
    $response = $client->request('GET','https://api.paymee.com.br/v1.1/transactions/{transactionUUID}', array(
        'headers' => $headers,
        'json' => $request_body,
       )
    );
    print_r($response->getBody()->getContents());
 }
 catch (\GuzzleHttp\Exception\BadResponseException $e) {
    // handle exception or api errors.
    print_r($e->getMessage());
 }

 // ...

```

```java
URL obj = new URL("https://api.paymee.com.br/v1.1/transactions/{transactionUUID}");
HttpURLConnection con = (HttpURLConnection) obj.openConnection();
con.setRequestMethod("GET");
int responseCode = con.getResponseCode();
BufferedReader in = new BufferedReader(
    new InputStreamReader(con.getInputStream()));
String inputLine;
StringBuffer response = new StringBuffer();
while ((inputLine = in.readLine()) != null) {
    response.append(inputLine);
}
in.close();
System.out.println(response.toString());

```

```go
package main

import (
       "bytes"
       "net/http"
)

func main() {

    headers := map[string][]string{
        "Accept": []string{"application/json"},
        "x-api-key": []string{"your-x-api-key"},
        "x-api-token": []string{"your-x-api-token"},
    }

    data := bytes.NewBuffer([]byte{jsonReq})
    req, err := http.NewRequest("GET", "https://api.paymee.com.br/v1.1/transactions/{transactionUUID}", data)
    req.Header = headers

    client := &http.Client{}
    resp, err := client.Do(req)
    // ...
}

```

`GET /v1.1/transactions/{transactionUUID}`

*Query a transaction*

Returns detailed information about a sale, payout or refund transaction.

<h3 id="queryatransaction-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|transactionUUID|path|string|true|Unique transaction identifier (UUID)|
|x-api-key|header|string|true|API authentication key|
|x-api-token|header|string|true|API authentication token|

> Example responses

> Success response

```json
{
  "amount": 0.99,
  "appliedRate": 0.1,
  "creation": "2018-07-03 23:51:18",
  "currency": "BRL",
  "discounts": 0,
  "id": 2404,
  "maxAge": "2018-07-04 01:00:18",
  "message": "success",
  "referenceCode": "0199201120510",
  "refunds": {
    "available": 0.98,
    "quantity": 1,
    "requested": 0.01,
    "transactions": [
      {
        "bankDetails": {
          "account": "00000-0",
          "bank": "218 - BANCO BS2 S.A.",
          "branch": "0000"
        },
        "creation": "2019-10-15 16:13:26",
        "creditDate": "2019-10-15 16:13:59",
        "creditTransferDetails": {
          "referenceCode": "example-999",
          "saleUUID": "00000000-0000-0000-0000-000000000000",
          "total": 0.01
        },
        "remark": "Créditos Transferidos",
        "status": "PAID",
        "total": 0.01,
        "uuid": "00000000-0000-0000-0000-000000000000"
      }
    ]
  },
  "shipping": 0,
  "shopper": {
    "bankDetails": {
      "account": "00011-0",
      "branch": "0001"
    },
    "document": {
      "number": "00000000000",
      "type": "CPF"
    },
    "email": "JOHN DOE",
    "phone": {
      "number": "11999999999",
      "type": "MOBILE"
    }
  },
  "situation": "CANCELLED",
  "status": 0,
  "total": 0.99,
  "type": "SALE",
  "uuid": "00000000-0000-0000-0000-000000000000"
}
```

```json
{
  "amount": 111,
  "appliedRate": 2.21,
  "creation": "2018-12-04 17:12:29",
  "currency": "BRL",
  "discounts": 0,
  "id": 5483,
  "maxAge": "2018-12-04 19:00:29",
  "message": "success",
  "referenceCode": "019922112127661",
  "refunds": {
    "available": 100,
    "quantity": 1,
    "requested": 11,
    "transactions": [
      {
        "bankDetails": {
          "account": "00000-0",
          "bank": "033 - BANCO SANTANDER S.A.",
          "branch": "0000"
        },
        "creation": "2018-12-04 17:13:44",
        "creditDate": null,
        "status": "PENDING",
        "total": 11,
        "uuid": "53897fb2-e3fb-3549-9fe3-8998545b5d86"
      }
    ]
  },
  "shipping": 0,
  "shopper": {
    "bankDetails": {
      "account": "00011-0",
      "branch": "0001"
    },
    "document": {
      "number": "00000000000",
      "type": "CPF"
    },
    "email": "foo@bar.com",
    "name": "JOHN DOE",
    "phone": {
      "number": "11999990000",
      "type": "MOBILE"
    }
  },
  "situation": "PAID",
  "status": 0,
  "total": 111,
  "type": "SALE",
  "uuid": "44ae74b4-572d-3dc6-a9a1-95c1c6d1c7e3"
}
```

```json
{
  "amount": 0.01,
  "beneficiary": {
    "account": "1234-5",
    "bank": "001 - BANCO DO BRASIL S.A.",
    "branch": "1234"
  },
  "creation": "2020-12-16 00:59:21",
  "currency": "BRL",
  "discounts": 0,
  "has_return_error": false,
  "id": 3615951,
  "message": "success",
  "serviceFee": 1.25,
  "situation": "PENDING",
  "status": 0,
  "total": 0.01,
  "type": "PAYOUT",
  "uuid": "{transactionUUID}"
}
```

```json
{
  "amount": 0.01,
  "beneficiary": {
    "account": "1234-5",
    "bank": "001 - BANCO DO BRASIL S.A.",
    "branch": "1234"
  },
  "creation": "2020-12-16 00:59:21",
  "currency": "BRL",
  "discounts": 0,
  "has_return_error": true,
  "id": 3615951,
  "message": "success",
  "return_error": {
    "code": "PE0001",
    "description": "Os dados bancarios fornecidos não validos"
  },
  "serviceFee": 1.25,
  "situation": "PENDING",
  "status": 0,
  "total": 0.01,
  "type": "PAYOUT",
  "uuid": "{transactionUUID}"
}
```

> Unauthorized access

```json
{
  "errorCount": 1,
  "errors": [
    {
      "error": "Forbidden",
      "message": "Access Denied"
    }
  ],
  "message": "field validation failure",
  "status": -1
}
```

> Transaction not found response

```json
{
  "errorCount": 1,
  "errors": [
    {
      "field": "saleToken",
      "message": "non-existent transaction"
    }
  ],
  "message": "transaction not found",
  "status": 998
}
```

<h3 id="queryatransaction-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|Success response|Inline|
|403|[Forbidden](https://tools.ietf.org/html/rfc7231#section-6.5.3)|Unauthorized access|Inline|
|404|[Not Found](https://tools.ietf.org/html/rfc7231#section-6.5.4)|Transaction not found response|Inline|

<h3 id="queryatransaction-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» status|number|false|none|Response status code.|
|» message|string|false|none|Response status message.|
|» id|number|false|none|Internal transaction ID.|
|» uuid|string|false|none|Transaction UUID.|
|» type|string|false|none|Transaction type.|
|» situation|string|false|none|Transaction situation.|
|» currency|string|false|none|ISO‑4217 currency code.|
|» amount|number|false|none|Order amount.|
|» shipping|number|false|none|Shipping costs.|
|» discounts|number|false|none|Discounts applied.|
|» total|number|false|none|(amount + shipping) − discounts.|
|» appliedRate|number|false|none|PayMee service fee.|
|» referenceCode|string|false|none|Merchant's unique sale identification.|
|» creation|string|false|none|Creation date and time (yyyy-MM-dd HH:mm:ss).|
|» maxAge|string|false|none|Order max age (yyyy-MM-dd HH:mm:ss).|
|» shopper|object|false|none|none|
|»» id|string|false|none|Internal shopper identifier.|
|»» email|string|false|none|Shopper e‑mail.|
|»» name|string|false|none|Shopper name.|
|»» document|object|false|none|none|
|»»» type|string|false|none|Document type.|
|»»» number|string|false|none|Document number.|
|»» phone|object|false|none|none|
|»»» type|string|false|none|Phone type.|
|»»» number|string|false|none|Phone number.|
|»» bankDetails|object|false|none|none|
|»»» bank|string|false|none|none|
|»»» branch|string|false|none|none|
|»»» account|string|false|none|none|
|» beneficiary|object|false|none|none|
|»» bank|string|false|none|none|
|»» branch|string|false|none|none|
|»» account|string|false|none|none|
|» refunds|object|false|none|none|
|»» quantity|number|false|none|none|
|»» requested|number|false|none|none|
|»» available|number|false|none|none|
|»» transactions|[object]|false|none|none|
|»»» uuid|string|false|none|none|
|»»» total|number|false|none|none|
|»»» status|string|false|none|none|
|»»» creation|string|false|none|none|
|»»» creditDate|string¦null|false|none|none|
|»»» remark|string|false|none|none|
|»»» bankDetails|object|false|none|none|
|»»»» bank|string|false|none|none|
|»»»» branch|string|false|none|none|
|»»»» account|string|false|none|none|
|»»» creditTransferDetails|object|false|none|none|
|»»»» saleUUID|string|false|none|none|
|»»»» referenceCode|string|false|none|none|
|»»»» total|number|false|none|none|
|» serviceFee|number|false|none|Service fee applied to payouts.|
|» has_return_error|boolean|false|none|Indicates if a return error is present.|
|» return_error|object|false|none|none|
|»» code|string|false|none|none|
|»» description|string|false|none|none|
|» track|object|false|none|none|
|»» is_opened|boolean|false|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|status|0|
|status|-1|
|status|999|
|situation|PENDING|
|situation|CANCELLED|
|situation|PAID|

Status Code **403**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» errorCount|number|false|none|none|
|» errors|[object]|false|none|none|
|»» error|string|false|none|none|
|»» message|string|false|none|none|
|» message|string|false|none|none|
|» status|number|false|none|none|

Status Code **404**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» errorCount|number|false|none|none|
|» errors|[object]|false|none|none|
|»» field|string|false|none|none|
|»» message|string|false|none|none|
|» message|string|false|none|none|
|» status|number|false|none|none|

### Response Headers

|Status|Header|Type|Format|Description|
|---|---|---|---|---|
|200|Access-Control-Allow-Headers|string||none|
|200|Access-Control-Allow-Methods|string||none|
|200|Access-Control-Allow-Origin|string||none|
|200|Date|string||none|
|200|Transfer-Encoding|string||none|
|403|Date|string||none|
|403|Transfer-Encoding|string||none|
|404|Date|string||none|
|404|Transfer-Encoding|string||none|

<aside class="success">
This operation does not require authentication
</aside>

## queryCurrentBalance

<a id="opIdqueryCurrentBalance"></a>

> Code samples

```shell
# You can also use wget
curl -X GET https://api.paymee.com.br/v1.1/balance \
  -H 'Accept: application/json' \
  -H 'x-api-key: your-x-api-key' \
  -H 'x-api-token: your-x-api-token'

```

```http
GET https://api.paymee.com.br/v1.1/balance HTTP/1.1
Host: api.paymee.com.br
Accept: application/json
x-api-key: your-x-api-key
x-api-token: your-x-api-token

```

```javascript

const headers = {
  'Accept':'application/json',
  'x-api-key':'your-x-api-key',
  'x-api-token':'your-x-api-token'
};

fetch('https://api.paymee.com.br/v1.1/balance',
{
  method: 'GET',

  headers: headers
})
.then(function(res) {
    return res.json();
}).then(function(body) {
    console.log(body);
});

```

```ruby
require 'rest-client'
require 'json'

headers = {
  'Accept' => 'application/json',
  'x-api-key' => 'your-x-api-key',
  'x-api-token' => 'your-x-api-token'
}

result = RestClient.get 'https://api.paymee.com.br/v1.1/balance',
  params: {
  }, headers: headers

p JSON.parse(result)

```

```python
import requests
headers = {
  'Accept': 'application/json',
  'x-api-key': 'your-x-api-key',
  'x-api-token': 'your-x-api-token'
}

r = requests.get('https://api.paymee.com.br/v1.1/balance', headers = headers)

print(r.json())

```

```php
<?php

require 'vendor/autoload.php';

$headers = array(
    'Accept' => 'application/json',
    'x-api-key' => 'your-x-api-key',
    'x-api-token' => 'your-x-api-token',
);

$client = new \GuzzleHttp\Client();

// Define array of request body.
$request_body = array();

try {
    $response = $client->request('GET','https://api.paymee.com.br/v1.1/balance', array(
        'headers' => $headers,
        'json' => $request_body,
       )
    );
    print_r($response->getBody()->getContents());
 }
 catch (\GuzzleHttp\Exception\BadResponseException $e) {
    // handle exception or api errors.
    print_r($e->getMessage());
 }

 // ...

```

```java
URL obj = new URL("https://api.paymee.com.br/v1.1/balance");
HttpURLConnection con = (HttpURLConnection) obj.openConnection();
con.setRequestMethod("GET");
int responseCode = con.getResponseCode();
BufferedReader in = new BufferedReader(
    new InputStreamReader(con.getInputStream()));
String inputLine;
StringBuffer response = new StringBuffer();
while ((inputLine = in.readLine()) != null) {
    response.append(inputLine);
}
in.close();
System.out.println(response.toString());

```

```go
package main

import (
       "bytes"
       "net/http"
)

func main() {

    headers := map[string][]string{
        "Accept": []string{"application/json"},
        "x-api-key": []string{"your-x-api-key"},
        "x-api-token": []string{"your-x-api-token"},
    }

    data := bytes.NewBuffer([]byte{jsonReq})
    req, err := http.NewRequest("GET", "https://api.paymee.com.br/v1.1/balance", data)
    req.Header = headers

    client := &http.Client{}
    resp, err := client.Do(req)
    // ...
}

```

`GET /v1.1/balance`

*Query current balance*

Returns the merchant’s current balance, the timestamp when the balance was calculated (GMT‑3), and a status/message pair indicating success or failure.

<h3 id="querycurrentbalance-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|x-api-key|header|string|true|none|
|x-api-token|header|string|true|none|

> Example responses

> success response

```json
{
  "status": 0,
  "message": "success",
  "balance": 17494.02,
  "date": "2020-02-17T17:49:28Z"
}
```

> invalid credentials

```json
{
  "status": -1,
  "message": "field validation failure",
  "errorCount": 1,
  "errors": [
    {
      "error": "Forbidden",
      "message": "Access Denied"
    }
  ]
}
```

<h3 id="querycurrentbalance-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|success response|Inline|
|403|[Forbidden](https://tools.ietf.org/html/rfc7231#section-6.5.3)|invalid credentials|Inline|

<h3 id="querycurrentbalance-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» status|number|false|none|Response status code (0 = success)|
|» message|string|false|none|Response status message|
|» balance|number|false|none|Current balance|
|» date|string|false|none|Balance timestamp (ISO‑8601, GMT‑3)|

Status Code **403**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» status|number|false|none|Response status code (-1 = unauthorized)|
|» message|string|false|none|Status message|
|» errorCount|number|false|none|Number of errors|
|» errors|[object]|false|none|List of errors|
|»» error|string|false|none|Error identifier|
|»» message|string|false|none|Error message|

### Response Headers

|Status|Header|Type|Format|Description|
|---|---|---|---|---|
|200|Date|string||none|
|200|Transfer-Encoding|string||none|
|403|Date|string||none|
|403|Transfer-Encoding|string||none|

<aside class="success">
This operation does not require authentication
</aside>

## queryTransactionsUsingFilters

<a id="opIdqueryTransactionsUsingFilters"></a>

> Code samples

```shell
# You can also use wget
curl -X GET https://api.paymee.com.br/v1.1/transactions \
  -H 'Accept: application/json' \
  -H 'x-api-key: your-x-api-key' \
  -H 'x-api-token: your-x-api-token'

```

```http
GET https://api.paymee.com.br/v1.1/transactions HTTP/1.1
Host: api.paymee.com.br
Accept: application/json
x-api-key: your-x-api-key
x-api-token: your-x-api-token

```

```javascript

const headers = {
  'Accept':'application/json',
  'x-api-key':'your-x-api-key',
  'x-api-token':'your-x-api-token'
};

fetch('https://api.paymee.com.br/v1.1/transactions',
{
  method: 'GET',

  headers: headers
})
.then(function(res) {
    return res.json();
}).then(function(body) {
    console.log(body);
});

```

```ruby
require 'rest-client'
require 'json'

headers = {
  'Accept' => 'application/json',
  'x-api-key' => 'your-x-api-key',
  'x-api-token' => 'your-x-api-token'
}

result = RestClient.get 'https://api.paymee.com.br/v1.1/transactions',
  params: {
  }, headers: headers

p JSON.parse(result)

```

```python
import requests
headers = {
  'Accept': 'application/json',
  'x-api-key': 'your-x-api-key',
  'x-api-token': 'your-x-api-token'
}

r = requests.get('https://api.paymee.com.br/v1.1/transactions', headers = headers)

print(r.json())

```

```php
<?php

require 'vendor/autoload.php';

$headers = array(
    'Accept' => 'application/json',
    'x-api-key' => 'your-x-api-key',
    'x-api-token' => 'your-x-api-token',
);

$client = new \GuzzleHttp\Client();

// Define array of request body.
$request_body = array();

try {
    $response = $client->request('GET','https://api.paymee.com.br/v1.1/transactions', array(
        'headers' => $headers,
        'json' => $request_body,
       )
    );
    print_r($response->getBody()->getContents());
 }
 catch (\GuzzleHttp\Exception\BadResponseException $e) {
    // handle exception or api errors.
    print_r($e->getMessage());
 }

 // ...

```

```java
URL obj = new URL("https://api.paymee.com.br/v1.1/transactions");
HttpURLConnection con = (HttpURLConnection) obj.openConnection();
con.setRequestMethod("GET");
int responseCode = con.getResponseCode();
BufferedReader in = new BufferedReader(
    new InputStreamReader(con.getInputStream()));
String inputLine;
StringBuffer response = new StringBuffer();
while ((inputLine = in.readLine()) != null) {
    response.append(inputLine);
}
in.close();
System.out.println(response.toString());

```

```go
package main

import (
       "bytes"
       "net/http"
)

func main() {

    headers := map[string][]string{
        "Accept": []string{"application/json"},
        "x-api-key": []string{"your-x-api-key"},
        "x-api-token": []string{"your-x-api-token"},
    }

    data := bytes.NewBuffer([]byte{jsonReq})
    req, err := http.NewRequest("GET", "https://api.paymee.com.br/v1.1/transactions", data)
    req.Header = headers

    client := &http.Client{}
    resp, err := client.Do(req)
    // ...
}

```

`GET /v1.1/transactions`

*Query transactions using filters*

Use the query parameters to filter the list of transactions returned.

<h3 id="querytransactionsusingfilters-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|initialDate|query|string(date)|false|Filter initial date (yyyy‑MM‑dd)|
|finalDate|query|string(date)|false|Filter final date (yyyy‑MM‑dd)|
|currency|query|string|false|ISO‑4217 currency code|
|amount|query|number(double)|false|Transaction amount|
|referenceCode|query|string|false|Merchant internal order identification|
|situation|query|string|false|Transaction status|
|type|query|string|false|Transaction type|
|maxPageResults|query|integer|false|Maximum results per page|
|page|query|integer|false|Page number (1‑based)|
|queryMode|query|string|false|Query mode:|
|x-api-key|header|string|true|API authentication key|
|x-api-token|header|string|true|API authentication token|

#### Detailed descriptions

**queryMode**: Query mode:
* **CREATION** – only transactions created within the period
* **PROCESSED** – only transactions processed within the period
* **REGULAR** – all transactions created and/or processed within the period (default)
* **RECONTILIATION** – transactions accounted within the period

#### Enumerated Values

|Parameter|Value|
|---|---|
|situation|PENDING|
|situation|CANCELLED|
|situation|PAID|
|type|SALE|
|type|REFUND|
|type|WITHDRAW|
|queryMode|CREATION|
|queryMode|PROCESSED|
|queryMode|REGULAR|
|queryMode|RECONTILIATION|

> Example responses

> 200 Response

```json
{
  "status": 0,
  "message": "success",
  "totalElements": 2,
  "totalPages": 1,
  "resultSize": 2,
  "result": [
    {
      "id": 346,
      "uuid": "a3b27666-49cc-3549-b621-60318a5e61e7",
      "referenceCode": "refcode12345",
      "type": "SALE",
      "situation": "PAID",
      "currency": "BRL",
      "amount": 206.9,
      "shipping": 0,
      "discounts": 0,
      "total": 206.9,
      "appliedRate": 2.05,
      "creation": "2018-01-02 16:20:00",
      "maxAge": "2018-01-02 18:00:00",
      "processingDate": "2018-01-02 16:20:05",
      "shopper": {
        "email": "foo@bar.com",
        "name": "JOHN DOE",
        "document": {
          "type": "CPF",
          "number": "00000000000"
        },
        "phone": {
          "type": "MOBILE",
          "number": "11999990000"
        },
        "bankDetails": {
          "account": "00011-0",
          "branch": "0001"
        }
      },
      "items": [
        {
          "id": "1",
          "name": "PRODUCT",
          "amount": 206.9,
          "taxes": 0,
          "discounts": 0,
          "total": 206.9,
          "quantity": 1
        }
      ]
    }
  ]
}
```

> unauthorized request

```json
{
  "status": -1,
  "message": "field validation failure",
  "errorCount": 1,
  "errors": [
    {
      "error": "Forbidden",
      "message": "Access Denied"
    }
  ]
}
```

<h3 id="querytransactionsusingfilters-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|success response|Inline|
|403|[Forbidden](https://tools.ietf.org/html/rfc7231#section-6.5.3)|unauthorized request|Inline|

<h3 id="querytransactionsusingfilters-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» status|number|false|none|Response status code (0 = success, -1 = validation failure)|
|» message|string|false|none|Response status message|
|» totalElements|number|false|none|Total number of transactions matching the filters|
|» totalPages|number|false|none|Total number of pages available|
|» resultSize|number|false|none|Number of records returned in this page|
|» result|[object]|false|none|List of transactions|
|»» id|number|false|none|Internal transaction ID|
|»» uuid|string|false|none|Transaction UUID|
|»» referenceCode|string|false|none|Merchant internal order identification|
|»» type|string|false|none|Transaction type|
|»» situation|string|false|none|Transaction situation|
|»» currency|string|false|none|ISO‑4217 currency code|
|»» amount|number|false|none|Order amount|
|»» shipping|number|false|none|Shipping costs (only for SALE)|
|»» discounts|number|false|none|Order discounts|
|»» total|number|false|none|(amount + shipping) − discounts|
|»» appliedRate|number|false|none|PayMee service fee|
|»» creation|string|false|none|Creation date and time (yyyy‑MM‑dd HH:mm:ss)|
|»» maxAge|string|false|none|Order max age (yyyy‑MM‑dd HH:mm:ss) – only for SALE|
|»» processingDate|string|false|none|Date/time when transaction was processed|
|»» shopper|object|false|none|Information about the shopper|
|»»» email|string|false|none|Shopper e‑mail|
|»»» name|string|false|none|Shopper full name|
|»»» document|object|false|none|Shopper document information|
|»»» phone|object|false|none|Shopper phone information|
|»»» bankDetails|object|false|none|Shopper bank details|
|»» items|[object]|false|none|List of items in the order|
|»»» id|string|false|none|Item identifier|
|»»» name|string|false|none|Item name|
|»»» amount|number|false|none|Item price|
|»»» taxes|number|false|none|Taxes applied to the item|
|»»» discounts|number|false|none|Discounts applied to the item|
|»»» total|number|false|none|Final item price after discounts and taxes|
|»»» quantity|number|false|none|Quantity purchased|

Status Code **403**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» status|number|false|none|Response status code (-1 = unauthorized)|
|» message|string|false|none|Status message|
|» errorCount|number|false|none|Number of errors|
|» errors|[object]|false|none|List of errors|
|»» error|string|false|none|Error identifier|
|»» message|string|false|none|Error message|

### Response Headers

|Status|Header|Type|Format|Description|
|---|---|---|---|---|
|200|Date|string||none|
|200|Transfer-Encoding|string||none|
|403|Date|string||none|
|403|Transfer-Encoding|string||none|

<aside class="success">
This operation does not require authentication
</aside>

## cancelATransaction

<a id="opIdcancelATransaction"></a>

> Code samples

```shell
# You can also use wget
curl -X PUT https://api.paymee.com.br/v1.1/transactions/{saleUUID}/void \
  -H 'Accept: application/json' \
  -H 'x-api-key: your-x-api-key' \
  -H 'x-api-token: your-x-api-token'

```

```http
PUT https://api.paymee.com.br/v1.1/transactions/{saleUUID}/void HTTP/1.1
Host: api.paymee.com.br
Accept: application/json
x-api-key: your-x-api-key
x-api-token: your-x-api-token

```

```javascript

const headers = {
  'Accept':'application/json',
  'x-api-key':'your-x-api-key',
  'x-api-token':'your-x-api-token'
};

fetch('https://api.paymee.com.br/v1.1/transactions/{saleUUID}/void',
{
  method: 'PUT',

  headers: headers
})
.then(function(res) {
    return res.json();
}).then(function(body) {
    console.log(body);
});

```

```ruby
require 'rest-client'
require 'json'

headers = {
  'Accept' => 'application/json',
  'x-api-key' => 'your-x-api-key',
  'x-api-token' => 'your-x-api-token'
}

result = RestClient.put 'https://api.paymee.com.br/v1.1/transactions/{saleUUID}/void',
  params: {
  }, headers: headers

p JSON.parse(result)

```

```python
import requests
headers = {
  'Accept': 'application/json',
  'x-api-key': 'your-x-api-key',
  'x-api-token': 'your-x-api-token'
}

r = requests.put('https://api.paymee.com.br/v1.1/transactions/{saleUUID}/void', headers = headers)

print(r.json())

```

```php
<?php

require 'vendor/autoload.php';

$headers = array(
    'Accept' => 'application/json',
    'x-api-key' => 'your-x-api-key',
    'x-api-token' => 'your-x-api-token',
);

$client = new \GuzzleHttp\Client();

// Define array of request body.
$request_body = array();

try {
    $response = $client->request('PUT','https://api.paymee.com.br/v1.1/transactions/{saleUUID}/void', array(
        'headers' => $headers,
        'json' => $request_body,
       )
    );
    print_r($response->getBody()->getContents());
 }
 catch (\GuzzleHttp\Exception\BadResponseException $e) {
    // handle exception or api errors.
    print_r($e->getMessage());
 }

 // ...

```

```java
URL obj = new URL("https://api.paymee.com.br/v1.1/transactions/{saleUUID}/void");
HttpURLConnection con = (HttpURLConnection) obj.openConnection();
con.setRequestMethod("PUT");
int responseCode = con.getResponseCode();
BufferedReader in = new BufferedReader(
    new InputStreamReader(con.getInputStream()));
String inputLine;
StringBuffer response = new StringBuffer();
while ((inputLine = in.readLine()) != null) {
    response.append(inputLine);
}
in.close();
System.out.println(response.toString());

```

```go
package main

import (
       "bytes"
       "net/http"
)

func main() {

    headers := map[string][]string{
        "Accept": []string{"application/json"},
        "x-api-key": []string{"your-x-api-key"},
        "x-api-token": []string{"your-x-api-token"},
    }

    data := bytes.NewBuffer([]byte{jsonReq})
    req, err := http.NewRequest("PUT", "https://api.paymee.com.br/v1.1/transactions/{saleUUID}/void", data)
    req.Header = headers

    client := &http.Client{}
    resp, err := client.Do(req)
    // ...
}

```

`PUT /v1.1/transactions/{saleUUID}/void`

*Cancel a transaction*

Allows the merchant to cancel a **PENDING** sale. Any other situation will return an error.

<h3 id="cancelatransaction-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|saleUUID|path|string|true|Sale UUID to be cancelled|
|x-api-key|header|string|true|none|
|x-api-token|header|string|true|none|

> Example responses

> success response

```json
{
  "status": 0,
  "message": "success"
}
```

> transaction not pending response

```json
{
  "status": 999,
  "message": "transaction not PENDING",
  "errorCount": 1,
  "errors": [
    {
      "field": "transactionToken",
      "message": "invalid transaction situation"
    }
  ]
}
```

> unauthorized request

```json
{
  "status": -1,
  "message": "field validation failure",
  "errorCount": 1,
  "errors": [
    {
      "error": "Forbidden",
      "message": "Access Denied"
    }
  ]
}
```

> Transaction not found response

```json
{
  "status": 998,
  "message": "transaction not found",
  "errorCount": 1,
  "errors": [
    {
      "field": "saleToken",
      "message": "non-existent transaction"
    }
  ]
}
```

<h3 id="cancelatransaction-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|success response|Inline|
|400|[Bad Request](https://tools.ietf.org/html/rfc7231#section-6.5.1)|transaction not pending response|Inline|
|403|[Forbidden](https://tools.ietf.org/html/rfc7231#section-6.5.3)|unauthorized request|Inline|
|404|[Not Found](https://tools.ietf.org/html/rfc7231#section-6.5.4)|Transaction not found response|Inline|

<h3 id="cancelatransaction-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» status|number|false|none|Response status code (0 = success)|
|» message|string|false|none|Response status message|

Status Code **400**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» status|number|false|none|Response status code (999 = wrong situation)|
|» message|string|false|none|Status message|
|» errorCount|number|false|none|Number of errors|
|» errors|[object]|false|none|List of validation errors|
|»» field|string|false|none|Field that failed|
|»» message|string|false|none|Error description|

Status Code **403**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» status|number|false|none|Response status code (-1 = unauthorized)|
|» message|string|false|none|Status message|
|» errorCount|number|false|none|Number of errors|
|» errors|[object]|false|none|List of errors|
|»» error|string|false|none|Error identifier|
|»» message|string|false|none|Error message|

Status Code **404**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» status|number|false|none|Response status code (998 = not found)|
|» message|string|false|none|Status message|
|» errorCount|number|false|none|Number of errors|
|» errors|[object]|false|none|List of errors|
|»» field|string|false|none|Field that failed|
|»» message|string|false|none|Error description|

### Response Headers

|Status|Header|Type|Format|Description|
|---|---|---|---|---|
|200|Date|string||none|
|200|Transfer-Encoding|string||none|
|400|Connection|string||none|
|400|Date|string||none|
|400|Transfer-Encoding|string||none|
|403|Date|string||none|
|403|Transfer-Encoding|string||none|
|404|Date|string||none|
|404|Transfer-Encoding|string||none|

<aside class="success">
This operation does not require authentication
</aside>

## requestASaleRefund

<a id="opIdrequestASaleRefund"></a>

> Code samples

```shell
# You can also use wget
curl -X PUT https://api.paymee.com.br/v1.1/transactions/{saleUUID}/refund \
  -H 'Content-Type: application/json' \
  -H 'Accept: application/json' \
  -H 'x-api-key: your-x-api-key' \
  -H 'x-api-token: your-x-api-token'

```

```http
PUT https://api.paymee.com.br/v1.1/transactions/{saleUUID}/refund HTTP/1.1
Host: api.paymee.com.br
Content-Type: application/json
Accept: application/json
x-api-key: your-x-api-key
x-api-token: your-x-api-token

```

```javascript
const inputBody = '{
  "amount": 150,
  "reason": "shopper's request"
}';
const headers = {
  'Content-Type':'application/json',
  'Accept':'application/json',
  'x-api-key':'your-x-api-key',
  'x-api-token':'your-x-api-token'
};

fetch('https://api.paymee.com.br/v1.1/transactions/{saleUUID}/refund',
{
  method: 'PUT',
  body: inputBody,
  headers: headers
})
.then(function(res) {
    return res.json();
}).then(function(body) {
    console.log(body);
});

```

```ruby
require 'rest-client'
require 'json'

headers = {
  'Content-Type' => 'application/json',
  'Accept' => 'application/json',
  'x-api-key' => 'your-x-api-key',
  'x-api-token' => 'your-x-api-token'
}

result = RestClient.put 'https://api.paymee.com.br/v1.1/transactions/{saleUUID}/refund',
  params: {
  }, headers: headers

p JSON.parse(result)

```

```python
import requests
headers = {
  'Content-Type': 'application/json',
  'Accept': 'application/json',
  'x-api-key': 'your-x-api-key',
  'x-api-token': 'your-x-api-token'
}

r = requests.put('https://api.paymee.com.br/v1.1/transactions/{saleUUID}/refund', headers = headers)

print(r.json())

```

```php
<?php

require 'vendor/autoload.php';

$headers = array(
    'Content-Type' => 'application/json',
    'Accept' => 'application/json',
    'x-api-key' => 'your-x-api-key',
    'x-api-token' => 'your-x-api-token',
);

$client = new \GuzzleHttp\Client();

// Define array of request body.
$request_body = array();

try {
    $response = $client->request('PUT','https://api.paymee.com.br/v1.1/transactions/{saleUUID}/refund', array(
        'headers' => $headers,
        'json' => $request_body,
       )
    );
    print_r($response->getBody()->getContents());
 }
 catch (\GuzzleHttp\Exception\BadResponseException $e) {
    // handle exception or api errors.
    print_r($e->getMessage());
 }

 // ...

```

```java
URL obj = new URL("https://api.paymee.com.br/v1.1/transactions/{saleUUID}/refund");
HttpURLConnection con = (HttpURLConnection) obj.openConnection();
con.setRequestMethod("PUT");
int responseCode = con.getResponseCode();
BufferedReader in = new BufferedReader(
    new InputStreamReader(con.getInputStream()));
String inputLine;
StringBuffer response = new StringBuffer();
while ((inputLine = in.readLine()) != null) {
    response.append(inputLine);
}
in.close();
System.out.println(response.toString());

```

```go
package main

import (
       "bytes"
       "net/http"
)

func main() {

    headers := map[string][]string{
        "Content-Type": []string{"application/json"},
        "Accept": []string{"application/json"},
        "x-api-key": []string{"your-x-api-key"},
        "x-api-token": []string{"your-x-api-token"},
    }

    data := bytes.NewBuffer([]byte{jsonReq})
    req, err := http.NewRequest("PUT", "https://api.paymee.com.br/v1.1/transactions/{saleUUID}/refund", data)
    req.Header = headers

    client := &http.Client{}
    resp, err := client.Do(req)
    // ...
}

```

`PUT /v1.1/transactions/{saleUUID}/refund`

*Request a sale refund*

Creates a refund request for a **PAID** sale. The body must include the amount to refund and may include a reason and a callback URL.

> Body parameter

```json
{
  "amount": 150,
  "reason": "shopper's request"
}
```

<h3 id="requestasalerefund-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|saleUUID|path|string|true|UUID of the sale to be refunded|
|x-api-key|header|string|true|none|
|x-api-token|header|string|true|none|
|body|body|object|true|none|
|» amount|body|number|true|Amount to refund (≤ original sale total)|
|» reason|body|string|false|Refund reason (optional)|
|» callbackURL|body|string|false|Callback URL (optional)|

> Example responses

> refund request accepted

```json
{
  "status": 0,
  "message": "success",
  "uuid": "a3b59529-ac56-3e24-9011-320456dd9491",
  "currency": "BRL",
  "originalAmount": 11,
  "totalToRefund": 11,
  "currentBalance": 16495.84,
  "discounts": 0,
  "shopper": {
    "bankDetails": {
      "account": "12345-6",
      "branch": "1234"
    },
    "document": {
      "type": "CPF",
      "number": "00000000000"
    },
    "email": "JOHN DOE",
    "phone": {
      "type": "MOBILE",
      "number": "11999990000"
    }
  }
}
```

> validation error (e.g., amount greater than sale total or sale not PAID)

```json
{
  "status": -1,
  "message": "field validation failure",
  "errorCount": 1,
  "errors": [
    {
      "field": "amount",
      "message": "O valor solicitado deve ser igual ou menor à R$ 100,02"
    }
  ]
}
```

```json
{
  "status": 1000,
  "message": "transaction not PAID",
  "errorCount": 1,
  "errors": [
    {
      "field": "transactionToken",
      "message": "invalid transaction situation"
    }
  ]
}
```

> unauthorized request

```json
{
  "status": -1,
  "message": "field validation failure",
  "errorCount": 1,
  "errors": [
    {
      "error": "Forbidden",
      "message": "Access Denied"
    }
  ]
}
```

> Transaction not found response

```json
{
  "status": 998,
  "message": "transaction not found",
  "errorCount": 1,
  "errors": [
    {
      "field": "saleToken",
      "message": "non-existent transaction"
    }
  ]
}
```

<h3 id="requestasalerefund-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|refund request accepted|Inline|
|400|[Bad Request](https://tools.ietf.org/html/rfc7231#section-6.5.1)|validation error (e.g., amount greater than sale total or sale not PAID)|Inline|
|403|[Forbidden](https://tools.ietf.org/html/rfc7231#section-6.5.3)|unauthorized request|Inline|
|404|[Not Found](https://tools.ietf.org/html/rfc7231#section-6.5.4)|Transaction not found response|Inline|

<h3 id="requestasalerefund-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» status|number|false|none|Response status code.<br><br>* **0** – success<br>* **‑1** – validation failure<br>* **998** – transaction not found<br>* **1000** – transaction not PAID|
|» message|string|false|none|Response message|
|» uuid|string|false|none|Refund transaction UUID|
|» currency|string|false|none|Currency code|
|» originalAmount|number|false|none|Original sale total|
|» totalToRefund|number|false|none|Total amount already refunded (after this request)|
|» currentBalance|number|false|none|Merchant balance after the request|
|» discounts|number|false|none|Discounts applied to sale|
|» shopper|object|false|none|Shopper information|
|»» document|object|false|none|none|
|»»» type|string|false|none|none|
|»»» number|string|false|none|none|
|»» email|string|false|none|none|
|»» phone|object|false|none|none|
|»»» type|string|false|none|none|
|»»» number|string|false|none|none|
|»» bankDetails|object|false|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|status|0|
|status|-1|
|status|998|
|status|1000|
|type|CPF|
|type|CNPJ|
|type|MOBILE|
|type|HOME|
|type|WORK|
|type|OTHER|

Status Code **400**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» status|number|false|none|Response status code (‑1 or 1000)|
|» message|string|false|none|Status message|
|» errorCount|number|false|none|Number of errors|
|» errors|[object]|false|none|List of validation errors|
|»» field|string|false|none|Field with error|
|»» message|string|false|none|Error description|

Status Code **403**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» status|number|false|none|Response status code (-1 = unauthorized)|
|» message|string|false|none|Status message|
|» errorCount|number|false|none|Number of errors|
|» errors|[object]|false|none|List of errors|
|»» error|string|false|none|Error identifier|
|»» message|string|false|none|Error message|

Status Code **404**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» status|number|false|none|Response status code<br><br>* **998** – transaction not found|
|» message|string|false|none|none|
|» errorCount|number|false|none|none|
|» errors|[object]|false|none|none|
|»» field|string|false|none|none|
|»» message|string|false|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|status|998|

### Response Headers

|Status|Header|Type|Format|Description|
|---|---|---|---|---|
|200|Date|string||none|
|200|Transfer-Encoding|string||none|
|400|Connection|string||none|
|400|Date|string||none|
|400|Transfer-Encoding|string||none|
|403|Date|string||none|
|403|Transfer-Encoding|string||none|

<aside class="success">
This operation does not require authentication
</aside>

## requestAWebhookRedeliver

<a id="opIdrequestAWebhookRedeliver"></a>

> Code samples

```shell
# You can also use wget
curl -X GET https://api.paymee.com.br/v1.1/transactions/{transactionUUID}/webhook/redeliver \
  -H 'Accept: application/json' \
  -H 'x-api-key: your-x-api-key' \
  -H 'x-api-token: your-x-api-token'

```

```http
GET https://api.paymee.com.br/v1.1/transactions/{transactionUUID}/webhook/redeliver HTTP/1.1
Host: api.paymee.com.br
Accept: application/json
x-api-key: your-x-api-key
x-api-token: your-x-api-token

```

```javascript

const headers = {
  'Accept':'application/json',
  'x-api-key':'your-x-api-key',
  'x-api-token':'your-x-api-token'
};

fetch('https://api.paymee.com.br/v1.1/transactions/{transactionUUID}/webhook/redeliver',
{
  method: 'GET',

  headers: headers
})
.then(function(res) {
    return res.json();
}).then(function(body) {
    console.log(body);
});

```

```ruby
require 'rest-client'
require 'json'

headers = {
  'Accept' => 'application/json',
  'x-api-key' => 'your-x-api-key',
  'x-api-token' => 'your-x-api-token'
}

result = RestClient.get 'https://api.paymee.com.br/v1.1/transactions/{transactionUUID}/webhook/redeliver',
  params: {
  }, headers: headers

p JSON.parse(result)

```

```python
import requests
headers = {
  'Accept': 'application/json',
  'x-api-key': 'your-x-api-key',
  'x-api-token': 'your-x-api-token'
}

r = requests.get('https://api.paymee.com.br/v1.1/transactions/{transactionUUID}/webhook/redeliver', headers = headers)

print(r.json())

```

```php
<?php

require 'vendor/autoload.php';

$headers = array(
    'Accept' => 'application/json',
    'x-api-key' => 'your-x-api-key',
    'x-api-token' => 'your-x-api-token',
);

$client = new \GuzzleHttp\Client();

// Define array of request body.
$request_body = array();

try {
    $response = $client->request('GET','https://api.paymee.com.br/v1.1/transactions/{transactionUUID}/webhook/redeliver', array(
        'headers' => $headers,
        'json' => $request_body,
       )
    );
    print_r($response->getBody()->getContents());
 }
 catch (\GuzzleHttp\Exception\BadResponseException $e) {
    // handle exception or api errors.
    print_r($e->getMessage());
 }

 // ...

```

```java
URL obj = new URL("https://api.paymee.com.br/v1.1/transactions/{transactionUUID}/webhook/redeliver");
HttpURLConnection con = (HttpURLConnection) obj.openConnection();
con.setRequestMethod("GET");
int responseCode = con.getResponseCode();
BufferedReader in = new BufferedReader(
    new InputStreamReader(con.getInputStream()));
String inputLine;
StringBuffer response = new StringBuffer();
while ((inputLine = in.readLine()) != null) {
    response.append(inputLine);
}
in.close();
System.out.println(response.toString());

```

```go
package main

import (
       "bytes"
       "net/http"
)

func main() {

    headers := map[string][]string{
        "Accept": []string{"application/json"},
        "x-api-key": []string{"your-x-api-key"},
        "x-api-token": []string{"your-x-api-token"},
    }

    data := bytes.NewBuffer([]byte{jsonReq})
    req, err := http.NewRequest("GET", "https://api.paymee.com.br/v1.1/transactions/{transactionUUID}/webhook/redeliver", data)
    req.Header = headers

    client := &http.Client{}
    resp, err := client.Do(req)
    // ...
}

```

`GET /v1.1/transactions/{transactionUUID}/webhook/redeliver`

*Request a webhook redeliver*

Triggers a new delivery attempt for the webhooks of a **SALE** transaction whose payment status is **PAID**.

<h3 id="requestawebhookredeliver-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|transactionUUID|path|string|true|UUID of the sale|
|x-api-key|header|string|true|none|
|x-api-token|header|string|true|none|

> Example responses

> Success Response

```json
{
  "status": 0,
  "message": "success"
}
```

> Validation Error

```json
{
  "status": -1,
  "message": "field validation failure",
  "errorCount": 1,
  "errors": [
    {
      "field": "transactionUUID",
      "message": "the transaction status must be 'PAID'"
    }
  ]
}
```

> Unauthorized Request

```json
{
  "status": -1,
  "message": "field validation failure",
  "errorCount": 1,
  "errors": [
    {
      "error": "Forbidden",
      "message": "Access Denied"
    }
  ]
}
```

> Transaction Not Found

```json
{
  "status": 998,
  "message": "transaction not found",
  "errorCount": 1,
  "errors": [
    {
      "field": "transactionUUID",
      "message": "the sale informed does not exist"
    }
  ]
}
```

<h3 id="requestawebhookredeliver-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|202|[Accepted](https://tools.ietf.org/html/rfc7231#section-6.3.3)|Success Response|Inline|
|400|[Bad Request](https://tools.ietf.org/html/rfc7231#section-6.5.1)|Validation Error|Inline|
|403|[Forbidden](https://tools.ietf.org/html/rfc7231#section-6.5.3)|Unauthorized Request|Inline|
|404|[Not Found](https://tools.ietf.org/html/rfc7231#section-6.5.4)|Transaction Not Found|Inline|

<h3 id="requestawebhookredeliver-responseschema">Response Schema</h3>

Status Code **202**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» status|number|false|none|none|
|» message|string|false|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|status|0|

Status Code **400**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» status|number|false|none|none|
|» message|string|false|none|none|
|» errorCount|number|false|none|none|
|» errors|[object]|false|none|none|
|»» field|string|false|none|none|
|»» message|string|false|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|status|-1|

Status Code **403**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» status|number|false|none|none|
|» message|string|false|none|none|
|» errorCount|number|false|none|none|
|» errors|[object]|false|none|none|
|»» error|string|false|none|none|
|»» message|string|false|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|status|-1|

Status Code **404**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» status|number|false|none|none|
|» message|string|false|none|none|
|» errorCount|number|false|none|none|
|» errors|[object]|false|none|none|
|»» field|string|false|none|none|
|»» message|string|false|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|status|998|

<aside class="success">
This operation does not require authentication
</aside>

## queryAWebhookRequests

<a id="opIdqueryAWebhookRequests"></a>

> Code samples

```shell
# You can also use wget
curl -X GET https://api.paymee.com.br/v1.1/transactions/{transactionUUID}/webhook \
  -H 'Accept: application/json' \
  -H 'x-api-key: your-x-api-key' \
  -H 'x-api-token: your-x-api-token'

```

```http
GET https://api.paymee.com.br/v1.1/transactions/{transactionUUID}/webhook HTTP/1.1
Host: api.paymee.com.br
Accept: application/json
x-api-key: your-x-api-key
x-api-token: your-x-api-token

```

```javascript

const headers = {
  'Accept':'application/json',
  'x-api-key':'your-x-api-key',
  'x-api-token':'your-x-api-token'
};

fetch('https://api.paymee.com.br/v1.1/transactions/{transactionUUID}/webhook',
{
  method: 'GET',

  headers: headers
})
.then(function(res) {
    return res.json();
}).then(function(body) {
    console.log(body);
});

```

```ruby
require 'rest-client'
require 'json'

headers = {
  'Accept' => 'application/json',
  'x-api-key' => 'your-x-api-key',
  'x-api-token' => 'your-x-api-token'
}

result = RestClient.get 'https://api.paymee.com.br/v1.1/transactions/{transactionUUID}/webhook',
  params: {
  }, headers: headers

p JSON.parse(result)

```

```python
import requests
headers = {
  'Accept': 'application/json',
  'x-api-key': 'your-x-api-key',
  'x-api-token': 'your-x-api-token'
}

r = requests.get('https://api.paymee.com.br/v1.1/transactions/{transactionUUID}/webhook', headers = headers)

print(r.json())

```

```php
<?php

require 'vendor/autoload.php';

$headers = array(
    'Accept' => 'application/json',
    'x-api-key' => 'your-x-api-key',
    'x-api-token' => 'your-x-api-token',
);

$client = new \GuzzleHttp\Client();

// Define array of request body.
$request_body = array();

try {
    $response = $client->request('GET','https://api.paymee.com.br/v1.1/transactions/{transactionUUID}/webhook', array(
        'headers' => $headers,
        'json' => $request_body,
       )
    );
    print_r($response->getBody()->getContents());
 }
 catch (\GuzzleHttp\Exception\BadResponseException $e) {
    // handle exception or api errors.
    print_r($e->getMessage());
 }

 // ...

```

```java
URL obj = new URL("https://api.paymee.com.br/v1.1/transactions/{transactionUUID}/webhook");
HttpURLConnection con = (HttpURLConnection) obj.openConnection();
con.setRequestMethod("GET");
int responseCode = con.getResponseCode();
BufferedReader in = new BufferedReader(
    new InputStreamReader(con.getInputStream()));
String inputLine;
StringBuffer response = new StringBuffer();
while ((inputLine = in.readLine()) != null) {
    response.append(inputLine);
}
in.close();
System.out.println(response.toString());

```

```go
package main

import (
       "bytes"
       "net/http"
)

func main() {

    headers := map[string][]string{
        "Accept": []string{"application/json"},
        "x-api-key": []string{"your-x-api-key"},
        "x-api-token": []string{"your-x-api-token"},
    }

    data := bytes.NewBuffer([]byte{jsonReq})
    req, err := http.NewRequest("GET", "https://api.paymee.com.br/v1.1/transactions/{transactionUUID}/webhook", data)
    req.Header = headers

    client := &http.Client{}
    resp, err := client.Do(req)
    // ...
}

```

`GET /v1.1/transactions/{transactionUUID}/webhook`

*Query webhook requests*

Returns every webhook attempt associated with the specified transaction. Optional query parameters let you filter by endpoint, event type, status, or HTTP response code.

<h3 id="queryawebhookrequests-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|transactionUUID|path|string|true|Transaction UUID|
|endpoint|query|string|false|Filter by requested endpoint|
|eventType|query|string|false|Webhook event type|
|status|query|string|false|Webhook request status|
|responseCode|query|integer|false|HTTP status code returned by endpoint|
|x-api-key|header|string|true|none|
|x-api-token|header|string|true|none|

#### Enumerated Values

|Parameter|Value|
|---|---|
|eventType|PAYMENT_CONFIRMATION|
|eventType|REFUND_PAID|
|eventType|REVERSAL_CREATE|
|eventType|REVERSAL_PAID|
|status|SUCCESS|
|status|HTTP_STATUS_NOT_EQUALS_200|
|status|HTTP_REQUEST_TIMEOUT|

> Example responses

> Success Response

```json
{
  "status": 0,
  "message": "success",
  "notifications": [
    {
      "date": "2019-02-12 16:59:12",
      "endpoint": "https://foo.bar/paymeeListener",
      "eventType": "PAYMENT_CONFIRMATION",
      "status": "SUCCESS",
      "responseCode": 200,
      "request": {
        "headers": {
          "Content-type": "application/json"
        },
        "payload": "{\"saleToken\":\"{transactionUUID}\",\"referenceCode\":\"01992211212782433139\",\"currency\":\"BRL\",\"amount\":11.00,\"shopper\":{\"fullName\":\"JOHN DOE\",\"firstName\":\"JHON\",\"lastName\":\"DOE\",\"email\":\"foo@bar.com\",\"cpf\":\"00000000000\"},\"date\":\"2019-02-12 17:29:33\",\"newStatus\":\"PAID\"}"
      },
      "response": {
        "headers": {
          "Content-Length": "0"
        },
        "body": "ok"
      }
    }
  ]
}
```

> Validation Error

```json
{
  "status": -1,
  "message": "field validation failure",
  "errorCount": 1,
  "errors": [
    {
      "field": "eventType",
      "message": "invalid value"
    }
  ]
}
```

> Unauthorized Request

```json
{
  "status": -1,
  "message": "field validation failure",
  "errorCount": 1,
  "errors": [
    {
      "error": "Forbidden",
      "message": "Access Denied"
    }
  ]
}
```

> Transaction Not Found

```json
{
  "status": 998,
  "message": "transaction not found",
  "errorCount": 1,
  "errors": [
    {
      "field": "transactionUUID",
      "message": "A transação informada não existe."
    }
  ]
}
```

<h3 id="queryawebhookrequests-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|Success Response|Inline|
|400|[Bad Request](https://tools.ietf.org/html/rfc7231#section-6.5.1)|Validation Error|Inline|
|403|[Forbidden](https://tools.ietf.org/html/rfc7231#section-6.5.3)|Unauthorized Request|Inline|
|404|[Not Found](https://tools.ietf.org/html/rfc7231#section-6.5.4)|Transaction Not Found|Inline|

<h3 id="queryawebhookrequests-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» status|number|false|none|none|
|» message|string|false|none|none|
|» notifications|[object]|false|none|none|
|»» endpoint|string|false|none|none|
|»» eventType|string|false|none|none|
|»» status|string|false|none|none|
|»» responseCode|integer|false|none|none|
|»» date|string|false|none|yyyy‑MM‑dd HH:mm:ss (GMT‑3)|
|»» request|object|false|none|none|
|»»» headers|object|false|none|none|
|»»» payload|string|false|none|none|
|»» response|object|false|none|none|
|»»» headers|object|false|none|none|
|»»» body|string|false|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|status|0|

Status Code **400**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» status|number|false|none|none|
|» message|string|false|none|none|
|» errorCount|number|false|none|none|
|» errors|[object]|false|none|none|
|»» field|string|false|none|none|
|»» message|string|false|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|status|-1|

Status Code **403**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» status|number|false|none|none|
|» message|string|false|none|none|
|» errorCount|number|false|none|none|
|» errors|[object]|false|none|none|
|»» error|string|false|none|none|
|»» message|string|false|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|status|-1|

Status Code **404**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» status|number|false|none|none|
|» message|string|false|none|none|
|» errorCount|number|false|none|none|
|» errors|[object]|false|none|none|
|»» field|string|false|none|none|
|»» message|string|false|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|status|998|

<aside class="success">
This operation does not require authentication
</aside>

## requestACreditTransfer

<a id="opIdrequestACreditTransfer"></a>

> Code samples

```shell
# You can also use wget
curl -X POST https://api.paymee.com.br/v1.1/transactions/creditTransfer \
  -H 'Content-Type: application/json' \
  -H 'Accept: application/json' \
  -H 'x-api-key: your-x-api-key' \
  -H 'x-api-token: your-x-api-token'

```

```http
POST https://api.paymee.com.br/v1.1/transactions/creditTransfer HTTP/1.1
Host: api.paymee.com.br
Content-Type: application/json
Accept: application/json
x-api-key: your-x-api-key
x-api-token: your-x-api-token

```

```javascript
const inputBody = '{
  "destSaleReferenceCode": "sample-99",
  "refundUUID": "00000000-0000-0000-0000-000000000000"
}';
const headers = {
  'Content-Type':'application/json',
  'Accept':'application/json',
  'x-api-key':'your-x-api-key',
  'x-api-token':'your-x-api-token'
};

fetch('https://api.paymee.com.br/v1.1/transactions/creditTransfer',
{
  method: 'POST',
  body: inputBody,
  headers: headers
})
.then(function(res) {
    return res.json();
}).then(function(body) {
    console.log(body);
});

```

```ruby
require 'rest-client'
require 'json'

headers = {
  'Content-Type' => 'application/json',
  'Accept' => 'application/json',
  'x-api-key' => 'your-x-api-key',
  'x-api-token' => 'your-x-api-token'
}

result = RestClient.post 'https://api.paymee.com.br/v1.1/transactions/creditTransfer',
  params: {
  }, headers: headers

p JSON.parse(result)

```

```python
import requests
headers = {
  'Content-Type': 'application/json',
  'Accept': 'application/json',
  'x-api-key': 'your-x-api-key',
  'x-api-token': 'your-x-api-token'
}

r = requests.post('https://api.paymee.com.br/v1.1/transactions/creditTransfer', headers = headers)

print(r.json())

```

```php
<?php

require 'vendor/autoload.php';

$headers = array(
    'Content-Type' => 'application/json',
    'Accept' => 'application/json',
    'x-api-key' => 'your-x-api-key',
    'x-api-token' => 'your-x-api-token',
);

$client = new \GuzzleHttp\Client();

// Define array of request body.
$request_body = array();

try {
    $response = $client->request('POST','https://api.paymee.com.br/v1.1/transactions/creditTransfer', array(
        'headers' => $headers,
        'json' => $request_body,
       )
    );
    print_r($response->getBody()->getContents());
 }
 catch (\GuzzleHttp\Exception\BadResponseException $e) {
    // handle exception or api errors.
    print_r($e->getMessage());
 }

 // ...

```

```java
URL obj = new URL("https://api.paymee.com.br/v1.1/transactions/creditTransfer");
HttpURLConnection con = (HttpURLConnection) obj.openConnection();
con.setRequestMethod("POST");
int responseCode = con.getResponseCode();
BufferedReader in = new BufferedReader(
    new InputStreamReader(con.getInputStream()));
String inputLine;
StringBuffer response = new StringBuffer();
while ((inputLine = in.readLine()) != null) {
    response.append(inputLine);
}
in.close();
System.out.println(response.toString());

```

```go
package main

import (
       "bytes"
       "net/http"
)

func main() {

    headers := map[string][]string{
        "Content-Type": []string{"application/json"},
        "Accept": []string{"application/json"},
        "x-api-key": []string{"your-x-api-key"},
        "x-api-token": []string{"your-x-api-token"},
    }

    data := bytes.NewBuffer([]byte{jsonReq})
    req, err := http.NewRequest("POST", "https://api.paymee.com.br/v1.1/transactions/creditTransfer", data)
    req.Header = headers

    client := &http.Client{}
    resp, err := client.Do(req)
    // ...
}

```

`POST /v1.1/transactions/creditTransfer`

*Request a credit transfer*

## Request a credit transfer

Request a credit transfer using a **PENDING REFUND** to processing a **PENDING SALE**

## Request parameters

| PROPERTY | TYPE | SIZE | REQUIRED | DESCRIPTION |
|---|---|---|---|---|
| refundUUID | string | 36 | true | a refund uuid source |
| destSaleReferenceCode | string | 50 | true | destination sale code |

## Response

| PROPERTY | TYPE | DESCRIPTION |
|---|---|---|
| status | number | response status code |
| message | string | response status message |

> Body parameter

```json
{
  "destSaleReferenceCode": "sample-99",
  "refundUUID": "00000000-0000-0000-0000-000000000000"
}
```

<h3 id="requestacredittransfer-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|x-api-key|header|string|false|none|
|x-api-token|header|string|false|none|
|body|body|object|false|none|
|» destSaleReferenceCode|body|string|false|none|
|» refundUUID|body|string|false|none|

> Example responses

> success

```json
{
  "message": "os créditos foram transferidos com sucesso.",
  "status": 0
}
```

> validation failure

```json
{
  "errorCount": 2,
  "errors": [
    {
      "field": "refundUUID",
      "message": "O reembolso informado não está pendente"
    },
    {
      "field": "destSaleReferenceCode",
      "message": "A venda informado já está paga."
    }
  ],
  "message": "field validation failure",
  "status": -1
}
```

<h3 id="requestacredittransfer-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|success|Inline|
|400|[Bad Request](https://tools.ietf.org/html/rfc7231#section-6.5.1)|validation failure|Inline|

<h3 id="requestacredittransfer-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» message|string|false|none|none|
|» status|number|false|none|none|

Status Code **400**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» errorCount|number|false|none|none|
|» errors|[object]|false|none|none|
|»» field|string|false|none|none|
|»» message|string|false|none|none|
|» message|string|false|none|none|
|» status|number|false|none|none|

### Response Headers

|Status|Header|Type|Format|Description|
|---|---|---|---|---|
|200|Date|string||none|
|200|Transfer-Encoding|string||none|
|400|Connection|string||none|
|400|Date|string||none|
|400|Transfer-Encoding|string||none|

<aside class="success">
This operation does not require authentication
</aside>

## getPayerInformation

<a id="opIdgetPayerInformation"></a>

> Code samples

```shell
# You can also use wget
curl -X GET https://api.paymee.com.br/v1.1/transactions/{sale_uuid}/payment_info \
  -H 'Accept: application/json' \
  -H 'x-api-key: {{x-api-key}}' \
  -H 'x-api-token: {{x-api-token}}'

```

```http
GET https://api.paymee.com.br/v1.1/transactions/{sale_uuid}/payment_info HTTP/1.1
Host: api.paymee.com.br
Accept: application/json
x-api-key: {{x-api-key}}
x-api-token: {{x-api-token}}

```

```javascript

const headers = {
  'Accept':'application/json',
  'x-api-key':'{{x-api-key}}',
  'x-api-token':'{{x-api-token}}'
};

fetch('https://api.paymee.com.br/v1.1/transactions/{sale_uuid}/payment_info',
{
  method: 'GET',

  headers: headers
})
.then(function(res) {
    return res.json();
}).then(function(body) {
    console.log(body);
});

```

```ruby
require 'rest-client'
require 'json'

headers = {
  'Accept' => 'application/json',
  'x-api-key' => '{{x-api-key}}',
  'x-api-token' => '{{x-api-token}}'
}

result = RestClient.get 'https://api.paymee.com.br/v1.1/transactions/{sale_uuid}/payment_info',
  params: {
  }, headers: headers

p JSON.parse(result)

```

```python
import requests
headers = {
  'Accept': 'application/json',
  'x-api-key': '{{x-api-key}}',
  'x-api-token': '{{x-api-token}}'
}

r = requests.get('https://api.paymee.com.br/v1.1/transactions/{sale_uuid}/payment_info', headers = headers)

print(r.json())

```

```php
<?php

require 'vendor/autoload.php';

$headers = array(
    'Accept' => 'application/json',
    'x-api-key' => '{{x-api-key}}',
    'x-api-token' => '{{x-api-token}}',
);

$client = new \GuzzleHttp\Client();

// Define array of request body.
$request_body = array();

try {
    $response = $client->request('GET','https://api.paymee.com.br/v1.1/transactions/{sale_uuid}/payment_info', array(
        'headers' => $headers,
        'json' => $request_body,
       )
    );
    print_r($response->getBody()->getContents());
 }
 catch (\GuzzleHttp\Exception\BadResponseException $e) {
    // handle exception or api errors.
    print_r($e->getMessage());
 }

 // ...

```

```java
URL obj = new URL("https://api.paymee.com.br/v1.1/transactions/{sale_uuid}/payment_info");
HttpURLConnection con = (HttpURLConnection) obj.openConnection();
con.setRequestMethod("GET");
int responseCode = con.getResponseCode();
BufferedReader in = new BufferedReader(
    new InputStreamReader(con.getInputStream()));
String inputLine;
StringBuffer response = new StringBuffer();
while ((inputLine = in.readLine()) != null) {
    response.append(inputLine);
}
in.close();
System.out.println(response.toString());

```

```go
package main

import (
       "bytes"
       "net/http"
)

func main() {

    headers := map[string][]string{
        "Accept": []string{"application/json"},
        "x-api-key": []string{"{{x-api-key}}"},
        "x-api-token": []string{"{{x-api-token}}"},
    }

    data := bytes.NewBuffer([]byte{jsonReq})
    req, err := http.NewRequest("GET", "https://api.paymee.com.br/v1.1/transactions/{sale_uuid}/payment_info", data)
    req.Header = headers

    client := &http.Client{}
    resp, err := client.Do(req)
    // ...
}

```

`GET /v1.1/transactions/{sale_uuid}/payment_info`

*Get payer information*

Returns PIX payer details for a **PAID** sale identified by `sale_uuid`.

<h3 id="getpayerinformation-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|sale_uuid|path|string(uuid)|true|UUID of the PAID sale|
|x-api-key|header|string|true|none|
|x-api-token|header|string|true|none|

> Example responses

> Success Response

```json
{
  "status": 0,
  "message": "success",
  "sale_id": 17403215,
  "payment_bank": "PIX",
  "end_to_end": "E60701190202206091923DY53C3Y9TIN",
  "payer": {
    "document": "00000000000",
    "name": "JOHN DOE"
  }
}
```

> Validation Error

```json
{
  "status": -1,
  "message": "sale must be PAID",
  "errorCount": 1,
  "errors": [
    {
      "field": "sale_uuid",
      "message": "the transaction status must be 'PAID'"
    }
  ]
}
```

> Unauthorized access

```json
{
  "status": -1,
  "message": "field validation failure",
  "errorCount": 1,
  "errors": [
    {
      "error": "Forbidden",
      "message": "Access Denied"
    }
  ]
}
```

> Transaction Not Found

```json
{
  "status": 998,
  "message": "transaction not found",
  "errorCount": 1,
  "errors": [
    {
      "field": "sale_uuid",
      "message": "the sale informed does not exist"
    }
  ]
}
```

<h3 id="getpayerinformation-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|Success Response|Inline|
|400|[Bad Request](https://tools.ietf.org/html/rfc7231#section-6.5.1)|Validation Error|Inline|
|403|[Forbidden](https://tools.ietf.org/html/rfc7231#section-6.5.3)|Unauthorized access|Inline|
|404|[Not Found](https://tools.ietf.org/html/rfc7231#section-6.5.4)|Transaction Not Found|Inline|

<h3 id="getpayerinformation-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» status|number|false|none|Response status code|
|» message|string|false|none|none|
|» sale_id|number|false|none|Requested sale ID|
|» payment_bank|string|false|none|Payment method (always PIX)|
|» end_to_end|string|false|none|BC endToEndId (PIX payment identifier)|
|» payer|object|false|none|none|
|»» document|string|false|none|Payer document number (CPF/CNPJ)|
|»» name|string|false|none|Payer name|

#### Enumerated Values

|Property|Value|
|---|---|
|status|0|
|status|-1|
|status|998|

Status Code **400**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» status|number|false|none|none|
|» message|string|false|none|none|
|» errorCount|number|false|none|none|
|» errors|[object]|false|none|none|
|»» field|string|false|none|none|
|»» message|string|false|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|status|-1|

Status Code **403**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» status|number|false|none|none|
|» message|string|false|none|none|
|» errorCount|number|false|none|none|
|» errors|[object]|false|none|none|
|»» error|string|false|none|none|
|»» message|string|false|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|status|-1|

Status Code **404**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» status|number|false|none|none|
|» message|string|false|none|none|
|» errorCount|number|false|none|none|
|» errors|[object]|false|none|none|
|»» field|string|false|none|none|
|»» message|string|false|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|status|998|

<aside class="success">
This operation does not require authentication
</aside>

## renewCanceledPix

<a id="opIdrenewCanceledPix"></a>

> Code samples

```shell
# You can also use wget
curl -X PUT https://api.paymee.com.br/v1.1/checkout/transparent/{transactionUUID} \
  -H 'Content-Type: application/json' \
  -H 'Accept: application/json' \
  -H 'x-api-key: your-x-api-key' \
  -H 'x-api-token: your-x-api-token'

```

```http
PUT https://api.paymee.com.br/v1.1/checkout/transparent/{transactionUUID} HTTP/1.1
Host: api.paymee.com.br
Content-Type: application/json
Accept: application/json
x-api-key: your-x-api-key
x-api-token: your-x-api-token

```

```javascript
const inputBody = '{
  "maxAge": 120
}';
const headers = {
  'Content-Type':'application/json',
  'Accept':'application/json',
  'x-api-key':'your-x-api-key',
  'x-api-token':'your-x-api-token'
};

fetch('https://api.paymee.com.br/v1.1/checkout/transparent/{transactionUUID}',
{
  method: 'PUT',
  body: inputBody,
  headers: headers
})
.then(function(res) {
    return res.json();
}).then(function(body) {
    console.log(body);
});

```

```ruby
require 'rest-client'
require 'json'

headers = {
  'Content-Type' => 'application/json',
  'Accept' => 'application/json',
  'x-api-key' => 'your-x-api-key',
  'x-api-token' => 'your-x-api-token'
}

result = RestClient.put 'https://api.paymee.com.br/v1.1/checkout/transparent/{transactionUUID}',
  params: {
  }, headers: headers

p JSON.parse(result)

```

```python
import requests
headers = {
  'Content-Type': 'application/json',
  'Accept': 'application/json',
  'x-api-key': 'your-x-api-key',
  'x-api-token': 'your-x-api-token'
}

r = requests.put('https://api.paymee.com.br/v1.1/checkout/transparent/{transactionUUID}', headers = headers)

print(r.json())

```

```php
<?php

require 'vendor/autoload.php';

$headers = array(
    'Content-Type' => 'application/json',
    'Accept' => 'application/json',
    'x-api-key' => 'your-x-api-key',
    'x-api-token' => 'your-x-api-token',
);

$client = new \GuzzleHttp\Client();

// Define array of request body.
$request_body = array();

try {
    $response = $client->request('PUT','https://api.paymee.com.br/v1.1/checkout/transparent/{transactionUUID}', array(
        'headers' => $headers,
        'json' => $request_body,
       )
    );
    print_r($response->getBody()->getContents());
 }
 catch (\GuzzleHttp\Exception\BadResponseException $e) {
    // handle exception or api errors.
    print_r($e->getMessage());
 }

 // ...

```

```java
URL obj = new URL("https://api.paymee.com.br/v1.1/checkout/transparent/{transactionUUID}");
HttpURLConnection con = (HttpURLConnection) obj.openConnection();
con.setRequestMethod("PUT");
int responseCode = con.getResponseCode();
BufferedReader in = new BufferedReader(
    new InputStreamReader(con.getInputStream()));
String inputLine;
StringBuffer response = new StringBuffer();
while ((inputLine = in.readLine()) != null) {
    response.append(inputLine);
}
in.close();
System.out.println(response.toString());

```

```go
package main

import (
       "bytes"
       "net/http"
)

func main() {

    headers := map[string][]string{
        "Content-Type": []string{"application/json"},
        "Accept": []string{"application/json"},
        "x-api-key": []string{"your-x-api-key"},
        "x-api-token": []string{"your-x-api-token"},
    }

    data := bytes.NewBuffer([]byte{jsonReq})
    req, err := http.NewRequest("PUT", "https://api.paymee.com.br/v1.1/checkout/transparent/{transactionUUID}", data)
    req.Header = headers

    client := &http.Client{}
    resp, err := client.Do(req)
    // ...
}

```

`PUT /v1.1/checkout/transparent/{transactionUUID}`

*Renew canceled pix*

# renew canceled pix

To renew a transaction throught PayMee, it is necessary to send a request using the PUT method to the **checkout** resource and a previous transaction UUID, as shown. This example covers the minimum of fields required to be submitted for authorization.

## Request

| PROPERTY | TYPE | SIZE | REQUIRED | DESCRIPTION |
| --- | --- | --- | --- | --- |
| maxAgefalse | number | 43200 | true | Sale max age in minutes |

| PROPERTY | TYPE | DESCRIPTION |
| --- | --- | --- |
| status | number | response status code |
| message | string | response status message |
| response.email | string | registered shopper email |
| response.referenceCode | string | registered unique order identifier |
| response.amount | number | registered order amount |
| response.saleCode | number | Paymee's internal sale code |
| response.uuid | string | internal transaction uuid |
| response.shopper.id | string | the same as stated previously (if stated) |
| response.shopper.email | string | the same as stated previously |
| response.shopper.name | string | the same as stated previously (if stated) |
| response.shopper.document.type | string | the same as stated previously |
| response.shopper.document.number | string | the same as stated previously |
| response.shopper.phone.type | string | the same as stated previously |
| response.shopper.phone.number | string | the same as stated previously |
| response.shopper.bankDetails.branch | string | the same as stated previously |
| response.shopper.bankDetails.account | string | the same as stated previously |
| response.instructions.chosen | string | chosen bank method |
| response.instructions.name | string | chosen bank name |
| response.instructions.label | string | chosen bank description |
| response.instructions.beneficiary_branch | string | branch number to pay |
| response.instructions.beneficiary_op | string | operation code (only CEF) |
| response.instructions.beneficiary_account | string | account number and verifing digit to pay |
| response.instructions.need_identification | boolean | tells if has identification code when pay |
| response.instructions.identification | string | identification code to pay |
| response.instructions.steps | array\[string\] | simple steps description to display |
| response.instructions.redirect_urls.desktop | string | default bank website URL |
| response.instructions.redirect_urls.ios | string | default bank app URL |
| response.instructions.redirect_urls.android | string | default bank app URL |
| response.instructions.redirect_urls.windows_phone | string | default bank app URL |

## Response for Pix checkout

| PROPERTY | TYPE | DESCRIPTION |
| --- | --- | --- |
| status | number | response status code |
| message | string | response status message |
| response.email | string | registered shopper email |
| response.referenceCode | string | registered unique order identifier |
| response.amount | number | registered order amount |
| response.saleCode | number | Paymee's internal sale code |
| response.uuid | string | internal transaction uuid |
| response.shopper.id | string | the same as stated previously (if stated) |
| response.shopper.email | string | the same as stated previously |
| response.shopper.name | string | the same as stated previously (if stated) |
| response.shopper.document.type | string | the same as stated previously |
| response.shopper.document.number | string | the same as stated previously |
| response.shopper.phone.type | string | the same as stated previously |
| response.shopper.phone.number | string | the same as stated previously |
| response.shopper.bankDetails.branch | string | the same as stated previously |
| response.shopper.bankDetails.account | string | the same as stated previously |
| response.instructions.chosen | string | pix chosen option |
| response.instructions.label | string | pix chosen description |
| response.instructions.beneficiary.qrcode.url | string | qrcode for payment url |
| response.instructions.beneficiary.qrcode.base64 | string | qrcode in base64 encode |
| response.instructions.beneficiary.qrcode.plain | string | qrcode in plain text |
| response.instructions.beneficiary.keys.email | string | pix beneficiary key - email option |
| response.instructions.beneficiary.keys.document | string | pix beneficiary key - document option |
| response.instructions.steps.qrcode | array\[string\] | minimal steps for qrcode description to display |
| response.instructions.steps.key | array\[string\] | minimal steps for key description to display |

## Response for OpenBanking Initiator

| PROPERTY | TYPE | DESCRIPTION |
| --- | --- | --- |
| status | number | response status code |
| message | string | response status message |
| response.email | string | registered shopper email |
| response.referenceCode | string | registered unique order identifier |
| response.amount | number | registered order amount |
| response.saleCode | number | Paymee's internal sale code |
| response.uuid | string | internal transaction uuid |
| response.shopper.id | string | the same as stated previously (if stated) |
| response.shopper.email | string | the same as stated previously |
| response.shopper.name | string | the same as stated previously (if stated) |
| response.shopper.document.type | string | the same as stated previously |
| response.shopper.document.number | string | the same as stated previously |
| response.shopper.phone.type | string | the same as stated previously |
| response.shopper.phone.number | string | the same as stated previously |
| response.shopper.bankDetails.branch | string | the same as stated previously |
| response.shopper.bankDetails.account | string | the same as stated previously |
| response.instructions.chosen | string | pix chosen option |
| response.instructions.label | string | pix chosen description |
| response.instructions.payment_id | string | openbanking payment id reference |
| response.instructions.redirect_url | string | redirect URL for openbanking authentication |

## available status values

| code | message | description |
| --- | --- | --- |
| 0 | "success" | operation was successful |
| \-1 | "validation failure" | A validation error occurred when processing the request |
| 1001 | "duplicated referenceCode" | the reference code must be unique |

## available validation vailure responses

| field | error | message |
| --- | --- | --- |
| shopper.document.number | shopper.document.number.banned | banned shopper |
| shopper.document.number | shopper.document.number.not_allowed | document type not allowed |
| referenceCode | referenceCode.duplicated | duplicated reference code |
| paymentMethod | paymentMethod.invalid | inform a valid payment method |
| shopper.bankDetails | shopper.bankDetails.isNull | please inform a valid bank details |
| shopper.phone.number | shopper.phone.number.isNull | please inform a valid phone number |
| shopper.document.number | shopper.document.number.invalidDocument | the shopper document refcode12345XX is not valid |
| shopper.document.number | shopper.document.number.limit | the amount exceeded daily limit |
| amount | amount.min | must be greater than 0.01 |
| referenceCode | referenceCode.lenght | referenceCode must have length 1 up to 100 |
| maxAge | maxAge.limit | maxAge must be at least 1 and up to 4320 minutes |
| paymentMethod | paymentMethod.is_null | paymentMethod cannot be null |
| shopper | shopper.is_null | shopper cannot be null |
| shopper.id | shopper.id.max_lenght | shopper id must have up to 255 characters |
| shopper.name | shopper.name.length | shopper name must have up to 255 characters |
| shopper.name | shopper.name.is_null | shopper name cannot be null |
| shopper.email | shopper.email.length | please fill a valid email |
| shopper.document | shopper.document.is_null | shopper document cannot be null |
| shopper.document.type | shopper.document.type.invalid | please inform a valid document type |
| shopper.document.number | shopper.document.number.invalid | lenght must be between 2 and 25 |
| shopper.bankDetails.branch | shopper.bankDetails.branch.invalid | please inform a valid branch |
| shopper.bankDetails.account | shopper.bankDetails.account.invalid | please inform a valid bank account |
| shopper.phone | shopper.phone.is_null | shopper.phone cannot be null |
| shopper.phone.type | shopper.phone.type.invalid | please inform a valid phone type |
| shopper.phone.number | shopper.phone.number.invalid | please inform a valid phone number |

> Body parameter

```json
{
  "maxAge": 120
}
```

<h3 id="renewcanceledpix-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|x-api-key|header|string|false|none|
|x-api-token|header|string|false|none|
|body|body|object|false|none|
|» maxAge|body|number|false|none|
|transactionUUID|path|string|true|none|

> Example responses

> success-response

```json
{
  "message": "success",
  "response": {
    "amount": 11,
    "id": 33200,
    "instructions": {
      "chosen": "PIX",
      "keys": {
        "document": "00000000000000",
        "email": "pix-sandbox@paymee.com.br"
      },
      "label": "Transferência via PIX",
      "name": "PIX",
      "qrCode": {
        "base64": "data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAPoAAAD6CAIAAAAHjs1qAAAEvklEQVR42u3ayXXDMBBEQeWftJ2ATzIw3QDqn7VwqeFhHj8/0jN9XALhLuEu4S7hLuEu4S7hLuEu4S7hLuEu3CXcJdwl3CXcJdwl3CXcJdwl3CXchbuEu4T7Xz8X7bsj3Hhxo+ee/Z3Je4o77rjjjjvuuOOOO+6444477rg3YPruVq361uQVmxz+tnuKO+6444477rjjjjvuuOOOO+647ya47zZkV5zZ85o85sl7ijvuuOOOO+6444477rjjjjvuuHdemraL3ong9EcY7rjjjjvuuOOOO+6444477rjjfu6l2XfMk6vSfUeIO+6444477rjjjjvuuOOOO+6438Q9O1qrjnlyAFYtNCevT6cf3HHHHXfccccdd9xxxx133HF/jfuJSzSfab6nuCOIO+6Y4o67z+COu8/gjrvP4P5aky88TS5Gn7uPKOOOu3DHHXfccccdd9xxx/1q7tklY9tCM/sCVvaY214+wx133HHHHXfccccdd9xxxx33F7hPDsm+BWLb+rJtaM9dleKOO+6444477rjjjjvuuOOO+2vcJy9fdj3XtkCcPNMGyrjjjjvuuOOOO+6444477rjjjnv2lCZfGlvF9MTVbXahiTvuuOOOO+6444477rjjjjvuuO9eDu5bok2i7F/h7Ru/zocj7rjjjjvuuOOOO+6444477rjfzX1yXZgdgP6FXfZ49t0L3HHHHXfccccdd9xxxx133HHH/W7K/QvESSiTr7Udv3fHHXfccccdd9xxxx133HHHHfcy7pO/0zZsd6Bc9a0nFpG444477rjjjjvuuOOOO+644x7l3r8K7F9oTg5bdgl7/CISd9xxxx133HHHHXfccccdd9zrua8aklNWXTvOtL/sgw933HHHHXfccccdd9xxxx133HFfu6Lat0TLLuyyS9jsGF+4iMQdd9xxxx133HHHHXfccccd9/pF5InLryzuycdB9uW8wNoXd9xxxx133HHHHXfccccdd9wf455F2X9jToHy/zv4xCISd9xxxx133HHHHXfccccdd9yj3PcRzNJp++XJ63zTgwZ33HHHHXfccccdd9xxxx133HFvI9g2ANlV4OT4BRamuOOOO+6444477rjjjjvuuOP+PPd9A7Dvv/aNaPZBkz3C+QHAHXfccccdd9xxxx133HHHHXfc2+hkobSde+erXbjjjjvuuOOOO+6444477rjjjvuJnfLqUgrl5Ng0rCZxxx133HHHHXfccccdd9xxx/1u7p9obbche177/mvV8Vy4d8cdd9xxxx133HHHHXfccccd9zLu2SXj5I2ZfPVtkvJNC1/ccccdd9xxxx133HHHHXfccce9bUHWj6BtbCYfGbjjjjvuuOOOO+6444477rjjjvvd3Pt/Jzv8+x4i86+R4Y477rjjjjvuuOOOO+6444477idyz55F/xifEu6444477rjjjjvuuOOOO+64454drVX/Nbn4y16N7GDjjjvuuOOOO+6444477rjjjjvua1nsq23pmX1AZAl2DgDuuOOOO+6444477rjjjjvuuN/NXWoOd+Eu4S7hLuEu4S7hLuEu4S7hLuEu4S7cJdwl3CXcJdwl3CXcJdwl3CXcJdyFu4S7dEm/llibksuxCZcAAAAASUVORK5CYII=",
        "plain": "00020101021226720014br.gov.bcb.pix2550bx.com.br/pix/0fa55bba-cb0d-3e5a-8dd3-88af60f2a324520400005303986540647.925802BR5913PayMee Brasil\n6008BRASILIA62190515RP12345678-2019630445C8",
        "url": "https://api.paymee.com.br/payments/pix/qrcode/14fe1d3d-26f9-3790-8161-df091e8585da"
      },
      "steps": {
        "key": [
          "Acesse o APP ou o Internet Banking do seu Banco",
          "Acesse o menu PIX",
          "Informe a chave pix-sandbox@paymee.com.br ou se preferir o nosso CNPJ 00000000000000",
          "Siga os passos no seu APP e finalize a transação"
        ],
        "qrCode": [
          "Acesse o APP ou o Internet Banking do seu Banco",
          "Acesse o menu PIX",
          "Abra o leitor de QRCode no APP e escaneie o código informado",
          "Siga os passos no APP e finalize a transação"
        ]
      }
    },
    "referenceCode": "0199221121276150",
    "saleCode": "00033200",
    "shopper": {
      "document": {
        "number": "00000000000",
        "type": "CPF"
      },
      "email": "foo@bar.com",
      "id": "24391203",
      "name": "JOHN DOE",
      "phone": {
        "number": "11999990000",
        "type": "MOBILE"
      }
    },
    "uuid": "14fe1d3d-26f9-3790-8161-df091e8585da"
  },
  "status": 0
}
```

<h3 id="renewcanceledpix-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|success-response|Inline|

<h3 id="renewcanceledpix-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» message|string|false|none|none|
|» response|object|false|none|none|
|»» amount|number|false|none|none|
|»» id|number|false|none|none|
|»» instructions|object|false|none|none|
|»»» chosen|string|false|none|none|
|»»» keys|object|false|none|none|
|»»»» document|string|false|none|none|
|»»»» email|string|false|none|none|
|»»» label|string|false|none|none|
|»»» name|string|false|none|none|
|»»» qrCode|object|false|none|none|
|»»»» base64|string|false|none|none|
|»»»» plain|string|false|none|none|
|»»»» url|string|false|none|none|
|»»» steps|object|false|none|none|
|»»»» key|[string]|false|none|none|
|»»»» qrCode|[string]|false|none|none|
|»» referenceCode|string|false|none|none|
|»» saleCode|string|false|none|none|
|»» shopper|object|false|none|none|
|»»» document|object|false|none|none|
|»»»» number|string|false|none|none|
|»»»» type|string|false|none|none|
|»»» email|string|false|none|none|
|»»» id|string|false|none|none|
|»»» name|string|false|none|none|
|»»» phone|object|false|none|none|
|»»»» number|string|false|none|none|
|»»»» type|string|false|none|none|
|»» uuid|string|false|none|none|
|» status|number|false|none|none|

### Response Headers

|Status|Header|Type|Format|Description|
|---|---|---|---|---|
|200|Date|string||none|
|200|Transfer-Encoding|string||none|

<aside class="warning">
To perform this operation, you must be authenticated by means of one of the following methods:
None
</aside>

<h1 id="api-paymee-1-1-checkouts">Checkouts</h1>

**Our API collections for checkout requests**

## create

<a id="opIdcreate"></a>

> Code samples

```shell
# You can also use wget
curl -X POST https://api.paymee.com.br/v1.1/loans/create \
  -H 'Content-Type: application/json' \
  -H 'Accept: application/json' \
  -H 'x-api-key: your-api-key-here' \
  -H 'x-api-token: your-api-token-here'

```

```http
POST https://api.paymee.com.br/v1.1/loans/create HTTP/1.1
Host: api.paymee.com.br
Content-Type: application/json
Accept: application/json
x-api-key: your-api-key-here
x-api-token: your-api-token-here

```

```javascript
const inputBody = '{
  "proposal_id": "abc1234",
  "reference_code": "ref01234",
  "max_age": 60,
  "discriminator": "John's transaction",
  "final_amount": 10,
  "customer": {
    "name": "John Doe",
    "birth": "1970-01-01",
    "email": "foo@bar.com",
    "mobile_number": "11999999999",
    "document": {
      "type": "CPF",
      "number": "00000000000"
    },
    "address": {
      "zipcode": "12345678",
      "street": "Main Street",
      "number": "123",
      "complement": "Apt 4",
      "neighborhood": "Downtown",
      "city": "City",
      "state": "ST"
    }
  }
}';
const headers = {
  'Content-Type':'application/json',
  'Accept':'application/json',
  'x-api-key':'your-api-key-here',
  'x-api-token':'your-api-token-here'
};

fetch('https://api.paymee.com.br/v1.1/loans/create',
{
  method: 'POST',
  body: inputBody,
  headers: headers
})
.then(function(res) {
    return res.json();
}).then(function(body) {
    console.log(body);
});

```

```ruby
require 'rest-client'
require 'json'

headers = {
  'Content-Type' => 'application/json',
  'Accept' => 'application/json',
  'x-api-key' => 'your-api-key-here',
  'x-api-token' => 'your-api-token-here'
}

result = RestClient.post 'https://api.paymee.com.br/v1.1/loans/create',
  params: {
  }, headers: headers

p JSON.parse(result)

```

```python
import requests
headers = {
  'Content-Type': 'application/json',
  'Accept': 'application/json',
  'x-api-key': 'your-api-key-here',
  'x-api-token': 'your-api-token-here'
}

r = requests.post('https://api.paymee.com.br/v1.1/loans/create', headers = headers)

print(r.json())

```

```php
<?php

require 'vendor/autoload.php';

$headers = array(
    'Content-Type' => 'application/json',
    'Accept' => 'application/json',
    'x-api-key' => 'your-api-key-here',
    'x-api-token' => 'your-api-token-here',
);

$client = new \GuzzleHttp\Client();

// Define array of request body.
$request_body = array();

try {
    $response = $client->request('POST','https://api.paymee.com.br/v1.1/loans/create', array(
        'headers' => $headers,
        'json' => $request_body,
       )
    );
    print_r($response->getBody()->getContents());
 }
 catch (\GuzzleHttp\Exception\BadResponseException $e) {
    // handle exception or api errors.
    print_r($e->getMessage());
 }

 // ...

```

```java
URL obj = new URL("https://api.paymee.com.br/v1.1/loans/create");
HttpURLConnection con = (HttpURLConnection) obj.openConnection();
con.setRequestMethod("POST");
int responseCode = con.getResponseCode();
BufferedReader in = new BufferedReader(
    new InputStreamReader(con.getInputStream()));
String inputLine;
StringBuffer response = new StringBuffer();
while ((inputLine = in.readLine()) != null) {
    response.append(inputLine);
}
in.close();
System.out.println(response.toString());

```

```go
package main

import (
       "bytes"
       "net/http"
)

func main() {

    headers := map[string][]string{
        "Content-Type": []string{"application/json"},
        "Accept": []string{"application/json"},
        "x-api-key": []string{"your-api-key-here"},
        "x-api-token": []string{"your-api-token-here"},
    }

    data := bytes.NewBuffer([]byte{jsonReq})
    req, err := http.NewRequest("POST", "https://api.paymee.com.br/v1.1/loans/create", data)
    req.Header = headers

    client := &http.Client{}
    resp, err := client.Do(req)
    // ...
}

```

`POST /v1.1/loans/create`

*Create a loan*

Creates a loan based on a previously approved proposal

> Body parameter

```json
{
  "proposal_id": "abc1234",
  "reference_code": "ref01234",
  "max_age": 60,
  "discriminator": "John's transaction",
  "final_amount": 10,
  "customer": {
    "name": "John Doe",
    "birth": "1970-01-01",
    "email": "foo@bar.com",
    "mobile_number": "11999999999",
    "document": {
      "type": "CPF",
      "number": "00000000000"
    },
    "address": {
      "zipcode": "12345678",
      "street": "Main Street",
      "number": "123",
      "complement": "Apt 4",
      "neighborhood": "Downtown",
      "city": "City",
      "state": "ST"
    }
  }
}
```

<h3 id="create-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|x-api-key|header|string|true|API authentication key|
|x-api-token|header|string|true|API authentication token|
|body|body|object|true|none|
|» proposal_id|body|string|true|Proposal ID from a previous loan simulation|
|» reference_code|body|string|true|Merchant reference code for tracking|
|» max_age|body|integer|true|Validity period in minutes|
|» discriminator|body|string|true|Optional field for categorization|
|» final_amount|body|number|true|Final amount to be paid|
|» customer|body|object|true|none|
|»» name|body|string|true|Customer's full name|
|»» birth|body|string(date)|true|Customer's birth date in YYYY-MM-DD format|
|»» email|body|string(email)|true|Customer email address|
|»» mobile_number|body|string|true|Customer mobile phone number|
|»» document|body|object|true|none|
|»»» type|body|string|true|Document type (CPF for individuals, CNPJ for companies)|
|»»» number|body|string|true|Document number|
|»» address|body|object|true|none|
|»»» zipcode|body|string|true|Postal code|
|»»» street|body|string|true|Street name|
|»»» number|body|string|true|Building number|
|»»» complement|body|string|false|Additional address information|
|»»» neighborhood|body|string|false|Neighborhood name|
|»»» city|body|string|true|City name|
|»»» state|body|string|true|State/province code|

#### Enumerated Values

|Parameter|Value|
|---|---|
|»»» type|CPF|
|»»» type|CNPJ|

> Example responses

> Loan creation response

```json
{
  "amount": 800,
  "appliedRate": 0,
  "chosen_term": {
    "amount": 800,
    "final_amount": 845.31,
    "interest_rate": 7.17,
    "label": "03x",
    "monthly_interest_rate": 2.39,
    "quotas": 281.77,
    "times": 3
  },
  "creation": "2021-03-17 10:43:49",
  "currency": "BRL",
  "discounts": 0,
  "id": 43863,
  "maxAge": "2021-03-19 02:40:49",
  "message": "success",
  "referenceCode": "5077AXC",
  "shipping": 0,
  "shopper": {
    "bankDetails": {},
    "document": {
      "number": "00000000000",
      "type": "CPF"
    },
    "email": "foo@bar.com",
    "name": "JOHN DOE",
    "phone": {
      "number": "+5511999999999",
      "type": "MOBILE"
    }
  },
  "situation": "PENDING",
  "status": 0,
  "total": 800,
  "type": "SALE",
  "uuid": "d7113995-e683-35b6-bf4f-7846ebc63819"
}
```

```json
{
  "status": -1,
  "message": "proposal not accepted",
  "proposalAccepted": false
}
```

> Validation errors

```json
{
  "status": -1,
  "message": "validation failure",
  "errorCount": 1,
  "errors": [
    {
      "field": "terms",
      "message": "must be between 1 and 12"
    }
  ],
  "main_error_message": "must be between 1 and 12"
}
```

```json
{
  "status": -1,
  "message": "validation failure",
  "errorCount": 1,
  "errors": [
    {
      "field": "invalid proposal id",
      "message": "invalid proposal id"
    }
  ]
}
```

> Unauthorized access

```json
{
  "errorCount": 1,
  "errors": [
    {
      "error": "Forbidden",
      "message": "Access Denied"
    }
  ],
  "message": "field validation failure",
  "status": -1
}
```

<h3 id="create-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|Loan creation response|Inline|
|400|[Bad Request](https://tools.ietf.org/html/rfc7231#section-6.5.1)|Validation errors|Inline|
|403|[Forbidden](https://tools.ietf.org/html/rfc7231#section-6.5.3)|Unauthorized access|Inline|

<h3 id="create-responseschema">Response Schema</h3>

Status Code **400**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» errorCount|number|false|none|Number of validation errors|
|» errors|[object]|false|none|List of validation errors|
|»» field|string|false|none|Field with error|
|»» message|string|false|none|Error message|
|» main_error_message|string|false|none|Primary error message|
|» message|string|false|none|General error message|
|» status|number|false|none|Error status code|

Status Code **403**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» errorCount|number|false|none|none|
|» errors|[object]|false|none|none|
|»» error|string|false|none|none|
|»» message|string|false|none|none|
|» message|string|false|none|none|
|» status|number|false|none|none|

<aside class="success">
This operation does not require authentication
</aside>

## simulation

<a id="opIdsimulation"></a>

> Code samples

```shell
# You can also use wget
curl -X POST https://api.paymee.com.br/v1.1/loans/simulation \
  -H 'Content-Type: application/json' \
  -H 'Accept: application/json' \
  -H 'x-api-key: your-api-key-here' \
  -H 'x-api-token: your-api-token-here'

```

```http
POST https://api.paymee.com.br/v1.1/loans/simulation HTTP/1.1
Host: api.paymee.com.br
Content-Type: application/json
Accept: application/json
x-api-key: your-api-key-here
x-api-token: your-api-token-here

```

```javascript
const inputBody = '{
  "amount": 10000,
  "customer": {
    "address": {
      "city": "City",
      "complement": "Apt 4",
      "number": "123",
      "state": "ST",
      "street": "Main Street",
      "zipcode": "12345678"
    },
    "birth": "1980-01-01",
    "document": {
      "number": "00000000000",
      "type": "CPF"
    },
    "email": "foo@bar.com",
    "income": 5000,
    "name": "John Doe",
    "phone": "11999999999"
  },
  "products": [
    {
      "amount": 2,
      "id": "prod1",
      "name": "Product 1",
      "value": 5000
    }
  ],
  "times": 12
}';
const headers = {
  'Content-Type':'application/json',
  'Accept':'application/json',
  'x-api-key':'your-api-key-here',
  'x-api-token':'your-api-token-here'
};

fetch('https://api.paymee.com.br/v1.1/loans/simulation',
{
  method: 'POST',
  body: inputBody,
  headers: headers
})
.then(function(res) {
    return res.json();
}).then(function(body) {
    console.log(body);
});

```

```ruby
require 'rest-client'
require 'json'

headers = {
  'Content-Type' => 'application/json',
  'Accept' => 'application/json',
  'x-api-key' => 'your-api-key-here',
  'x-api-token' => 'your-api-token-here'
}

result = RestClient.post 'https://api.paymee.com.br/v1.1/loans/simulation',
  params: {
  }, headers: headers

p JSON.parse(result)

```

```python
import requests
headers = {
  'Content-Type': 'application/json',
  'Accept': 'application/json',
  'x-api-key': 'your-api-key-here',
  'x-api-token': 'your-api-token-here'
}

r = requests.post('https://api.paymee.com.br/v1.1/loans/simulation', headers = headers)

print(r.json())

```

```php
<?php

require 'vendor/autoload.php';

$headers = array(
    'Content-Type' => 'application/json',
    'Accept' => 'application/json',
    'x-api-key' => 'your-api-key-here',
    'x-api-token' => 'your-api-token-here',
);

$client = new \GuzzleHttp\Client();

// Define array of request body.
$request_body = array();

try {
    $response = $client->request('POST','https://api.paymee.com.br/v1.1/loans/simulation', array(
        'headers' => $headers,
        'json' => $request_body,
       )
    );
    print_r($response->getBody()->getContents());
 }
 catch (\GuzzleHttp\Exception\BadResponseException $e) {
    // handle exception or api errors.
    print_r($e->getMessage());
 }

 // ...

```

```java
URL obj = new URL("https://api.paymee.com.br/v1.1/loans/simulation");
HttpURLConnection con = (HttpURLConnection) obj.openConnection();
con.setRequestMethod("POST");
int responseCode = con.getResponseCode();
BufferedReader in = new BufferedReader(
    new InputStreamReader(con.getInputStream()));
String inputLine;
StringBuffer response = new StringBuffer();
while ((inputLine = in.readLine()) != null) {
    response.append(inputLine);
}
in.close();
System.out.println(response.toString());

```

```go
package main

import (
       "bytes"
       "net/http"
)

func main() {

    headers := map[string][]string{
        "Content-Type": []string{"application/json"},
        "Accept": []string{"application/json"},
        "x-api-key": []string{"your-api-key-here"},
        "x-api-token": []string{"your-api-token-here"},
    }

    data := bytes.NewBuffer([]byte{jsonReq})
    req, err := http.NewRequest("POST", "https://api.paymee.com.br/v1.1/loans/simulation", data)
    req.Header = headers

    client := &http.Client{}
    resp, err := client.Do(req)
    // ...
}

```

`POST /v1.1/loans/simulation`

*Simulation*

Create a loan simulation based on customer information and requested amount

> Body parameter

```json
{
  "amount": 10000,
  "customer": {
    "address": {
      "city": "City",
      "complement": "Apt 4",
      "number": "123",
      "state": "ST",
      "street": "Main Street",
      "zipcode": "12345678"
    },
    "birth": "1980-01-01",
    "document": {
      "number": "00000000000",
      "type": "CPF"
    },
    "email": "foo@bar.com",
    "income": 5000,
    "name": "John Doe",
    "phone": "11999999999"
  },
  "products": [
    {
      "amount": 2,
      "id": "prod1",
      "name": "Product 1",
      "value": 5000
    }
  ],
  "times": 12
}
```

<h3 id="simulation-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|x-api-key|header|string|true|API authentication key|
|x-api-token|header|string|true|API authentication token|
|body|body|object|true|none|
|» amount|body|number|true|Total loan amount requested|
|» customer|body|object|true|none|
|»» address|body|object|false|none|
|»»» city|body|string|false|none|
|»»» complement|body|string|false|none|
|»»» number|body|string|false|none|
|»»» state|body|string|false|none|
|»»» street|body|string|false|none|
|»»» zipcode|body|string|false|none|
|»» birth|body|string(date)|true|Customer's birth date in YYYY-MM-DD format|
|»» document|body|object|true|none|
|»»» number|body|string|true|Document number (CPF for individuals)|
|»»» type|body|string|true|Document type (CPF for individuals, CNPJ for companies)|
|»» email|body|string(email)|true|Customer's email address|
|»» income|body|number|true|Customer's monthly income|
|»» name|body|string|true|Customer's full name|
|»» phone|body|string|true|Customer's phone number|
|» products|body|[object]|false|List of products being financed (optional)|
|»» amount|body|number|true|Quantity of this product|
|»» id|body|string|true|Product identifier|
|»» name|body|string|true|Product name|
|»» value|body|number|true|Price per unit|
|» times|body|number|true|Number of installments requested|

#### Enumerated Values

|Parameter|Value|
|---|---|
|»»» type|CPF|
|»»» type|CNPJ|

> Example responses

> Successful Response

```json
{
  "hasProposal": true,
  "proposals": [
    {
      "amount": 10000,
      "max_term_months": 12,
      "monthly_interest_rate": 0.02,
      "partner": "Bank A",
      "product": "Personal Loan",
      "proposal_id": "abc123",
      "terms": [
        {
          "amount": 10000,
          "down_payment": false,
          "down_payment_value": 0,
          "final_amount": 10500,
          "first_due_date": "2024-06-01",
          "grace_period": 0,
          "interest_rate": 0.24,
          "label": "Standard Term",
          "monthly_interest_rate": 0.02,
          "proposal_id": "abc123",
          "quotas": 12,
          "times": 12,
          "total_iof": 50
        }
      ]
    }
  ]
}
```

```json
{
  "status": -1,
  "message": "no proposals were found",
  "hasProposal": false
}
```

> Error Response

```json
{
  "errorCount": 1,
  "errors": [
    {
      "field": "customer.name",
      "message": "must not be empty"
    }
  ],
  "main_error_message": "must not be empty",
  "message": "field validation failure",
  "status": -1
}
```

> Unauthorized access

```json
{
  "errorCount": 1,
  "errors": [
    {
      "error": "Forbidden",
      "message": "Access Denied"
    }
  ],
  "message": "field validation failure",
  "status": -1
}
```

<h3 id="simulation-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|Successful Response|Inline|
|400|[Bad Request](https://tools.ietf.org/html/rfc7231#section-6.5.1)|Error Response|Inline|
|403|[Forbidden](https://tools.ietf.org/html/rfc7231#section-6.5.3)|Unauthorized access|Inline|

<h3 id="simulation-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» hasProposal|boolean|false|none|Indicates if any financing proposals are available|
|» proposals|[object]|false|none|List of available financing proposals|
|»» amount|number|false|none|Total loan amount|
|»» max_term_months|number|false|none|Maximum term in months|
|»» monthly_interest_rate|number|false|none|Monthly interest rate (decimal)|
|»» partner|string|false|none|Financing partner/bank name|
|»» product|string|false|none|Financial product name|
|»» proposal_id|string|false|none|Unique proposal identifier|
|»» terms|[object]|false|none|Available term options|
|»»» amount|number|false|none|Principal amount|
|»»» down_payment|boolean|false|none|Whether down payment is required|
|»»» down_payment_value|number|false|none|Down payment amount if required|
|»»» final_amount|number|false|none|Total amount to be paid|
|»»» first_due_date|string(date)|false|none|First installment due date|
|»»» grace_period|number|false|none|Grace period in months|
|»»» interest_rate|number|false|none|Annual interest rate (decimal)|
|»»» label|string|false|none|Term plan label/name|
|»»» monthly_interest_rate|number|false|none|Monthly interest rate (decimal)|
|»»» proposal_id|string|false|none|Reference to parent proposal|
|»»» quotas|number|false|none|Number of installments|
|»»» times|number|false|none|Number of installments (alias)|
|»»» total_iof|number|false|none|Total IOF tax|

Status Code **400**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» errorCount|number|false|none|Number of validation errors|
|» errors|[object]|false|none|List of validation errors|
|»» field|string|false|none|Field that failed validation|
|»» message|string|false|none|Error message|
|» main_error_message|string|false|none|Primary error message|
|» message|string|false|none|General error message|
|» status|string|false|none|Error status code|

Status Code **403**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» errorCount|number|false|none|none|
|» errors|[object]|false|none|none|
|»» error|string|false|none|none|
|»» message|string|false|none|none|
|» message|string|false|none|none|
|» status|number|false|none|none|

<aside class="success">
This operation does not require authentication
</aside>

## semiTransparentWireTransferPixAndCreditCardAndOpenbankingCheckout

<a id="opIdsemiTransparentWireTransferPixAndCreditCardAndOpenbankingCheckout"></a>

> Code samples

```shell
# You can also use wget
curl -X POST https://api.paymee.com.br/v1.1/checkout/transparent \
  -H 'Content-Type: application/json' \
  -H 'Accept: application/json' \
  -H 'x-api-key: your-x-api-key' \
  -H 'x-api-token: your-x-api-token'

```

```http
POST https://api.paymee.com.br/v1.1/checkout/transparent HTTP/1.1
Host: api.paymee.com.br
Content-Type: application/json
Accept: application/json
x-api-key: your-x-api-key
x-api-token: your-x-api-token

```

```javascript
const inputBody = '{
  "currency": "BRL",
  "amount": 11,
  "referenceCode": "refcode12345",
  "maxAge": 120,
  "paymentMethod": "PIX",
  "callbackURL": "https://foo.bar/paymeeListener",
  "shopper": {
    "id": "24391203",
    "email": "foo@bar.com",
    "name": "JOHN DOE",
    "document": {
      "type": "CPF",
      "number": "00000000000"
    },
    "phone": {
      "type": "MOBILE",
      "number": "11999990000"
    },
    "bankDetails": {
      "branch": "0001",
      "account": "00011-0"
    }
  }
}';
const headers = {
  'Content-Type':'application/json',
  'Accept':'application/json',
  'x-api-key':'your-x-api-key',
  'x-api-token':'your-x-api-token'
};

fetch('https://api.paymee.com.br/v1.1/checkout/transparent',
{
  method: 'POST',
  body: inputBody,
  headers: headers
})
.then(function(res) {
    return res.json();
}).then(function(body) {
    console.log(body);
});

```

```ruby
require 'rest-client'
require 'json'

headers = {
  'Content-Type' => 'application/json',
  'Accept' => 'application/json',
  'x-api-key' => 'your-x-api-key',
  'x-api-token' => 'your-x-api-token'
}

result = RestClient.post 'https://api.paymee.com.br/v1.1/checkout/transparent',
  params: {
  }, headers: headers

p JSON.parse(result)

```

```python
import requests
headers = {
  'Content-Type': 'application/json',
  'Accept': 'application/json',
  'x-api-key': 'your-x-api-key',
  'x-api-token': 'your-x-api-token'
}

r = requests.post('https://api.paymee.com.br/v1.1/checkout/transparent', headers = headers)

print(r.json())

```

```php
<?php

require 'vendor/autoload.php';

$headers = array(
    'Content-Type' => 'application/json',
    'Accept' => 'application/json',
    'x-api-key' => 'your-x-api-key',
    'x-api-token' => 'your-x-api-token',
);

$client = new \GuzzleHttp\Client();

// Define array of request body.
$request_body = array();

try {
    $response = $client->request('POST','https://api.paymee.com.br/v1.1/checkout/transparent', array(
        'headers' => $headers,
        'json' => $request_body,
       )
    );
    print_r($response->getBody()->getContents());
 }
 catch (\GuzzleHttp\Exception\BadResponseException $e) {
    // handle exception or api errors.
    print_r($e->getMessage());
 }

 // ...

```

```java
URL obj = new URL("https://api.paymee.com.br/v1.1/checkout/transparent");
HttpURLConnection con = (HttpURLConnection) obj.openConnection();
con.setRequestMethod("POST");
int responseCode = con.getResponseCode();
BufferedReader in = new BufferedReader(
    new InputStreamReader(con.getInputStream()));
String inputLine;
StringBuffer response = new StringBuffer();
while ((inputLine = in.readLine()) != null) {
    response.append(inputLine);
}
in.close();
System.out.println(response.toString());

```

```go
package main

import (
       "bytes"
       "net/http"
)

func main() {

    headers := map[string][]string{
        "Content-Type": []string{"application/json"},
        "Accept": []string{"application/json"},
        "x-api-key": []string{"your-x-api-key"},
        "x-api-token": []string{"your-x-api-token"},
    }

    data := bytes.NewBuffer([]byte{jsonReq})
    req, err := http.NewRequest("POST", "https://api.paymee.com.br/v1.1/checkout/transparent", data)
    req.Header = headers

    client := &http.Client{}
    resp, err := client.Do(req)
    // ...
}

```

`POST /v1.1/checkout/transparent`

*Semi-transparent wire-transfer, pix and credit card and openbanking checkout*

To generate a transaction throught PayMee, it is necessary to send a request using the POST method to the **checkout** resource, as shown. This example covers the minimum of fields required to be submitted for authorization.

> Body parameter

```json
{
  "currency": "BRL",
  "amount": 11,
  "referenceCode": "refcode12345",
  "maxAge": 120,
  "paymentMethod": "PIX",
  "callbackURL": "https://foo.bar/paymeeListener",
  "shopper": {
    "id": "24391203",
    "email": "foo@bar.com",
    "name": "JOHN DOE",
    "document": {
      "type": "CPF",
      "number": "00000000000"
    },
    "phone": {
      "type": "MOBILE",
      "number": "11999990000"
    },
    "bankDetails": {
      "branch": "0001",
      "account": "00011-0"
    }
  }
}
```

<h3 id="semitransparentwiretransferpixandcreditcardandopenbankingcheckout-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|x-api-key|header|string|true|none|
|x-api-token|header|string|true|none|
|body|body|object|true|none|
|» currency|body|string|true|ISO-4217 currency code|
|» amount|body|number(float)|true|Order Amount|
|» referenceCode|body|string|true|Unique order identifier|
|» maxAge|body|number|false|Sale max age in minutes|
|» paymentMethod|body|string|true|Chosen payment method|
|» callbackURL|body|string(uri)|false|Callback URL for transaction updates|
|» redirectURL|body|string(uri)|false|Confirmation redirect URL|
|» observation|body|string|false|Any internal reference|
|» recurrence|body|number|false|Recurrence in months (required when paymentMethod is INITIATOR)|
|» brand_id|body|string(uuid)|false|Brand ID of the bank (required when paymentMethod is INITIATOR)|
|» shopper|body|object|true|none|
|»» id|body|string|false|Internal shopper identifier|
|»» name|body|string|true|Shopper's name|
|»» email|body|string(email)|true|Shopper's email|
|»» document|body|object|true|none|
|»»» type|body|string|true|Shopper's document type|
|»»» number|body|string|true|Shopper's document number|
|»» phone|body|object|true|none|
|»»» type|body|string|true|Shopper's phone type|
|»»» number|body|string|true|Shopper's phone number|
|»» bankDetails|body|object|true|none|
|»»» branch|body|string|true|Shopper's branch number|
|»»» account|body|string|true|Shopper's account number|

#### Enumerated Values

|Parameter|Value|
|---|---|
|» paymentMethod|BB_TRANSFER|
|» paymentMethod|BB_DI|
|» paymentMethod|BRADESCO_TRANSFER|
|» paymentMethod|ITAU_TRANSFER_GENERIC|
|» paymentMethod|ITAU_TRANSFER_PF|
|» paymentMethod|ITAU_TRANSFER_PJ|
|» paymentMethod|ITAU_DI|
|» paymentMethod|CEF_TRANSFER|
|» paymentMethod|ORIGINAL_TRANSFER|
|» paymentMethod|SANTANDER_TRANSFER|
|» paymentMethod|SANTANDER_DI|
|» paymentMethod|INTER_TRANSFER|
|» paymentMethod|BS2_TRANSFER|
|» paymentMethod|OUTROS_BANCOS|
|» paymentMethod|PIX|
|» paymentMethod|CREDIT_CARD|
|» paymentMethod|INITIATOR|
|»»» type|CPF|
|»»» type|CNPJ|
|»»» type|OTHER|
|»»» type|MOBILE|
|»»» type|HOME|
|»»» type|WORK|
|»»» type|OTHER|

> Example responses

> success response / semi-transparent checkout (outros bancos example) / pix-success-response / pix-success-response split

```json
{
  "status": 0,
  "message": "success",
  "response": {
    "amount": 11,
    "id": 33200,
    "uuid": "14fe1d3d-26f9-3790-8161-df091e8585da",
    "referenceCode": "0199221121276150",
    "saleCode": "00033200",
    "shopper": {
      "document": {
        "number": "00000000000",
        "type": "CPF"
      },
      "email": "foo@bar.com",
      "id": "24391203",
      "name": "JOHN DOE",
      "phone": {
        "number": "11999990000",
        "type": "MOBILE"
      }
    },
    "instructions": {
      "chosen": "PIX",
      "label": "Transferência via PIX",
      "name": "PIX",
      "qrCode": {
        "base64": "data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAPoAAAD6CAIAAAAHjs1qAAAEvklEQVR42u3ayXXDMBBEQeWftJ2ATzIw3QDqn7VwqeFhHj8/0jN9XALhLuEu4S7hLuEu4S7hLuEu4S7hLuEu3CXcJdwl3CXcJdwl3CXcJdwl3CXchbuEu4T7Xz8X7bsj3Hhxo+ee/Z3Je4o77rjjjjvuuOOOO+6444477rg3YPruVq361uQVmxz+tnuKO+6444477rjjjjvuuOOOO+647ya47zZkV5zZ85o85sl7ijvuuOOOO+6444477rjjjjvuuHdemraL3ong9EcY7rjjjjvuuOOOO+6444477rjjfu6l2XfMk6vSfUeIO+6444477rjjjjvuuOOOO+6438Q9O1qrjnlyAFYtNCevT6cf3HHHHXfccccdd9xxxx133HF/jfuJSzSfab6nuCOIO+6Y4o67z+COu8/gjrvP4P5aky88TS5Gn7uPKOOOu3DHHXfccccdd9xxx/1q7tklY9tCM/sCVvaY214+wx133HHHHXfccccdd9xxxx33F7hPDsm+BWLb+rJtaM9dleKOO+6444477rjjjjvuuOOO+2vcJy9fdj3XtkCcPNMGyrjjjjvuuOOOO+6444477rjjjnv2lCZfGlvF9MTVbXahiTvuuOOOO+6444477rjjjjvuuO9eDu5bok2i7F/h7Ru/zocj7rjjjjvuuOOOO+6444477rjfzX1yXZgdgP6FXfZ49t0L3HHHHXfccccdd9xxxx133HHH/W7K/QvESSiTr7Udv3fHHXfccccdd9xxxx133HHHHfcy7pO/0zZsd6Bc9a0nFpG444477rjjjjvuuOOOO+644x7l3r8K7F9oTg5bdgl7/CISd9xxxx133HHHHXfccccdd9zrua8aklNWXTvOtL/sgw933HHHHXfccccdd9xxxx133HFfu6Lat0TLLuyyS9jsGF+4iMQdd9xxxx133HHHHXfccccd9/pF5InLryzuycdB9uW8wNoXd9xxxx133HHHHXfccccdd9wf455F2X9jToHy/zv4xCISd9xxxx133HHHHXfccccdd9yj3PcRzNJp++XJ63zTgwZ33HHHHXfccccdd9xxxx133HFvI9g2ANlV4OT4BRamuOOOO+6444477rjjjjvuuOP+PPd9A7Dvv/aNaPZBkz3C+QHAHXfccccdd9xxxx133HHHHXfc2+hkobSde+erXbjjjjvuuOOOO+6444477rjjjvuJnfLqUgrl5Ng0rCZxxx133HHHHXfccccdd9xxx/1u7p9obbche177/mvV8Vy4d8cdd9xxxx133HHHHXfccccd9zLu2SXj5I2ZfPVtkvJNC1/ccccdd9xxxx133HHHHXfccce9bUHWj6BtbCYfGbjjjjvuuOOOO+6444477rjjjvvd3Pt/Jzv8+x4i86+R4Y477rjjjjvuuOOOO+6444477idyz55F/xifEu6444477rjjjjvuuOOOO+64454drVX/Nbn4y16N7GDjjjvuuOOOO+6444477rjjjjvua1nsq23pmX1AZAl2DgDuuOOOO+6444477rjjjjvuuN/NXWoOd+Eu4S7hLuEu4S7hLuEu4S7hLuEu4S7cJdwl3CXcJdwl3CXcJdwl3CXcJdyFu4S7dEm/llibksuxCZcAAAAASUVORK5CYII=",
        "plain": "00020101021226720014br.gov.bcb.pix2550bx.com.br/pix/0fa55bba-cb0d-3e5a-8dd3-88af60f2a324520400005303986540647.925802BR5913PayMee Brasil\n6008BRASILIA62190515RP12345678-2019630445C8",
        "url": "https://api.paymee.com.br/payments/pix/qrcode/14fe1d3d-26f9-3790-8161-df091e8585da"
      },
      "steps": {
        "key": [
          "Acesse o APP ou o Internet Banking do seu Banco",
          "Acesse o menu PIX",
          "Informe a chave pix-sandbox@paymee.com.br ou se preferir o nosso CNPJ 00000000000000",
          "Siga os passos no seu APP e finalize a transação"
        ],
        "qrCode": [
          "Acesse o APP ou o Internet Banking do seu Banco",
          "Acesse o menu PIX",
          "Abra o leitor de QRCode no APP e escaneie o código informado",
          "Siga os passos no APP e finalize a transação"
        ]
      }
    }
  }
}
```

```json
{
  "status": 0,
  "message": "success",
  "response": {
    "amount": 11,
    "id": 27736,
    "uuid": "56087f1f-9944-3d52-ac3d-50f23ad74ec2",
    "referenceCode": "refcode12345",
    "saleCode": "00027736",
    "shopper": {
      "bankDetails": {
        "account": "00011-0",
        "branch": "0001"
      },
      "document": {
        "number": "00000000000",
        "type": "CPF"
      },
      "email": "foo@bar.com",
      "id": "24391203",
      "name": "JOHN DOE",
      "phone": {
        "number": "11999990000",
        "type": "MOBILE"
      }
    },
    "instructions": {
      "beneficiary_account": "13000000-1",
      "beneficiary_branch": "0001",
      "beneficiary_document": "28.683.892/0001-91",
      "beneficiary_name": "PayMee Brasil Serviços de Pagamentos S.A",
      "beneficiary_op": "",
      "chosen": "OUTROS_BANCOS",
      "identification": "50027",
      "label": "TED para Santander Brasil S.A",
      "name": "TED para Santander",
      "need_identification": false,
      "redirect_urls": {
        "android": "https://portal.febraban.org.br/pagina/3164/12/pt-br/associados",
        "desktop": "https://www.febraban.org.br/associados/utilitarios/bancos.asp",
        "ios": "https://www.febraban.org.br/associados/utilitarios/bancos.asp",
        "windows_phone": null
      },
      "steps": [
        "Acesse o site ou APP do seu banco",
        "No menu de serviços, selecione 'Transferências > TED'",
        "Informe a agência: 0001 e conta-corrente: 13000000-1",
        "Informe o nosso documento: 28.683.892/0001-91",
        "Informe o nosso nome (caso necessário): PayMee Brasil",
        "Efetue o pagamento no valor exato de R$ 11,00.",
        "PRONTO! O pagamento do seu pedido será confirmado ao comércio XXXXX"
      ]
    }
  }
}
```

```json
{
  "status": 0,
  "message": "success",
  "response": {
    "amount": 11,
    "id": 2423,
    "uuid": "81d588cb-4fec-3088-960d-6881b8749247",
    "referenceCode": "refcode12345",
    "shopper": {
      "bankDetails": {
        "account": "00011-0",
        "branch": "0001"
      },
      "document": {
        "number": "00000000000",
        "type": "CPF"
      },
      "email": "foo@bar.com",
      "id": "24391203",
      "name": "JOHN DOE",
      "phone": {
        "number": "11999990000",
        "type": "MOBILE"
      }
    },
    "instructions": {
      "beneficiary_account": "13002973-8",
      "beneficiary_branch": "0386",
      "beneficiary_name": "PayMee Brasil Serviços de Pagamentos S.A",
      "chosen": "SANTANDER_TRANSFER",
      "identification": "2423",
      "label": "transferência entre contas Santander Brasil",
      "name": "Santander Brasil",
      "need_identification": false,
      "redirect_urls": {
        "android": "https://play.google.com/store/apps/details?id=com.santander.app&hl=pt_BR",
        "desktop": "https://www.santander.com.br/",
        "ios": "https://itunes.apple.com/br/app/santander-brasil/id613365711?mt=8",
        "windows_phone": "https://www.microsoft.com/pt-br/store/p/minha-conta-pf/9wzdncrdc5g4"
      },
      "steps": [
        "ATENÇÃO: Não aceitamos pagamentos de contas de terceiros, pague o valor exato!",
        "Acesse o APP ou o site do Banco Santander",
        "No menu de serviços, selecione 'Transferência entre Contas Santander'",
        "Informe a agência: 0386 e conta-corrente: 13002973-8",
        "Confirme o nome do favorecido:  PayMee Brasil Serviços de Pagamentos S.A.",
        "Efetue o pagamento no valor exato de R$ 11,00."
      ]
    }
  }
}
```

> duplicated referenceCode response / validation failure response

```json
{
  "errorCount": 1,
  "errors": [
    {
      "field": "referenceCode",
      "message": "referenceCode duplicated para o comércio."
    }
  ],
  "message": "field validation failure",
  "status": -1
}
```

```json
{
  "errorCount": 12,
  "errors": [
    {
      "field": "shopper.name",
      "message": "may not be empty"
    },
    {
      "field": "shopper.bankDetails.branch",
      "message": "may not be empty"
    },
    {
      "field": "shopper.document.type",
      "message": "may not be null"
    },
    {
      "field": "shopper.phone.number",
      "message": "may not be null"
    },
    {
      "field": "shopper.email",
      "message": "may not be empty"
    },
    {
      "field": "shopper.document.number",
      "message": "may not be null"
    },
    {
      "field": "shopper.email",
      "message": "may not be null"
    },
    {
      "field": "shopper.bankDetails.branch",
      "message": "may not be null"
    },
    {
      "field": "shopper.bankDetails.account",
      "message": "may not be empty"
    },
    {
      "field": "shopper.phone.type",
      "message": "may not be null"
    },
    {
      "field": "shopper.bankDetails.account",
      "message": "may not be null"
    },
    {
      "field": "referenceCode",
      "message": "referenceCode duplicated para o comércio."
    }
  ],
  "message": "field validation failure",
  "status": -1
}
```

> Unauthorized response

```json
{
  "errorCount": 1,
  "errors": [
    {
      "error": "Forbidden",
      "message": "Access Denied"
    }
  ],
  "message": "field validation failure",
  "status": -1
}
```

<h3 id="semitransparentwiretransferpixandcreditcardandopenbankingcheckout-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|success response / semi-transparent checkout (outros bancos example) / pix-success-response / pix-success-response split|Inline|
|400|[Bad Request](https://tools.ietf.org/html/rfc7231#section-6.5.1)|duplicated referenceCode response / validation failure response|Inline|
|403|[Forbidden](https://tools.ietf.org/html/rfc7231#section-6.5.3)|Unauthorized response|Inline|

<h3 id="semitransparentwiretransferpixandcreditcardandopenbankingcheckout-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» message|string|false|none|none|
|» response|object|false|none|none|
|»» amount|number|false|none|none|
|»» id|number|false|none|none|
|»» instructions|object|false|none|none|
|»»» beneficiary_account|string|false|none|none|
|»»» beneficiary_branch|string|false|none|none|
|»»» beneficiary_document|string|false|none|none|
|»»» beneficiary_name|string|false|none|none|
|»»» beneficiary_op|string|false|none|none|
|»»» chosen|string|false|none|none|
|»»» identification|string|false|none|none|
|»»» keys|object|false|none|none|
|»»»» document|string|false|none|none|
|»»»» email|string|false|none|none|
|»»» label|string|false|none|none|
|»»» name|string|false|none|none|
|»»» need_identification|boolean|false|none|none|
|»»» qrCode|object|false|none|none|
|»»»» base64|string|false|none|none|
|»»»» plain|string|false|none|none|
|»»»» url|string|false|none|none|
|»»» redirect_urls|object|false|none|none|
|»»»» android|string|false|none|none|
|»»»» desktop|string|false|none|none|
|»»»» ios|string|false|none|none|
|»»»» windows_phone|string¦null|false|none|none|
|»»» steps|any|false|none|none|

*anyOf*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»» *anonymous*|[string]|false|none|none|

*or*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»» *anonymous*|object|false|none|none|
|»»»»» key|[string]|false|none|none|
|»»»»» qrCode|[string]|false|none|none|

*continued*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»» referenceCode|string|false|none|none|
|»» saleCode|string|false|none|none|
|»» shopper|object|false|none|none|
|»»» bankDetails|object|false|none|none|
|»»»» account|string|false|none|none|
|»»»» branch|string|false|none|none|
|»»» document|object|false|none|none|
|»»»» number|string|false|none|none|
|»»»» type|string|false|none|none|
|»»» email|string|false|none|none|
|»»» id|string|false|none|none|
|»»» name|string|false|none|none|
|»»» phone|object|false|none|none|
|»»»» number|string|false|none|none|
|»»»» type|string|false|none|none|
|»» status|string|false|none|none|
|»» uuid|string|false|none|none|
|» status|number|false|none|none|

Status Code **400**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» errorCount|number|false|none|none|
|» errors|[object]|false|none|none|
|»» field|string|false|none|none|
|»» message|string|false|none|none|
|» message|string|false|none|none|
|» status|number|false|none|none|

Status Code **403**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» errorCount|number|false|none|none|
|» errors|[object]|false|none|none|
|»» error|string|false|none|none|
|»» message|string|false|none|none|
|» message|string|false|none|none|
|» status|number|false|none|none|

### Response Headers

|Status|Header|Type|Format|Description|
|---|---|---|---|---|
|200|Date|string||none|
|200|Transfer-Encoding|string||none|
|400|Connection|string||none|
|400|Date|string||none|
|400|Transfer-Encoding|string||none|
|403|Date|string||none|
|403|Transfer-Encoding|string||none|

<aside class="warning">
To perform this operation, you must be authenticated by means of one of the following methods:
None
</aside>

## redirectCheckout

<a id="opIdredirectCheckout"></a>

> Code samples

```shell
# You can also use wget
curl -X POST https://api.paymee.com.br/v1.1/checkout \
  -H 'Content-Type: application/json' \
  -H 'Accept: application/json' \
  -H 'x-api-key: your-x-api-key' \
  -H 'x-api-token: your-x-api-token'

```

```http
POST https://api.paymee.com.br/v1.1/checkout HTTP/1.1
Host: api.paymee.com.br
Content-Type: application/json
Accept: application/json
x-api-key: your-x-api-key
x-api-token: your-x-api-token

```

```javascript
const inputBody = '{
  "amount": 1.99,
  "callbackURL": "https://foo.bar/paymeeListener",
  "currency": "BRL",
  "maxAge": 120,
  "paymentMethod": "ITAU_TRANSFER_PFF",
  "referenceCode": "refcode12345",
  "shopper": {
    "bankDetails": {
      "account": "12345-6",
      "branch": "1234"
    },
    "document": {
      "number": "00000000000",
      "type": "CPF"
    },
    "email": "foo@bar.com.br",
    "name": "JOHN DOE",
    "phone": {
      "number": "11999990000",
      "type": "WORK"
    }
  }
}';
const headers = {
  'Content-Type':'application/json',
  'Accept':'application/json',
  'x-api-key':'your-x-api-key',
  'x-api-token':'your-x-api-token'
};

fetch('https://api.paymee.com.br/v1.1/checkout',
{
  method: 'POST',
  body: inputBody,
  headers: headers
})
.then(function(res) {
    return res.json();
}).then(function(body) {
    console.log(body);
});

```

```ruby
require 'rest-client'
require 'json'

headers = {
  'Content-Type' => 'application/json',
  'Accept' => 'application/json',
  'x-api-key' => 'your-x-api-key',
  'x-api-token' => 'your-x-api-token'
}

result = RestClient.post 'https://api.paymee.com.br/v1.1/checkout',
  params: {
  }, headers: headers

p JSON.parse(result)

```

```python
import requests
headers = {
  'Content-Type': 'application/json',
  'Accept': 'application/json',
  'x-api-key': 'your-x-api-key',
  'x-api-token': 'your-x-api-token'
}

r = requests.post('https://api.paymee.com.br/v1.1/checkout', headers = headers)

print(r.json())

```

```php
<?php

require 'vendor/autoload.php';

$headers = array(
    'Content-Type' => 'application/json',
    'Accept' => 'application/json',
    'x-api-key' => 'your-x-api-key',
    'x-api-token' => 'your-x-api-token',
);

$client = new \GuzzleHttp\Client();

// Define array of request body.
$request_body = array();

try {
    $response = $client->request('POST','https://api.paymee.com.br/v1.1/checkout', array(
        'headers' => $headers,
        'json' => $request_body,
       )
    );
    print_r($response->getBody()->getContents());
 }
 catch (\GuzzleHttp\Exception\BadResponseException $e) {
    // handle exception or api errors.
    print_r($e->getMessage());
 }

 // ...

```

```java
URL obj = new URL("https://api.paymee.com.br/v1.1/checkout");
HttpURLConnection con = (HttpURLConnection) obj.openConnection();
con.setRequestMethod("POST");
int responseCode = con.getResponseCode();
BufferedReader in = new BufferedReader(
    new InputStreamReader(con.getInputStream()));
String inputLine;
StringBuffer response = new StringBuffer();
while ((inputLine = in.readLine()) != null) {
    response.append(inputLine);
}
in.close();
System.out.println(response.toString());

```

```go
package main

import (
       "bytes"
       "net/http"
)

func main() {

    headers := map[string][]string{
        "Content-Type": []string{"application/json"},
        "Accept": []string{"application/json"},
        "x-api-key": []string{"your-x-api-key"},
        "x-api-token": []string{"your-x-api-token"},
    }

    data := bytes.NewBuffer([]byte{jsonReq})
    req, err := http.NewRequest("POST", "https://api.paymee.com.br/v1.1/checkout", data)
    req.Header = headers

    client := &http.Client{}
    resp, err := client.Do(req)
    // ...
}

```

`POST /v1.1/checkout`

*Redirect checkout*

To generate a transaction through PayMee, it is necessary to send a request using the POST method to the checkout resource, as shown. This example covers the minimum of submitted fields required for authorization.

> Body parameter

```json
{
  "amount": 1.99,
  "callbackURL": "https://foo.bar/paymeeListener",
  "currency": "BRL",
  "maxAge": 120,
  "paymentMethod": "ITAU_TRANSFER_PFF",
  "referenceCode": "refcode12345",
  "shopper": {
    "bankDetails": {
      "account": "12345-6",
      "branch": "1234"
    },
    "document": {
      "number": "00000000000",
      "type": "CPF"
    },
    "email": "foo@bar.com.br",
    "name": "JOHN DOE",
    "phone": {
      "number": "11999990000",
      "type": "WORK"
    }
  }
}
```

<h3 id="redirectcheckout-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|x-api-key|header|string|true|none|
|x-api-token|header|string|true|none|
|body|body|object|true|none|
|» currency|body|string|true|ISO-4217 currency code|
|» amount|body|number(float)|true|Order Amount|
|» referenceCode|body|string|true|Unique order identifier|
|» maxAge|body|number|false|Sale max age in minutes|
|» paymentMethod|body|string|false|Chosen payment method|
|» callbackURL|body|string(uri)|false|Callback URL for transaction updates|
|» observation|body|string|false|Any internal reference|
|» shopper|body|object|true|none|
|»» id|body|string|false|Internal shopper identifier|
|»» name|body|string|false|Shopper's name|
|»» email|body|string(email)|true|Shopper's email|
|»» document|body|object|false|none|
|»»» type|body|string|false|Shopper's document type|
|»»» number|body|string|false|Shopper's document number|
|»» phone|body|object|false|none|
|»»» type|body|string|false|Shopper's phone type|
|»»» number|body|string|false|Shopper's phone number|
|»» bankDetails|body|object|false|none|
|»»» branch|body|string|false|Shopper's branch number|
|»»» account|body|string|false|Shopper's account number|

#### Enumerated Values

|Parameter|Value|
|---|---|
|»»» type|CPF|
|»»» type|CNPJ|
|»»» type|OTHER|
|»»» type|MOBILE|
|»»» type|HOME|
|»»» type|WORK|
|»»» type|OTHER|

> Example responses

> Success response

```json
{
  "status": 0,
  "message": "success",
  "response": {
    "amount": 1.99,
    "gatewayURL": "https://secure.paymee.com.br/redir/55f6bb4e-d1aa-361e-bce3-758fd57ab6d0",
    "referenceCode": "refcode12345",
    "saleCode": "0002405",
    "shopper": {
      "bankDetails": {
        "account": "12345-6",
        "branch": "1234"
      },
      "document": {
        "number": "00000000000",
        "type": "CPF"
      },
      "email": "foo@bar.com.br",
      "name": "JOHN DOE",
      "phone": {
        "number": "11999990000",
        "type": "MOBILE"
      }
    },
    "uuid": "55f6bb4e-d1aa-361e-bce3-758fd57ab6d0"
  }
}
```

> Duplicated reference code response / Field validation failure response

```json
{
  "errorCount": 1,
  "errors": [
    {
      "field": "referenceCode",
      "message": "referenceCode duplicated para o comércio."
    }
  ],
  "message": "field validation failure",
  "status": -1
}
```

```json
{
  "errorCount": 11,
  "errors": [
    {
      "field": "shopper.bankDetails.account",
      "message": "may not be null"
    },
    {
      "field": "shopper.document.number",
      "message": "may not be null"
    },
    {
      "field": "shopper.phone.number",
      "message": "may not be null"
    },
    {
      "field": "paymentMethod",
      "message": "unknown payment method"
    },
    {
      "field": "shopper.bankDetails.account",
      "message": "may not be empty"
    },
    {
      "field": "shopper.bankDetails.branch",
      "message": "may not be empty"
    },
    {
      "field": "shopper.bankDetails.branch",
      "message": "may not be null"
    },
    {
      "field": "shopper.phone.type",
      "message": "may not be null"
    },
    {
      "field": "shopper.email",
      "message": "may not be empty"
    },
    {
      "field": "shopper.email",
      "message": "may not be null"
    },
    {
      "field": "referenceCode",
      "message": "referenceCode duplicated para o comércio."
    }
  ],
  "message": "field validation failure",
  "status": -1
}
```

> Unauthorized response

```json
{
  "errorCount": 1,
  "errors": [
    {
      "error": "Forbidden",
      "message": "Access Denied"
    }
  ],
  "message": "field validation failure",
  "status": -1
}
```

<h3 id="redirectcheckout-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|Success response|Inline|
|400|[Bad Request](https://tools.ietf.org/html/rfc7231#section-6.5.1)|Duplicated reference code response / Field validation failure response|Inline|
|403|[Forbidden](https://tools.ietf.org/html/rfc7231#section-6.5.3)|Unauthorized response|Inline|

<h3 id="redirectcheckout-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» message|string|false|none|none|
|» response|object|false|none|none|
|»» amount|number|false|none|none|
|»» gatewayURL|string|false|none|none|
|»» referenceCode|string|false|none|none|
|»» saleCode|string|false|none|none|
|»» shopper|object|false|none|none|
|»»» bankDetails|object|false|none|none|
|»»»» account|string|false|none|none|
|»»»» branch|string|false|none|none|
|»»» document|object|false|none|none|
|»»»» number|string|false|none|none|
|»»»» type|string|false|none|none|
|»»» email|string|false|none|none|
|»»» name|string|false|none|none|
|»»» phone|object|false|none|none|
|»»»» number|string|false|none|none|
|»»»» type|string|false|none|none|
|»» uuid|string|false|none|none|
|» status|number|false|none|none|

Status Code **400**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» errorCount|number|false|none|none|
|» errors|[object]|false|none|none|
|»» field|string|false|none|none|
|»» message|string|false|none|none|
|» message|string|false|none|none|
|» status|number|false|none|none|

Status Code **403**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» errorCount|number|false|none|none|
|» errors|[object]|false|none|none|
|»» error|string|false|none|none|
|»» message|string|false|none|none|
|» message|string|false|none|none|
|» status|number|false|none|none|

### Response Headers

|Status|Header|Type|Format|Description|
|---|---|---|---|---|
|200|Date|string||none|
|200|Transfer-Encoding|string||none|
|400|Connection|string||none|
|400|Date|string||none|
|400|Transfer-Encoding|string||none|
|403|Date|string||none|
|403|Transfer-Encoding|string||none|

<aside class="success">
This operation does not require authentication
</aside>

## createPixAutomaticCheckout

<a id="opIdcreatePixAutomaticCheckout"></a>

> Code samples

```shell
# You can also use wget
curl -X POST https://api.paymee.com.br/v1.1/checkout/pix-automatic \
  -H 'Content-Type: application/json' \
  -H 'Accept: application/json' \
  -H 'x-api-key: your-x-api-key' \
  -H 'x-api-token: your-x-api-token'

```

```http
POST https://api.paymee.com.br/v1.1/checkout/pix-automatic HTTP/1.1
Host: api.paymee.com.br
Content-Type: application/json
Accept: application/json
x-api-key: your-x-api-key
x-api-token: your-x-api-token

```

```javascript
const inputBody = '{
  "currency": "BRL",
  "referenceCode": "ref-journey2-xyz-123",
  "maxAge": 120,
  "callbackURL": "https://foo.bar/paymeeListener",
  "shopper": {
    "id": "24391203",
    "email": "foo@bar.com",
    "name": "JOHN DOE",
    "document": {
      "type": "CPF",
      "number": "00000000000"
    },
    "phone": {
      "type": "MOBILE",
      "number": "11999990000"
    }
  },
  "recurrence": {
    "planId": "6852768495f9a7e75febdbe3",
    "immediatePayment": false,
    "amount": {
      "recurrenceAmount": 11
    },
    "calendar": {
      "initialDate": "2025-07-25",
      "finalDate": "2025-12-25",
      "periodicity": "MONTHLY"
    },
    "retry": true
  }
}';
const headers = {
  'Content-Type':'application/json',
  'Accept':'application/json',
  'x-api-key':'your-x-api-key',
  'x-api-token':'your-x-api-token'
};

fetch('https://api.paymee.com.br/v1.1/checkout/pix-automatic',
{
  method: 'POST',
  body: inputBody,
  headers: headers
})
.then(function(res) {
    return res.json();
}).then(function(body) {
    console.log(body);
});

```

```ruby
require 'rest-client'
require 'json'

headers = {
  'Content-Type' => 'application/json',
  'Accept' => 'application/json',
  'x-api-key' => 'your-x-api-key',
  'x-api-token' => 'your-x-api-token'
}

result = RestClient.post 'https://api.paymee.com.br/v1.1/checkout/pix-automatic',
  params: {
  }, headers: headers

p JSON.parse(result)

```

```python
import requests
headers = {
  'Content-Type': 'application/json',
  'Accept': 'application/json',
  'x-api-key': 'your-x-api-key',
  'x-api-token': 'your-x-api-token'
}

r = requests.post('https://api.paymee.com.br/v1.1/checkout/pix-automatic', headers = headers)

print(r.json())

```

```php
<?php

require 'vendor/autoload.php';

$headers = array(
    'Content-Type' => 'application/json',
    'Accept' => 'application/json',
    'x-api-key' => 'your-x-api-key',
    'x-api-token' => 'your-x-api-token',
);

$client = new \GuzzleHttp\Client();

// Define array of request body.
$request_body = array();

try {
    $response = $client->request('POST','https://api.paymee.com.br/v1.1/checkout/pix-automatic', array(
        'headers' => $headers,
        'json' => $request_body,
       )
    );
    print_r($response->getBody()->getContents());
 }
 catch (\GuzzleHttp\Exception\BadResponseException $e) {
    // handle exception or api errors.
    print_r($e->getMessage());
 }

 // ...

```

```java
URL obj = new URL("https://api.paymee.com.br/v1.1/checkout/pix-automatic");
HttpURLConnection con = (HttpURLConnection) obj.openConnection();
con.setRequestMethod("POST");
int responseCode = con.getResponseCode();
BufferedReader in = new BufferedReader(
    new InputStreamReader(con.getInputStream()));
String inputLine;
StringBuffer response = new StringBuffer();
while ((inputLine = in.readLine()) != null) {
    response.append(inputLine);
}
in.close();
System.out.println(response.toString());

```

```go
package main

import (
       "bytes"
       "net/http"
)

func main() {

    headers := map[string][]string{
        "Content-Type": []string{"application/json"},
        "Accept": []string{"application/json"},
        "x-api-key": []string{"your-x-api-key"},
        "x-api-token": []string{"your-x-api-token"},
    }

    data := bytes.NewBuffer([]byte{jsonReq})
    req, err := http.NewRequest("POST", "https://api.paymee.com.br/v1.1/checkout/pix-automatic", data)
    req.Header = headers

    client := &http.Client{}
    resp, err := client.Do(req)
    // ...
}

```

`POST /v1.1/checkout/pix-automatic`

*Create Pix Automatic Recurrence*

Creates a Pix Automatic recurrence journey. Use the `immediatePayment` flag inside the `recurrence` object to control the flow: `false` for Journey 2 (authorization only), or `true` for Journey 3 (authorization with an immediate payment).

> Body parameter

```json
{
  "currency": "BRL",
  "referenceCode": "ref-journey2-xyz-123",
  "maxAge": 120,
  "callbackURL": "https://foo.bar/paymeeListener",
  "shopper": {
    "id": "24391203",
    "email": "foo@bar.com",
    "name": "JOHN DOE",
    "document": {
      "type": "CPF",
      "number": "00000000000"
    },
    "phone": {
      "type": "MOBILE",
      "number": "11999990000"
    }
  },
  "recurrence": {
    "planId": "6852768495f9a7e75febdbe3",
    "immediatePayment": false,
    "amount": {
      "recurrenceAmount": 11
    },
    "calendar": {
      "initialDate": "2025-07-25",
      "finalDate": "2025-12-25",
      "periodicity": "MONTHLY"
    },
    "retry": true
  }
}
```

<h3 id="createpixautomaticcheckout-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|x-api-key|header|string|true|none|
|x-api-token|header|string|true|none|
|body|body|object|true|none|
|» currency|body|string|false|ISO-4217 currency code.|
|» referenceCode|body|string|false|Unique order identifier.|
|» amount|body|number|false|Required only when `recurrence.immediatePayment` is `true` (Journey 3).|
|» maxAge|body|number|false|QR Code expiration time in minutes.|
|» callbackURL|body|string|false|Callback URL for transaction updates.|
|» shopper|body|object|false|none|
|»» id|body|string|false|none|
|»» name|body|string|false|none|
|»» email|body|string|false|none|
|»» document|body|object|false|none|
|»»» type|body|string|false|none|
|»»» number|body|string|false|none|
|»» phone|body|object|false|none|
|»»» type|body|string|false|none|
|»»» number|body|string|false|none|
|» recurrence|body|object|false|none|
|»» planId|body|string|false|ID of the previously created plan.|
|»» immediatePayment|body|boolean|false|Controls the flow. `true` for Journey 3 (with immediate payment), `false` for Journey 2 (authorization only).|
|»» amount|body|object|false|none|
|»»» recurrenceAmount|body|number|false|none|
|»» calendar|body|object|false|none|
|»»» initialDate|body|string(date)|false|none|
|»»» finalDate|body|string(date)|false|none|
|»»» periodicity|body|string|false|none|
|»» retry|body|boolean|false|none|

#### Enumerated Values

|Parameter|Value|
|---|---|
|»»» type|CPF|
|»»» type|CNPJ|
|»»» type|MOBILE|
|»»» type|HOME|
|»»» type|WORK|

> Example responses

> Pix Automatic journey created successfully. The response contains QR code instructions to start the flow.

```json
{
  "status": 0,
  "message": "success",
  "response": {
    "id": 215832474,
    "referenceCode": "ref-journey3-abc-456",
    "amount": 15,
    "saleCode": "215832474",
    "uuid": "ed700503-e3e0-3c3f-867f-0bd7bba3424a",
    "shopper": {
      "id": "24391204",
      "name": "JOHN DOE",
      "email": "jane@doe.com",
      "document": {
        "type": "CPF",
        "number": "00000000000"
      },
      "phone": {
        "type": "MOBILE",
        "number": "11999990000"
      }
    },
    "instructions": {
      "chosen": "PIX_RECURRENCE",
      "name": "Pagamento Recorrente via PIX",
      "label": "Pagamento Recorrente via PIX",
      "qrCode": {
        "url": "https://api.paymee.com.br/resources/payments/pix/qrcode/ed700503-e3e0-3c3f-867f-0bd7bba3424a",
        "base64": "iVBORw0KGgoAAAANSUhEUgAAAPoAAAD6CAIAAAAHjs1q... (base64 image data)",
        "plain": "00020101021226930014br.gov.bcb.pix... (qr code text)"
      },
      "steps": {
        "qrCode": [
          "Acesse o APP ou o Internet Banking do seu Banco",
          "Acesse o menu PIX",
          "Abra o leitor de QRCode no APP e escaneie o código informado",
          "Siga os passos no APP e finalize a transação"
        ]
      }
    }
  }
}
```

> 400 Response

```json
{
  "errorCount": 1,
  "errors": [
    {
      "field": "referenceCode",
      "message": "referenceCode may not be empty"
    }
  ],
  "message": "field validation failure",
  "status": -1
}
```

<h3 id="createpixautomaticcheckout-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|Pix Automatic journey created successfully. The response contains QR code instructions to start the flow.|Inline|
|400|[Bad Request](https://tools.ietf.org/html/rfc7231#section-6.5.1)|Bad Request. The request body is malformed or contains invalid data.|Inline|
|403|[Forbidden](https://tools.ietf.org/html/rfc7231#section-6.5.3)|Unauthorized response|Inline|

<h3 id="createpixautomaticcheckout-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» status|integer|false|none|none|
|» message|string|false|none|none|
|» response|object|false|none|none|

Status Code **400**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» errorCount|number|false|none|none|
|» errors|[object]|false|none|none|
|»» field|string|false|none|none|
|»» message|string|false|none|none|
|» message|string|false|none|none|
|» status|number|false|none|none|

Status Code **403**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» errorCount|number|false|none|none|
|» errors|[object]|false|none|none|
|»» error|string|false|none|none|
|»» message|string|false|none|none|
|» message|string|false|none|none|
|» status|number|false|none|none|

<aside class="success">
This operation does not require authentication
</aside>

<h1 id="api-paymee-1-1-payouts">Payouts</h1>

**Our API collection for Payouts requests**

## batchPayoutRequest

<a id="opIdbatchPayoutRequest"></a>

> Code samples

```shell
# You can also use wget
curl -X POST https://api.paymee.com.br/v1.1/payout/create/batch \
  -H 'Content-Type: application/json' \
  -H 'Accept: application/json' \
  -H 'x-api-key: your-x-api-key' \
  -H 'x-api-token: your-x-api-token'

```

```http
POST https://api.paymee.com.br/v1.1/payout/create/batch HTTP/1.1
Host: api.paymee.com.br
Content-Type: application/json
Accept: application/json
x-api-key: your-x-api-key
x-api-token: your-x-api-token

```

```javascript
const inputBody = '{
  "payments": [
    {
      "amount": 1,
      "beneficiary": {
        "bankDetails": {
          "account": "000001-5",
          "bankCode": "077",
          "branch": "0001",
          "type": "CHECKING"
        },
        "document": {
          "number": "00000000000",
          "type": "CPF"
        }
      },
      "email": "foo@bar.com",
      "notes": "example-01",
      "referenceCode": "X540QG02",
      "scheduledDate": "2020-11-29 15:00:00"
    },
    {
      "amount": 1.99,
      "beneficiary": {
        "bankDetails": {
          "account": "000002-2",
          "branch": "7444",
          "type": "CHECKING"
        },
        "document": {
          "number": "00000000001",
          "type": "CPF"
        }
      },
      "email": "foo1@bar.com",
      "notes": "example-02",
      "referenceCode": "X540QG12",
      "scheduledDate": "2020-11-25 00:00:00"
    },
    {
      "amount": 3.99,
      "beneficiary": {
        "bankDetails": {
          "account": "000022-0",
          "bankCode": "104",
          "branch": "7474",
          "type": "SAVING"
        },
        "document": {
          "number": "00000000002",
          "type": "CPF"
        }
      },
      "email": "foo2@bar.com",
      "notes": "saving-account-example",
      "referenceCode": "X540QG22"
    }
  ]
}';
const headers = {
  'Content-Type':'application/json',
  'Accept':'application/json',
  'x-api-key':'your-x-api-key',
  'x-api-token':'your-x-api-token'
};

fetch('https://api.paymee.com.br/v1.1/payout/create/batch',
{
  method: 'POST',
  body: inputBody,
  headers: headers
})
.then(function(res) {
    return res.json();
}).then(function(body) {
    console.log(body);
});

```

```ruby
require 'rest-client'
require 'json'

headers = {
  'Content-Type' => 'application/json',
  'Accept' => 'application/json',
  'x-api-key' => 'your-x-api-key',
  'x-api-token' => 'your-x-api-token'
}

result = RestClient.post 'https://api.paymee.com.br/v1.1/payout/create/batch',
  params: {
  }, headers: headers

p JSON.parse(result)

```

```python
import requests
headers = {
  'Content-Type': 'application/json',
  'Accept': 'application/json',
  'x-api-key': 'your-x-api-key',
  'x-api-token': 'your-x-api-token'
}

r = requests.post('https://api.paymee.com.br/v1.1/payout/create/batch', headers = headers)

print(r.json())

```

```php
<?php

require 'vendor/autoload.php';

$headers = array(
    'Content-Type' => 'application/json',
    'Accept' => 'application/json',
    'x-api-key' => 'your-x-api-key',
    'x-api-token' => 'your-x-api-token',
);

$client = new \GuzzleHttp\Client();

// Define array of request body.
$request_body = array();

try {
    $response = $client->request('POST','https://api.paymee.com.br/v1.1/payout/create/batch', array(
        'headers' => $headers,
        'json' => $request_body,
       )
    );
    print_r($response->getBody()->getContents());
 }
 catch (\GuzzleHttp\Exception\BadResponseException $e) {
    // handle exception or api errors.
    print_r($e->getMessage());
 }

 // ...

```

```java
URL obj = new URL("https://api.paymee.com.br/v1.1/payout/create/batch");
HttpURLConnection con = (HttpURLConnection) obj.openConnection();
con.setRequestMethod("POST");
int responseCode = con.getResponseCode();
BufferedReader in = new BufferedReader(
    new InputStreamReader(con.getInputStream()));
String inputLine;
StringBuffer response = new StringBuffer();
while ((inputLine = in.readLine()) != null) {
    response.append(inputLine);
}
in.close();
System.out.println(response.toString());

```

```go
package main

import (
       "bytes"
       "net/http"
)

func main() {

    headers := map[string][]string{
        "Content-Type": []string{"application/json"},
        "Accept": []string{"application/json"},
        "x-api-key": []string{"your-x-api-key"},
        "x-api-token": []string{"your-x-api-token"},
    }

    data := bytes.NewBuffer([]byte{jsonReq})
    req, err := http.NewRequest("POST", "https://api.paymee.com.br/v1.1/payout/create/batch", data)
    req.Header = headers

    client := &http.Client{}
    resp, err := client.Do(req)
    // ...
}

```

`POST /v1.1/payout/create/batch`

*Batch payout request*

Payouts at PayMee are cash-out transactions;

Before we start, there are some important tips:
 1. you must have sufficient funds;
 2. you shoud provide a valid and complete beneficiary details
 3. Register your server CIDR for production enviroment

> Body parameter

```json
{
  "payments": [
    {
      "amount": 1,
      "beneficiary": {
        "bankDetails": {
          "account": "000001-5",
          "bankCode": "077",
          "branch": "0001",
          "type": "CHECKING"
        },
        "document": {
          "number": "00000000000",
          "type": "CPF"
        }
      },
      "email": "foo@bar.com",
      "notes": "example-01",
      "referenceCode": "X540QG02",
      "scheduledDate": "2020-11-29 15:00:00"
    },
    {
      "amount": 1.99,
      "beneficiary": {
        "bankDetails": {
          "account": "000002-2",
          "branch": "7444",
          "type": "CHECKING"
        },
        "document": {
          "number": "00000000001",
          "type": "CPF"
        }
      },
      "email": "foo1@bar.com",
      "notes": "example-02",
      "referenceCode": "X540QG12",
      "scheduledDate": "2020-11-25 00:00:00"
    },
    {
      "amount": 3.99,
      "beneficiary": {
        "bankDetails": {
          "account": "000022-0",
          "bankCode": "104",
          "branch": "7474",
          "type": "SAVING"
        },
        "document": {
          "number": "00000000002",
          "type": "CPF"
        }
      },
      "email": "foo2@bar.com",
      "notes": "saving-account-example",
      "referenceCode": "X540QG22"
    }
  ]
}
```

<h3 id="batchpayoutrequest-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|x-api-key|header|string|true|none|
|x-api-token|header|string|true|none|
|body|body|object|true|none|
|» payments|body|[object]|true|Array of payment requests|
|»» currency|body|string|true|ISO-4217 currency code - default: BRL|
|»» amount|body|number|true|Payout amount|
|»» referenceCode|body|string|true|Unique order identifier|
|»» scheduledDate|body|string(date-time)|false|Payment trigger date - format 'yyyy-MM-dd HH:mm:ss' (timezone GMT-3)|
|»» callbackURL|body|string(uri)|false|Individual callback URL|
|»» notes|body|string|false|Payment optional notes|
|»» email|body|string(email)|false|Email for notifications|
|»» beneficiary|body|object|true|none|
|»»» email|body|string(email)|false|Beneficiary's email|
|»»» document|body|object|true|none|
|»»»» type|body|string|true|Beneficiary's document type|
|»»»» number|body|string|true|Beneficiary's document number|
|»»» bankDetails|body|object|true|none|
|»»»» type|body|string|true|Beneficiary's bank account type|
|»»»» bankCode|body|string|true|Beneficiary's bank code (COMPE code)|
|»»»» branch|body|string|true|Beneficiary's bank branch number|
|»»»» account|body|string|true|Beneficiary's bank account number|

#### Enumerated Values

|Parameter|Value|
|---|---|
|»»»» type|CPF|
|»»»» type|CNPJ|
|»»»» type|OTHER|
|»»»» type|CHECKING|
|»»»» type|SAVING|
|»»»» type|UNKNOWN|

> Example responses

> Success or partial success response

```json
{
  "message": "success",
  "result": [
    {
      "amount": 1,
      "appliedRate": 3.5,
      "creation": "2019-11-21 19:33:00",
      "currency": "BRL",
      "discounts": 0,
      "id": 1103460,
      "situation": "PENDING",
      "total": 1,
      "type": "PAYOUT",
      "uuid": "734150a2-3377-3717-832c-6f142ece7fa8"
    },
    {
      "amount": 1.99,
      "appliedRate": 3.5,
      "creation": "2019-11-21 19:33:00",
      "currency": "BRL",
      "discounts": 0,
      "id": 1103461,
      "situation": "PENDING",
      "total": 1.99,
      "type": "PAYOUT",
      "uuid": "b8784391-8d54-3d72-a9e6-5db10499c674"
    },
    {
      "amount": 3.99,
      "appliedRate": 3.5,
      "creation": "2019-11-21 19:33:00",
      "currency": "BRL",
      "discounts": 0,
      "id": 1103462,
      "situation": "PENDING",
      "total": 3.99,
      "type": "PAYOUT",
      "uuid": "ad4d5020-c896-324e-8979-03b7b5e63895"
    }
  ],
  "resultSize": 3,
  "status": 0,
  "totalElements": 3,
  "totalPages": 1
}
```

```json
{
  "message": "partial success",
  "result": [
    {
      "amount": 1,
      "creation": "2020-10-23 18:20:20",
      "currency": "BRL",
      "discounts": 0,
      "id": 33585,
      "referenceCode": "X540QG02",
      "serviceFee": 3.5,
      "situation": "PENDING",
      "total": 1,
      "type": "PAYOUT",
      "uuid": "d074debe-433b-342f-bcac-3088f403719e"
    },
    {
      "amount": 3.99,
      "creation": "2020-10-23 18:20:20",
      "currency": "BRL",
      "discounts": 0,
      "id": 33586,
      "referenceCode": "X540QG22",
      "serviceFee": 3.5,
      "situation": "PENDING",
      "total": 3.99,
      "type": "PAYOUT",
      "uuid": "1898d70c-105a-3542-9736-46c0c15dce9d"
    }
  ],
  "rejectedItems": [
    {
      "amount": 1.99,
      "beneficiary": {
        "bankDetails": {
          "account": "000002-2",
          "bankCode": null,
          "branch": "7444",
          "type": "CHECKING"
        },
        "document": {
          "number": "00000000001",
          "type": "CPF"
        }
      },
      "email": "foo1@bar.com",
      "errorCount": 1,
      "errors": [
        {
          "field": "payments[1].beneficiary.bankDetails.bankCode",
          "message": "por favor informe um codigo COMPE valido."
        }
      ],
      "notes": "example-02",
      "referenceCode": "X540QG12",
      "scheduledDate": "2020-11-25 00:00:00"
    }
  ],
  "resultSize": 2,
  "status": 1,
  "totalElements": 2,
  "totalPages": 1
}
```

> Validation failure

```json
{
  "errorCount": 1,
  "errors": [
    {
      "field": "payments",
      "message": "payments cannot be empty"
    }
  ],
  "message": "field validation failure",
  "status": -1
}
```

> Unauthorized access

```json
{
  "errorCount": 1,
  "errors": [
    {
      "error": "Forbidden",
      "message": "Access Denied"
    }
  ],
  "message": "field validation failure",
  "status": -1
}
```

<h3 id="batchpayoutrequest-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|Success or partial success response|Inline|
|400|[Bad Request](https://tools.ietf.org/html/rfc7231#section-6.5.1)|Validation failure|Inline|
|403|[Forbidden](https://tools.ietf.org/html/rfc7231#section-6.5.3)|Unauthorized access|Inline|

<h3 id="batchpayoutrequest-responseschema">Response Schema</h3>

Status Code **400**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» errorCount|number|false|none|none|
|» errors|[object]|false|none|none|
|»» field|string|false|none|none|
|»» message|string|false|none|none|
|» message|string|false|none|none|
|» status|number|false|none|none|

Status Code **403**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» errorCount|number|false|none|none|
|» errors|[object]|false|none|none|
|»» error|string|false|none|none|
|»» message|string|false|none|none|
|» message|string|false|none|none|
|» status|number|false|none|none|

<aside class="success">
This operation does not require authentication
</aside>

## individualPayoutRequest

<a id="opIdindividualPayoutRequest"></a>

> Code samples

```shell
# You can also use wget
curl -X POST https://api.paymee.com.br/v1.1/payout/create \
  -H 'Content-Type: application/json' \
  -H 'Accept: application/json' \
  -H 'x-api-key: your-x-api-key' \
  -H 'x-api-token: your-x-api-token'

```

```http
POST https://api.paymee.com.br/v1.1/payout/create HTTP/1.1
Host: api.paymee.com.br
Content-Type: application/json
Accept: application/json
x-api-key: your-x-api-key
x-api-token: your-x-api-token

```

```javascript
const inputBody = '{
  "amount": 0.01,
  "referenceCode": "ABC012363YAA",
  "beneficiary": {
    "bankDetails": {
      "pixKey": "00000000000"
    },
    "document": {
      "number": "00000000000",
      "type": "CPF"
    }
  },
  "callbackURL": "https://www2.paymee.com.br/callback.php",
  "email": "foo@bar.com",
  "notes": "pixKey"
}';
const headers = {
  'Content-Type':'application/json',
  'Accept':'application/json',
  'x-api-key':'your-x-api-key',
  'x-api-token':'your-x-api-token'
};

fetch('https://api.paymee.com.br/v1.1/payout/create',
{
  method: 'POST',
  body: inputBody,
  headers: headers
})
.then(function(res) {
    return res.json();
}).then(function(body) {
    console.log(body);
});

```

```ruby
require 'rest-client'
require 'json'

headers = {
  'Content-Type' => 'application/json',
  'Accept' => 'application/json',
  'x-api-key' => 'your-x-api-key',
  'x-api-token' => 'your-x-api-token'
}

result = RestClient.post 'https://api.paymee.com.br/v1.1/payout/create',
  params: {
  }, headers: headers

p JSON.parse(result)

```

```python
import requests
headers = {
  'Content-Type': 'application/json',
  'Accept': 'application/json',
  'x-api-key': 'your-x-api-key',
  'x-api-token': 'your-x-api-token'
}

r = requests.post('https://api.paymee.com.br/v1.1/payout/create', headers = headers)

print(r.json())

```

```php
<?php

require 'vendor/autoload.php';

$headers = array(
    'Content-Type' => 'application/json',
    'Accept' => 'application/json',
    'x-api-key' => 'your-x-api-key',
    'x-api-token' => 'your-x-api-token',
);

$client = new \GuzzleHttp\Client();

// Define array of request body.
$request_body = array();

try {
    $response = $client->request('POST','https://api.paymee.com.br/v1.1/payout/create', array(
        'headers' => $headers,
        'json' => $request_body,
       )
    );
    print_r($response->getBody()->getContents());
 }
 catch (\GuzzleHttp\Exception\BadResponseException $e) {
    // handle exception or api errors.
    print_r($e->getMessage());
 }

 // ...

```

```java
URL obj = new URL("https://api.paymee.com.br/v1.1/payout/create");
HttpURLConnection con = (HttpURLConnection) obj.openConnection();
con.setRequestMethod("POST");
int responseCode = con.getResponseCode();
BufferedReader in = new BufferedReader(
    new InputStreamReader(con.getInputStream()));
String inputLine;
StringBuffer response = new StringBuffer();
while ((inputLine = in.readLine()) != null) {
    response.append(inputLine);
}
in.close();
System.out.println(response.toString());

```

```go
package main

import (
       "bytes"
       "net/http"
)

func main() {

    headers := map[string][]string{
        "Content-Type": []string{"application/json"},
        "Accept": []string{"application/json"},
        "x-api-key": []string{"your-x-api-key"},
        "x-api-token": []string{"your-x-api-token"},
    }

    data := bytes.NewBuffer([]byte{jsonReq})
    req, err := http.NewRequest("POST", "https://api.paymee.com.br/v1.1/payout/create", data)
    req.Header = headers

    client := &http.Client{}
    resp, err := client.Do(req)
    // ...
}

```

`POST /v1.1/payout/create`

*Individual payout request*

Payouts at PayMee are cash-out transactions;

Before we start, there are some important tips:

1. you must have sufficient funds;
2. you shoud provide a valid and complete beneficiary details
3. Register your server CIDR for production enviroment

> Body parameter

```json
{
  "amount": 0.01,
  "referenceCode": "ABC012363YAA",
  "beneficiary": {
    "bankDetails": {
      "pixKey": "00000000000"
    },
    "document": {
      "number": "00000000000",
      "type": "CPF"
    }
  },
  "callbackURL": "https://www2.paymee.com.br/callback.php",
  "email": "foo@bar.com",
  "notes": "pixKey"
}
```

<h3 id="individualpayoutrequest-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|x-api-key|header|string|true|none|
|x-api-token|header|string|true|none|
|body|body|object|true|none|
|» currency|body|string|true|ISO-4217 currency code - default: BRL|
|» amount|body|number|true|Payout amount|
|» referenceCode|body|string|true|Unique order identifier|
|» scheduledDate|body|string(date-time)|false|Payment trigger date - format 'yyyy-MM-dd HH:mm:ss' (timezone GMT-3)|
|» callbackURL|body|string(uri)|false|Individual callback URL|
|» notes|body|string|false|Payment optional notes|
|» email|body|string(email)|false|Email for notifications|
|» beneficiary|body|object|true|none|
|»» email|body|string(email)|false|Beneficiary's email|
|»» document|body|object|true|none|
|»»» type|body|string|true|Beneficiary's document type|
|»»» number|body|string|true|Beneficiary's document number|
|»» bankDetails|body|object|false|none|
|»»» *anonymous*|body|object|false|none|
|»»»» type|body|string|true|Beneficiary's bank account type|
|»»»» bankCode|body|string|true|Beneficiary's bank code (COMPE code)|
|»»»» branch|body|string|true|Beneficiary's bank branch number|
|»»»» account|body|string|true|Beneficiary's bank account number|
|»»» *anonymous*|body|object|false|none|
|»»»» pixKey|body|string|true|Beneficiary's PIX key|

#### Enumerated Values

|Parameter|Value|
|---|---|
|»»» type|CPF|
|»»» type|CNPJ|
|»»» type|OTHER|
|»»»» type|CHECKING|
|»»»» type|SAVING|
|»»»» type|UNKNOWN|

> Example responses

> Success response

```json
{
  "status": 0,
  "message": "success",
  "id": 49913,
  "uuid": "b1b6c32c-65a5-3ecc-8c20-d1d24d7a1e40",
  "type": "PAYOUT",
  "situation": "PENDING",
  "currency": "BRL",
  "amount": 0.01,
  "discounts": 0,
  "total": 0.01,
  "serviceFee": 3.5,
  "creation": "2021-05-26 22:47:42",
  "beneficiary": {
    "bank": "000 - PIX",
    "branch": "0000",
    "account": "000000-0"
  },
  "authorization_key": "2d1e28e2-b820-48fb-8b58-d2dc32b25944"
}
```

```json
{
  "status": 0,
  "message": "success",
  "id": 21526,
  "uuid": "8701772d-2d96-370d-a7a3-e2709e632658",
  "type": "PAYOUT",
  "situation": "PENDING",
  "currency": "BRL",
  "amount": 1,
  "discounts": 0,
  "total": 1,
  "serviceFee": 3.5,
  "creation": "2020-01-20 19:50:20"
}
```

> Field validation error response / PIX key not found

```json
{
  "errorCount": 1,
  "errors": [
    {
      "field": "referenceCode",
      "message": "referenceCode duplicated"
    }
  ],
  "message": "field validation failure",
  "status": -1
}
```

```json
{
  "errorCount": 1,
  "errors": [
    {
      "field": "beneficiary.bankDetails.pixKey",
      "message": "Chave DICT não encontrada"
    }
  ],
  "message": "field validation failure",
  "status": -1
}
```

> Unauthorized response

```json
{
  "errorCount": 1,
  "errors": [
    {
      "error": "Forbidden",
      "message": "Access Denied"
    }
  ],
  "message": "field validation failure",
  "status": -1
}
```

<h3 id="individualpayoutrequest-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|Success response|Inline|
|400|[Bad Request](https://tools.ietf.org/html/rfc7231#section-6.5.1)|Field validation error response / PIX key not found|Inline|
|403|[Forbidden](https://tools.ietf.org/html/rfc7231#section-6.5.3)|Unauthorized response|Inline|

<h3 id="individualpayoutrequest-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» amount|number|false|none|none|
|» authorization_key|string|false|none|none|
|» beneficiary|object|false|none|none|
|»» account|string|false|none|none|
|»» bank|string|false|none|none|
|»» branch|string|false|none|none|
|» creation|string|false|none|none|
|» currency|string|false|none|none|
|» discounts|number|false|none|none|
|» id|number|false|none|none|
|» message|string|false|none|none|
|» serviceFee|number|false|none|none|
|» situation|string|false|none|none|
|» status|number|false|none|none|
|» total|number|false|none|none|
|» type|string|false|none|none|
|» uuid|string|false|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|situation|PENDING|
|situation|CANCELLED|
|situation|PAID|

Status Code **400**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» errorCount|number|false|none|none|
|» errors|[object]|false|none|none|
|»» field|string|false|none|none|
|»» message|string|false|none|none|
|» message|string|false|none|none|
|» status|number|false|none|none|

Status Code **403**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» errorCount|number|false|none|none|
|» errors|[object]|false|none|none|
|»» error|string|false|none|none|
|»» message|string|false|none|none|
|» message|string|false|none|none|
|» status|number|false|none|none|

### Response Headers

|Status|Header|Type|Format|Description|
|---|---|---|---|---|
|200|Date|string||none|
|200|Transfer-Encoding|string||none|
|400|Connection|string||none|
|400|Date|string||none|
|400|Transfer-Encoding|string||none|
|403|Date|string||none|
|403|Transfer-Encoding|string||none|

<aside class="success">
This operation does not require authentication
</aside>

## cancelAPayout

<a id="opIdcancelAPayout"></a>

> Code samples

```shell
# You can also use wget
curl -X DELETE https://api.paymee.com.br/v1.1/payout \
  -H 'Content-Type: application/json' \
  -H 'Accept: application/json' \
  -H 'x-api-key: your-x-api-key' \
  -H 'x-api-token: your-x-api-token'

```

```http
DELETE https://api.paymee.com.br/v1.1/payout HTTP/1.1
Host: api.paymee.com.br
Content-Type: application/json
Accept: application/json
x-api-key: your-x-api-key
x-api-token: your-x-api-token

```

```javascript
const inputBody = '{
  "reason": "Customer requested cancellation",
  "uuid": "83abc908-9233-4b66-b455-2339809886b1"
}';
const headers = {
  'Content-Type':'application/json',
  'Accept':'application/json',
  'x-api-key':'your-x-api-key',
  'x-api-token':'your-x-api-token'
};

fetch('https://api.paymee.com.br/v1.1/payout',
{
  method: 'DELETE',
  body: inputBody,
  headers: headers
})
.then(function(res) {
    return res.json();
}).then(function(body) {
    console.log(body);
});

```

```ruby
require 'rest-client'
require 'json'

headers = {
  'Content-Type' => 'application/json',
  'Accept' => 'application/json',
  'x-api-key' => 'your-x-api-key',
  'x-api-token' => 'your-x-api-token'
}

result = RestClient.delete 'https://api.paymee.com.br/v1.1/payout',
  params: {
  }, headers: headers

p JSON.parse(result)

```

```python
import requests
headers = {
  'Content-Type': 'application/json',
  'Accept': 'application/json',
  'x-api-key': 'your-x-api-key',
  'x-api-token': 'your-x-api-token'
}

r = requests.delete('https://api.paymee.com.br/v1.1/payout', headers = headers)

print(r.json())

```

```php
<?php

require 'vendor/autoload.php';

$headers = array(
    'Content-Type' => 'application/json',
    'Accept' => 'application/json',
    'x-api-key' => 'your-x-api-key',
    'x-api-token' => 'your-x-api-token',
);

$client = new \GuzzleHttp\Client();

// Define array of request body.
$request_body = array();

try {
    $response = $client->request('DELETE','https://api.paymee.com.br/v1.1/payout', array(
        'headers' => $headers,
        'json' => $request_body,
       )
    );
    print_r($response->getBody()->getContents());
 }
 catch (\GuzzleHttp\Exception\BadResponseException $e) {
    // handle exception or api errors.
    print_r($e->getMessage());
 }

 // ...

```

```java
URL obj = new URL("https://api.paymee.com.br/v1.1/payout");
HttpURLConnection con = (HttpURLConnection) obj.openConnection();
con.setRequestMethod("DELETE");
int responseCode = con.getResponseCode();
BufferedReader in = new BufferedReader(
    new InputStreamReader(con.getInputStream()));
String inputLine;
StringBuffer response = new StringBuffer();
while ((inputLine = in.readLine()) != null) {
    response.append(inputLine);
}
in.close();
System.out.println(response.toString());

```

```go
package main

import (
       "bytes"
       "net/http"
)

func main() {

    headers := map[string][]string{
        "Content-Type": []string{"application/json"},
        "Accept": []string{"application/json"},
        "x-api-key": []string{"your-x-api-key"},
        "x-api-token": []string{"your-x-api-token"},
    }

    data := bytes.NewBuffer([]byte{jsonReq})
    req, err := http.NewRequest("DELETE", "https://api.paymee.com.br/v1.1/payout", data)
    req.Header = headers

    client := &http.Client{}
    resp, err := client.Do(req)
    // ...
}

```

`DELETE /v1.1/payout`

*Cancel a payout*

Before we start, there are some important notes:
 1. the transactions should have **PENDING** status and not marked as **IN PROCESSING**

> Body parameter

```json
{
  "reason": "Customer requested cancellation",
  "uuid": "83abc908-9233-4b66-b455-2339809886b1"
}
```

<h3 id="cancelapayout-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|x-api-key|header|string|true|none|
|x-api-token|header|string|true|none|
|body|body|object|true|none|
|» uuid|body|string(uuid)|true|Payout's UUID to be canceled|
|» reason|body|string|false|Cancel reason (optional)|

> Example responses

> Payout successfully canceled

```json
{
  "status": 0,
  "message": "success"
}
```

> Invalid request or payout not found

```json
{
  "status": -1,
  "message": "field validation failure",
  "errorCount": 1,
  "errors": [
    {
      "field": "uuid",
      "message": "payout with UUID 83abc908-9233-4b66-b455-2339809886b1 not found"
    }
  ]
}
```

```json
{
  "status": -1,
  "message": "field validation failure",
  "errorCount": 1,
  "errors": [
    {
      "field": "uuid",
      "message": "payout is already in processing/not pending"
    }
  ]
}
```

> Unauthorized access

```json
{
  "status": -1,
  "message": "field validation failure",
  "errorCount": 1,
  "errors": [
    {
      "error": "Forbidden",
      "message": "Access Denied"
    }
  ]
}
```

<h3 id="cancelapayout-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|Payout successfully canceled|Inline|
|400|[Bad Request](https://tools.ietf.org/html/rfc7231#section-6.5.1)|Invalid request or payout not found|Inline|
|403|[Forbidden](https://tools.ietf.org/html/rfc7231#section-6.5.3)|Unauthorized access|Inline|

<h3 id="cancelapayout-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» status|number|false|none|Response status code|
|» message|string|false|none|Response status message|

Status Code **400**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» status|number|false|none|none|
|» message|string|false|none|none|
|» errorCount|number|false|none|none|
|» errors|[object]|false|none|none|
|»» field|string|false|none|none|
|»» message|string|false|none|none|

Status Code **403**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» status|number|false|none|none|
|» message|string|false|none|none|
|» errorCount|number|false|none|none|
|» errors|[object]|false|none|none|
|»» error|string|false|none|none|
|»» message|string|false|none|none|

<aside class="success">
This operation does not require authentication
</aside>

## authorizePayout

<a id="opIdauthorizePayout"></a>

> Code samples

```shell
# You can also use wget
curl -X POST https://api.paymee.com.br/v1.1/payout/authorize/{tid}/{authorizationCode} \
  -H 'Accept: application/json' \
  -H 'x-api-key: your-x-api-key' \
  -H 'x-api-token: your-x-api-token'

```

```http
POST https://api.paymee.com.br/v1.1/payout/authorize/{tid}/{authorizationCode} HTTP/1.1
Host: api.paymee.com.br
Accept: application/json
x-api-key: your-x-api-key
x-api-token: your-x-api-token

```

```javascript

const headers = {
  'Accept':'application/json',
  'x-api-key':'your-x-api-key',
  'x-api-token':'your-x-api-token'
};

fetch('https://api.paymee.com.br/v1.1/payout/authorize/{tid}/{authorizationCode}',
{
  method: 'POST',

  headers: headers
})
.then(function(res) {
    return res.json();
}).then(function(body) {
    console.log(body);
});

```

```ruby
require 'rest-client'
require 'json'

headers = {
  'Accept' => 'application/json',
  'x-api-key' => 'your-x-api-key',
  'x-api-token' => 'your-x-api-token'
}

result = RestClient.post 'https://api.paymee.com.br/v1.1/payout/authorize/{tid}/{authorizationCode}',
  params: {
  }, headers: headers

p JSON.parse(result)

```

```python
import requests
headers = {
  'Accept': 'application/json',
  'x-api-key': 'your-x-api-key',
  'x-api-token': 'your-x-api-token'
}

r = requests.post('https://api.paymee.com.br/v1.1/payout/authorize/{tid}/{authorizationCode}', headers = headers)

print(r.json())

```

```php
<?php

require 'vendor/autoload.php';

$headers = array(
    'Accept' => 'application/json',
    'x-api-key' => 'your-x-api-key',
    'x-api-token' => 'your-x-api-token',
);

$client = new \GuzzleHttp\Client();

// Define array of request body.
$request_body = array();

try {
    $response = $client->request('POST','https://api.paymee.com.br/v1.1/payout/authorize/{tid}/{authorizationCode}', array(
        'headers' => $headers,
        'json' => $request_body,
       )
    );
    print_r($response->getBody()->getContents());
 }
 catch (\GuzzleHttp\Exception\BadResponseException $e) {
    // handle exception or api errors.
    print_r($e->getMessage());
 }

 // ...

```

```java
URL obj = new URL("https://api.paymee.com.br/v1.1/payout/authorize/{tid}/{authorizationCode}");
HttpURLConnection con = (HttpURLConnection) obj.openConnection();
con.setRequestMethod("POST");
int responseCode = con.getResponseCode();
BufferedReader in = new BufferedReader(
    new InputStreamReader(con.getInputStream()));
String inputLine;
StringBuffer response = new StringBuffer();
while ((inputLine = in.readLine()) != null) {
    response.append(inputLine);
}
in.close();
System.out.println(response.toString());

```

```go
package main

import (
       "bytes"
       "net/http"
)

func main() {

    headers := map[string][]string{
        "Accept": []string{"application/json"},
        "x-api-key": []string{"your-x-api-key"},
        "x-api-token": []string{"your-x-api-token"},
    }

    data := bytes.NewBuffer([]byte{jsonReq})
    req, err := http.NewRequest("POST", "https://api.paymee.com.br/v1.1/payout/authorize/{tid}/{authorizationCode}", data)
    req.Header = headers

    client := &http.Client{}
    resp, err := client.Do(req)
    // ...
}

```

`POST /v1.1/payout/authorize/{tid}/{authorizationCode}`

*Authorize payout*

Authorize Payout

<h3 id="authorizepayout-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|x-api-key|header|string|true|none|
|x-api-token|header|string|true|none|
|tid|path|string|true|none|
|authorizationCode|path|string|true|none|

> Example responses

> Success

```json
{
  "message": "success",
  "status": 0,
  "tid": 49916,
  "uuid": "1e637332-b2dd-3d3a-b76c-03e5bcb1dd0a",
  "response": {
    "response": {
      "endToEndId": "E710278662021052700421198558814A",
      "pagador": {
        "conta": {
          "agencia": "0000",
          "banco": "000",
          "bancoNome": "PAYMEE BRASIL",
          "numero": "000000-0",
          "tipo": "ContaCorrente"
        },
        "ispb": "00000000",
        "pessoa": {
          "documento": "00000000000",
          "nome": "JOHN DOE",
          "nomeFantasia": null,
          "tipoDocumento": "CPF"
        }
      },
      "pagamentoId": "2d18f284-19fc-4434-887d-765b6b1a44ec",
      "recebedor": {
        "conta": {
          "agencia": "0000",
          "banco": "000",
          "bancoNome": "PAYMEE BRASIL",
          "numero": "00000-5",
          "tipo": "ContaCorrente"
        },
        "ispb": "00000000",
        "pessoa": {
          "documento": "28683892000191",
          "nome": "PAYMEE BRASIL SERVICOS DE PAGAMENTOS S.A",
          "nomeFantasia": "PAYMEE",
          "tipoDocumento": "CNPJ"
        }
      }
    }
  }
}
```

> Authorize Payout - fail

```json
{
  "errorCount": 1,
  "errors": [
    {
      "field": "token",
      "message": "invalid authorization token"
    }
  ],
  "message": "field validation failure",
  "status": -1
}
```

> Unauthorized access

```json
{
  "errorCount": 1,
  "errors": [
    {
      "error": "Forbidden",
      "message": "Access Denied"
    }
  ],
  "message": "field validation failure",
  "status": -1
}
```

<h3 id="authorizepayout-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|Success|Inline|
|400|[Bad Request](https://tools.ietf.org/html/rfc7231#section-6.5.1)|Authorize Payout - fail|Inline|
|403|[Forbidden](https://tools.ietf.org/html/rfc7231#section-6.5.3)|Unauthorized access|Inline|

<h3 id="authorizepayout-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» message|string|false|none|none|
|» response|object|false|none|none|
|»» response|object|false|none|none|
|»»» endToEndId|string|false|none|none|
|»»» pagador|object|false|none|none|
|»»»» conta|object|false|none|none|
|»»»»» agencia|string|false|none|none|
|»»»»» banco|string|false|none|none|
|»»»»» bancoNome|string|false|none|none|
|»»»»» numero|string|false|none|none|
|»»»»» tipo|string|false|none|none|
|»»»» ispb|string|false|none|none|
|»»»» pessoa|object|false|none|none|
|»»»»» documento|string|false|none|none|
|»»»»» nome|string|false|none|none|
|»»»»» nomeFantasia|any|false|none|none|
|»»»»» tipoDocumento|string|false|none|none|
|»»» pagamentoId|string|false|none|none|
|»»» recebedor|object|false|none|none|
|»»»» conta|object|false|none|none|
|»»»»» agencia|string|false|none|none|
|»»»»» banco|string|false|none|none|
|»»»»» bancoNome|string|false|none|none|
|»»»»» numero|string|false|none|none|
|»»»»» tipo|string|false|none|none|
|»»»» ispb|string|false|none|none|
|»»»» pessoa|object|false|none|none|
|»»»»» documento|string|false|none|none|
|»»»»» nome|string|false|none|none|
|»»»»» nomeFantasia|any|false|none|none|
|»»»»» tipoDocumento|string|false|none|none|
|»» tid|number|false|none|none|
|» status|number|false|none|none|
|» tid|number|false|none|none|
|» uuid|string|false|none|none|

Status Code **400**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» errorCount|number|false|none|none|
|» errors|[object]|false|none|none|
|»» field|string|false|none|none|
|»» message|string|false|none|none|
|» message|string|false|none|none|
|» status|number|false|none|none|

Status Code **403**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» errorCount|number|false|none|none|
|» errors|[object]|false|none|none|
|»» error|string|false|none|none|
|»» message|string|false|none|none|
|» message|string|false|none|none|
|» status|number|false|none|none|

### Response Headers

|Status|Header|Type|Format|Description|
|---|---|---|---|---|
|200|Date|string||none|
|200|Transfer-Encoding|string||none|
|400|Connection|string||none|
|400|Date|string||none|
|400|Transfer-Encoding|string||none|

<aside class="success">
This operation does not require authentication
</aside>

<h1 id="api-paymee-1-1-pix-automatic">Pix Automatic</h1>

## createPixAutomaticPlan

<a id="opIdcreatePixAutomaticPlan"></a>

> Code samples

```shell
# You can also use wget
curl -X POST https://api.paymee.com.br/v1.1/pix-automatic/plans \
  -H 'Content-Type: application/json' \
  -H 'Accept: application/json' \
  -H 'x-api-key: your-x-api-key' \
  -H 'x-api-token: your-x-api-token'

```

```http
POST https://api.paymee.com.br/v1.1/pix-automatic/plans HTTP/1.1
Host: api.paymee.com.br
Content-Type: application/json
Accept: application/json
x-api-key: your-x-api-key
x-api-token: your-x-api-token

```

```javascript
const inputBody = '{
  "title": "Plano Premium Mensal",
  "description": "Acesso completo a todas as funcionalidades da plataforma.",
  "frequency": "MONTHLY",
  "period": 1,
  "imediateAmount": 49.9,
  "recurrenceAmount": 29.9,
  "minimumRecurrenceAmount": 29.9,
  "retryPolicy": "ACCEPT_3R_7D",
  "enabled": true
}';
const headers = {
  'Content-Type':'application/json',
  'Accept':'application/json',
  'x-api-key':'your-x-api-key',
  'x-api-token':'your-x-api-token'
};

fetch('https://api.paymee.com.br/v1.1/pix-automatic/plans',
{
  method: 'POST',
  body: inputBody,
  headers: headers
})
.then(function(res) {
    return res.json();
}).then(function(body) {
    console.log(body);
});

```

```ruby
require 'rest-client'
require 'json'

headers = {
  'Content-Type' => 'application/json',
  'Accept' => 'application/json',
  'x-api-key' => 'your-x-api-key',
  'x-api-token' => 'your-x-api-token'
}

result = RestClient.post 'https://api.paymee.com.br/v1.1/pix-automatic/plans',
  params: {
  }, headers: headers

p JSON.parse(result)

```

```python
import requests
headers = {
  'Content-Type': 'application/json',
  'Accept': 'application/json',
  'x-api-key': 'your-x-api-key',
  'x-api-token': 'your-x-api-token'
}

r = requests.post('https://api.paymee.com.br/v1.1/pix-automatic/plans', headers = headers)

print(r.json())

```

```php
<?php

require 'vendor/autoload.php';

$headers = array(
    'Content-Type' => 'application/json',
    'Accept' => 'application/json',
    'x-api-key' => 'your-x-api-key',
    'x-api-token' => 'your-x-api-token',
);

$client = new \GuzzleHttp\Client();

// Define array of request body.
$request_body = array();

try {
    $response = $client->request('POST','https://api.paymee.com.br/v1.1/pix-automatic/plans', array(
        'headers' => $headers,
        'json' => $request_body,
       )
    );
    print_r($response->getBody()->getContents());
 }
 catch (\GuzzleHttp\Exception\BadResponseException $e) {
    // handle exception or api errors.
    print_r($e->getMessage());
 }

 // ...

```

```java
URL obj = new URL("https://api.paymee.com.br/v1.1/pix-automatic/plans");
HttpURLConnection con = (HttpURLConnection) obj.openConnection();
con.setRequestMethod("POST");
int responseCode = con.getResponseCode();
BufferedReader in = new BufferedReader(
    new InputStreamReader(con.getInputStream()));
String inputLine;
StringBuffer response = new StringBuffer();
while ((inputLine = in.readLine()) != null) {
    response.append(inputLine);
}
in.close();
System.out.println(response.toString());

```

```go
package main

import (
       "bytes"
       "net/http"
)

func main() {

    headers := map[string][]string{
        "Content-Type": []string{"application/json"},
        "Accept": []string{"application/json"},
        "x-api-key": []string{"your-x-api-key"},
        "x-api-token": []string{"your-x-api-token"},
    }

    data := bytes.NewBuffer([]byte{jsonReq})
    req, err := http.NewRequest("POST", "https://api.paymee.com.br/v1.1/pix-automatic/plans", data)
    req.Header = headers

    client := &http.Client{}
    resp, err := client.Do(req)
    // ...
}

```

`POST /v1.1/pix-automatic/plans`

*Create a recurrence plan*

Creates a new recurrence plan for Pix Automatic.

> Body parameter

```json
{
  "title": "Plano Premium Mensal",
  "description": "Acesso completo a todas as funcionalidades da plataforma.",
  "frequency": "MONTHLY",
  "period": 1,
  "imediateAmount": 49.9,
  "recurrenceAmount": 29.9,
  "minimumRecurrenceAmount": 29.9,
  "retryPolicy": "ACCEPT_3R_7D",
  "enabled": true
}
```

<h3 id="createpixautomaticplan-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|x-api-key|header|string|true|none|
|x-api-token|header|string|true|none|
|body|body|object|true|none|
|» title|body|string|false|Title of the plan.|
|» description|body|string|false|Detailed description of the plan.|
|» frequency|body|string|false|Billing frequency.|
|» period|body|integer|false|Period of the frequency (e.g., 1 for monthly, 3 for quarterly).|
|» imediateAmount|body|number(float)|false|Immediate charge amount upon joining the plan.|
|» recurrenceAmount|body|number(float)|false|Amount for recurring charges.|
|» minimumRecurrenceAmount|body|number(float)|false|Minimum amount accepted for recurring charges.|
|» retryPolicy|body|string|false|Retry policy in case of failure.|
|» enabled|body|boolean|false|Indicates if the plan is active.|

#### Enumerated Values

|Parameter|Value|
|---|---|
|» retryPolicy|ACCEPT_3R_7D|
|» retryPolicy|NOT_ACCEPT|

> Example responses

> Plan created successfully.

```json
{
  "status": 0,
  "message": "success",
  "id": "6851c0842bf40769ead1fde5",
  "title": "Plano Premium Mensal",
  "description": "Acesso completo a todas as funcionalidades da plataforma.",
  "frequency": "MONTHLY",
  "period": 1,
  "imediateAmount": 49.9,
  "recurrenceAmount": 29.9,
  "minimumRecurrenceAmount": 29.9,
  "retryPolicy": "ACCEPT_3R_7D",
  "enabled": true,
  "creation": "2025-06-17 19:22:44"
}
```

> 400 Response

```json
{
  "errorCount": 1,
  "errors": [
    {
      "field": "title",
      "message": "may not be empty"
    }
  ],
  "message": "field validation failure",
  "status": -1
}
```

<h3 id="createpixautomaticplan-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|201|[Created](https://tools.ietf.org/html/rfc7231#section-6.3.2)|Plan created successfully.|Inline|
|400|[Bad Request](https://tools.ietf.org/html/rfc7231#section-6.5.1)|validation failure response|Inline|
|403|[Forbidden](https://tools.ietf.org/html/rfc7231#section-6.5.3)|Unauthorized response|Inline|

<h3 id="createpixautomaticplan-responseschema">Response Schema</h3>

Status Code **201**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» status|integer|false|none|none|
|» message|string|false|none|none|
|» id|string|false|none|Unique ID of the generated plan.|
|» title|string|false|none|none|
|» description|string|false|none|none|
|» frequency|string|false|none|none|
|» period|integer|false|none|none|
|» imediateAmount|number|false|none|none|
|» recurrenceAmount|number|false|none|none|
|» minimumRecurrenceAmount|number|false|none|none|
|» retryPolicy|string|false|none|none|
|» enabled|boolean|false|none|none|
|» creation|string(date-time)|false|none|Date and time of plan creation.|

Status Code **400**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» errorCount|number|false|none|none|
|» errors|[object]|false|none|none|
|»» field|string|false|none|none|
|»» message|string|false|none|none|
|» message|string|false|none|none|
|» status|number|false|none|none|

Status Code **403**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» errorCount|number|false|none|none|
|» errors|[object]|false|none|none|
|»» error|string|false|none|none|
|»» message|string|false|none|none|
|» message|string|false|none|none|
|» status|number|false|none|none|

<aside class="success">
This operation does not require authentication
</aside>

## listPixAutomaticPlans

<a id="opIdlistPixAutomaticPlans"></a>

> Code samples

```shell
# You can also use wget
curl -X GET https://api.paymee.com.br/v1.1/pix-automatic/plans \
  -H 'Accept: application/json' \
  -H 'x-api-key: your-x-api-key' \
  -H 'x-api-token: your-x-api-token'

```

```http
GET https://api.paymee.com.br/v1.1/pix-automatic/plans HTTP/1.1
Host: api.paymee.com.br
Accept: application/json
x-api-key: your-x-api-key
x-api-token: your-x-api-token

```

```javascript

const headers = {
  'Accept':'application/json',
  'x-api-key':'your-x-api-key',
  'x-api-token':'your-x-api-token'
};

fetch('https://api.paymee.com.br/v1.1/pix-automatic/plans',
{
  method: 'GET',

  headers: headers
})
.then(function(res) {
    return res.json();
}).then(function(body) {
    console.log(body);
});

```

```ruby
require 'rest-client'
require 'json'

headers = {
  'Accept' => 'application/json',
  'x-api-key' => 'your-x-api-key',
  'x-api-token' => 'your-x-api-token'
}

result = RestClient.get 'https://api.paymee.com.br/v1.1/pix-automatic/plans',
  params: {
  }, headers: headers

p JSON.parse(result)

```

```python
import requests
headers = {
  'Accept': 'application/json',
  'x-api-key': 'your-x-api-key',
  'x-api-token': 'your-x-api-token'
}

r = requests.get('https://api.paymee.com.br/v1.1/pix-automatic/plans', headers = headers)

print(r.json())

```

```php
<?php

require 'vendor/autoload.php';

$headers = array(
    'Accept' => 'application/json',
    'x-api-key' => 'your-x-api-key',
    'x-api-token' => 'your-x-api-token',
);

$client = new \GuzzleHttp\Client();

// Define array of request body.
$request_body = array();

try {
    $response = $client->request('GET','https://api.paymee.com.br/v1.1/pix-automatic/plans', array(
        'headers' => $headers,
        'json' => $request_body,
       )
    );
    print_r($response->getBody()->getContents());
 }
 catch (\GuzzleHttp\Exception\BadResponseException $e) {
    // handle exception or api errors.
    print_r($e->getMessage());
 }

 // ...

```

```java
URL obj = new URL("https://api.paymee.com.br/v1.1/pix-automatic/plans");
HttpURLConnection con = (HttpURLConnection) obj.openConnection();
con.setRequestMethod("GET");
int responseCode = con.getResponseCode();
BufferedReader in = new BufferedReader(
    new InputStreamReader(con.getInputStream()));
String inputLine;
StringBuffer response = new StringBuffer();
while ((inputLine = in.readLine()) != null) {
    response.append(inputLine);
}
in.close();
System.out.println(response.toString());

```

```go
package main

import (
       "bytes"
       "net/http"
)

func main() {

    headers := map[string][]string{
        "Accept": []string{"application/json"},
        "x-api-key": []string{"your-x-api-key"},
        "x-api-token": []string{"your-x-api-token"},
    }

    data := bytes.NewBuffer([]byte{jsonReq})
    req, err := http.NewRequest("GET", "https://api.paymee.com.br/v1.1/pix-automatic/plans", data)
    req.Header = headers

    client := &http.Client{}
    resp, err := client.Do(req)
    // ...
}

```

`GET /v1.1/pix-automatic/plans`

*List recurrence plans*

Lists Pix Automatic plans based on provided filters.

<h3 id="listpixautomaticplans-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|x-api-key|header|string|true|none|
|x-api-token|header|string|true|none|
|title|query|string|false|Filter by plan title.|
|enabled|query|boolean|false|Filter by plan status (active/inactive).|
|frequency|query|string|false|Filter by frequency.|
|page|query|integer|false|Page number for the query.|
|maxPageResults|query|integer|false|Maximum number of results per page.|

> Example responses

> List of plans returned successfully.

```json
{
  "status": 0,
  "message": "success",
  "totalElements": 1,
  "totalPages": 1,
  "resultSize": 1,
  "result": [
    {
      "id": "6851c0842bf40769ead1fde5",
      "title": "Plano Premium Mensal",
      "description": "Acesso completo a todas as funcionalidades da plataforma.",
      "frequency": "MONTHLY",
      "period": 1,
      "imediateAmount": 49.9,
      "recurrenceAmount": 29.9,
      "minimumRecurrenceAmount": 29.9,
      "retryPolicy": "ACCEPT_3R_7D",
      "enabled": true,
      "creation": "2025-06-17 19:22:44",
      "totalAuthorizations": 0
    }
  ]
}
```

> 403 Response

```json
{
  "errorCount": 1,
  "errors": [
    {
      "error": "Forbidden",
      "message": "Access Denied"
    }
  ],
  "message": "field validation failure",
  "status": -1
}
```

<h3 id="listpixautomaticplans-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|List of plans returned successfully.|Inline|
|403|[Forbidden](https://tools.ietf.org/html/rfc7231#section-6.5.3)|Unauthorized response|Inline|

<h3 id="listpixautomaticplans-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» status|integer|false|none|none|
|» message|string|false|none|none|
|» totalElements|integer|false|none|Total number of plans found.|
|» totalPages|integer|false|none|Total number of pages.|
|» resultSize|integer|false|none|Number of plans returned in the current page.|
|» result|[object]|false|none|none|
|»» id|string|false|none|none|
|»» title|string|false|none|none|
|»» description|string|false|none|none|
|»» frequency|string|false|none|none|
|»» period|integer|false|none|none|
|»» imediateAmount|number|false|none|none|
|»» recurrenceAmount|number|false|none|none|
|»» minimumRecurrenceAmount|number|false|none|none|
|»» retryPolicy|string|false|none|none|
|»» enabled|boolean|false|none|none|
|»» creation|string(date-time)|false|none|none|
|»» totalAuthorizations|integer|false|none|none|

Status Code **403**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» errorCount|number|false|none|none|
|» errors|[object]|false|none|none|
|»» error|string|false|none|none|
|»» message|string|false|none|none|
|» message|string|false|none|none|
|» status|number|false|none|none|

<aside class="success">
This operation does not require authentication
</aside>

## getPixAutomaticPlanById

<a id="opIdgetPixAutomaticPlanById"></a>

> Code samples

```shell
# You can also use wget
curl -X GET https://api.paymee.com.br/v1.1/pix-automatic/plans/{planId} \
  -H 'Accept: application/json' \
  -H 'x-api-key: your-x-api-key' \
  -H 'x-api-token: your-x-api-token'

```

```http
GET https://api.paymee.com.br/v1.1/pix-automatic/plans/{planId} HTTP/1.1
Host: api.paymee.com.br
Accept: application/json
x-api-key: your-x-api-key
x-api-token: your-x-api-token

```

```javascript

const headers = {
  'Accept':'application/json',
  'x-api-key':'your-x-api-key',
  'x-api-token':'your-x-api-token'
};

fetch('https://api.paymee.com.br/v1.1/pix-automatic/plans/{planId}',
{
  method: 'GET',

  headers: headers
})
.then(function(res) {
    return res.json();
}).then(function(body) {
    console.log(body);
});

```

```ruby
require 'rest-client'
require 'json'

headers = {
  'Accept' => 'application/json',
  'x-api-key' => 'your-x-api-key',
  'x-api-token' => 'your-x-api-token'
}

result = RestClient.get 'https://api.paymee.com.br/v1.1/pix-automatic/plans/{planId}',
  params: {
  }, headers: headers

p JSON.parse(result)

```

```python
import requests
headers = {
  'Accept': 'application/json',
  'x-api-key': 'your-x-api-key',
  'x-api-token': 'your-x-api-token'
}

r = requests.get('https://api.paymee.com.br/v1.1/pix-automatic/plans/{planId}', headers = headers)

print(r.json())

```

```php
<?php

require 'vendor/autoload.php';

$headers = array(
    'Accept' => 'application/json',
    'x-api-key' => 'your-x-api-key',
    'x-api-token' => 'your-x-api-token',
);

$client = new \GuzzleHttp\Client();

// Define array of request body.
$request_body = array();

try {
    $response = $client->request('GET','https://api.paymee.com.br/v1.1/pix-automatic/plans/{planId}', array(
        'headers' => $headers,
        'json' => $request_body,
       )
    );
    print_r($response->getBody()->getContents());
 }
 catch (\GuzzleHttp\Exception\BadResponseException $e) {
    // handle exception or api errors.
    print_r($e->getMessage());
 }

 // ...

```

```java
URL obj = new URL("https://api.paymee.com.br/v1.1/pix-automatic/plans/{planId}");
HttpURLConnection con = (HttpURLConnection) obj.openConnection();
con.setRequestMethod("GET");
int responseCode = con.getResponseCode();
BufferedReader in = new BufferedReader(
    new InputStreamReader(con.getInputStream()));
String inputLine;
StringBuffer response = new StringBuffer();
while ((inputLine = in.readLine()) != null) {
    response.append(inputLine);
}
in.close();
System.out.println(response.toString());

```

```go
package main

import (
       "bytes"
       "net/http"
)

func main() {

    headers := map[string][]string{
        "Accept": []string{"application/json"},
        "x-api-key": []string{"your-x-api-key"},
        "x-api-token": []string{"your-x-api-token"},
    }

    data := bytes.NewBuffer([]byte{jsonReq})
    req, err := http.NewRequest("GET", "https://api.paymee.com.br/v1.1/pix-automatic/plans/{planId}", data)
    req.Header = headers

    client := &http.Client{}
    resp, err := client.Do(req)
    // ...
}

```

`GET /v1.1/pix-automatic/plans/{planId}`

*Get plan by ID*

Retrieves a specific plan by its ID.

<h3 id="getpixautomaticplanbyid-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|x-api-key|header|string|true|none|
|x-api-token|header|string|true|none|
|planId|path|string|true|ID of the plan to be retrieved.|

> Example responses

> Plan data returned successfully.

```json
{
  "status": 0,
  "message": "success",
  "id": "6851c0842bf40769ead1fde5",
  "title": "Plano Premium Mensal",
  "description": "Acesso completo a todas as funcionalidades da plataforma.",
  "frequency": "MONTHLY",
  "period": 1,
  "imediateAmount": 49.9,
  "recurrenceAmount": 29.9,
  "minimumRecurrenceAmount": 29.9,
  "retryPolicy": "ACCEPT_3R_7D",
  "enabled": true,
  "creation": "2025-06-17 19:22:44",
  "totalAuthorizations": 7,
  "authorizations": [
    {
      "id": 21,
      "idRec": "RR286838922025061602541904516",
      "status": "APPROVED",
      "payer": {
        "document": "00000000000",
        "name": "JOHN DOE",
        "ispb": "99999919"
      },
      "nextRecurrenceDate": "2025-07-19"
    },
    {
      "id": 25,
      "idRec": "RR286838922025061603533095545",
      "status": "CANCELLED",
      "payer": {
        "document": "00000000000",
        "name": "JOHN DOE",
        "ispb": "99999919"
      },
      "nextRecurrenceDate": "2025-07-19"
    }
  ]
}
```

> 403 Response

```json
{
  "errorCount": 1,
  "errors": [
    {
      "error": "Forbidden",
      "message": "Access Denied"
    }
  ],
  "message": "field validation failure",
  "status": -1
}
```

<h3 id="getpixautomaticplanbyid-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|Plan data returned successfully.|Inline|
|403|[Forbidden](https://tools.ietf.org/html/rfc7231#section-6.5.3)|Unauthorized response|Inline|
|404|[Not Found](https://tools.ietf.org/html/rfc7231#section-6.5.4)|Plan not found response|Inline|

<h3 id="getpixautomaticplanbyid-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» status|integer|false|none|none|
|» message|string|false|none|none|
|» id|string|false|none|none|
|» title|string|false|none|none|
|» description|string|false|none|none|
|» frequency|string|false|none|none|
|» period|integer|false|none|none|
|» imediateAmount|number|false|none|none|
|» recurrenceAmount|number|false|none|none|
|» minimumRecurrenceAmount|number|false|none|none|
|» retryPolicy|string|false|none|none|
|» enabled|boolean|false|none|none|
|» creation|string(date-time)|false|none|none|
|» totalAuthorizations|integer|false|none|none|
|» authorizations|[object]|false|none|none|
|»» id|integer|false|none|none|
|»» idRec|string|false|none|none|
|»» status|string|false|none|none|
|»» payer|object|false|none|none|
|»»» document|string|false|none|none|
|»»» name|string|false|none|none|
|»»» ispb|string|false|none|none|
|»» nextRecurrenceDate|string(date)|false|none|none|

Status Code **403**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» errorCount|number|false|none|none|
|» errors|[object]|false|none|none|
|»» error|string|false|none|none|
|»» message|string|false|none|none|
|» message|string|false|none|none|
|» status|number|false|none|none|

Status Code **404**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» status|number|false|none|none|
|» message|string|false|none|none|
|» errorCount|number|false|none|none|
|» errors|[object]|false|none|none|
|»» field|string|false|none|none|
|»» message|string|false|none|none|

<aside class="success">
This operation does not require authentication
</aside>

## listScheduledPayments

<a id="opIdlistScheduledPayments"></a>

> Code samples

```shell
# You can also use wget
curl -X GET https://api.paymee.com.br/v1.1/pix-automatic/scheduled-payments \
  -H 'Accept: application/json' \
  -H 'x-api-key: your-x-api-key' \
  -H 'x-api-token: your-x-api-token'

```

```http
GET https://api.paymee.com.br/v1.1/pix-automatic/scheduled-payments HTTP/1.1
Host: api.paymee.com.br
Accept: application/json
x-api-key: your-x-api-key
x-api-token: your-x-api-token

```

```javascript

const headers = {
  'Accept':'application/json',
  'x-api-key':'your-x-api-key',
  'x-api-token':'your-x-api-token'
};

fetch('https://api.paymee.com.br/v1.1/pix-automatic/scheduled-payments',
{
  method: 'GET',

  headers: headers
})
.then(function(res) {
    return res.json();
}).then(function(body) {
    console.log(body);
});

```

```ruby
require 'rest-client'
require 'json'

headers = {
  'Accept' => 'application/json',
  'x-api-key' => 'your-x-api-key',
  'x-api-token' => 'your-x-api-token'
}

result = RestClient.get 'https://api.paymee.com.br/v1.1/pix-automatic/scheduled-payments',
  params: {
  }, headers: headers

p JSON.parse(result)

```

```python
import requests
headers = {
  'Accept': 'application/json',
  'x-api-key': 'your-x-api-key',
  'x-api-token': 'your-x-api-token'
}

r = requests.get('https://api.paymee.com.br/v1.1/pix-automatic/scheduled-payments', headers = headers)

print(r.json())

```

```php
<?php

require 'vendor/autoload.php';

$headers = array(
    'Accept' => 'application/json',
    'x-api-key' => 'your-x-api-key',
    'x-api-token' => 'your-x-api-token',
);

$client = new \GuzzleHttp\Client();

// Define array of request body.
$request_body = array();

try {
    $response = $client->request('GET','https://api.paymee.com.br/v1.1/pix-automatic/scheduled-payments', array(
        'headers' => $headers,
        'json' => $request_body,
       )
    );
    print_r($response->getBody()->getContents());
 }
 catch (\GuzzleHttp\Exception\BadResponseException $e) {
    // handle exception or api errors.
    print_r($e->getMessage());
 }

 // ...

```

```java
URL obj = new URL("https://api.paymee.com.br/v1.1/pix-automatic/scheduled-payments");
HttpURLConnection con = (HttpURLConnection) obj.openConnection();
con.setRequestMethod("GET");
int responseCode = con.getResponseCode();
BufferedReader in = new BufferedReader(
    new InputStreamReader(con.getInputStream()));
String inputLine;
StringBuffer response = new StringBuffer();
while ((inputLine = in.readLine()) != null) {
    response.append(inputLine);
}
in.close();
System.out.println(response.toString());

```

```go
package main

import (
       "bytes"
       "net/http"
)

func main() {

    headers := map[string][]string{
        "Accept": []string{"application/json"},
        "x-api-key": []string{"your-x-api-key"},
        "x-api-token": []string{"your-x-api-token"},
    }

    data := bytes.NewBuffer([]byte{jsonReq})
    req, err := http.NewRequest("GET", "https://api.paymee.com.br/v1.1/pix-automatic/scheduled-payments", data)
    req.Header = headers

    client := &http.Client{}
    resp, err := client.Do(req)
    // ...
}

```

`GET /v1.1/pix-automatic/scheduled-payments`

*List scheduled payments*

Lists scheduled payments (payment instructions) based on filters.

<h3 id="listscheduledpayments-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|x-api-key|header|string|true|none|
|x-api-token|header|string|true|none|
|status|query|string|false|Filter by payment status.|
|initialDueDate|query|string(date)|false|Filter by start due date.|
|finalDueDate|query|string(date)|false|Filter by end due date.|
|page|query|integer|false|Page number for the query.|
|maxPageResults|query|integer|false|Maximum number of results per page.|
|sortBy|query|string|false|Field to sort the results by.|
|order|query|string|false|Sort order.|

#### Enumerated Values

|Parameter|Value|
|---|---|
|status|PENDING_SCHEDULE|
|status|SCHEDULED|
|status|PAID|
|status|FAILED|
|status|CANCELLED|
|sortBy|dueDate|
|sortBy|creation|
|order|ASC|
|order|DESC|

> Example responses

> List of scheduled payments returned successfully.

```json
{
  "status": 0,
  "message": "success",
  "totalElements": 1,
  "totalPages": 1,
  "resultSize": 1,
  "results": [
    {
      "id": 12,
      "instructionId": "000000000000000000000215832344",
      "status": "CANCELLED",
      "dueDate": "2025-06-19 03:00:00",
      "amount": 11,
      "endToEnd": "E2868389220250619150000215832384",
      "rejectionCode": "SLCR",
      "rejectionReason": "1",
      "creation": "2025-06-16 07:16:45",
      "authorization": {
        "id": 26,
        "idRec": "RR286838922025061604150842631",
        "initialDate": "2025-06-19",
        "finalDate": "2025-12-15",
        "creation": "2025-06-16 07:16:42"
      },
      "sale": {
        "id": 215832384,
        "uuid": "16a5e040-8b55-4dbe-8563-035d2876a766",
        "situation": "CANCELLED"
      }
    }
  ]
}
```

> 403 Response

```json
{
  "errorCount": 1,
  "errors": [
    {
      "error": "Forbidden",
      "message": "Access Denied"
    }
  ],
  "message": "field validation failure",
  "status": -1
}
```

<h3 id="listscheduledpayments-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|List of scheduled payments returned successfully.|Inline|
|403|[Forbidden](https://tools.ietf.org/html/rfc7231#section-6.5.3)|Unauthorized response|Inline|

<h3 id="listscheduledpayments-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» status|integer|false|none|none|
|» message|string|false|none|none|
|» totalElements|integer|false|none|none|
|» totalPages|integer|false|none|none|
|» resultSize|integer|false|none|none|
|» results|[object]|false|none|none|
|»» id|integer|false|none|none|
|»» instructionId|string|false|none|none|
|»» status|string|false|none|none|
|»» dueDate|string(date-time)|false|none|none|
|»» amount|number|false|none|none|
|»» endToEnd|string|false|none|none|
|»» rejectionCode|string|false|none|none|
|»» rejectionReason|string|false|none|none|
|»» creation|string(date-time)|false|none|none|
|»» authorization|object|false|none|none|
|»»» id|integer|false|none|none|
|»»» idRec|string|false|none|none|
|»»» initialDate|string(date)|false|none|none|
|»»» finalDate|string(date)|false|none|none|
|»»» creation|string(date-time)|false|none|none|
|»» sale|object|false|none|none|
|»»» id|integer|false|none|none|
|»»» uuid|string(uuid)|false|none|none|
|»»» situation|string|false|none|none|

Status Code **403**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» errorCount|number|false|none|none|
|» errors|[object]|false|none|none|
|»» error|string|false|none|none|
|»» message|string|false|none|none|
|» message|string|false|none|none|
|» status|number|false|none|none|

<aside class="success">
This operation does not require authentication
</aside>

## getScheduledPaymentById

<a id="opIdgetScheduledPaymentById"></a>

> Code samples

```shell
# You can also use wget
curl -X GET https://api.paymee.com.br/v1.1/pix-automatic/scheduled-payments/{instructionId} \
  -H 'Accept: application/json' \
  -H 'x-api-key: your-x-api-key' \
  -H 'x-api-token: your-x-api-token'

```

```http
GET https://api.paymee.com.br/v1.1/pix-automatic/scheduled-payments/{instructionId} HTTP/1.1
Host: api.paymee.com.br
Accept: application/json
x-api-key: your-x-api-key
x-api-token: your-x-api-token

```

```javascript

const headers = {
  'Accept':'application/json',
  'x-api-key':'your-x-api-key',
  'x-api-token':'your-x-api-token'
};

fetch('https://api.paymee.com.br/v1.1/pix-automatic/scheduled-payments/{instructionId}',
{
  method: 'GET',

  headers: headers
})
.then(function(res) {
    return res.json();
}).then(function(body) {
    console.log(body);
});

```

```ruby
require 'rest-client'
require 'json'

headers = {
  'Accept' => 'application/json',
  'x-api-key' => 'your-x-api-key',
  'x-api-token' => 'your-x-api-token'
}

result = RestClient.get 'https://api.paymee.com.br/v1.1/pix-automatic/scheduled-payments/{instructionId}',
  params: {
  }, headers: headers

p JSON.parse(result)

```

```python
import requests
headers = {
  'Accept': 'application/json',
  'x-api-key': 'your-x-api-key',
  'x-api-token': 'your-x-api-token'
}

r = requests.get('https://api.paymee.com.br/v1.1/pix-automatic/scheduled-payments/{instructionId}', headers = headers)

print(r.json())

```

```php
<?php

require 'vendor/autoload.php';

$headers = array(
    'Accept' => 'application/json',
    'x-api-key' => 'your-x-api-key',
    'x-api-token' => 'your-x-api-token',
);

$client = new \GuzzleHttp\Client();

// Define array of request body.
$request_body = array();

try {
    $response = $client->request('GET','https://api.paymee.com.br/v1.1/pix-automatic/scheduled-payments/{instructionId}', array(
        'headers' => $headers,
        'json' => $request_body,
       )
    );
    print_r($response->getBody()->getContents());
 }
 catch (\GuzzleHttp\Exception\BadResponseException $e) {
    // handle exception or api errors.
    print_r($e->getMessage());
 }

 // ...

```

```java
URL obj = new URL("https://api.paymee.com.br/v1.1/pix-automatic/scheduled-payments/{instructionId}");
HttpURLConnection con = (HttpURLConnection) obj.openConnection();
con.setRequestMethod("GET");
int responseCode = con.getResponseCode();
BufferedReader in = new BufferedReader(
    new InputStreamReader(con.getInputStream()));
String inputLine;
StringBuffer response = new StringBuffer();
while ((inputLine = in.readLine()) != null) {
    response.append(inputLine);
}
in.close();
System.out.println(response.toString());

```

```go
package main

import (
       "bytes"
       "net/http"
)

func main() {

    headers := map[string][]string{
        "Accept": []string{"application/json"},
        "x-api-key": []string{"your-x-api-key"},
        "x-api-token": []string{"your-x-api-token"},
    }

    data := bytes.NewBuffer([]byte{jsonReq})
    req, err := http.NewRequest("GET", "https://api.paymee.com.br/v1.1/pix-automatic/scheduled-payments/{instructionId}", data)
    req.Header = headers

    client := &http.Client{}
    resp, err := client.Do(req)
    // ...
}

```

`GET /v1.1/pix-automatic/scheduled-payments/{instructionId}`

*Get scheduled payment by instruction ID*

Retrieves a specific scheduled payment by its instruction ID.

<h3 id="getscheduledpaymentbyid-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|x-api-key|header|string|true|none|
|x-api-token|header|string|true|none|
|instructionId|path|string|true|Instruction ID of the scheduled payment.|

> Example responses

> Scheduled payment data returned successfully.

```json
{
  "status": 0,
  "message": "success",
  "paymentSchedule": {
    "id": 12,
    "instructionId": "000000000000000000000215832344",
    "status": "CANCELLED",
    "dueDate": "2025-06-19 03:00:00",
    "amount": 11,
    "endToEnd": "E2868389220250619150000215832384",
    "rejectionCode": "SLCR",
    "rejectionReason": "1",
    "creation": "2025-06-16 07:16:45"
  },
  "authorization": {
    "id": 26,
    "idRec": "RR286838922025061604150842631",
    "initialDate": "2025-06-19",
    "finalDate": "2025-12-15",
    "creation": "2025-06-16 07:16:42"
  },
  "sale": {
    "id": 215832384,
    "uuid": "16a5e040-8b55-4dbe-8563-035d2876a766",
    "situation": "CANCELLED"
  }
}
```

> 403 Response

```json
{
  "errorCount": 1,
  "errors": [
    {
      "error": "Forbidden",
      "message": "Access Denied"
    }
  ],
  "message": "field validation failure",
  "status": -1
}
```

<h3 id="getscheduledpaymentbyid-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|Scheduled payment data returned successfully.|Inline|
|403|[Forbidden](https://tools.ietf.org/html/rfc7231#section-6.5.3)|Unauthorized response|Inline|
|404|[Not Found](https://tools.ietf.org/html/rfc7231#section-6.5.4)|Scheduled payment not found|Inline|

<h3 id="getscheduledpaymentbyid-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» status|integer|false|none|none|
|» message|string|false|none|none|
|» paymentSchedule|object|false|none|none|
|»» id|integer|false|none|none|
|»» instructionId|string|false|none|none|
|»» status|string|false|none|none|
|»» dueDate|string(date-time)|false|none|none|
|»» amount|number|false|none|none|
|»» endToEnd|string|false|none|none|
|»» rejectionCode|string|false|none|none|
|»» rejectionReason|string|false|none|none|
|»» creation|string(date-time)|false|none|none|
|» authorization|object|false|none|none|
|»» id|integer|false|none|none|
|»» idRec|string|false|none|none|
|»» initialDate|string(date)|false|none|none|
|»» finalDate|string(date)|false|none|none|
|»» creation|string(date-time)|false|none|none|
|» sale|object|false|none|none|
|»» id|integer|false|none|none|
|»» uuid|string(uuid)|false|none|none|
|»» situation|string|false|none|none|

Status Code **403**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» errorCount|number|false|none|none|
|» errors|[object]|false|none|none|
|»» error|string|false|none|none|
|»» message|string|false|none|none|
|» message|string|false|none|none|
|» status|number|false|none|none|

Status Code **404**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» status|number|false|none|none|
|» message|string|false|none|none|
|» errorCount|number|false|none|none|
|» errors|[object]|false|none|none|
|»» field|string|false|none|none|
|»» message|string|false|none|none|

<aside class="success">
This operation does not require authentication
</aside>

## cancelScheduledPayment

<a id="opIdcancelScheduledPayment"></a>

> Code samples

```shell
# You can also use wget
curl -X PUT https://api.paymee.com.br/v1.1/pix-automatic/scheduled-payments/{instructionId}/cancel \
  -H 'Content-Type: application/json' \
  -H 'Accept: application/json' \
  -H 'x-api-key: your-x-api-key' \
  -H 'x-api-token: your-x-api-token'

```

```http
PUT https://api.paymee.com.br/v1.1/pix-automatic/scheduled-payments/{instructionId}/cancel HTTP/1.1
Host: api.paymee.com.br
Content-Type: application/json
Accept: application/json
x-api-key: your-x-api-key
x-api-token: your-x-api-token

```

```javascript
const inputBody = '{
  "reason": "Customer request."
}';
const headers = {
  'Content-Type':'application/json',
  'Accept':'application/json',
  'x-api-key':'your-x-api-key',
  'x-api-token':'your-x-api-token'
};

fetch('https://api.paymee.com.br/v1.1/pix-automatic/scheduled-payments/{instructionId}/cancel',
{
  method: 'PUT',
  body: inputBody,
  headers: headers
})
.then(function(res) {
    return res.json();
}).then(function(body) {
    console.log(body);
});

```

```ruby
require 'rest-client'
require 'json'

headers = {
  'Content-Type' => 'application/json',
  'Accept' => 'application/json',
  'x-api-key' => 'your-x-api-key',
  'x-api-token' => 'your-x-api-token'
}

result = RestClient.put 'https://api.paymee.com.br/v1.1/pix-automatic/scheduled-payments/{instructionId}/cancel',
  params: {
  }, headers: headers

p JSON.parse(result)

```

```python
import requests
headers = {
  'Content-Type': 'application/json',
  'Accept': 'application/json',
  'x-api-key': 'your-x-api-key',
  'x-api-token': 'your-x-api-token'
}

r = requests.put('https://api.paymee.com.br/v1.1/pix-automatic/scheduled-payments/{instructionId}/cancel', headers = headers)

print(r.json())

```

```php
<?php

require 'vendor/autoload.php';

$headers = array(
    'Content-Type' => 'application/json',
    'Accept' => 'application/json',
    'x-api-key' => 'your-x-api-key',
    'x-api-token' => 'your-x-api-token',
);

$client = new \GuzzleHttp\Client();

// Define array of request body.
$request_body = array();

try {
    $response = $client->request('PUT','https://api.paymee.com.br/v1.1/pix-automatic/scheduled-payments/{instructionId}/cancel', array(
        'headers' => $headers,
        'json' => $request_body,
       )
    );
    print_r($response->getBody()->getContents());
 }
 catch (\GuzzleHttp\Exception\BadResponseException $e) {
    // handle exception or api errors.
    print_r($e->getMessage());
 }

 // ...

```

```java
URL obj = new URL("https://api.paymee.com.br/v1.1/pix-automatic/scheduled-payments/{instructionId}/cancel");
HttpURLConnection con = (HttpURLConnection) obj.openConnection();
con.setRequestMethod("PUT");
int responseCode = con.getResponseCode();
BufferedReader in = new BufferedReader(
    new InputStreamReader(con.getInputStream()));
String inputLine;
StringBuffer response = new StringBuffer();
while ((inputLine = in.readLine()) != null) {
    response.append(inputLine);
}
in.close();
System.out.println(response.toString());

```

```go
package main

import (
       "bytes"
       "net/http"
)

func main() {

    headers := map[string][]string{
        "Content-Type": []string{"application/json"},
        "Accept": []string{"application/json"},
        "x-api-key": []string{"your-x-api-key"},
        "x-api-token": []string{"your-x-api-token"},
    }

    data := bytes.NewBuffer([]byte{jsonReq})
    req, err := http.NewRequest("PUT", "https://api.paymee.com.br/v1.1/pix-automatic/scheduled-payments/{instructionId}/cancel", data)
    req.Header = headers

    client := &http.Client{}
    resp, err := client.Do(req)
    // ...
}

```

`PUT /v1.1/pix-automatic/scheduled-payments/{instructionId}/cancel`

*Cancel scheduled payment*

Cancels a scheduled payment that has not yet been processed.

> Body parameter

```json
{
  "reason": "Customer request."
}
```

<h3 id="cancelscheduledpayment-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|x-api-key|header|string|true|none|
|x-api-token|header|string|true|none|
|instructionId|path|string|true|Instruction ID of the scheduled payment to be canceled.|
|body|body|object|true|Reason for the cancellation.|
|» reason|body|string|false|A brief reason for the cancellation.|

> Example responses

> Scheduled payment canceled successfully.

```json
{
  "status": 0,
  "message": "success"
}
```

> 400 Response

```json
{
  "errorCount": 1,
  "errors": [
    {
      "field": "instructionId",
      "message": "payment cannot be canceled"
    }
  ],
  "message": "field validation failure",
  "status": -1
}
```

<h3 id="cancelscheduledpayment-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|Scheduled payment canceled successfully.|Inline|
|400|[Bad Request](https://tools.ietf.org/html/rfc7231#section-6.5.1)|Bad Request. The payment might already be processed or in a non-cancelable state.|Inline|
|403|[Forbidden](https://tools.ietf.org/html/rfc7231#section-6.5.3)|Unauthorized response|Inline|
|404|[Not Found](https://tools.ietf.org/html/rfc7231#section-6.5.4)|Scheduled payment not found|Inline|

<h3 id="cancelscheduledpayment-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» status|integer|false|none|none|
|» message|string|false|none|none|

Status Code **400**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» errorCount|number|false|none|none|
|» errors|[object]|false|none|none|
|»» field|string|false|none|none|
|»» message|string|false|none|none|
|» message|string|false|none|none|
|» status|number|false|none|none|

Status Code **403**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» errorCount|number|false|none|none|
|» errors|[object]|false|none|none|
|»» error|string|false|none|none|
|»» message|string|false|none|none|
|» message|string|false|none|none|
|» status|number|false|none|none|

Status Code **404**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» status|number|false|none|none|
|» message|string|false|none|none|
|» errorCount|number|false|none|none|
|» errors|[object]|false|none|none|
|»» field|string|false|none|none|
|»» message|string|false|none|none|

<aside class="success">
This operation does not require authentication
</aside>

## listAuthorizations

<a id="opIdlistAuthorizations"></a>

> Code samples

```shell
# You can also use wget
curl -X GET https://api.paymee.com.br/v1.1/pix-automatic/authorizations \
  -H 'Accept: application/json' \
  -H 'x-api-key: your-x-api-key' \
  -H 'x-api-token: your-x-api-token'

```

```http
GET https://api.paymee.com.br/v1.1/pix-automatic/authorizations HTTP/1.1
Host: api.paymee.com.br
Accept: application/json
x-api-key: your-x-api-key
x-api-token: your-x-api-token

```

```javascript

const headers = {
  'Accept':'application/json',
  'x-api-key':'your-x-api-key',
  'x-api-token':'your-x-api-token'
};

fetch('https://api.paymee.com.br/v1.1/pix-automatic/authorizations',
{
  method: 'GET',

  headers: headers
})
.then(function(res) {
    return res.json();
}).then(function(body) {
    console.log(body);
});

```

```ruby
require 'rest-client'
require 'json'

headers = {
  'Accept' => 'application/json',
  'x-api-key' => 'your-x-api-key',
  'x-api-token' => 'your-x-api-token'
}

result = RestClient.get 'https://api.paymee.com.br/v1.1/pix-automatic/authorizations',
  params: {
  }, headers: headers

p JSON.parse(result)

```

```python
import requests
headers = {
  'Accept': 'application/json',
  'x-api-key': 'your-x-api-key',
  'x-api-token': 'your-x-api-token'
}

r = requests.get('https://api.paymee.com.br/v1.1/pix-automatic/authorizations', headers = headers)

print(r.json())

```

```php
<?php

require 'vendor/autoload.php';

$headers = array(
    'Accept' => 'application/json',
    'x-api-key' => 'your-x-api-key',
    'x-api-token' => 'your-x-api-token',
);

$client = new \GuzzleHttp\Client();

// Define array of request body.
$request_body = array();

try {
    $response = $client->request('GET','https://api.paymee.com.br/v1.1/pix-automatic/authorizations', array(
        'headers' => $headers,
        'json' => $request_body,
       )
    );
    print_r($response->getBody()->getContents());
 }
 catch (\GuzzleHttp\Exception\BadResponseException $e) {
    // handle exception or api errors.
    print_r($e->getMessage());
 }

 // ...

```

```java
URL obj = new URL("https://api.paymee.com.br/v1.1/pix-automatic/authorizations");
HttpURLConnection con = (HttpURLConnection) obj.openConnection();
con.setRequestMethod("GET");
int responseCode = con.getResponseCode();
BufferedReader in = new BufferedReader(
    new InputStreamReader(con.getInputStream()));
String inputLine;
StringBuffer response = new StringBuffer();
while ((inputLine = in.readLine()) != null) {
    response.append(inputLine);
}
in.close();
System.out.println(response.toString());

```

```go
package main

import (
       "bytes"
       "net/http"
)

func main() {

    headers := map[string][]string{
        "Accept": []string{"application/json"},
        "x-api-key": []string{"your-x-api-key"},
        "x-api-token": []string{"your-x-api-token"},
    }

    data := bytes.NewBuffer([]byte{jsonReq})
    req, err := http.NewRequest("GET", "https://api.paymee.com.br/v1.1/pix-automatic/authorizations", data)
    req.Header = headers

    client := &http.Client{}
    resp, err := client.Do(req)
    // ...
}

```

`GET /v1.1/pix-automatic/authorizations`

*List authorizations*

Lists recurring payment authorizations based on provided filters.

<h3 id="listauthorizations-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|x-api-key|header|string|true|none|
|x-api-token|header|string|true|none|
|situation|query|string|false|Filter by authorization status.|
|payerName|query|string|false|Filter by the payer's name.|
|initialDate|query|string(date)|false|Filter by start date (format YYYY-MM-DD).|
|finalDate|query|string(date)|false|Filter by end date (format YYYY-MM-DD).|
|page|query|integer|false|Page number for the query.|
|maxPageResults|query|integer|false|Maximum number of results per page.|
|sortBy|query|string|false|Field to sort the results by.|
|order|query|string|false|Sort order.|

#### Enumerated Values

|Parameter|Value|
|---|---|
|situation|CREATED|
|situation|APPROVING|
|situation|APPROVED|
|situation|REJECTED|
|situation|EXPIRED|
|situation|CANCELLING|
|situation|CANCELLED|
|situation|ACTIVE|
|situation|CONCLUDED|
|sortBy|updatedAt|
|sortBy|creation|
|sortBy|payerName|
|order|ASC|
|order|DESC|

> Example responses

> List of authorizations returned successfully.

```json
{
  "status": 0,
  "message": "success",
  "totalElements": 1,
  "totalPages": 1,
  "resultSize": 1,
  "results": [
    {
      "id": 27,
      "idRec": "RR286838922025061604202710881",
      "status": "APPROVED",
      "periodicity": "MONTHLY",
      "minimumAmount": 11,
      "initialDate": "2025-06-19",
      "finalDate": "2025-12-15",
      "nextRecurrenceDate": "2025-07-19 03:00:00",
      "creation": "2025-06-16 07:21:04",
      "payer": {
        "document": "00000000000",
        "name": "JOHN DOE",
        "ispb": "99999919"
      },
      "contract": {
        "id": "6851c0842bf40769ead1fde5",
        "description": "contract-456"
      }
    }
  ]
}
```

> 403 Response

```json
{
  "errorCount": 1,
  "errors": [
    {
      "error": "Forbidden",
      "message": "Access Denied"
    }
  ],
  "message": "field validation failure",
  "status": -1
}
```

<h3 id="listauthorizations-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|List of authorizations returned successfully.|Inline|
|403|[Forbidden](https://tools.ietf.org/html/rfc7231#section-6.5.3)|Unauthorized response|Inline|

<h3 id="listauthorizations-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» status|integer|false|none|none|
|» message|string|false|none|none|
|» totalElements|integer|false|none|none|
|» totalPages|integer|false|none|none|
|» resultSize|integer|false|none|none|
|» results|[object]|false|none|none|
|»» id|integer|false|none|none|
|»» idRec|string|false|none|none|
|»» status|string|false|none|none|
|»» periodicity|string|false|none|none|
|»» minimumAmount|number|false|none|none|
|»» initialDate|string(date)|false|none|none|
|»» finalDate|string(date)|false|none|none|
|»» nextRecurrenceDate|string(date-time)|false|none|none|
|»» creation|string(date-time)|false|none|none|
|»» payer|object|false|none|none|
|»»» document|string|false|none|none|
|»»» name|string|false|none|none|
|»»» ispb|string|false|none|none|
|»» contract|object|false|none|none|
|»»» id|string|false|none|none|
|»»» description|string|false|none|none|

Status Code **403**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» errorCount|number|false|none|none|
|» errors|[object]|false|none|none|
|»» error|string|false|none|none|
|»» message|string|false|none|none|
|» message|string|false|none|none|
|» status|number|false|none|none|

<aside class="success">
This operation does not require authentication
</aside>

## getAuthorizationById

<a id="opIdgetAuthorizationById"></a>

> Code samples

```shell
# You can also use wget
curl -X GET https://api.paymee.com.br/v1.1/pix-automatic/authorizations/{idRec} \
  -H 'Accept: application/json' \
  -H 'x-api-key: your-x-api-key' \
  -H 'x-api-token: your-x-api-token'

```

```http
GET https://api.paymee.com.br/v1.1/pix-automatic/authorizations/{idRec} HTTP/1.1
Host: api.paymee.com.br
Accept: application/json
x-api-key: your-x-api-key
x-api-token: your-x-api-token

```

```javascript

const headers = {
  'Accept':'application/json',
  'x-api-key':'your-x-api-key',
  'x-api-token':'your-x-api-token'
};

fetch('https://api.paymee.com.br/v1.1/pix-automatic/authorizations/{idRec}',
{
  method: 'GET',

  headers: headers
})
.then(function(res) {
    return res.json();
}).then(function(body) {
    console.log(body);
});

```

```ruby
require 'rest-client'
require 'json'

headers = {
  'Accept' => 'application/json',
  'x-api-key' => 'your-x-api-key',
  'x-api-token' => 'your-x-api-token'
}

result = RestClient.get 'https://api.paymee.com.br/v1.1/pix-automatic/authorizations/{idRec}',
  params: {
  }, headers: headers

p JSON.parse(result)

```

```python
import requests
headers = {
  'Accept': 'application/json',
  'x-api-key': 'your-x-api-key',
  'x-api-token': 'your-x-api-token'
}

r = requests.get('https://api.paymee.com.br/v1.1/pix-automatic/authorizations/{idRec}', headers = headers)

print(r.json())

```

```php
<?php

require 'vendor/autoload.php';

$headers = array(
    'Accept' => 'application/json',
    'x-api-key' => 'your-x-api-key',
    'x-api-token' => 'your-x-api-token',
);

$client = new \GuzzleHttp\Client();

// Define array of request body.
$request_body = array();

try {
    $response = $client->request('GET','https://api.paymee.com.br/v1.1/pix-automatic/authorizations/{idRec}', array(
        'headers' => $headers,
        'json' => $request_body,
       )
    );
    print_r($response->getBody()->getContents());
 }
 catch (\GuzzleHttp\Exception\BadResponseException $e) {
    // handle exception or api errors.
    print_r($e->getMessage());
 }

 // ...

```

```java
URL obj = new URL("https://api.paymee.com.br/v1.1/pix-automatic/authorizations/{idRec}");
HttpURLConnection con = (HttpURLConnection) obj.openConnection();
con.setRequestMethod("GET");
int responseCode = con.getResponseCode();
BufferedReader in = new BufferedReader(
    new InputStreamReader(con.getInputStream()));
String inputLine;
StringBuffer response = new StringBuffer();
while ((inputLine = in.readLine()) != null) {
    response.append(inputLine);
}
in.close();
System.out.println(response.toString());

```

```go
package main

import (
       "bytes"
       "net/http"
)

func main() {

    headers := map[string][]string{
        "Accept": []string{"application/json"},
        "x-api-key": []string{"your-x-api-key"},
        "x-api-token": []string{"your-x-api-token"},
    }

    data := bytes.NewBuffer([]byte{jsonReq})
    req, err := http.NewRequest("GET", "https://api.paymee.com.br/v1.1/pix-automatic/authorizations/{idRec}", data)
    req.Header = headers

    client := &http.Client{}
    resp, err := client.Do(req)
    // ...
}

```

`GET /v1.1/pix-automatic/authorizations/{idRec}`

*Get authorization by recurrence ID*

Retrieves a specific recurring payment authorization by its recurrence ID (idRec).

<h3 id="getauthorizationbyid-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|x-api-key|header|string|true|none|
|x-api-token|header|string|true|none|
|idRec|path|string|true|Recurrence ID (`idRec`) of the authorization to be retrieved.|

> Example responses

> Authorization data returned successfully.

```json
{
  "status": 0,
  "message": "success",
  "authorization": {
    "id": 27,
    "idRec": "RR286838922025061604202710881",
    "status": "APPROVED",
    "periodicity": "MONTHLY",
    "minimumAmount": 11,
    "initialDate": "2025-06-19",
    "finalDate": "2025-12-15",
    "nextRecurrenceDate": "2025-07-19 03:00:00",
    "creation": "2025-06-16 07:21:04",
    "payer": {
      "document": "00000000000",
      "name": "JOHN DOE",
      "ispb": "99999919"
    },
    "contract": {
      "id": "6851c0842bf40769ead1fde5",
      "description": "contract-456"
    }
  },
  "paymentSchedules": [
    {
      "id": 13,
      "status": "PAID",
      "dueDate": "2025-06-16 07:20:24",
      "amount": 11,
      "sale": {
        "id": 215832385,
        "uuid": "3b1e4441-9060-3b9e-81a4-d0fa4cab19b8",
        "situation": "CANCELLED"
      }
    }
  ]
}
```

> 403 Response

```json
{
  "errorCount": 1,
  "errors": [
    {
      "error": "Forbidden",
      "message": "Access Denied"
    }
  ],
  "message": "field validation failure",
  "status": -1
}
```

<h3 id="getauthorizationbyid-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|Authorization data returned successfully.|Inline|
|403|[Forbidden](https://tools.ietf.org/html/rfc7231#section-6.5.3)|Unauthorized response|Inline|
|404|[Not Found](https://tools.ietf.org/html/rfc7231#section-6.5.4)|Authorization not found|Inline|

<h3 id="getauthorizationbyid-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» status|integer|false|none|none|
|» message|string|false|none|none|
|» authorization|object|false|none|none|
|»» id|integer|false|none|none|
|»» idRec|string|false|none|none|
|»» status|string|false|none|none|
|»» periodicity|string|false|none|none|
|»» minimumAmount|number|false|none|none|
|»» initialDate|string(date)|false|none|none|
|»» finalDate|string(date)|false|none|none|
|»» nextRecurrenceDate|string(date-time)|false|none|none|
|»» creation|string(date-time)|false|none|none|
|»» payer|object|false|none|none|
|»»» document|string|false|none|none|
|»»» name|string|false|none|none|
|»»» ispb|string|false|none|none|
|»» contract|object|false|none|none|
|»»» id|string|false|none|none|
|»»» description|string|false|none|none|
|» paymentSchedules|[object]|false|none|none|
|»» id|integer|false|none|none|
|»» status|string|false|none|none|
|»» dueDate|string(date-time)|false|none|none|
|»» amount|number|false|none|none|
|»» sale|object|false|none|none|
|»»» id|integer|false|none|none|
|»»» uuid|string(uuid)|false|none|none|
|»»» situation|string|false|none|none|

Status Code **403**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» errorCount|number|false|none|none|
|» errors|[object]|false|none|none|
|»» error|string|false|none|none|
|»» message|string|false|none|none|
|» message|string|false|none|none|
|» status|number|false|none|none|

Status Code **404**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» status|number|false|none|none|
|» message|string|false|none|none|
|» errorCount|number|false|none|none|
|» errors|[object]|false|none|none|
|»» field|string|false|none|none|
|»» message|string|false|none|none|

<aside class="success">
This operation does not require authentication
</aside>

## cancelAuthorization

<a id="opIdcancelAuthorization"></a>

> Code samples

```shell
# You can also use wget
curl -X PUT https://api.paymee.com.br/v1.1/pix-automatic/authorizations/{idRec}/cancel \
  -H 'Content-Type: application/json' \
  -H 'Accept: application/json' \
  -H 'x-api-key: your-x-api-key' \
  -H 'x-api-token: your-x-api-token'

```

```http
PUT https://api.paymee.com.br/v1.1/pix-automatic/authorizations/{idRec}/cancel HTTP/1.1
Host: api.paymee.com.br
Content-Type: application/json
Accept: application/json
x-api-key: your-x-api-key
x-api-token: your-x-api-token

```

```javascript
const inputBody = '{
  "reason": "Customer requested cancellation."
}';
const headers = {
  'Content-Type':'application/json',
  'Accept':'application/json',
  'x-api-key':'your-x-api-key',
  'x-api-token':'your-x-api-token'
};

fetch('https://api.paymee.com.br/v1.1/pix-automatic/authorizations/{idRec}/cancel',
{
  method: 'PUT',
  body: inputBody,
  headers: headers
})
.then(function(res) {
    return res.json();
}).then(function(body) {
    console.log(body);
});

```

```ruby
require 'rest-client'
require 'json'

headers = {
  'Content-Type' => 'application/json',
  'Accept' => 'application/json',
  'x-api-key' => 'your-x-api-key',
  'x-api-token' => 'your-x-api-token'
}

result = RestClient.put 'https://api.paymee.com.br/v1.1/pix-automatic/authorizations/{idRec}/cancel',
  params: {
  }, headers: headers

p JSON.parse(result)

```

```python
import requests
headers = {
  'Content-Type': 'application/json',
  'Accept': 'application/json',
  'x-api-key': 'your-x-api-key',
  'x-api-token': 'your-x-api-token'
}

r = requests.put('https://api.paymee.com.br/v1.1/pix-automatic/authorizations/{idRec}/cancel', headers = headers)

print(r.json())

```

```php
<?php

require 'vendor/autoload.php';

$headers = array(
    'Content-Type' => 'application/json',
    'Accept' => 'application/json',
    'x-api-key' => 'your-x-api-key',
    'x-api-token' => 'your-x-api-token',
);

$client = new \GuzzleHttp\Client();

// Define array of request body.
$request_body = array();

try {
    $response = $client->request('PUT','https://api.paymee.com.br/v1.1/pix-automatic/authorizations/{idRec}/cancel', array(
        'headers' => $headers,
        'json' => $request_body,
       )
    );
    print_r($response->getBody()->getContents());
 }
 catch (\GuzzleHttp\Exception\BadResponseException $e) {
    // handle exception or api errors.
    print_r($e->getMessage());
 }

 // ...

```

```java
URL obj = new URL("https://api.paymee.com.br/v1.1/pix-automatic/authorizations/{idRec}/cancel");
HttpURLConnection con = (HttpURLConnection) obj.openConnection();
con.setRequestMethod("PUT");
int responseCode = con.getResponseCode();
BufferedReader in = new BufferedReader(
    new InputStreamReader(con.getInputStream()));
String inputLine;
StringBuffer response = new StringBuffer();
while ((inputLine = in.readLine()) != null) {
    response.append(inputLine);
}
in.close();
System.out.println(response.toString());

```

```go
package main

import (
       "bytes"
       "net/http"
)

func main() {

    headers := map[string][]string{
        "Content-Type": []string{"application/json"},
        "Accept": []string{"application/json"},
        "x-api-key": []string{"your-x-api-key"},
        "x-api-token": []string{"your-x-api-token"},
    }

    data := bytes.NewBuffer([]byte{jsonReq})
    req, err := http.NewRequest("PUT", "https://api.paymee.com.br/v1.1/pix-automatic/authorizations/{idRec}/cancel", data)
    req.Header = headers

    client := &http.Client{}
    resp, err := client.Do(req)
    // ...
}

```

`PUT /v1.1/pix-automatic/authorizations/{idRec}/cancel`

*Cancel authorization*

Cancels a recurring payment authorization.

> Body parameter

```json
{
  "reason": "Customer requested cancellation."
}
```

<h3 id="cancelauthorization-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|x-api-key|header|string|true|none|
|x-api-token|header|string|true|none|
|idRec|path|string|true|Recurrence ID (`idRec`) of the authorization to be canceled.|
|body|body|object|true|Reason for the cancellation.|
|» reason|body|string|false|A brief reason for the cancellation.|

> Example responses

> Authorization canceled successfully.

```json
{
  "status": 0,
  "message": "success"
}
```

> 400 Response

```json
{
  "errorCount": 1,
  "errors": [
    {
      "field": "idRec",
      "message": "authorization cannot be canceled"
    }
  ],
  "message": "field validation failure",
  "status": -1
}
```

<h3 id="cancelauthorization-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|Authorization canceled successfully.|Inline|
|400|[Bad Request](https://tools.ietf.org/html/rfc7231#section-6.5.1)|Bad Request. The authorization might be in a non-cancelable state.|Inline|
|403|[Forbidden](https://tools.ietf.org/html/rfc7231#section-6.5.3)|Unauthorized response|Inline|
|404|[Not Found](https://tools.ietf.org/html/rfc7231#section-6.5.4)|Authorization not found|Inline|

<h3 id="cancelauthorization-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» status|integer|false|none|none|
|» message|string|false|none|none|

Status Code **400**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» errorCount|number|false|none|none|
|» errors|[object]|false|none|none|
|»» field|string|false|none|none|
|»» message|string|false|none|none|
|» message|string|false|none|none|
|» status|number|false|none|none|

Status Code **403**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» errorCount|number|false|none|none|
|» errors|[object]|false|none|none|
|»» error|string|false|none|none|
|»» message|string|false|none|none|
|» message|string|false|none|none|
|» status|number|false|none|none|

Status Code **404**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» status|number|false|none|none|
|» message|string|false|none|none|
|» errorCount|number|false|none|none|
|» errors|[object]|false|none|none|
|»» field|string|false|none|none|
|»» message|string|false|none|none|

<aside class="success">
This operation does not require authentication
</aside>

