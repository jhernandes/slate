#Signature Integration

##Consult Signature

**`GET`** `/service/v1?ctrl=assinatura&action=consultar&id_assinatura=<id_assinatura>`

> Example to Consult Signature

```shell
curl -X GET \
  'https://sandbox.ipag.com.br/service/v1?ctrl=assinatura&action=consultar&id_assinatura=105' \
  -H 'authorization: Basic am9uYXRoYW24NzFGMC00QzMyODdEMy0xQzE1N0VFMy1BQ0FFTEY2MD0yRkU0'
```

```php
<?php

$url = 'https://sandbox.ipag.com.br/service/v1?ctrl=assinatura&action=consultar&id_assinatura=105';
$username = <IPAG_ID>;
$password = <IPAG_KEY>;

$options  = array(
    'http' => array(
        'method'  => 'GET',
        'user_agent' => 'My Own Application',
        'header'  => "Authorization: Basic " . base64_encode("$username:$password")
    )
);
$context = stream_context_create($options);

$result = file_get_contents($url, null, $context);

print_r($result);
```

Parameter | value | type | Required | description
--------- | ----- | ----- | ----------- | ---------
ctrl | assinatura | string | yes | selected controller
action | consultar | string | yes | selected action
id_assinatura | Ex .: 1000 | string | yes / no | Subscription reference ID (required if you do not have profile_id)
profile_id | Eg .: 99282 | string | not | Reference id of the signature, created by the user (mandatory if you do not have signature_id)

##Response - Consult Signature

```xml
<?xml version="1.0" encoding="utf-8"?>
<retorno>
    <code>000</code>
    <message>success</message>
    <id>105</id>
    <profile_id>20170327084547</profile_id>
    <is_active>1</is_active>
    <description>Assinatura #20170327084547</description>
    <value>3.00</value>
    <billing_date>04</billing_date>
    <frequency>1</frequency>
    <interval>month</interval>
    <token>e738-d7d28e1b-c08be6c7-80823d54-5a5d</token>
    <credit_card>
        <bin>000000</bin>
        <last4>0004</last4>
        <expiry>10-2017</expiry>
        <brand>visa</brand>
    </credit_card>
    <billing>
        <payment_1>
            <number>1</number>
            <expiry_date>2017-03-27</expiry_date>
            <value>4.00</value>
            <is_payed>1</is_payed>
            <payed_value>4.00</payed_value>
            <payed_date>2017-03-27</payed_date>
            <description>Assinatura #20170327084547</description>
            <transaction>
                <installment_number>1</installment_number>
                <transaction_id>993921</transaction_id>
                <payed_value>4.00</payed_value>
                <payed_date>2017-03-27</payed_date>
            </transaction>
        </payment_1>
    </billing>
</retorno>
```


Parameter | description
--------- | ---------
retorno | *Container*
code | Return code (000 = success)
message | Return message
id | Signature ID created by iPag
profile_id | User-created signature ID
is_active | Current signature status (1 = active, 0 = inactive)
description | Subscription Description
value | Recurring amount charged in the period
billing_date | Due date
frequency | Frequency of charges
interval | Time difference for collections (day, week, month)
token | Signed token card token
credit_card | *Container*
bin | Credit Card Bin
last4 | Last4 credit card
expiry | Credit card expiration date
brand | Card Operator
billing | *Container*
payment_1 * | *Container* (Can be repeated)
number | Payment number
expiry_date | Payment Due
value | Payment Amount
is_payed | 1 = Payment, 0 = Not paid
payed_value | Amount paid
payed_date | Date payment was made
description | Payment Description
transaction | *Container*
installment_number | Number of installments chosen in payment
transaction_id | Paid transaction ID on iPag
payed_value | Amount paid on transaction
payed_date | Date the transaction was performed

<aside class = "notice">
    * For each payment made on the subscription will have a container <b> payment _ </b>
