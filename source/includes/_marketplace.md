# (Marketplace) - Register & Accredit Vendor

### What is it ?

The accreditation service is a set of web-services that allow full automation of the accreditation process, with APIs to register new vendors, management of documentation necessary to the process of "Know Your Customer".

To use the Accreditation service, you must:

1. Register a [new seller](#cadastrar-um-seller-amp-parceiro-marketplace-via-api) through the API;
2. Send the necessary documentation to the "Know Your Customer" process;
3. Once the registration is validated and released by our team, you can send transactions and split on behalf of this seller;

## Register a Seller & Marketplace Partner via API

`POST / service / v1`

## Register a seller of type Legal (Business Seller)

> Integration example

``` php
<? php

$ url = 'https://sandbox.ipag.com.br/service/v1';

$ content = http_build_query (array (
    "ctrl" => 'zoopbusiness',
    "business_name" => 'Test Co.',
    "business_phone" => '1187654321',
    "business_email" => 'test@business.com',
    "business_description" => 'A test business',
    "business_cnpj" => '29654759000170',
    "address_street" => 'P Sherman',
    "address_number" => '42',
    "address_complement" => '',
    "address_neighborhood" => 'Wallaby Way',
    "address_postalcode" => '01000000',
    "address_city" => 'Sydney',
    "address_state" => 'SP',
    "owner_firstname" => 'Test',
    "owner_lastname" => 'Seller',
    "owner_phone" => '1112345678',
    "owner_email" => 'test@owner.com',
    "owner_cpf" => '45237426182',
    "owner_birthdate" => '1987-11-11',
    "bank_name" => 'Test Seller',
    "bank_cpf" => '45237426182',
    // If informed, cpf does not inform cnpj
    "bank_cnpj" => '29654759000170',
    "bank_code" => '341',
    "bank_agencynumber" => '1234',
    "bank_accountnumber" => '12345-4'
));

$username = 'API_ID';
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
  -u <IPAG_ID>:<IPAG_KEY> \
  --url https://sandbox.ipag.com.br/service/v1 \
  --header 'content-type: application/x-www-form-urlencoded' \
  --data 'ctrl=zoopbusiness&owner_phone=1112345678&owner_cpf=64028478286&owner_email=test%40owner.com&owner_birthdate=1987-11-11&owner_firstname=Test&owner_lastname=Seller&address_street=P%20Sherman&address_number=42&address_neighborhood=Wallaby%20Way&address_postalcode=01000000&address_city=Sydney&address_state=SP&bank_name=Test%20Seller&bank_cnpj=77441655000120&bank_code=341&bank_agencynumber=1234&bank_accountnumber=12345-4&business_name=Test%20Co.&business_phone=1187654321&business_email=test%40business.com&business_description=A%20test%20business&business_cnpj=05533862000101'
```

Field | type | Required | description
------ | ----- | ----------- | ---------
ctrl | string | sim | Send: **'zoopbusiness'**
business_name | string | sim | Company Fantasy Name
business_phone | string | sim | Company Telephone, DDD + Telephone of minimum of 10 digits and maximum of 11 digits
business_email | string | sim | Company Contact Email
business_description | string | sim | Company Description
business_cnpj | string | sim | CNPJ of the Company
address_street | string | sim | Company Address (Street, Av.)
address_number | string | sim | Address number
address_complement | string | not | Address complement
address_neighborhood | string | sim | Address Neighborhood
address_postalcode | string | sim | Address zip (8 digits)
address_city | string | sim | City
address_state | string | sim | State (2 digits)
owner_firstname | string | sim | First name of Member responsible for the Company
owner_lastname | string | sim | Last name of the Member responsible for the Company
owner_phone | string | sim | Direct contact telephone with the partner
owner_email | string | sim | E-mail of the Member responsible
owner_cpf | string | sim | CPF of the Member responsible for the Company
owner_birthdate | string | not | Date of Birth of Partner in charge
bank_name | string | not * | Bank Account Holder
bank_cpf | string | not * | CPF of the Account Holder if it is in the name of Individual
bank_cnpj | string | not * | CNPJ of the Account Holder if it is in the name of Legal Entity
bank_code | string | not * | [Bank Code(3 digits)](https://pt.wikipedia.org/wiki/Lista_de_bancos_do_Brasil)
bank_agencynumber | string | not * | Bank Agency Number
bank_accountnumber | string | not * | Bank account number

* If you want to link a bank account you must inform all *bank* fields

### Return **Business Seller**

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

Field | description
------ | ----------
retorno | Container
ipag_id | Marketplace Partner id on iPag
seller_id | Seller Id from Seller at Zoop
owner | Container
owner [name] | Name of member
owner [email] | Partner Email
owner [birthdate] | Partner's date of birth
owner [phone] | Member Phone
owner [cpf] | Partner CPF
address | Container
address [street] | Company's adress
address [number] | Address number
address [complement] | Complement
address [neighborhood] | Neighborhood
address [city] | City
address [state] | state
address [postacode] | Zip code
address [countrycode] | Parents
business | Container
business [name] | Fantasy name
business [phone] | telephone
business [email] | Company Email
business [website] | The company's website
business [description] | Company Description
business [facebook] | Company Facebook
business [twitter] | Company Twitter
business [cnpj] | CNPJ of the Company
bank | Container
bank [code] | Bank Code
bank [bankname] | Bank name
bank [name] | Cardholder Name
bank [agencynumber] | Agency Number
bank [accountnumber] | Account number
bank [document] | Holder's Document

## Register a seller of the type Individual (Individual Seller)

> Integration example

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

$username = 'API_ID';
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
  -u <IPAG_ID>:<IPAG_KEY> \
  --url https://sandbox.ipag.com.br/service/v1 \
  --header 'content-type: application/x-www-form-urlencoded' \
  --data 'ctrl=zoopindividual&owner_phone=1112345678&owner_cpf=08935742449&owner_email=test%40owner.com&owner_birthdate=1987-11-11&owner_firstname=Test&owner_lastname=Seller&address_street=P%20Sherman&address_number=42&address_neighborhood=Wallaby%20Way&address_postalcode=01000000&address_city=Sydney&address_state=SP&bank_name=Test%20Seller&bank_cnpj=77441655000120&bank_code=341&bank_agencynumber=1234&bank_accountnumber=12345-4'
```

Field | type | Required | description
------ | ----- | ----------- | ---------
ctrl | string | sim | Send: **'zoopindividual'**
address_street | string | sim | Company Address (Street, Av.)
address_number | string | sim | Address number
address_complement | string | not | Address complement
address_neighborhood | string | sim | Address Neighborhood
address_postalcode | string | sim | Address zip (8 digits)
address_city | string | sim | City
address_state | string | sim | State (2 digits)
owner_firstname | string | sim | First name of Member responsible for the Company
owner_lastname | string | sim | Last name of the Member responsible for the Company
owner_phone | string | sim | Direct contact telephone with the partner
owner_email | string | sim | E-mail of the Member responsible
owner_cpf | string | sim | CPF of the Member responsible for the Company
owner_birthdate | string | not | Date of Birth of Partner in charge
bank_name | string | not * | Bank Account Holder
bank_cpf | string | not * | CPF of the Account Holder if it's in the name of Individual
bank_cnpj | string | not * | CNPJ of the Account Holder if it is in the name of Legal Entity
bank_code | string | not * | [Bank Code (3 digits)](https://pt.wikipedia.org/wiki/Lista_de_bancos_do_Brasil)
bank_agencynumber | string | not * | Bank Agency Number
bank_accountnumber | string | not * | Bank account number

* If you want to link a bank account you must inform all *bank* fields 

### Return ** Business Individual **

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
Field | description
------ | ----------
retorno | Container
ipag_id | Marketplace Partner id on iPag
seller_id | Seller Id from Seller at Zoop
owner | Container
owner [name] | Name of member
owner [email] | Partner Email
owner [birthdate] | Partner's date of birth
owner [phone] | Member Phone
owner [cpf] | Partner CPF
address | Container
address [street] | Company's adress
address [number] | Address number
address [complement] | Complement
address [neighborhood] | Neighborhood
address [city] | City
address [state] | state
address [postacode] | Zip code
address [countrycode] | Parents
bank | Container
bank [code] | Bank Code
bank [bankname] | Bank name
bank [name] | Cardholder Name
bank [agencynumber] | Agency Number
bank [accountnumber] | Account number
bank [document] | Holder's Document

## Registering a bank (Bank)

> Integration example

```shell
curl -X POST \
-u <IPAG_ID>:<IPAG_KEY> \
--url https://sandbox.ipag.com.br/service/v1 \
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

$username = 'API_ID';
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
Field | type | Required | description
------ | ----- | ----------- | ---------
ctrl | string | sim | Send: ** 'zoopbank' **
bank_name | string | sim | Bank Account Holder
bank_cpf | string | not * | CPF of the Account Holder if it is in the name of Individual
bank_cnpj | string | not * | CNPJ of the Account Holder if it is in the name of Legal Entity
bank_code | string | sim | [Bank Code (3 digits)] (https://pt.wikipedia.org/wiki/Lista_de_bancos_do_Brasil)
bank_agencynumber | string | sim | Bank Agency Number
bank_accountnumber | string | sim | Bank account number

* It is necessary to send CPF or CNPJ.

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

Field | description
------ | ----------
retorno | Container
bank_id | Bank ID
bank | Container
bank [code] | Bank Code
bank [bankname] | Bank name
bank [name] | Cardholder Name
bank [agencynumber] | Agency Number
bank [accountnumber] | Account number
bank [document] | Holder's Document

## Link Bank to Seller (Sync Bank)

> Integration example

```shell
curl -X POST \
-u <IPAG_ID>:<IPAG_KEY> \
--url https://sandbox.ipag.com.br/service/v1 \
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

$username = 'API_ID';
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
Field | type | Required | description
------ | ----- | ----------- | ---------
ctrl | string | sim | Send: ** 'zoopsyncbank' **
bank_id | string | sim | Bank ID
seller_id | string | sim | Seller ID

### Sync Bank Return

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

Field | description
------ | ----------
retorno | Container
ipag_id | Marketplace Partner id on iPag
seller_id | Seller Id from Seller at Zoop
owner | Container
owner [name] | Name of member
owner [email] | Partner Email
owner [birthdate] | Partner's date of birth
owner [phone] | Member Phone
owner [cpf] | Partner CPF
address | Container
address [street] | Company's adress
address [number] | Address number
address [complement] | Complement
address [neighborhood] | Neighborhood
address [city] | City
address [state] | state
address [postacode] | Zip code
address [countrycode] | Parents
bank | Container
bank [code] | Bank Code
bank [bankname] | Bank name
bank [name] | Cardholder Name
bank [agencynumber] | Agency Number
bank [accountnumber] | Account number
bank [document] | Holder's Document

## Consult a seller (Get Seller)

> Integration example


```shell
curl -X POST \
-u <IPAG_ID>:<IPAG_KEY> \
--url https://sandbox.ipag.com.br/service/v1 \
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

$username = 'API_ID';
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

Field | type | Required | description
------ | ----- | ----------- | ---------
ctrl | string | sim | Send: ** 'zoopgetseller' **
seller_id | string | sim | Zoop Seller ID

### Query Return

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

Field | description
------ | ----------
retorno | *Container*
ipag_id | Marketplace Partner id on iPag
seller_id | Seller Id from Seller at Zoop
owner | *Container*
owner [name] | Name of member
owner [email] | Partner Email
owner [birthdate] | Partner's date of birth
owner [phone] | Member Phone
owner [cpf] | Partner CPF
address | *Container*
address [street] | Company's adress
address [number] | Address number
address [complement] | Complement
address [neighborhood] | Neighborhood
address [city] | City
address [state] | state
address [postacode] | Zip code
address [countrycode] | Parents
bank | *Container*
bank [code] | Bank Code
bank [bankname] | Bank name
bank [name] | Cardholder Name
bank [agencynumber] | Agency Number
bank [accountnumber] | Account number
bank [document] | Holder's Document

## Know Your Customer (Document Delivery)

### Files
- Up to 2MB max.
- Image or extension: .jpg, .png and .pdf.
- Files can be photographed or scanned.
- The documents need to be complete with all the information, without cuts or erasures.
- Send two-sided documents.
- The deadline for review of the documentation is up to 5 business days.

It can be sent via iPag Panel also: [Send Documents] (https://painel-sandbox.ipag.com.br/?r=admin/documentos)

`POST /service/v1`

> Exemplo de integração

```shell
curl --request POST \
  --url 'https://sandbox.ipag.com.br/service/v1 \
  --header 'Authorization: Basic am9uYXRoYW46REM4QS00QzE2383M5NS1EQTZBRUY2OC0wRkQ2RDMyOC0wRjAz' \
  --header 'Content-Type: multipart/form-data; \
  --form cpf=@/Users/user/Pictures/cpf.jpg \
  --form cnpj=@/Users/user/Pictures/cnpj.jpg \
  --form atividade=@/Users/user/Pictures/atividade.jpg \
  --form comprovante=@/Users/user/Pictures/comprovante.jpg \
  --form ctrl=seller_documents \
  --form seller_id=8dc2dbf71518482a859e6057359bba46
```

Field | type | Required | description
------ | ----- | ----------- | ---------
ctrl | string | sim | Send: ** 'seller_documents' **
seller_id | string | sim | Partner / Vendor Login on iPag
cpf | file | not | Image of the CPF, RG or CNH of the Resposible Member (JPG, PNG or PDF)
cnpj | file | not | Image of Seller's CNPJ Card (if legal person) (JPG, PNG or PDF)
comprovate | file | not | Image of Invoice or Business Card (JPG, PNG or PDF)
comprovate | file | not | Image of Light, water or telephone bill (JPG, PNG or PDF)

### Return

> Retorno (XML)

```xml
<?xml version="1.0" encoding="utf-8"?>
<retorno>
    <success>ok</success>
    <message>Arquivo(s) enviado(s) com sucesso!</message>
    <cpf>recebido</cpf>
    <cnpj>recebido</cnpj>
    <atividade>recebido</atividade>
    <comprovante>recebido</comprovante>
</retorno>
```

Field | description
------ | ----------
retorno | Container
success | If it was sent successfully it will return "ok"
message | Return message
cpf | file status
cnpj | file status
atividade | file status
comprovante | file status