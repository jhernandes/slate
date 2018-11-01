# ipag.js

Ipag.js helps handling sensitive credit card data, creating safer transactions directly by the customer (avoids sending sensitive data from the card to the Store server).
## Including ipag.js

Include the script before de **body** closing tag

[See Script on HTML Tab](?html#ipag-js)

```html
<!-- Include script before tag close </ body> -->
<script type="text/javascript" src="https://sandbox.ipag.com.br/js/dist/ipag.js"></script>
```

## Retrieving your SessionToken

`GET /service/sessionToken`

```php
<?php
// Exemplo em PHP
try {
    $username = "IPAG_ID";
    $password = "IPAG_KEY";
    $remote_url = 'https://sandbox.ipag.com.br/service/sessionToken';

    $opts = array(
        'http' => array(
            'method' => "GET",
            'user_agent' => 'Meu site.com/Loja Vritual',
            'header' => [
                "Authorization: Basic ".base64_encode("$username:$password"),
                'Accept: application/json',
            ],
        ),
    );

    $context = stream_context_create($opts);

    $response = file_get_contents($remote_url, false, $context);
    $sessionToken = json_decode($response, true)['token'];

    // atribua o $sessionToken ao script
} catch (Exception $e) {
    print_r($e->__toString());
}
```

To retrieve the **SessionToken**, send a GET to `/ service / sessionToken` with only the headers:

`"Content-Type": "application/json"`

`"Authorization": "Basic am9uYXRoYW46REM4QS00QzE2ONHSNS1EQTZBRUY2O92SDRkQ2RDMyOC0wRjAz"`

You should retrieve the **SessionToken** via Server Side for security, use only the Session Token on the Client Side.

> SessionToken call response

```
{
    "token": "E0D5-246E12FD-431CDAD2-01D05180-B27E",
    "expires_at": "2018-07-30 09:17:05"
}
```

<aside class="notice">
   Change Authorization for your credentials <a href="?javascript#autentica-o-api"> View Authentication </a>
</aside>

After retrieving your **SessionToken**, assign it in the **iPag.setIpagId ('SESSION_TOKEN')** method of ipag.js [See the JavaScript tab] (? Javascript # incluindo-o-ipag-js).

```javascript
iPag.setIpagId('PUT YOUR SESSION TOKEN HERE');
```


## Enabling Sandbox Mode

```javascript
...
iPag.setTestMode();
...
```

Sandbox (Test) mode is used so that temporary tokens are created in a sandbox environment.

If it is not called, an attempt will be made to send it to iPag's production environment.

[See the JavaScript tab](?javascript#habilitando-o-modo-sandbox)

## Formatter.js Formatted Text Inputs

Ipag.js automatically detects if you have Formatter.js and tries to format the card data entries. You can learn more about Formatter.js at [GitHub](https://github.com/jaridmargolin/formatter.js).

For this you must set the following attribute to the card number input: data-ipag = "number" and call the iPag.setup() function.

>Example input with data-ipag = "number"

```
//html
<input id="cc-number" name="cc-number" type="tel" class="cc-number" required="" data-ipag="number" value="">
```

```
//js
...
iPag.setup();
...
```

## Creating a payment token (Complete process with validation)

[Veja na aba JavaScript](?javascript#criando-um-token-de-pagamento-processo-completo-com-valida-o)

>The script below uses the JQuery library

```javascript
jQuery(function($) {
  $(document).ready(function() {
    iPag.setup();
  })

  $("#payment-button").click(function(evt) {
      var form = $(this).parents('form');
      var createToken = function() {
        iPag.setIpagId('PUT YOUR SESSION TOKEN HERE');
        iPag.setTestMode();

        // Valida se o campo titular do cartão não está vazio.
        if (!iPag.validateHolder($('*[data-ipag="holder"]').val())) {
            return alert('Invalid Holder!');
        }
        // Valida se a data de vencimento é válida.
        if (!iPag.validateExpiration(
            $('*[data-ipag="expiry_month"]').val(),
            $('*[data-ipag="expiry_year"]').val()
        )) {
            return alert('Invalid Expiry date.');
        }

        /** Valida se o número do cartão de crédito é um número válido.
         ** Se sim, recupera os dados do form, instância um cartão e faz a chamada para criar um token
         ** Se não houver nenhum erro, o token será devolvido no método then(function(response)) ...
         **/
        if (iPag.validateCreditCardNumber($('*[data-ipag="number"]').val())) {
            iPag.setCreditCard(
                $('*[data-ipag="holder"]').val(),
                $('*[data-ipag="number"]').val(),
                $('*[data-ipag="expiry_month"]').val(),
                $('*[data-ipag="expiry_year"]').val(),
                $('*[data-ipag="cvv"]').val()
            );
            iPag.createToken()
            .then(function(response){
                if (response.token) {
                    $("#token").val( response.token );
                    $('*[data-ipag="holder"]').val('');
                    $('*[data-ipag="number"]').val('');
                    $('*[data-ipag="expiry_month"]').val('');
                    $('*[data-ipag="expiry_year"]').val('');
                    $('*[data-ipag="cvv"]').val('');
                    // You can do other checks before submitting
                    form.get(0).submit();
                } else {
                    alert("Error saving card: " + JSON.stringify(response));
                }
            })
            .catch(function(error){
                alert("Error saving card:  " + JSON.stringify(error));
            });
        } else {
            return alert('Invalid Card Number!');
        }
      }
      createToken();
  });
});
```

The Token represents a credit card, and must be submitted in the ** token_cartao ** field for the iPag API.

The form fields must have the data-ipag attribute for each field below:

* holder => Card Holder
* number => Card number
* cvv => Security verification code
* expiry_month => Expiry Month
* expiry_year => Expiry Year

## Auxiliar Functions

[See on JavaScript Tab](?javascript#fun-es-auxiliares)

### iPag.validateCreditCardNumber
Validate the credit card number using the Luhn algorithm

> iPag.validateCreditCardNumber

```javascript
iPag.validateCreditCardNumber('5454545454545454') // returns true
iPag.validateCreditCardNumber('1234') // returns false
```

### iPag.validateHolder

> iPag.validateHolder

Validates if the cardholder is not empty

```javascript
iPag.validateHolder('FULANO DA SILVA') // retorna true
iPag.validateHolder('') // retorna false
```

### iPag.validateExpiration

> iPag.validateExpiration

Validates if the card's due date is valid.

```javascript
iPag.validateExpiration('10', '2021') // retorna true
iPag.validateExpiration('10', '21') // retorna true
iPag.validateExpiration('08', '2016') // retorna false
```

## Sample Checkout Form

[Click here to view the form](https://sandbox.ipag.com.br/checkout-demo)
