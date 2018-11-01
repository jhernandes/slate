# Exemplos

Example HTMLs

 Production Endpoint: //Request to iPag on atedimento@ipag.com.br

Sandbox Endpoint 

`https://sandbox.ipag.com.br/`

## Submit Payment

**Endpoint:**
`POST /service/payment`

```php
> Submission Example via PHP SDK

<?php
// VIA IPAG-SDK-PHP

require 'vendor/autoload.php';

use Ipag\Ipag;
use Ipag\Classes\Authentication;
use Ipag\Classes\Endpoint;

try {
    $ipag = new Ipag(new Authentication('my_ipag_id', 'my_ipag_key'), Endpoint::SANDBOX);

    $customer = $ipag->customer()
        ->setName('Fulano da Silva')
        ->setTaxpayerId('799.993.388-01')
        ->setPhone('11', '98888-3333')
        ->setEmail('fulanodasilva@gmail.com')
        ->setAddress($ipag->address()
            ->setStreet('Rua Júlio Gonzalez')
            ->setNumber('1000')
            ->setNeighborhood('Barra Funda')
            ->setCity('São Paulo')
            ->setState('SP')
            ->setZipCode('01156-060')
    );

    $creditCard = $ipag->creditCard()
        ->setNumber('4066553613548107')
        ->setHolder('FULANO')
        ->setExpiryMonth('10')
        ->setExpiryYear('2025')
        ->setCvc('123');

    $cart = $ipag->cart(
        ['Produto 1', 5.00, 1, 'ABDC1'],
        ['Produto 2', 3.50, 2, 'ABDC2'],
        ['Produto 3', 5.50, 1, 'ABDC3'],
        ['Produto 4', 8.50, 5, 'ABDC4']
    );

    $transaction = $ipag->transaction();
    $transaction->getOrder()
        ->setOrderId($orderId)
        ->setCallbackUrl('https://minha_loja.com.br/ipag/callback')
        ->setAmount(10.00)
        ->setInstallments(1)
        ->setPayment($ipag->payment()
            ->setMethod(Method::VISA)
            ->setCreditCard($creditCard)
        )
        ->setCustomer($customer)
        ->setCart($cart);

    $response = $transaction->execute();

    //Retornou algum erro?
    if (!empty($response->error)) {
        throw new \Exception($response->errorMessage);
    }

    //Pagamento Aprovado (5) ou Aprovado e Capturado(8) ?
    if ($response->payment->status == '5' || $response->payment->status == '8') {
        //Faz alguma coisa...
        return $response;
    }
} catch(\Exception $e) {
    print_r($e->__toString());
}
```
<aside class="notice">
    Remember, this is just an example to do payment submission testing.
</aside>

## Consult Payment

**Endpoint:**
`POST /service/consult`

```php
> Consult Example PHP SDK

<?php
// VIA IPAG-SDK-PHP

require 'vendor/autoload.php';

use Ipag\Ipag;
use Ipag\Classes\Authentication;
use Ipag\Classes\Endpoint;

try {
  $ipag = new Ipag(new Authentication('my_ipag_id', 'my_ipag_key'), Endpoint::SANDBOX);

  $response = $ipag->transaction()->setTid('123456789')->consult();
} catch (Exception $e) {
  print_r($e->__toString());
}
```

Parameter | size | type | Required | description
--------- | ----- | ----- | ----------- | ---------
identificacao | 60 | string | yes | Identification code of the establishment in iPag (panel access login)
transId | 255 | string | yes/no | Transaction ID code.
numPedido | 20 | string | no/yes | Order identification code.
retorno_tipo | 20 | string | no | `xml`
url_retorno | 255 | string | no | Your store URL.

<aside class="notice">
    At least one of the fields must be sent: `transId` or` numPedido`
</aside>
<aside class="notice">
   Remember, this is just an example to do payment query testing.
</aside>


## Capture Payment

**Endpoint:**
`POST /service/capture`

```php
> Capture Example via PHP SDK

<?php
// VIA IPAG-SDK-PHP

require 'vendor/autoload.php';

use Ipag\Ipag;
use Ipag\Classes\Authentication;
use Ipag\Classes\Endpoint;

try {
  $ipag = new Ipag(new Authentication('my_ipag_id', 'my_ipag_key'), Endpoint::SANDBOX);

  $response = $ipag->transaction()->setTid('123456789')->capture();
} catch (Exception $e) {
  print_r($e->__toString());
}
```

Parameter | size | type | Required | description
--------- | ----- | ----- | ----------- | ---------
identificacao | 60 | string | yes | Identification code of the establishment in iPag (panel access login)
transId | 255 | string | yes | Transaction ID code.
url_retorno | 255 | string | yes |`xml` or Url from your store.

<aside class="notice">
  Remember, this is just an example to take payment catch tests.
</aside>

## Cancel Payment

**Endpoint:**
`POST /service/cancel`

```php
> Cancel Example via PHP SDK

<?php
// VIA IPAG-SDK-PHP

require 'vendor/autoload.php';

use Ipag\Ipag;
use Ipag\Classes\Authentication;
use Ipag\Classes\Endpoint;

try {
  $ipag = new Ipag(new Authentication('my_ipag_id', 'my_ipag_key'), Endpoint::SANDBOX);

  $response = $ipag->transaction()->setTid('123456789')->cancel();
} catch (Exception $e) {
  print_r($e->__toString());
}
```

Parameter | size | type | Required | description
--------- | ----- | ----- | ----------- | ---------
identificacao | 60 | string | yes | Identification code of the establishment in iPag (panel access login)
transId | 255 | string | yes | Transaction ID code.
url_retorno | 255 | string | yes | It can be `xml` or a url from your store.

<aside class="notice">
   Remember, this is just an example to make payment cancellation tests.
</aside>
