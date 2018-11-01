# API Authentication

## What is the API Key used for?

The API Key is used to authenticate  the operations  in the iPag API.

## IPAG API KEY

**Access:**

[SANDBOX - iPag Panel - My Account](https://painel-sandbox.ipag.com.br/?r=admin/alterardados&navegation=addplus).

<aside class="notice">
<b>If you don't have an API KEY in your account, make the request to support@ipag.com.br.</b>
</aside>

##Authentication

The authentication for the API occurs through the  [HTTP basic access authentication](https://en.wikipedia.org/wiki/Basic_access_authentication). 
You'll find more details on Basic Authentication in [IETF RFC 2617](https://www.ietf.org/rfc/rfc2617.txt).

Provide your login and your API secret key as the basic authentication user name and password.

All communication must be encrypted via SSL. The basic authentication token is reversible, however, when all communication is over HTTPS the security context is completely protected. An application must send a HEADER HTTP Authorization containing the username and password with each request.

All API calls must be made on HTTPS to ensure security.

Basic Auth is trivial to use for HTTP client libraries. Tools such as cURL provide corresponding command-line options.

<aside class="notice">
<b>You should keep your API Key safe no matter what! DO NOT SHARE your API key with anyone, not even on iPag support channels. 
No one who legitimately represents iPag will ask you for your API key!</b>
</aside>
....