# (Marketplace) - Cadastrar Vendedor

## Criando um Seller & Parceiro Marketplace

`POST /service/v1`

## Criando um vendedor do tipo Pessoa Jurídica (Business Seller)

> Exemplo de integração

```php
<?php

$url = 'https://sandbox.ipag.com.br/service/v1';

$content = http_build_query(array(
    "ctrl"                 => 'zoopbusiness',
    "business_name"        => 'Test Co.',
    "business_phone"       => '1187654321',
    "business_email"       => 'test@business.com',
    "business_description" => 'A test business',
    "business_cnpj"        => '29654759000170',
    "address_street"       => 'P Sherman',
    "address_number"       => '42',
    "address_complement"   => '',
    "address_neighborhood" => 'Wallaby Way',
    "address_postalcode"   => '01000000',
    "address_city"         => 'Sydney',
    "address_state"        => 'SP',
    "owner_firstname"      => 'Test',
    "owner_lastname"       => 'Seller',
    "owner_phone"          => '1112345678',
    "owner_email"          => 'test@owner.com',
    "owner_cpf"            => '45237426182',
    "owner_birthdate"      => '1987-11-11',
    "bank_name"            => 'Test Seller',
    "bank_cpf"             => '45237426182',
    //Se informado cpf não informar cnpj
    "bank_cnpj"            => '29654759000170',
    "bank_code"            => '341',
    "bank_agencynumber"    => '1234',
    "bank_accountnumber"   => '12345-4'
));

$username = 'SEU_LOGIN';
$password = 'API_KEY';

$context = stream_context_create(array(
    'http' => array(
        'method'  => 'POST',
        'header'  => "Authorization: Basic " . base64_encode("$username:$password"),
        'content' => $content,
    )
));

$result = file_get_contents($url, null, $context);

echo print_r($result, true);
```

```shell
curl --request POST \
  --url https://sandbox.ipag.com.br/service/v1 \
  --header 'authorization: Basic aaaaaYXRoYW46REM4QS00QzE2OUM5NS1EQTZBRUY2OC0wRkQ2RDMy00001' \
  --header 'content-type: application/x-www-form-urlencoded' \
  --data 'ctrl=zoopbusiness&owner_phone=1112345678&owner_cpf=64028478286&owner_email=test%40owner.com&owner_birthdate=1987-11-11&owner_firstname=Test&owner_lastname=Seller&address_street=P%20Sherman&address_number=42&address_neighborhood=Wallaby%20Way&address_postalcode=01000000&address_city=Sydney&address_state=SP&bank_name=Test%20Seller&bank_cnpj=77441655000120&bank_code=341&bank_agencynumber=1234&bank_accountnumber=12345-4&business_name=Test%20Co.&business_phone=1187654321&business_email=test%40business.com&business_description=A%20test%20business&business_cnpj=05533862000101'
```