</aside>

## Activate Signature

`POST /service/v1`

> Example to Activate Sigature

```shell
curl --include --header "accept: application/xml" \
-u <IPAG_ID>:<IPAG_KEY> \
--url https://sandbox.ipag.com.br/service/v1 \
--request POST \
--data 'ctrl=assinatura&action=ativar&id_assinatura=1000'
```

```php
<?php
$curl = curl_init();

curl_setopt_array($curl, array(
  CURLOPT_URL => "https://sandbox.ipag.com.br/service/v1",
  CURLOPT_RETURNTRANSFER => true,
  CURLOPT_ENCODING => "",
  CURLOPT_USERPWD => "<IPAG_ID>:<IPAG_KEY>"
  CURLOPT_MAXREDIRS => 10,
  CURLOPT_TIMEOUT => 30,
  CURLOPT_CUSTOMREQUEST => "POST",
  CURLOPT_POSTFIELDS => "ctrl=assinatura&action=ativar&id_assinatura=1000",
  CURLOPT_HTTPHEADER => array(
    "accept: application/xml",
  ),
));

$response = curl_exec($curl);
$err = curl_error($curl);

curl_close($curl);

if ($err) {
  echo "cURL Error #:" . $err;
} else {
  echo $response;
}
```

Parameter | value | type | Required | description
--------- | ----- | ----- | ----------- | ---------
ctrl | assinatura | string | yes | selected controller
action | ativar | string | yes | selected action
id_assinatura | Ex .: 1000 | string | yes / no | Subscription reference ID (required if you do not have profile_id)
profile_id | Eg .: 99282 | string | not | Subscription reference ID, created by the user (mandatory if you do not have an id]

##Response - Activate Signature (XML)

```xml
<?xml version="1.0" encoding="utf-8"?>
<retorno>
    <code>000</code>
    <message>Assinatura ativada com sucesso.</message>
    <id>1000</id>
    <profile_id>10002000</profile_id>
    <is_active>1</is_active>
    <description>Assinatura #10002000</description>
    <value>1.00</value>
    <billing_date>04</billing_date>
    <frequency>1</frequency>
    <interval>month</interval>
    <token>4f25-15af1c4d-dfdc9d02-82da0a8e-9a76</token>
    <credit_card>
        <bin>412345</bin>
        <last4>6459</last4>
        <expiry>08-2021</expiry>
        <brand>visa</brand>
    </credit_card>
</retorno>
```

Parameter | description
--------- | ---------
retorno | *Container*
code | Return code (000 = success)
message | Return message
id | Signature ID created by iPag
profile_id | User-created signature ID
is_active | Current signature status (1 = active, 0 = inactive)
description | Subscription Description
value | Recurring amount charged in the period
billing_date | Due date
frequency | Frequency of charges
interval | Time difference for collections (day, week, month)
token | Signed token card token
credit_card | *Container*
bin | Credit Card Bin
last4 | Last4 credit card
expiry | Credit card expiration date
brand | Card Operator

## Inactivate Signature

`POST /service/v1`

> Example to Inactivate Signature

```shell
curl --include --header "accept: application/xml" \
-u <IPAG_ID>:<IPAG_KEY> \
--url https://sandbox.ipag.com.br/service/v1 \
--request POST \
--data 'ctrl=assinatura&action=inativar&id_assinatura=1000'
```

```php
<?php
$curl = curl_init();

curl_setopt_array($curl, array(
  CURLOPT_URL => "https://sandbox.ipag.com.br/service/v1",
  CURLOPT_RETURNTRANSFER => true,
  CURLOPT_ENCODING => "",
  CURLOPT_USERPWD => "<IPAG_ID>:<IPAG_KEY>"
  CURLOPT_MAXREDIRS => 10,
  CURLOPT_TIMEOUT => 30,
  CURLOPT_CUSTOMREQUEST => "POST",
  CURLOPT_POSTFIELDS => "ctrl=assinatura&action=inativar&id_assinatura=1000",
  CURLOPT_HTTPHEADER => array(
    "accept: application/xml",
  ),
));

$response = curl_exec($curl);
$err = curl_error($curl);

curl_close($curl);

if ($err) {
  echo "cURL Error #:" . $err;
} else {
  echo $response;
}
```

