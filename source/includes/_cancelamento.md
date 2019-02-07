# Cancelar Transação

`POST /service/cancel`

### Campos

Campo | Tamanho | Tipo | Obrigatório | Descrição
--------- | ----- | ----- | ----------- | ---------
identificacao | 60 | string | sim | Código de identificação do estabelecimento no iPag (login de acesso ao painel)
transId | 60 | string | sim | Código de identificação da transação (TID)
valor | 10| float | não | Valor desejado do cancelamento, pode ser igual ou menor o valor da transação.
url_retorno | 255 | string | não | Url de callback.
retorno_tipo | 20 | string | não | `xml`

<aside class="info">
    <b>Obs1:</b> Cancelamento Parcial está disponível apenas nas adquirentes: iPag, Zoop, Cielo, Rede (D+1) e Stone.
    <br>
    <b>Obs2:</b> O Cancelamento Parcial só pode ser realizado em Transações Capturadas. Nas transações Aprovadas o cancelamento é total.
</aside>

> Exemplo via [SDK PHP](https://github.com/jhernandes/ipag-sdk-php#cancelamento)

```php
<?php
// VIA IPAG-SDK-PHP
require 'vendor/autoload.php';

use Ipag\Ipag;
use Ipag\Classes\Authentication;
use Ipag\Classes\Endpoint;

try {
    $ipag = new Ipag(new Authentication('my_ipag_id', 'my_ipag_key'), Endpoint::SANDBOX);

    $response = $ipag->transaction()->setTid('123456789')->cancel();
} catch(\Exception $e) {
    print_r($e->__toString());
}
```

```shell
curl -X POST \
  https://sandbox.ipag.com.br/service/cancel \
  -H 'Authorization: Basic am9uYXRoYW46REM4QS00QzE2Yu75NS1EQTZBRUY2OC0wRkQ2RDMyOC0wRjAz' \
  -H 'content-type: multipart/form-data' \
  -F identificacao=teste \
  -F transId=201803061703 \
  -F valor=10.00 \
```

