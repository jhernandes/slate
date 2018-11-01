# Errors
If error is returned, it will have the following format:

> Error returned by iPag API

```xml
<?xml version="1.0" encoding="utf-8" ?>
<error>
    <code>002</code>
    <message>Payment method not configured or inactive on iPag. Access the Dashboard to verify.</message>
</error>

<?xml version="1.0" encoding="utf-8" ?>
<error>
    <code>011</code>
    <message>(4066553618107) - Card number is invalid</message>
</error>
```

> Error returned by iPag API (via SDK-PHP)

```php
<?php

$response->error; // Error code
$response->errorMessage; // Error message
```

## Submission Errors


Code | description
------ | ------
001 | Your account is not eligible to transact on iPag.
002 | Payment method not configured or inactive on iPag.
003 | The request is already being processed. If you have any questions, please contact the retailer.


## Query Errors

Code | description
------ | ------
100 | The field [xpto] is required.
101 | Your account is not eligible to conduct transaction queries on iPag.
102 | Transaction was not accepted by the operator, or there was a problem to recover the return of the same.
103 | There is no transaction on iPag with an order number entered.

## Catch Errors

Code | description
------ | ------
100 | The field [xpto] is required.
201 | Your account is not eligible to perform transaction capture on iPag.
202 | Transaction with the transId informed was not found.
203 | Payment method not configured or inactive on iPag.
204 | The transaction has already been captured.
205 | Operation not allowed

## Cancellation Errors

Code | description
------ | ------
100 | The field [xpto] is required.
301 | There is no transaction on iPag with an order number entered.
302 | Operation not allowed.


## IP Block

IP blocking occurs after 3 transactions have been declined from the same client.

```xml
<?xml version="1.0" encoding="utf-8" ?>
<retorno>
    <num_pedido>20170420155155</num_pedido>
    <status_pagamento>7</status_pagamento>
    <mensagem_transacao>(99) Transaction not authorized</mensagem_transacao>
    <operadora_mensagem>Transaction blocked: Customer email blocked after exceeding the limit of 3 refused transactions in less than one hour. To unlock, go to iPag / Security / Blacklist Panel.</operadora_mensagem>
    <metodo>visa</metodo>
    <redirect>false</redirect>
</retorno>
```

Code | description
------- | ----------
99 | Transaction blocked: Customer email blocked after exceeding the limit of 3 refused transactions in less than one hour. To unlock, go to iPag / Security / Blacklist Panel.