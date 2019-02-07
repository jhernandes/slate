# Consultar Transação

`POST /service/consult`

### Campos

Campo | Tamanho | Tipo | Obrigatório | Descrição
--------- | ----- | ----- | ----------- | ---------
identificacao | 60 | string | sim | Código de identificação do estabelecimento no iPag (login de acesso ao painel)
transId | 60 | string | sim | Código de identificação da transação (TID)


> Exemplo via [SDK PHP](https://github.com/jhernandes/ipag-sdk-php#consulta)

```php
<?php
// VIA IPAG-SDK-PHP
require 'vendor/autoload.php';

use Ipag\Ipag;
use Ipag\Classes\Authentication;
use Ipag\Classes\Endpoint;

try {
    $ipag = new Ipag(new Authentication('my_ipag_id', 'my_ipag_key'), Endpoint::SANDBOX);

    $response = $ipag->transaction()->setTid('123456789')->consult();
} catch(\Exception $e) {
    print_r($e->__toString());
}
```

```shell
curl -X POST \
  https://sandbox.ipag.com.br/service/consult \
  -H 'Authorization: Basic am9uYXRoYW46REM4QS00QzE2Yu75NS1EQTZBRUY2OC0wRkQ2RDMyOC0wRjAz' \
  -H 'content-type: multipart/form-data' \
  -F identificacao=teste \
  -F transId=201803061703 \
```