Parameter | value | type | Required | description
--------- | ----- | ----- | ----------- | ---------
ctrl | assinatura | string | yes | selected controller
action | inativar | string | yes | selected action
id_assinatura | Ex .: 1000 | string | yes / no | Subscription reference ID (required if you do not have profile_id)
profile_id | Eg .: 99282 | string | not | Subscription reference ID, created by the user (mandatory if you do not have an id]


## Response - Inactivate Signature (XML)

```xml
<?xml version="1.0" encoding="utf-8"?>
<retorno>
    <code>000</code>
    <message>Assinatura desativada com sucesso.</message>
    <id>1000</id>
    <profile_id>10002000</profile_id>
    <is_active>0</is_active>
    <description>Assinatura #10002000</description>
    <value>1.00</value>
    <billing_date>04</billing_date>
    <frequency>1</frequency>
    <interval>month</interval>
    <token>4f25-15af1c4d-dfdc9d02-82da0a8e-9a76</token>
    <credit_card>
        <bin>412345</bin>
        <last4>6459</last4>
        <expiry>08-2021</expiry>
        <brand>visa</brand>
    </credit_card>
</retorno>
```

Parameter | description
--------- | ---------
retorno | *Container*
code | Return code (000 = success)
message | Return message
id | Signature ID created by iPag
profile_id | User-created signature ID
is_active | Current signature status (1 = active, 0 = inactive)
description | Subscription Description
value | Recurring amount charged in the period
billing_date | Due date
frequency | Frequency of charges
interval | Time difference for collections (day, week, month)
token | Signed token card token
credit_card | *Container*
bin | Credit Card Bin
last4 | Last4 credit card
expiry | Credit card expiration date
brand | Card Operator

## Change Subscription Expiration Date

`POST /service/v1`

>Example to Change Subscription Due Date

```shell
curl --include --header "accept: application/xml" \
-u <IPAG_ID>:<IPAG_KEY> \
--url https://sandbox.ipag.com.br/service/v1 \
--request POST \
--data 'ctrl=assinatura&action=vencto&id_assinatura=1000&data=2017-02-10'
```

```php
<?php
$curl = curl_init();

curl_setopt_array($curl, array(
  CURLOPT_URL => "https://sandbox.ipag.com.br/service/v1",
  CURLOPT_RETURNTRANSFER => true,
  CURLOPT_ENCODING => "",
  CURLOPT_USERPWD => "<login_ipag>:<API_KEY>"
  CURLOPT_MAXREDIRS => 10,
  CURLOPT_TIMEOUT => 30,
  CURLOPT_CUSTOMREQUEST => "POST",
  CURLOPT_POSTFIELDS => "ctrl=assinatura&action=vencto&id_assinatura=1000&data=2017-02-10",
  CURLOPT_HTTPHEADER => array(
    "accept: application/xml",
  ),
));

$response = curl_exec($curl);
$err = curl_error($curl);

curl_close($curl);

if ($err) {
  echo "cURL Error #:" . $err;
} else {
  echo $response;
}
```
Parameter | value | type | Required | description
--------- | ----- | ----- | ----------- | ---------
ctrl | assinatura | string | yes | selected controller
action | vencto | string | yes | selected action
vencto | Ex .: 2017-02-10 | string | yes | New signature expiration date (format = yyyy-mm-dd)
id_assinatura | Ex .: 1000 | string | yes / no | Subscription reference ID (required if you do not have profile_id)
profile_id | Eg .: 99282 | string | not | Subscription reference ID, created by the user (mandatory if you do not have an id]

