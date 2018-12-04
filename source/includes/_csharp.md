# Exemplo C# - Consumindo a api com HttpClient

## Autenticação
###Crie um objeto do tipo HttpClient 
<aside style="background-color:rgba(166, 255, 164,0.5)">
    <br>public HttpClient client = new HttpClient();
</aside>
### Adicione a Url base (Sandbox ou Produção)
<aside style="background-color:rgba(166, 255, 164,0.5)">
    <br>client.BaseAddress = new Uri("https://sandbox.ipag.com.br/");
    <br>client.DefaultRequestHeaders.Accept.Clear();
</aside>
###Criando nosso token de acesso (Basic Authorization)
###Criamos o token convertendo nossas informações obtidas em Minha Conta no Painel iPag
<aside style="background-color:rgba(166, 255, 164,0.5)">
    <br>var plainTextBytes = System.Text.Encoding.UTF8.GetBytes("**Seu login no iPag** " + ":" + "**Sua Ipag Key** ");
    <br>var authToken = Convert.ToBase64String(plainTextBytes);
</aside>
###Adicionamos nosso token no parâmetro _Authorization_ dos Headers
<aside style="background-color:rgba(166, 255, 164,0.5)">
    <br>client.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Basic", authToken);
    <br>client.DefaultRequestHeaders.Accept.Add(new MediaTypeWithQualityHeaderValue("multipart/formdata"));
</aside>
###Por último, adicionamos os headers _Accept_ e _User-Agent_
<aside style="background-color:rgba(166, 255, 164,0.5)">
    <br>client.DefaultRequestHeaders.Add("User-Agent", "Nome do Estabelecimento");
</aside>

## Enviando Requisição
### Criaremos o Body a ser enviado no request
**Utilizaremos um objeto FormUrlEncodedContent para armazenar os campos**

<aside style="background-color:rgba(166, 255, 164,0.5)">
var content = new FormUrlEncodedContent(new[]<br>
    {<br>
        new KeyValuePair<string, string>("identificacao", "Seu Login no iPag"),<br>
        new KeyValuePair<string, string>("pedido", "Número do Pedido"),<br>
        new KeyValuePair<string, string>("operacao", "Pagamento"),<br>
        new KeyValuePair<string, string>("valor","Total da venda"),<br>
        new KeyValuePair<string, string>("metodo", "Método de pagamento, por ex: visa"),<br>
        new KeyValuePair<string, string>("url_retorno", "http://localhost:15536/payment/return"),<br>
        new KeyValuePair<string, string>("email", "email@email.co"),<br>
        new KeyValuePair<string, string>("fone", "Telefone do cliente"),<br>
        new KeyValuePair<string, string>("nome", "Nome completo"),<br>
        new KeyValuePair<string, string>("endereco", "Endereço"),<br>
        new KeyValuePair<string, string>("numero_endereco", "Número"),<br>
        new KeyValuePair<string, string>("bairro", "Bairro"),<br>
        new KeyValuePair<string, string>("cidade", "Cidade"),<br>
        new KeyValuePair<string, string>("estado", "Estado"),<br>
        new KeyValuePair<string, string>("cep", "Cep"),<br>
        new KeyValuePair<string, string>("retorno_tipo", "xml"),<br>
        new KeyValuePair<string, string>("token_cartao", token),<br>
        new KeyValuePair<string, string>("gera_token_cartao", "1 - Sim, 0 - Não"),<br>
    });<br>
</aside>

### Agora enviamos o request e recuperamos a resposta. "service/payment" é o endpoint da api que enviaremos.
<aside style="background-color:rgba(166, 255, 164,0.5)">
 HttpResponseMessage response = client.PostAsync("service/payment",content).Result;
 string resposta = response.Content.ReadAsStringAsync().Result;
</aside>

### Para ler a resposta, utilizaremos o XmlDocument
<aside style="background-color:rgba(166, 255, 164,0.5)">
 var xmlDoc = new XmlDocument();<br>
 xmlDoc.LoadXml(resposta);<br>

<b> Para lermos um nó do xml, utilizamos: </b><br>
var auth_transNode = xmlDoc.GetElementsByTagName("autorizacao_id");<br>
var auth_trans = auth_transNode[0].InnerXml;<br>


</aside>

## Bibliotecas Utilizadas
using System.Net.Http;<br>
using System.Net.Http.Headers;<br>
using System.Xml;<br>
.NET Framework 4.6<br>
<aside class="notice">
    Em caso de dúvidas, entre em contato conosco enviando um email para suporte@ipag.com.br
</aside>
