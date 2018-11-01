# iPag Checkout

> iPag Checkout (VPOS)

```php
<form method='post' action='https://sandbox.ipag.com.br/vpos'>
    <input type='hidden' name='identificacao' value='LOGIN_IPAG'/>
    <input type='hidden' name='operacao' value='Pagamento'/>
    <input type='hidden' name='pedido' value=''/>
    <input type='hidden' name='valor' value='1.00'/>
    <input type="hidden" name="plugin" value="html" />
    <input type="hidden" name="item_quant_1" value="1" />
    <input type="hidden" name="item_descr_1" value="PRODUCT_NAME" />
    <input type="hidden" name="item_valor_1" value="1.00" />
    <input type="hidden" name="item_id_1" value="123" />
    <input type="image" src="https://sandbox.ipag.com.br/images/comprar100px.png" name="submit" alt="Buy" />
</form>
```

```html
<form method='post' action='https://sandbox.ipag.com.br/vpos'>
    <input type='hidden' name='identificacao' value='LOGIN_IPAG'/>
    <input type='hidden' name='operacao' value='Pagamento'/>
    <input type='hidden' name='pedido' value=''/>
    <input type='hidden' name='valor' value='1.00'/>
    <input type="hidden" name="plugin" value="html" />
    <input type="hidden" name="item_quant_1" value="1" />
    <input type="hidden" name="item_descr_1" value="PRODUCT_NAME" />
    <input type="hidden" name="item_valor_1" value="1.00" />
    <input type="hidden" name="item_id_1" value="123" />
    <input type="image" src="https://sandbox.ipag.com.br/images/comprar100px.png" name="submit" alt="Buy" />
</form>
```

**Endpoint:**
`POST /vpos`


With **iPag Checkout** you don't need to develop your own Checkout, just submit a POST with the information below to create a transaction on iPag on your behalf.

To set up Checkout iPag, go to
**iPag Panel** Settings> Checkout Settings

**iPag Checkout Parameters**

Parameter | Required | description
----------|-------------|---------
identificacao|yes|IPag ID
operacao|yes|Type of Operation (Payment)
pedido|no|Order Number
valor|yes|Amount to be charged to the customer
plugin|yes|Parameter needed to send a request to Checkout iPag
item_quant_1|no|Quantity of Product / Service
item_descr_1|no|Item Description
item_valor_1|no|Product unit value
item_id_1|no|Product SKU
url_retorno|no|Endpoint that iPag will send a POST when there is an update status of the transaction
nome|no|Customer Name
documento|no|Customer Document
email|no|Customer Email
fone|no|Customer phone
endereco|no|Customer address
numero_endereco|no|Customer address number 
bairro|no|Customer neighborhood
complemento|no|Customer complement
cidade|no|Customer city
estado|no|Customer state/province
cep|no|Customer zipcode
maxparcelas | no | Maximum installment number
parcelasemjuros | no | Maximum installment payment without interest
valorminparcela | no | Minimum installment amount
jurosparcela | no |Interest to be applied to installments