## Reply - Change Subscription Expiration Date (XML)

```xml
<?xml version="1.0" encoding="utf-8"?>
<retorno>
    <code>000</code>
    <message>Nova data de vencimento da Assinatura alterado com sucesso.</message>
    <id>1000</id>
    <profile_id>10002000</profile_id>
    <is_active>1</is_active>
    <description>Assinatura #10002000</description>
    <value>1.00</value>
    <billing_date>10</billing_date>
    <frequency>1</frequency>
    <interval>month</interval>
    <token>4f25-15af1c4d-dfdc9d02-82da0a8e-9a76</token>
    <credit_card>
        <bin>412345</bin>
        <last4>6459</last4>
        <expiry>08-2021</expiry>
        <brand>visa</brand>
    </credit_card>
</retorno>

```

Parameter | description
--------- | ---------
retorno | *Container*
code | Return code (000 = success)
message | Return message
id | Signature ID created by iPag
profile_id | User-created signature ID
is_active | Current signature status (1 = active, 0 = inactive)
description | Subscription Description
value | Recurring amount charged in the period
billing_date | Due date
frequency | Frequency of charges
interval | Time difference for collections (day, week, month)
token | Signed token card token
credit_card | *Container*
bin | Credit Card Bin
last4 | Last4 credit card
expiry | Credit card expiration date
brand | Card Operator

## Change Subscription Value

`POST /service/v1`

> Exemplo para Alterar Valor da Assinatura

```shell
curl --include --header "accept: application/xml" \
-u <IPAG_ID>:<IPAG_KEY> \
--url https://sandbox.ipag.com.br/service/v1 \
--request POST \
--data 'ctrl=assinatura&action=valor&id_assinatura=1000&valor=99.00'
```

```php
<?php
$curl = curl_init();

curl_setopt_array($curl, array(
  CURLOPT_URL => "https://sandbox.ipag.com.br/service/v1",
  CURLOPT_RETURNTRANSFER => true,
  CURLOPT_ENCODING => "",
  CURLOPT_USERPWD => "<IPAG_ID>:<IPAG_KEY>"
  CURLOPT_MAXREDIRS => 10,
  CURLOPT_TIMEOUT => 30,
  CURLOPT_CUSTOMREQUEST => "POST",
  CURLOPT_POSTFIELDS => "ctrl=assinatura&action=valor&id_assinatura=1000&valor=99.00",
  CURLOPT_HTTPHEADER => array(
    "accept: application/xml",
  ),
));

$response = curl_exec($curl);
$err = curl_error($curl);

curl_close($curl);

if ($err) {
  echo "cURL Error #:" . $err;
} else {
  echo $response;
}
```

Parameter | value | type | Required | description
--------- | ----- | ----- | ----------- | ---------
ctrl | assinatura | string | yes | selected controller
action | valor | string | yes | selected action
value | Ex .: 1.99 | string | yes | Amount to be changed (1.00 format)
id_assinatura | Ex .: 1000 | string | yes / no | Subscription reference ID (required if you do not have profile_id)
profile_id | Eg .: 99282 | string | not | Subscription reference ID, created by the user (mandatory if you do not have an id]

## Response - Change Signature Value (XML)

```xml
<?xml version="1.0" encoding="utf-8"?>
<retorno>
    <code>000</code>
    <message>Novo valor da assinatura alterado com sucesso.</message>
    <id>1000</id>
    <profile_id>10002000</profile_id>
    <is_active>1</is_active>
    <description>Assinatura #10002000</description>
    <value>99.00</value>
    <billing_date>04</billing_date>
    <frequency>1</frequency>
    <interval>month</interval>
    <token>4f25-15af1c4d-dfdc9d02-82da0a8e-9a76</token>
    <credit_card>
        <bin>412345</bin>
        <last4>6459</last4>
        <expiry>08-2021</expiry>
        <brand>visa</brand>
    </credit_card>
</retorno>
```

