# Parceled Billet

## Additional fields for parcel billet
Two additional fields are required to send split billet transaction:

Parameter | size | type | Required | description
--------- | ----- | ----- | ----------- | ---------
boleto_parcelas | 128 | number | yes | Number of installments that the billet will be parceled out.
url_parcelamento | 256 | string | yes | Return url with the account data (iPag account) that will manage the charges

## Return Data of Parceled Billet

Parameter | Description
--------- | ---------
valor | Total Amount
num_pedido | Order Number
quant_parcelas | Amount of Installments
idconta | Linked account ID on iPag
primeiro_vencimento | Due date of first charge
operadora | Banking Billet Method (ex. Boleto_itau)

## Comments

In the parcelled billet, both return Urls are used

* **url_retorno** (Url that will return the payments made for billets)
* **url_parcelamento** (Url that will return the account data created on iPag)
