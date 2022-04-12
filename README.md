# alfaben-interview


<p align="center">

### Laravel_quiz
</p>

<hr>

## 1- What is JWT (Json Web Token)? Briefly explain the working principle.

First, JWT is approach to authorize or identify someone or something. But what is inside of JWT?
<br>
The main structures of JWT are:
- Header
- Payload 
- Signature

### Header
First part of the normal token contains header section. typically, header have <code>algo</code> and <code>typ</code> section. these are representing <u>type of token</u> and <u>algorithm of signature</u> which encoded with RSA or SHA256 or etc.
<br>At last this json is encrypted by <code>Base64Url</code>

for example:
``` json
{
  "alg": "HS256",
  "typ": "JWT"
}
```


### Payload
The second section of a token is the Payload. which contains claims. in fact the Payload includes user claims and other necessary data.
Payload even contains sections such as <code>Registered</code> , <code>Public</code> and <code>Private</code> .
<br>This section is encrypted too by <code>Base64Url</code>.

for example:
``` json
{
  "sub": "1234567890",
  "name": "John Doe",
  "admin": true
}
```

### Signature
To create Signature section, sign the encoded Header & Payload with a secret code and sign whole content with specified algorithm that specified in header section.
<br>This was for detect the <u>Header</u> and <u>Payload</u> section doesn't change along the way.

```php
<?php
$separator_character = '.';
$sig = hash_hmac(
    algo: 'sha256',
    data: $header_enc . $separator_character . $payload_enc,
    key: $secret);
```

#### At last

Format of JWT like below.
```
 header encoded        payload encoded         Signature hash  
xxxxxxxxxxxxxxxxxx . yyyyyyyyyyyyyyyyyyyy . zzzzzzzzzzzzzzzzzzzzz
```

When user after Authorized (user successfully logsIn using their credentials) server generate this token using the user info and attach it to every response header or include it into the response content (body).
<br>
Whenever the user want to access protected Routes he should send the token thorough request header as <code>Authorization: Bearer xxxxx.yyyyy.zzzzzz</code>
<br>Server check the received token and if it is? and/or if correct header & payload data has been retrieved from token? then user can access protected area.
Token doesn't need to store in the server, but It depends on requirement of ecosystem also it could be store in server for feature needs.


<hr>

## 2- Summarize the working logic of a classical session system over the Cookie-Server relationship.

#### Cookies
Cookies are key/value data to store state information on the browser. When the browser requests a webpage the server can send cookies to store information on the user browser thorough the response header like below.

```http response
HTTP/1.1 200 OK
Content-type: text/html
Set-Cookie: data1=value1
Set-Cookie: data2=value2; Expires=Fri, 30 Sep 2011 11:48:00 GMT
... rest  of headers
```
However, cookies has expiration date time, and it's no longer kept in browser if the cookie's expiration time not updated.
user can send back cookies thorough each request header to server and server use this data to better serve and remember what does user done before.

#### Session
Session is server side cookies. instead of storing sensitive data on user browser such as user info who is logged in, web service store this data on server as a file by default or as a record in database session table.

In each request if user session doesn't find it generate automatically in server when session being start on request lifecycle.
Set an unrecognizable ID for each session file name or DB column id value to identify each from another, then send back this ID as cookies data to find the relevant data from session when needed.

Each approach have benefits and defects.
#### cookies have 
- scalability: all the data is stored in the browser so each request can go through a load balancer to different webservers and you have all the information needed to fullfill the request.
- they can be accessed via javascript on the browser;
- not being on the server they will survive server restarts;
- RESTful: requests don't depend on server state
#### but dont have
- storage is limited.
- secure cookies are not easy to implement.

<hr>

#### session have
- generally easier to use.
- unlimited storage

#### but dont have
- on web server restarts you can lose all sessions or not depending on the implementation
- not RESTful