Campo |  tipo | Obrigatório | Descrição
------| ----- | ----------- | ---------
ctrl  | string | sim | Enviar: **'zoopbusiness'**
business_name | string | sim | Nome Fantasia da Empresa
business_phone | string | sim | Telefone da Empresa, DDD+Telefone mínimo de 10 digitos e máximo de 11 digitos
business_email | string | sim | Email de contato da Empresa
business_description | string | sim | Descrição da Empresa
business_cnpj | string | sim | CNPJ da Empresa
address_street | string | sim | Endereço da Empresa (Rua, Av.)
address_number | string | sim | Número do endereço
address_complement | string | não | Complemento do endereço
address_neighborhood | string | sim | Bairro do endereço
address_postalcode | string | sim | CEP do endereço (8 digitos)
address_city | string | sim | Cidade
address_state | string | sim  | Estado (2 digitos)
owner_firstname | string | sim | Primeiro nome do Sócio responsável pela Empresa
owner_lastname | string | sim | Último nome do Sócio responsável pela Empresa
owner_phone | string | sim | Telefone de contato direto com o sócio
owner_email | string | sim | E-mail do Sócio responsável
owner_cpf | string | sim | CPF do Sócio responsável pela Empresa
owner_birthdate | string | não | Data de Nascimento do Sócio responsável
bank_name | string | não * | Titular da Conta Bancária
bank_cpf | string | não * | CPF do Titular da Conta caso esteja em nome de Pessoa Física
bank_cnpj | string | não * | CNPJ do Titular da Conta caso esteja em nome de Pessoa Jurídica
bank_code | string | não * | [Código do Banco (3 digitos)](https://pt.wikipedia.org/wiki/Lista_de_bancos_do_Brasil)
bank_agencynumber | string | não * | Número da Agência Bancária
bank_accountnumber | string | não * | Número da Conta Bancária

* Se deseja vincular uma conta bancária deve ser informado todos os campos *bank*

### Retorno **Business Seller**

> Retorno (XML)

```xml
<?xml version="1.0" encoding="utf-8"?>
<retorno>
    <ipag_id>8dc2dbf71518482a859e6057359aaa46</ipag_id>
    <seller_id>8dc2dbf71518482a859e6057359aaa46</seller_id>
    <owner>
        <name>Test Seller</name>
        <email>test@owner.com</email>
        <birthdate>1987-11-11</birthdate>
        <phone>1112345678</phone>
        <cpf>45237426182</cpf>
    </owner>
    <address>
        <street>P Sherman</street>
        <number>42</number>
        <complement/>
        <neighborhood>Wallaby Way</neighborhood>
        <city>Sydney</city>
        <state>SP</state>
        <postalcode>01000000</postalcode>
        <countrycode>BR</countrycode>
    </address>
    <business>
        <name>Test Co.</name>
        <phone>1187654321</phone>
        <email>test@business.com</email>
        <website/>
        <description>A test business</description>
        <facebook/>
        <twitter/>
        <cnpj>29654759000170</cnpj>
    </business>
    <bank>
        <code>341</code>
        <bankname>Itaú Unibanco S.A.</bankname>
        <name>Test Seller</name>
        <agencynumber>1234</agencynumber>
        <accountnumber>123454</accountnumber>
        <document>45237426182</document>
    </bank>
</retorno>
```

Campo | Descrição
------|----------
retorno | Container
ipag_id | id do Parceiro Marketplace no iPag
seller_id | Seller Id do Vendedor na Zoop
owner| Container
owner[name] | Nome do sócio
owner[email] | Email do sócio
owner[birthdate] | Data de nascimento do sócio
owner[phone] | Telefone do sócio
owner[cpf] | CPF do sócio
address | Container
address[street] | Endereço da Empresa
address[number] | Número do endereço
address[complement] | Complemento
address[neighborhood] | Bairro
address[city] | Cidade
address[state] | Estado
address[postacode] | CEP
address[countrycode] | País
business | Container
business[name] | Nome fantasia
business[phone] | Telefone
business[email] | Email da Empresa
business[website] | Website da Empresa
business[description] | Descrição da Empresa
business[facebook] | Facebook da Empresa
business[twitter] | Twitter da Empresa
business[cnpj] | CNPJ da Empresa
bank | Container
bank[code] | Código do banco
bank[bankname] | Nome do Banco
bank[name] | Nome do Titular
bank[agencynumber] | Número da Agência
bank[accountnumber] | Número da Conta
bank[document] | Documento do Titular

## Criando um vendedor do tipo Pessoa Física (Individual Seller)

> Exemplo de integração

```php
<?php

$url = 'https://sandbox.ipag.com.br/service/v1';

$content = http_build_query(array(
    "ctrl"                 => 'zoopindividual',
    "address_street"       => 'P Sherman',
    "address_number"       => '42',
    "address_complement"   => '',
    "address_neighborhood" => 'Wallaby Way',
    "address_postalcode"   => '01000000',
    "address_city"         => 'Sydney',
    "address_state"        => 'SP',
    "owner_firstname"      => 'Test',
    "owner_lastname"       => 'Seller',
    "owner_phone"          => '1112345678',
    "owner_email"          => 'test@owner.com',
    "owner_cpf"            => '45237426182',
    "owner_birthdate"      => '1987-11-11',
    "bank_name"            => 'Test Seller',
    "bank_cpf"             => '45237426182',
    "bank_code"            => '341',
    "bank_agencynumber"    => '1234',
    "bank_accountnumber"   => '12345-4'
));

$username = 'SEU_LOGIN_IPAG';
$password = 'API_KEY';

$context = stream_context_create(array(
    'http' => array(
        'method'  => 'POST',
        'header'  => "Authorization: Basic " . base64_encode("$username:$password"),
        'content' => $content,
    )
));

$result = file_get_contents($url, null, $context);

echo print_r($result, true);
```

```shell
curl --request POST \
  --url https://sandbox.ipag.com.br/service/v1 \
  --header 'authorization: Basic aaaaaYXRoYW46REM4QS00QzE2OUM5NS1EQTZBRUY2OC0wRkQ2RDMy00001' \
  --header 'content-type: application/x-www-form-urlencoded' \
  --data 'ctrl=zoopindividual&owner_phone=1112345678&owner_cpf=08935742449&owner_email=test%40owner.com&owner_birthdate=1987-11-11&owner_firstname=Test&owner_lastname=Seller&address_street=P%20Sherman&address_number=42&address_neighborhood=Wallaby%20Way&address_postalcode=01000000&address_city=Sydney&address_state=SP&bank_name=Test%20Seller&bank_cnpj=77441655000120&bank_code=341&bank_agencynumber=1234&bank_accountnumber=12345-4'
```

Campo |  tipo | Obrigatório | Descrição
------| ----- | ----------- | ---------
ctrl  | string | sim | Enviar: **'zoopindividual'**
address_street | string | sim | Endereço da Empresa (Rua, Av.)
address_number | string | sim | Número do endereço
address_complement | string | não | Complemento do endereço
address_neighborhood | string | sim | Bairro do endereço
address_postalcode | string | sim | CEP do endereço (8 digitos)
address_city | string | sim | Cidade
address_state | string | sim  | Estado (2 digitos)
owner_firstname | string | sim | Primeiro nome do Sócio responsável pela Empresa
owner_lastname | string | sim | Último nome do Sócio responsável pela Empresa
owner_phone | string | sim | Telefone de contato direto com o sócio
owner_email | string | sim | E-mail do Sócio responsável
owner_cpf | string | sim | CPF do Sócio responsável pela Empresa
owner_birthdate | string | não | Data de Nascimento do Sócio responsável
bank_name | string | não * | Titular da Conta Bancária
bank_cpf | string | não * | CPF do Titular da Conta caso esteja em nome de Pessoa Física
bank_cnpj | string | não * | CNPJ do Titular da Conta caso esteja em nome de Pessoa Jurídica
bank_code | string | não * | [Código do Banco (3 digitos)](https://pt.wikipedia.org/wiki/Lista_de_bancos_do_Brasil)
bank_agencynumber | string | não * | Número da Agência Bancária
bank_accountnumber | string | não * | Número da Conta Bancária

* Se deseja vincular uma conta bancária deve ser informado todos os campos *bank*

### Retorno **Business Individual**

> Retorno (XML)

```xml
<?xml version="1.0" encoding="utf-8"?>
<retorno>
    <ipag_id>79d6c43a637a40cfa6aaabff1af0c5cc</ipag_id>
    <seller_id>79d6c43a637a40cfa6aaabff1af0c5cc</seller_id>
    <owner>
        <name>Test Seller</name>
        <email>test@owner.com</email>
        <birthdate>1987-11-11</birthdate>
        <phone>1112345678</phone>
        <cpf>78431204320</cpf>
    </owner>
    <address>
        <street>P Sherman</street>
        <number>42</number>
        <complement/>
        <neighborhood>Wallaby Way</neighborhood>
        <city>Sydney</city>
        <state>SP</state>
        <postalcode>01000000</postalcode>
        <countrycode>BR</countrycode>
    </address>
    <bank>
        <code>341</code>
        <bankname>Itaú Unibanco S.A.</bankname>
        <name>Test Seller</name>
        <agencynumber>1234</agencynumber>
        <accountnumber>123454</accountnumber>
        <document>78431204320</document>
    </bank>
</retorno>
```

Campo | Descrição
------|----------
retorno | Container
ipag_id | id do Parceiro Marketplace no iPag
seller_id | Seller Id do Vendedor na Zoop
owner| Container
owner[name] | Nome do sócio
owner[email] | Email do sócio
owner[birthdate] | Data de nascimento do sócio
owner[phone] | Telefone do sócio
owner[cpf] | CPF do sócio
address | Container
address[street] | Endereço da Empresa
address[number] | Número do endereço
address[complement] | Complemento
address[neighborhood] | Bairro
address[city] | Cidade
address[state] | Estado
address[postacode] | CEP
address[countrycode] | País
bank | Container
bank[code] | Código do banco
bank[bankname] | Nome do Banco
bank[name] | Nome do Titular
bank[agencynumber] | Número da Agência
bank[accountnumber] | Número da Conta
bank[document] | Documento do Titular

## Criando um banco (Bank)

> Exemplo de integração

```shell
curl -X POST \
https://sandbox.ipag.com.br/service/v1 \
-H 'authorization: Basic aaaaaYXRoYW46REM4QS00QzE2OUM5NS1EQTZBRUY2OC0wRkQ2RDMy00001' \
-F ctrl=zoopbank \
-F bank_name=Teste \
-F bank_code=341 \
-F bank_agencynumber=1234 \
-F bank_accountnumber=12345-4
```

```php
<?php

$url = 'https://sandbox.ipag.com.br/service/v1';

$content = http_build_query(array(
    "ctrl"               => 'zoopbank',
    "bank_name"          => 'Frank Mark'
    "bank_cpf"           => '79999338801'
    "bank_code"          => '341'
    "bank_agencynumber"  => '1000'
    "bank_accountnumber" => '0110233-3',
));

$username = 'SEU_LOGIN';
$password = 'API_KEY';

$context = stream_context_create(array(
    'http' => array(
        'method'  => 'POST',
        'header'  => "Authorization: Basic " . base64_encode("$username:$password"),
        'content' => $content,
    )
));

$result = file_get_contents($url, null, $context);

echo print_r($result, true);
```

Campo |  tipo | Obrigatório | Descrição
------| ----- | ----------- | ---------
ctrl  | string | sim | Enviar: **'zoopbank'**
bank_name | string | sim | Titular da Conta Bancária
bank_cpf | string | não * | CPF do Titular da Conta caso esteja em nome de Pessoa Física
bank_cnpj | string | não * | CNPJ do Titular da Conta caso esteja em nome de Pessoa Jurídica
bank_code | string | sim | [Código do Banco (3 digitos)](https://pt.wikipedia.org/wiki/Lista_de_bancos_do_Brasil)
bank_agencynumber | string | sim | Número da Agência Bancária
bank_accountnumber | string | sim | Número da Conta Bancária

* É necessário enviar CPF ou CNPJ.

### Retorno (Bank)

> Retorno (XML)

```xml
<?xml version="1.0" encoding="utf-8"?>
<retorno>
    <bank_id>313f5e0174df42548e8eae97c202f4a7</bank_id>
    <bank>
        <code>341</code>
        <bankname>Itaú Unibanco S.A.</bankname>
        <name>Teste</name>
        <agencynumber>1234</agencynumber>
        <accountnumber>123454</accountnumber>
        <document/>
    </bank>
</retorno>
```

Campo | Descrição
------|----------
retorno | Container
bank_id | ID do banco
bank | Container
bank[code] | Código do banco
bank[bankname] | Nome do Banco
bank[name] | Nome do Titular
bank[agencynumber] | Número da Agência
bank[accountnumber] | Número da Conta
bank[document] | Documento do Titular

## Vincular Banco ao Vendedor (Sync Bank)

> Exemplo de integração

```shell
curl -X POST \
https://sandbox.ipag.com.br/service/v1 \
-H 'authorization: Basic aaaaaYXRoYW46REM4QS00QzE2OUM5NS1EQTZBRUY2OC0wRkQ2RDMy00001' \
-F ctrl=zoopsyncbank \
-F bank_id=313f5e0174df42548e8eae97c202f4a7 \
-F seller_id=31883d1a2bbe4b728f3fde175b74447c
```

```php
<?php

$url = 'https://sandbox.ipag.com.br/service/v1';

$content = http_build_query(array(
    "ctrl"      => 'zoopsyncbank',
    "bank_id"   => '721bee64eb5943559791c14f4207e389'
    "seller_id" => '31883d1a2bbe4b728f3fde175b74447c'
));

$username = 'SEU_LOGIN';
$password = 'API_KEY';

$context = stream_context_create(array(
    'http' => array(
        'method'  => 'POST',
        'header'  => "Authorization: Basic " . base64_encode("$username:$password"),
        'content' => $content,
    )
));

$result = file_get_contents($url, null, $context);

echo print_r($result, true);
```

Campo |  tipo | Obrigatório | Descrição
------| ----- | ----------- | ---------
ctrl  | string | sim | Enviar: **'zoopsyncbank'**
bank_id | string | sim | ID do Banco
seller_id | string | sim | ID do Seller

### Retorno Sync Bank

> Retorno (XML)

```xml
<?xml version="1.0" encoding="utf-8"?>
<retorno>
    <ipag_id>31883d1a2bbe4b728f3fde175b74447c</ipag_id>
    <seller_id>31883d1a2bbe4b728f3fde175b74447c</seller_id>
    <seller_status>pending</seller_status>
    <owner>
        <name>Test Seller</name>
        <email>test@owner.com</email>
        <birthdate>1987-11-11</birthdate>
        <phone>1112345678</phone>
        <cpf>71787788490</cpf>
    </owner>
    <address>
        <street>P Sherman</street>
        <number>42</number>
        <complement/>
        <neighborhood>Wallaby Way</neighborhood>
        <city>Sydney</city>
        <state>SP</state>
        <postalcode>01000000</postalcode>
        <countrycode>BR</countrycode>
    </address>
    <bank>
        <code>341</code>
        <bankname>Itaú Unibanco S.A.</bankname>
        <name>Teste</name>
        <agencynumber>1234</agencynumber>
        <accountnumber>123454</accountnumber>
        <document/>
    </bank>
</retorno>
```

Campo | Descrição
------|----------
retorno | Container
ipag_id | id do Parceiro Marketplace no iPag
seller_id | Seller Id do Vendedor na Zoop
owner| Container
owner[name] | Nome do sócio
owner[email] | Email do sócio
owner[birthdate] | Data de nascimento do sócio
owner[phone] | Telefone do sócio
owner[cpf] | CPF do sócio
address | Container
address[street] | Endereço da Empresa
address[number] | Número do endereço
address[complement] | Complemento
address[neighborhood] | Bairro
address[city] | Cidade
address[state] | Estado
address[postacode] | CEP
address[countrycode] | País
bank | Container
bank[code] | Código do banco
bank[bankname] | Nome do Banco
bank[name] | Nome do Titular
bank[agencynumber] | Número da Agência
bank[accountnumber] | Número da Conta
bank[document] | Documento do Titular

## Consultando um vendedor (Get Seller)

> Exemplo de integração


```shell
curl -X POST \
https://sandbox.ipag.com.br/service/v1 \
-H 'authorization: Basic aaaaaYXRoYW46REM4QS00QzE2OUM5NS1EQTZBRUY2OC0wRkQ2RDMy00001' \
-F ctrl=zoopgetseller \
-F seller_id=4c2048c8c4414cd0abd29ad7716fee03
```

```php
<?php

$url = 'https://sandbox.ipag.com.br/service/v1';

$content = http_build_query(array(
    "ctrl"      => 'zoopgetseller',
    "seller_id" => '4c2048c8c4414cd0abd29ad7716fee03',
));

$username = 'SEU_LOGIN';
$password = 'API_KEY';

$context = stream_context_create(array(
    'http' => array(
        'method'  => 'POST',
        'header'  => "Authorization: Basic " . base64_encode("$username:$password"),
        'content' => $content,
    )
));

$result = file_get_contents($url, null, $context);

echo print_r($result, true);
```

Campo |  tipo | Obrigatório | Descrição
------| ----- | ----------- | ---------
ctrl  | string | sim | Enviar: **'zoopgetseller'**
seller_id | string | sim | ID do Zoop Seller

### Retorno da consulta

> Retorno (XML)

```xml
<?xml version="1.0" encoding="utf-8"?>
<retorno>
    <ipag_id>4c2048c8c4414cd0abd29ad7716fee03</ipag_id>
    <seller_id>4c2048c8c4414cd0abd29ad7716fee03</seller_id>
    <seller_status>pending</seller_status>
    <owner>
        <name>Test Seller</name>
        <email>test@owner.com</email>
        <birthdate>1987-11-11</birthdate>
        <phone>1112345678</phone>
        <cpf>27873555950</cpf>
    </owner>
    <address>
        <street>P Sherman</street>
        <number>42</number>
        <complement/>
        <neighborhood>Wallaby Way</neighborhood>
        <city>Sydney</city>
        <state>SP</state>
        <postalcode>01000000</postalcode>
        <countrycode>BR</countrycode>
    </address>
    <bank>
        <code>341</code>
        <bankname>Itaú Unibanco S.A.</bankname>
        <name>Owner</name>
        <agencynumber>0243</agencynumber>
        <accountnumber>123451</accountnumber>
        <document>21234879611</document>
    </bank>
</retorno>
```

Campo | Descrição
------|----------
retorno | Container
ipag_id | id do Parceiro Marketplace no iPag
seller_id | Seller Id do Vendedor na Zoop
owner| Container
owner[name] | Nome do sócio
owner[email] | Email do sócio
owner[birthdate] | Data de nascimento do sócio
owner[phone] | Telefone do sócio
owner[cpf] | CPF do sócio
address | Container
address[street] | Endereço da Empresa
address[number] | Número do endereço
address[complement] | Complemento
address[neighborhood] | Bairro
address[city] | Cidade
address[state] | Estado
address[postacode] | CEP
address[countrycode] | País
bank | Container
bank[code] | Código do banco
bank[bankname] | Nome do Banco
bank[name] | Nome do Titular
bank[agencynumber] | Número da Agência
bank[accountnumber] | Número da Conta
bank[document] | Documento do Titular