Parameter | description
--------- | ---------
retorno | *Container*
code | Return code (000 = success)
message | Return message
id | Signature ID created by iPag
profile_id | User-created signature ID
is_active | Current signature status (1 = active, 0 = inactive)
description | Subscription Description
value | Recurring amount charged in the period
billing_date | Due date
frequency | Frequency of charges
interval | Time difference for collections (day, week, month)
token | Signed token card token
credit_card | *Container*
bin | Credit Card Bin
last4 | Last4 credit card
expiry | Credit card expiration date
brand | Card Operator

## Generate Unique Token for Signature

`POST /service/v1`

> Example to Generate Unique Token for Signature

```shell
curl --include --header "accept: application/xml" \
-u <IPAG_ID>:<IPAG_KEY> \
--url https://sandbox.ipag.com.br/service/v1 \
--request POST \
--data 'ctrl=token&action=novo&numero_cartao=4024007109760958&nome_cartao=fulano
&mes_cartao=10&ano_cartao=2021'
```

```php
<?php
$curl = curl_init();

curl_setopt_array($curl, array(
  CURLOPT_URL => "https://sandbox.ipag.com.br/service/v1",
  CURLOPT_RETURNTRANSFER => true,
  CURLOPT_ENCODING => "",
  CURLOPT_USERPWD => "<IPAG_ID>:<IPAG_KEY>"
  CURLOPT_MAXREDIRS => 10,
  CURLOPT_TIMEOUT => 30,
  CURLOPT_CUSTOMREQUEST => "POST",
  CURLOPT_POSTFIELDS => "ctrl=token&action=novo&numero_cartao=4024007109760958&nome_cartao=fulano&mes_cartao=10&ano_cartao=2021",
  CURLOPT_HTTPHEADER => array(
    "accept: application/xml",
  ),
));

$response = curl_exec($curl);
$err = curl_error($curl);

curl_close($curl);

if ($err) {
  echo "cURL Error #:" . $err;
} else {
  echo $response;
}
```

Parameter | value | type | Required | description
--------- | ----- | ----- | ----------- | ---------
ctrl | token | string | yes | selected controller
action | novo | string | yes | selected action
numero_cartao | Ex.: 4024007109760958 | string | yes | card number
nome_cartao | Ex. string | yes | Card holder name
mes_cartao | Ex .: 10 | string | yes | Card winning month
ano_cartao | Ex .: 2021 | string | yes | Card expiration date

## Reply - Generate Unique Token for Signature (XML)

```xml
<?xml version="1.0" encoding="utf-8"?>
<retorno>
    <code>000</code>
    <message>Token criado com sucesso.</message>
    <token>fa59-7b796cff-ed8b9bca-f8600ac9-1328</token>
</retorno>
```

Parameter | description
--------- | ---------
retorno | *Container*
code | Return code (000 = success)
message | Return message
token | Single Card Token


##Change Subscription Token

`POST /service/v1`

> Example to Change Subscription Token

```shell
curl --include --header "accept: application/xml" \
-u <IPAG_ID>:<IPAG_KEY> \
--url https://sandbox.ipag.com.br/service/v1 \
--request POST \
--data 'ctrl=assinatura&action=token&id_assinatura=1000&token=fa59-7b796cff-ed8b9bca-f8600ac9-1328'
```

