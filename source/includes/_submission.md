# Payments Submission

To integrate your Virtual Store or Website with iPag, send the following parameters via POST to the iPag system

##Endpoint

**Sandbox**

`https://sandbox.ipag.com.br/`

**Production**

`Make the request to suporte@ipag.com.br`

##Make Payment (Create Transaction)

`POST /service/payment`

> Example via [SDK PHP](https://github.com/jhernandes/ipag-sdk-php)

```php
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
        // Nome do Produto, Valor Unitário, Quantidade, SKU (Código do Produto)
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
        ->setIp('200.100.10.1')
        ->setPayment($ipag->payment()
            ->setMethod(Method::VISA)
            ->setCreditCard($creditCard)
        )
        ->setCart($cart)
        ->setCustomer($customer);

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

```shell
curl -X POST \
  https://sandbox.ipag.com.br/service/payment \
  -H 'Authorization: Basic am9uYXRoYW46REM4QS00QzE2Yu75NS1EQTZBRUY2OC0wRkQ2RDMyOC0wRjAz' \
  -H 'content-type: multipart/form-data' \
  -F identificacao=teste \
  -F pedido=201803061703 \
  -F operacao=Pagamento \
  -F valor=2.00 \
  -F metodo=visa \
  -F url_retorno=https://empresa.com/retorno \
  -F email=jose@teste.com.br \
  -F fone=11111111111 \
  -F 'nome=Jean Orlando' \
  -F 'endereco=Rua 1' \
  -F numero_endereco=111 \
  -F bairro=Bairro1 \
  -F 'cidade=Cidade 1' \
  -F estado=SP \
  -F pais=Brasil \
  -F cep=14400330 \
  -F retorno_tipo=xml \
  -F num_cartao=4012001038166662 \
  -F mes_cartao=07 \
  -F ano_cartao=2018 \
  -F cvv_cartao=306 \
  -F documento=79999338801

  <?xml version="1.0" encoding="utf-8" ?>
  <retorno>
      <id_transacao>80f1e6f5d38947b3bf413369f5b75d3d</id_transacao>
      <valor>2.00</valor>
      <num_pedido>201803061703</num_pedido>
      <status_pagamento>8</status_pagamento>
      <mensagem_transacao>Transação aprovada e capturada</mensagem_transacao>
      <metodo>visa</metodo>
      <operadora>zoop</operadora>
      <operadora_mensagem>succeeded</operadora_mensagem>
      <id_librepag>1003838</id_librepag>
      <autorizacao_id>Z133937-000260004</autorizacao_id>
      <url_autenticacao></url_autenticacao>
  </retorno>
```

### Identification Data

Field | Size | Type | Required | Description
--------- | ----- | ----- | ----------- | ---------
identificacao | 60 | string | yes | Estabilishment identification code on iPag (Access Login on iPag Panel).
identificacao2 | 60 | string | no | Partner identification code on iPag (Marketplace)


### Operation Data
Field | Size | Type | Required | Description
--------- | ----- | ----- | ----------- | ---------
metodo | 15 | string | yes | Payment Method * see possible values at section [Methods](#m-todos)
operacao | 10 | string | yes | Operation * see the possible values at section [Operations](#opera-es)
pedido| 20 | string | yes | Order number (Restriction is that it can not be the same as another already sent to iPag. We advise a sequential number)
url_retorno | 50 | string | yes | Return URL to the Virtual Store or Site.
retorno_tipo | 5 | string | yes | Inform 'xml'.
boleto_tipo | 5 | string | yes | Inform 'xml'.

### Transaction Data

Field | Size | Type | Required | Description
--------- | ----- | ----- | ----------- | ---------
valor | 12 | decimal | yes | Total a amount of purchase. Points must be used as a decimal point separator, eg: 100.00
captura | 1 | string | no | Sets whether to authorize or capture the transaction. If not informed will use the default setting set in iPag Panel. <br> Default = 'P' <br> Only Authorize = 'A' <br> Automatic Capture = 'C'
parcelas | 3 | number | no (if card, yes) | Number of Installments, minimum: 1, maximum: 12
ip | 60 | string | no (but recommended) | computer's ip,  IPV4 and IPV6 allowed
frete_valor | 12 | decimal | no | Freight amount charged, this is just informative, not added to the total amount
frete_tipo | 100 | string | no | Freight description, example: Pac (Approximately 5 days for delivery)
antifraude | 1 | boolean | no | Sets whether to check in Anti-Fraud or not, only if anti-fraud is configured in the account. Default value: true (1)

### Debit/Credit Data

Field | Size | Type | Required | Description
--------- | ----- | ----- | ----------- | ---------
softdescriptor | 13 | string | no| Identification on the Card Invoice (Cielo, Zoop). Ex: MYSTORE
nome_cartao | 30 | string | no (if card, yes) | Name of the credit card holder.
num_cartao | 16 | number | no (if card, yes) | Credit card number.
cvv_cartao | 3 | number | no  (if card, yes) | Credit card verification number.
mes_cartao | 2 | number | no  (if card, yes) | Credit card expiry month.
ano_cartao | 2 | number | no  (if card, yes) |  Credit card expiry year.

### Customer Data

Field | Size | Type | Required | Description
--------- | ----- | ----- | ----------- | ---------
nome | 30 | string | yes | Customer name
tipo_pessoa | 1 | char | no | "J" for legal persons and "f" for individuals
documento | 18 | string | no | Customer CPF or CNPJ
email | 30 | string | no| Customer email
fone | 10 | string | no | Customer phone
birthdate | 10 | string | no | Customer birthdate (Ex.: 1989-03-28)[AAAA-MM-DD]

### Billing / Shipping Address Data

Field | Size | Type | Required | Description
--------- | ----- | ----- | ----------- | ---------
endereco | 30 | string | no | Customer full address
numero_endereco | 5 | number | no | Address number
complemento | 100 | string | no | Address complement
bairro | 15 | string | no | Customer neighborhood
cidade | 20 | string | no | Customer city
estado | 2 | string | no | Customer state/province
pais | 15 | string | no | Customer country
cep | 8 | string | no | Customer zipcode

### Product/Cart Data

Field | Size | Type | Required | Description
--------- | ----- | ----- | ----------- | ---------
produtos[#][nome] | 100 | string | no | Product name
produtos[#][quantidade] | 100 | integer | no | Quantity
produtos[#][valor] | 10 | float | no | Unit price
produtos[#][sku] | 100 | string | no | Unique product code 
produtos[#][descricao] | 255 | string | no | Product desciption

<aside class="info">
<b>Replace # with a number that identifies this product.</b><br><br>

Example:<br><br>
produtos[1][nome] = 'Blue shirt'<br>
produtos[1][quantidade] = 2<br>
produtos[1][valor] = 17.89<br>
produtos[1][sku] = 'CAS8172AA'<br>
produtos[1][descricao] = 'BLUE SHIRT WINTER COLLECTION 2018'<br>
<br><br>
produtos[2][nome] = 'Green Shirt'<br>
produtos[2][quantidade] = 1<br>
produtos[2][valor] = 19.89<br>
produtos[2][sku] = 'CAS817255'<br>
produtos[2][descricao] = 'GREEN SHIRT WINTER COLLECTION 2018'
</aside>


## Additional fields for Billet (iPag/Zoop)

> Exemplo:

```php
<?php
// VIA IPAG-SDK-PHP
// ...
$creditCard = $ipag->payment()
    ->setInstructions('Instrução 1')
    ->setInstructions('Instrução 2')
    ->setInstructions('Instrução 3')
    ->setDemonstratives('Demonstrativo 1')
    ->setDemonstratives('Demonstrativo 2');
// ...
```

```shell
curl -X POST \
  https://sandbox.ipag.com.br/service/payment \
  -H 'Authorization: Basic am9uYXRoYW46REM4QS00QzE2OUM7DSsdDEQTZBRUY2OC0wRkQ2RDMyOC0wRjAz' \
  -F pedido=201803061703 \
  -F operacao=Pagamento \
  -F valor=2.00 \
  -F metodo=boletozoop \
  -F url_retorno=https://empresa.com/retorno \
  -F email=jose@teste.com.br \
  -F fone=11111111111 \
  -F 'nome=Jose Francisco da Silva' \
  -F 'endereco=Rua 1' \
  -F numero_endereco=111 \
  -F bairro=Bairro1 \
  -F 'cidade=Cidade 1' \
  -F estado=SP \
  -F pais=Brasil \
  -F cep=14400330 \
  -F retorno_tipo=xml \
  -F documento=79999338801 \
  -F 'instrucoes[1]=Instrução 1' \
  -F 'instrucoes[2]=Instrução 2' \
  -F 'instrucoes[3]=Instrução 3' \
  -F 'demonstrativos[1]=Demonstrativo 1'
  -F 'demonstrativos[2]=Demonstrativo 2'
```

Field | Size | Type | Required | Descrription
--------- | ----- | ----- | ----------- | ---------
vencto | 10 | date | yes | Due date (DD/MM/YYYY). If not informed, the expiration date will be today + the deadline entered in the iPag settings.
instrucoes[] | 80 | string | no | To change the instruction lines of the Boards issued by iPag / Zoop, send the field instructions [1], instructions [2] and instructions [3] if necessary.
demonstrativos[] | 80 | string | no | To change the statement lines of the Billets issued by iPag / Zoop send the field demonstratives [1], statements [2] if necessary.
## Additional Fields for Split with Billets (iPag/Zoop Marketplace Only)

In order to be able to split with billets it's necessary to send the split rule along with the fields in the transaction request.

> Example:

```php
<?php
// VIA IPAG-SDK-PHP
$splitRule = new SplitRule();
$splitRule->setSellerId('c66fabf44786459e81e3c65e339a4fc9')
  ->setPercentage(95)
  ->setLiable(1);
// ->setAmount(9.90)

$ipag->payment()->addSplitRule($splitRule);
/*
  Você pode adicionar quantas regras forem necessárias e permitidas.
*/
```

```shell
curl -X POST \
  https://sandbox.ipag.com.br/service/payment \
  -H 'Authorization: Basic am9uYXRoYW46REM4QS00QzE2OUM7DSsdDEQTZBRUY2OC0wRkQ2RDMyOC0wRjAz' \
  -F pedido=201803061703 \
  -F operacao=Pagamento \
  -F valor=2.00 \
  -F metodo=boletozoop \
  -F url_retorno=https://empresa.com/retorno \
  -F email=jose@teste.com.br \
  -F fone=11111111111 \
  -F 'nome=Jose Francisco da Silva' \
  -F 'endereco=Rua 1' \
  -F numero_endereco=111 \
  -F bairro=Bairro1 \
  -F 'cidade=Cidade 1' \
  -F estado=SP \
  -F pais=Brasil \
  -F cep=14400330 \
  -F retorno_tipo=xml \
  -F documento=79999338801 \
  -F 'split[1][seller_id]=c66fabf44786459e81e3c65e339a4fc9' \
  -F 'split[1][percentage]=42' \
  -F 'split[1][liable]=1' \
  -F 'split[2][seller_id]=d56fabf44786459e81e3c65e339a4fc9' \
  -F 'split[2][percentage]=50' \
  -F 'split[2][liable]=1' \
```

Field | Size | Type | Required | Description
--------- | ----- | ----- | ----------- | ---------
split[] | - | container | no | Container (Array)
split[1][seller_id] | 50 | string | yes | SellerId or Vendor Login. This information is passed when creating a seller.
split[1][percentage] | 2 | integer | yes/no | Percentage Value that will be passed on to the seller can't be greater than the total amount plus the fee that will be charged. Not required if sent 'amount';
split[1][amount] | 5 | double | yes/no | Absolute value in Brazilian Real that will be passed on to the seller can't be greater than the total amount plus the fee that will be charged. Not required if sent 'percentage'
split[1][liable] | 1 | integer | no | Defines whether the receiver will take the charge in case of chargeback or not. 1- Yes, 2- No.


To add another rule, increase the value: split [1], split [2], etc ...

<aside class="warning">
<b>If informed a higher value than the sale in the 'percentage' or 'amount' fields, payment will be refused.</b>
</aside>

## Additional Fields for  1-click buy

> Example:

```php
<?php
// VIA IPAG-SDK-PHP
// ...
$creditCard = $ipag->creditCard()
    ->setNumber('4066553613548107')
    ->setHolder('FULANO')
    ->setExpiryMonth('10')
    ->setExpiryYear('2025')
    ->setCvc('123')
    ->setSave(true); //True para gerar o token do cartão (one-click-buy)
// ...
```

```shell
curl -X POST \
  https://sandbox.ipag.com.br/service/payment \
  -H 'Authorization: Basic am9uYXRoYW46REM4QS00QzE2OUM7DSsdDEQTZBRUY2OC0wRkQ2RDMyOC0wRjAz' \
  -H 'content-type: multipart/form-data' \
  -F identificacao=teste \
  -F pedido=201803061704 \
  -F operacao=Pagamento \
  -F valor=2.00 \
  -F metodo=visa \
  -F url_retorno=https://empresa.com/retorno \
  -F email=jose@teste.com.br \
  -F fone=11111111111 \
  -F 'nome=Jean Orlando' \
  -F 'endereco=Rua 1' \
  -F numero_endereco=111 \
  -F bairro=Bairro1 \
  -F 'cidade=Cidade 1' \
  -F estado=SP \
  -F pais=Brasil \
  -F cep=14400330 \
  -F retorno_tipo=xml \
  -F num_cartao=4012001038166662 \
  -F mes_cartao=07 \
  -F ano_cartao=2018 \
  -F cvv_cartao=306 \
  -F documento=79999338801 \
  -F gera_token_cartao=1

<?xml version="1.0" encoding="utf-8" ?>
<retorno>
    <id_transacao>c82263b4dd2c43e2a73d3f7950820e78</id_transacao>
    <valor>2.00</valor>
    <num_pedido>201803061704</num_pedido>
    <status_pagamento>8</status_pagamento>
    <mensagem_transacao>Transação aprovada e capturada</mensagem_transacao>
    <metodo>visa</metodo>
    <operadora>zoop</operadora>
    <operadora_mensagem>succeeded</operadora_mensagem>
    <id_librepag>1003839</id_librepag>
    <autorizacao_id>Z133937-000260004</autorizacao_id>
    <url_autenticacao></url_autenticacao>
    <token>c565-80ccecf3-27995e1e-a9c1ebc3-4b10</token>
    <last4>6662</last4>
    <mes>07</mes>
    <ano>2018</ano>
</retorno>
```

Field | Size | Type | Required | Description
--------- | ----- | ----- | ----------- | ---------
gera_token_cartao | 5 | boolean | Required when creating order | Used to perform the transaction in which the Card data is stored in iPag. This parameter is used to implement the 1 Click Payment feature.
token_cartao | 37 | string | Required when using token | When the customer makes a purchase using the 1-Click Payment option, this parameter must be sent. In this case, the following parameters should not be sent: nome_cartao, num_cartao, metodo, cvv_cartao, mes_cartao, ano_cartao

<aside class="info">
<b>For purchases via OneClick and Subscriptions, it is necessary to contact the Acquirer and request the liberation of transactions without the need for the CVV (Card Security Code).</b>
</aside>

## Additional Fields for Recurrence (Signature)

> Recurrence Example (Signature)

```php
<?php
// VIA IPAG-SDK-PHP
// ...

$transaction = $ipag->transaction();
$transaction->getOrder()
    ->setSubscription($ipag->subscription()
      ->setProfileId('1000000')
      ->setFrequency(1)
      ->setInterval('month')
      ->setStart('10/10/2018')
);

$response = $transaction->execute();
```

```shell
curl -X POST \
  https://sandbox.ipag.com.br/service/payment \
  -H 'Authorization: Basic am9uYXRoYW46REM4QS00QzE2OUM7DSsdDEQTZBRUY2OC0wRkQ2RDMyOC0wRjAz' \
  -H 'content-type: multipart/form-data' \
  -F identificacao=teste \
  -F pedido=201803061705 \
  -F operacao=Pagamento \
  -F valor=50.00 \
  -F metodo=visa \
  -F url_retorno=https://empresa.com/retorno/profile_id/123456 \
  -F email=jose@teste.com.br \
  -F fone=11111111111 \
  -F 'nome=Jean Orlando' \
  -F 'endereco=Rua 1' \
  -F numero_endereco=111 \
  -F bairro=Bairro1 \
  -F 'cidade=Cidade 1' \
  -F estado=SP \
  -F pais=Brasil \
  -F cep=14400330 \
  -F retorno_tipo=xml \
  -F num_cartao=4012001038166662 \
  -F mes_cartao=07 \
  -F ano_cartao=2018 \
  -F cvv_cartao=306 \
  -F documento=79999338801 \
  -F frequencia=1 \
  -F intervalo=month \
  -F inicio=10/09/2018 \
  -F valor_rec=99.00

<?xml version="1.0" encoding="utf-8" ?>
<retorno>
    <id_transacao>803b898fbf7b4d0bb6b19081360bebd9</id_transacao>
    <valor>50.00</valor>
    <num_pedido>201803061705</num_pedido>
    <status_pagamento>8</status_pagamento>
    <mensagem_transacao>Transação aprovada e capturada</mensagem_transacao>
    <metodo>visa</metodo>
    <operadora>zoop</operadora>
    <operadora_mensagem>succeeded</operadora_mensagem>
    <id_librepag>1003840</id_librepag>
    <autorizacao_id>Z133937-000260004</autorizacao_id>
    <url_autenticacao></url_autenticacao>
    <token>6bab-460635e7-1c2ee1a2-d4e554a9-1a59</token>
    <last4>6662</last4>
    <mes>07</mes>
    <ano>2018</ano>
    <id_assinatura>1006</id_assinatura>
    <profile_id>123456</profile_id>
</retorno>
```

Parameter | Size | Type | Required | Description
--------- | ----- | ----- | ----------- | ---------
frequencia | 2 | number | yes | Used when creating a recurring transaction. This field should set the frequency of the intervals that will be charged.
intervalo | 5 | string | yes | Used when creating a recurring transaction. Sets the interval unit to be used. ('day', 'week' or 'month')
inicio | 10 | date | yes | Used when creating a recurring transaction. The first charge occurs on the day the recurrence is created. The next charges will occur on the day specified at the beginning + (frequency * interval). FORMAT: "DD / MM / YYYY"
valor_rec | 12 | decimal | no | Amount to be charged on the first maturity of the recurrence. It is not mandatory, if not informed, the value of the transaction (value) will be used.
assinatura_parcela | 2 | integer | no | Installment that will be used to collect the Subscription.
ciclos | 2 | number | no | Defines the number of recurring transaction cycles that will be performed.
trial | 1 | boolean | no  | If trial = 1 or true then the first charge (transaction value) will be R$ 1.00 in order to perform a validation transaction to generate the token.
trial_ciclos | 2 | number | no | Sets the number of recurring transaction cycles in trial period.
trial_frequencia | 2 | number | no | Defines the frequency at which each cycle of the recurrence will be executed. (Ex: Every 1 month, or, every 6 months).
trial_valor | 12 | decimal | no  | Sets the amount that will be charged during the trial period.

### Comments
The first charge occurs at the time of recurrence creation.

Ex: Creation of a monthly recurrence on 10/05/2016 and beginning = 01/06/2016.
The first charge will be made on 10/05/2016 and the next charge will occur on 01/07/2016. 01/06/2016 + 1 * month

### Examples of recurring transactions

**a)** frequencia = 1, intervalo = month, ciclos = 3, inicio = 01/01/2015
A recurring monthly charge will be created, which will start on 01/01/2015 and will be charged three times, ending on 01/03/2015.

**b)** frequencia = 3, intervalo = month, inicio = 01/01/2015
A recurring quarterly charge will be created, which will start on 01/01/2015 and will be charged indefinitely.


**c)** If you want to create a recurrence with a TRIAL period, the first charge with a value of R$ 1.00 is only authorized (not debit from the customer card), also send the trial = (1 or true) parameter, in this case it is necessary to inform the * value_rec * that will be charged on the start date.

**d)** If you want to create a recurrence with TRIAL period with a specific value during that period, it is mandatory to send the following parameters: * trial_ciclos, trial_frequencia and trial_valor *.

** Example: ** Charge R$ 10.00 membership, with a promotional period of 3 months of R$ 5.00, and in the 4 month charge the normal value of R$ 10.00. Please submit the following:

intevalo = 'month'<br/>
frequencia = 1<br/>
valor = 10.00<br/>
valor_rec = 10.00<br/>
trial_ciclos = 4 (adesão + 3 meses trial)<br/>
trial_frequencia = 1<br/>
trial_valor = 5.00<br/>

** Example: ** Charge R$ 1.00 in the trial (trial = true), with a promotional period of 3 months of R$ 5.00, and in the 4 month charge the normal value of R$ 10.00. Please submit the following:

intevalo = 'month'<br/>
frequencia = 1<br/>
trial = true<br/>
valor = 1.00<br/>
valor_rec = 10.00<br/>
trial_ciclos = 4 (adesão + 3 meses trial)<br/>
trial_frequencia = 1<br/>
trial_valor = 5.00<br/>

<aside class="warning">
<b>If the TRIAL parameter is set to true, a transaction of R$ 1.00 will be performed, only as approved (It will not generate a charge to the customer). This transaction is performed to validate the customer's card and create the recurrence token. This transaction should not be captured, but may, if desired, be canceled via API or Dashboard.</b>
</aside>

<aside class="info">
The <b>trial_ciclos</b> is calculated as follows: <i>first charge + desired period</i>, if you want 3 trial cycles, inform trial_ciclos = 4.
</aside>

### Important

<aside class="info">
<b>For purchases via OneClick and Subscriptions, it is necessary to contact the Acquirer and request the liberation of transactions without the need for the CVV (Card Security Code). This type of transaction does not report this data, therefore release is required.</b>
</aside>

When creating a recurring transaction, ** make sure ** to inform the profile_id in the return URL. (profile_id is a unique number that should be generated by the store and will be the recurrence reference for the store and iPag). The return URL for this case should have the following structure:

**HTTP://www.loja.com.br/controller/profile_id/<profile_id>**

**Example: http://sualoja.com.br/retornoipag/profile_id/123456**

<aside class="info">
The <b>profile_id</b> is a unique id that must be generated and controlled by the integrator, it will reference the recurring transaction.
When there is a payment update, iPag will try to send a POST with the updated Payment status. If iPag is unable to send the response (HTTP 200), it will try again after some time.
</aside>

<aside class="warning">
<b>If there is no endpoint (url_retorno) with profile_id informed  iPag will not be able to send the updated status of recurring payments.</b>
</aside>

## Return

iPag returns the following parameters via POST to the URL reported by the "url_backup" parameter.

If the return has been requested in XML, the same parameters will be returned, but in XML format.

```xml
<?xml version="1.0" encoding="utf-8" ?>
<retorno>
    <id_transacao>123456789</id_transacao>
    <valor>10.00</valor>
    <num_pedido>123456</num_pedido>
    <status_pagamento>8</status_pagamento>
    <mensagem_transacao>Transação Autorizada</mensagem_transacao>
    <metodo>visa</metodo>
    <operadora>cielo</operadora>
    <operadora_mensagem>Transação autorizada</operadora_mensagem>
    <id_librepag>12345</id_librepag>
    <autorizacao_id>123456</autorizacao_id>
    <redirect>false</redirect>
    <url_autenticacao>https://minhaloja.com.br/ipag/retorno</url_autenticacao>
</retorno>
```

Parameter | Description
--------- | ----------------
id_transacao | TID (Number issued by the acquirer to identify the transaction)
valor | Total Transaction Amount
num_pedido | Store order number
status_pagamento | Transaction status. See the possible values ​​in the Transaction Status section of this document
mensagem_transacao | Transaction message
metodo | Payment method used for the transaction. See the possible values ​​in the Methods section of this document
operadora | Acquirer where the transaction was performed
operadora_mensagem | Message sent by the Acquirer 
id_librepag |iIPag Transaction Id
autorizacao_id | Auth Id issued by the Acquirer
url_autenticacao | Valid debit card validation url, or billet print link


**Additional parameters when Recurrence (signature) or OneClick**

Parameters | Description
-----------|----------
token |  This is the token generated when the gera_token_cartao parameter is sent.
last4 | Refer to the last 4 digits of the card. It is only returned when the gera_token_cartao parameter is sent.
mes | Regarding the month of expiration of the card. It is only returned when the gera_token_cartao parameter is sent.
ano | Referring year of the card. It is only returned when the gera_token_cartao parameter is sent.
id_assinatura | Signature Id created by iPag.

## Operations

*Note that the first letter of each operation must be uppercase*

Operations | Methods | Description
--------- | ------ | --------
Pagamento | POST | /service/payment
Consulta | POST | /service/consult
Captura | POST | /service/capture
Cancela | POST | /service/cancel

## Methods
###Cards

**Method** | Type
-----------|--------
**visa** | credit
**mastercard** | credit
**diners** | credit
**amex** | credit
**elo** | credit
**discover** | credit
**hipercard** | credit
**hiper** | credit
**jcb** | credit
**aura** | credit
**visaelectron** | debit
**maestro** | debit

###Billet

Company | **Method** | Type
--------| -----------|--------
Santander | **boleto_banespasantander** | printed billet
Banco do Brasil | **boletobb** | printed billet
Zoop | **boletozoop** | printed billet
Itaú | **boletoitaushopline** | printed billet
Bradesco | **boletoshopfacil** | printed billet
Sicredi | **boletosicredi** | printed billet

###Transfer (Office Bank)

**Method** | Type
-----------|--------
**itaushopline** | Transfer and Billet
**bancobrasil** | Transfer


## Transaction Status

Code | Description
------- | --------------
1 | Iniciado
2 | Boleto impresso
3 | Cancelado
4 | Em análise
5 | Aprovado
6 | Aprovado valor parcial (Status Reservado pelo iPag)
7 | Recusado
8 | Aprovado e Capturado
9 | Chargeback
10 | Em Disputa
