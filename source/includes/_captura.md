# Capturar Transação

`POST /service/capture`

### Campos

Campo | Tamanho | Tipo | Obrigatório | Descrição
--------- | ----- | ----- | ----------- | ---------
identificacao | 60 | string | sim | Código de identificação do estabelecimento no iPag (login de acesso ao painel)
transId | 60 | string | sim | Código de identificação da transação (TID)
valor | 10 | float | não | Valor desejado da captura, pode ser igual ou menor o valor da transação. Caso seja menor será realizado a captura parcial.
url_retorno | 255 | string | não | Url de callback.
retorno_tipo | 20 | string | não | `xml`


> Exemplo via [SDK PHP](https://github.com/jhernandes/ipag-sdk-php#captura)

```php
<?php
// VIA IPAG-SDK-PHP
require 'vendor/autoload.php';

use Ipag\Ipag;
use Ipag\Classes\Authentication;
use Ipag\Classes\Endpoint;

try {
    $ipag = new Ipag(new Authentication('my_ipag_id', 'my_ipag_key'), Endpoint::SANDBOX);

    $response = $ipag->transaction()->setTid('123456789')->capture();
} catch(\Exception $e) {
    print_r($e->__toString());
}
```

```shell
curl -X POST \
  https://sandbox.ipag.com.br/service/capture \
  -H 'Authorization: Basic am9uYXRoYW46REM4QS00QzE2Yu75NS1EQTZBRUY2OC0wRkQ2RDMyOC0wRjAz' \
  -H 'content-type: multipart/form-data' \
  -F identificacao=teste \
  -F transId=201803061703 \
  -F valor=10.00 \
```