```php
<?php
$curl = curl_init();

curl_setopt_array($curl, array(
  CURLOPT_URL => "https://sandbox.ipag.com.br/service/v1",
  CURLOPT_RETURNTRANSFER => true,
  CURLOPT_ENCODING => "",
  CURLOPT_USERPWD => "<IPAG_ID>:<IPAG_KEY>"
  CURLOPT_MAXREDIRS => 10,
  CURLOPT_TIMEOUT => 30,
  CURLOPT_CUSTOMREQUEST => "POST",
  CURLOPT_POSTFIELDS => "ctrl=assinatura&action=token&id_assinatura=1000&token=fa59-7b796cff-ed8b9bca-f8600ac9-1328",
  CURLOPT_HTTPHEADER => array(
    "accept: application/xml",
  ),
));

$response = curl_exec($curl);
$err = curl_error($curl);

curl_close($curl);

if ($err) {
  echo "cURL Error #:" . $err;
} else {
  echo $response;
}
```

Parameter | value | type | Required | description
--------- | ----- | ----- | ----------- | ---------
ctrl | assinatura | string | yes | selected controller
action | token | string | yes | selected action
token | Ex .: fa59-7b796cff-ed8b9bca-f8600ac9-1328 | string | yes | Previously generated card Token
id_assinatura | Ex .: 1000 | string | yes / no | Subscription reference ID (required if you do not have profile_id)
profile_id | Eg .: 99282 | string | not | Subscription reference ID, created by the user (mandatory if you do not have an id]

## Response - Change Signature Token (XML)

```xml
<?xml version="1.0" encoding="utf-8"?>
<retorno>
    <code>000</code>
    <message>Token alterado com sucesso.</message>
    <id>1000</id>
    <profile_id>10002000</profile_id>
    <is_active>1</is_active>
    <description>Assinatura #10002000</description>
    <value>99.00</value>
    <billing_date>04</billing_date>
    <frequency>1</frequency>
    <interval>month</interval>
    <token>fa59-7b796cff-ed8b9bca-f8600ac9-1328</token>
    <credit_card>
        <bin>412345</bin>
        <last4>6459</last4>
        <expiry>08-2021</expiry>
        <brand>visa</brand>
    </credit_card>
</retorno>
```


Parameter | description
--------- | ---------
retorno | *Container*
code | Return code (000 = success)
message | Return message
id | Signature ID created by iPag
profile_id | User-created signature ID
is_active | Current signature status (1 = active, 0 = inactive)
description | Subscription Description
value | Recurring amount charged in the period
billing_date | Due date
frequency | Frequency of charges
interval | Time difference for collections (day, week, month)
token | Signed token card token
credit_card | *Container*
bin | Credit Card Bin
last4 | Last4 credit card
expiry | Credit card expiration date
brand | Card Operator

## Change Amount and / or Expiry of a Subscription Installment

`POST /service/v1`

> Example to Change Amount and / or Expiry of a Subscription Installment

```shell
curl --include --header "accept: application/xml" \
-u <IPAG_ID>:<IPAG_KEY> \
--url https://sandbox.ipag.com.br/service/v1 \
--request POST \
--data 'ctrl=assinatura&action=parcela&id_assinatura=1000&valor=19.00&vencto=2017-12-25'
```

```php
<?php
$curl = curl_init();

curl_setopt_array($curl, array(
  CURLOPT_URL => "https://sandbox.ipag.com.br/service/v1",
  CURLOPT_RETURNTRANSFER => true,
  CURLOPT_ENCODING => "",
  CURLOPT_USERPWD => "<IPAG_ID>:<IPAG_KEY>"
  CURLOPT_MAXREDIRS => 10,
  CURLOPT_TIMEOUT => 30,
  CURLOPT_CUSTOMREQUEST => "POST",
  CURLOPT_POSTFIELDS => "ctrl=assinatura&action=parcela&id_assinatura=1000&valor=19.00&vencto=2017-12-25",
  CURLOPT_HTTPHEADER => array(
    "accept: application/xml",
  ),
));

$response = curl_exec($curl);
$err = curl_error($curl);

curl_close($curl);

if ($err) {
  echo "cURL Error #:" . $err;
} else {
  echo $response;
}
```

Parameter | value | type | Required | description
--------- | ----- | ----- | ----------- | ---------
ctrl | assinatura | string | yes | selected controller
action | parcela | string | yes | selected action
parcela | Ex: 1 | int | yes | Installment number to be changed
valor | Ex .: 19.99 | double | yes / no | Installment Value
vencto | Ex .: 2017-12-25 (YYYY-MM-DD) | string | yes / no | Date of payment of the installment
profile_id | Eg .: 99282 | string | no | Subscription reference ID, created by the user (mandatory if you do not have an id]
id_assinatura | Ex .: 1000 | int | no / yes | Subscription reference ID created by iPag (required if you do not have profile_id)

## Response - Change Value and / or Expiry of a Subscription Installment (XML)

```xml
<?xml version="1.0" encoding="utf-8"?>
<retorno>
    <code>000</code>
    <message>success</message>
    <id>1000</id>
    <profile_id/>
    <is_active>1</is_active>
    <description>Assinatura #</description>
    <value>1.50</value>
    <billing_date>12</billing_date>
    <frequency>1</frequency>
    <interval>month</interval>
    <token>a440-d1bc21ae-c00dc795-f088acb6-00bd</token>
    <credit_card>
        <bin>406655</bin>
        <last4>8107</last4>
        <expiry>12-2017</expiry>
        <brand>visa</brand>
    </credit_card>
    <billing>
        <payment_1>
            <number>1</number>
            <expiry_date>2017-12-06</expiry_date>
            <value>1.00</value>
            <is_payed>1</is_payed>
            <payed_value>1.00</payed_value>
            <payed_date>2017-12-06</payed_date>
            <description>Assinatura # 1</description>
            <transaction>
                <installment_number>1</installment_number>
                <transaction_id>995117</transaction_id>
                <payed_value>1.00</payed_value>
                <payed_date>2017-12-06</payed_date>
            </transaction>
        </payment_1>
        <payment_2>
            <number>2</number>
            <expiry_date>2017-12-25</expiry_date>
            <value>19.99</value>
            <is_payed>0</is_payed>
            <payed_value>0.00</payed_value>
            <payed_date>0000-00-00</payed_date>
            <description>Assinatura # 2</description>
            <transaction/>
        </payment_2>
    </billing>
</retorno>
```


Parameter | description
--------- | ---------
retotno | *Container*
codigo | Return code (000 = success)
message | Return message
id | Signature ID created by iPag
profile_id | User-created signature ID
is_active | Current signature status (1 = active, 0 = inactive)
description | Subscription Description
value | Recurring amount charged in the period
billing_date | Due date
frequency | Frequency of charges
interval | Time difference for collections (day, week, month)
token | Signed token card token
credit_card | *Container*
bin | Credit Card Bin
last4 | Last4 credit card
expiry | Credit card expiration date
brand | Card Operator
billing | *Container*
payment_1 * | *Container* (Can be repeated)
number | Payment number
expiry_date | Payment Due
value | Payment Amount
is_payed | 1 = Payment, 0 = Not paid
payed_value | Amount paid
payed_date | Date payment was made
description | Payment Description
transaction | *Container*
installment_number | Number of installments chosen in payment
transaction_id | Paid transaction ID on iPag
payed_value | Amount paid on transaction
payed_date | Date the transaction was performed

<aside class="notice">
   * For each payment / installment in the subscription will have a container <b> payment _ </b>
</aside>

## Error Table (Signature Integration)

HTTP Code | Code | Message
---- | ---- | ---------
404 | 001 | Signature not found.

406 | 002 | Value not informed in requisition.
406 | 002 | Portion number not entered in requisition.
406 | 002 | Portion not found.

406 | 003 | Date not informed or not in yyyy-mm-dd format.
404 | 004 | Action is not valid.
406 | 005 | Token is not valid.
406 | 006 | id_name or profile_id not entered in the request.
406 | 096 | Invalid card number.
406 | 095 | Overdue card or invalid informed date.
500 | 098 | Internal Server Error