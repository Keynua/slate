# OTP

Conjunto de APIs que permiten enviar, verificar y revisar códigos de un solo
uso para realizar la firma de documentos.

## Propiedades de Auth

Atributo | Tipo | Opcional | Descripción
--------- | ----------- | ----------- | -----------
type | `contract` o `generic` | No | Tipo de autenticación
token | string | No (`type='contract'`) | El token del usuario dado al crear el contrato.
id | string | No (`type='generic'`) | Identificador genérico.

## Enviar OTP

```ruby
require "uri"
require "net/http"

url = URI("https://api.stg.keynua.com/send-otp/v1")

https = Net::HTTP.new(url.host, url.port)
https.use_ssl = true

request = Net::HTTP::Put.new(url)
request["Content-Type"] = 'application/json'
request.body = "{\n\t\"auth\": {\n\t\t\"type\": \"contract\",\n\t\t\"token\": \"eyJ0eXAiOiJ...Dq_5VSiZo\"\n\t}\n}"

response = https.request(request)
puts response.read_body
```

```python
import http.client
import json

conn = http.client.HTTPSConnection("api.stg.keynua.com")

payload = json.dumps({
	{
		"auth": {
			"type": "contract",
			"token": "eyJ0eXAiOiJ...Dq_5VSiZo"
		}
	}
})

headers = {
  'Content-Type': 'application/json'
}

conn.request("PUT", "/send-otp/v1", payload, headers)
res = conn.getresponse()
data = res.read()
print(data.decode("utf-8"))
```

```shell
curl --request PUT \
  --url https://api.stg.keynua.com/send-otp/v1 \
  --header 'Content-Type: application/json' \
  --data '{
	"auth": {
		"type": "contract",
		"token": "eyJ0eXAiOiJKV1Q...Dq_5VSiZo"
	}
}'
```

```javascript
const data = JSON.stringify({
  "auth": {
    "type": "contract",
    "token": "eyJ0eXA..._2iDq_5VSiZo"
  }
});

const xhr = new XMLHttpRequest();
xhr.withCredentials = true;

xhr.addEventListener("readystatechange", function () {
  if (this.readyState === this.DONE) {
    console.log(this.responseText);
  }
});

xhr.open("PUT", "https://api.stg.keynua.com/send-otp/v1");
xhr.setRequestHeader("Content-Type", "application/json");

xhr.send(data);
```

> Success response

```json
{
  "sent": true,
  "channel": "sms",
  "resendInSec": 60,
  "availableAttempts": 3
}
```

Permite enviar un código OTP al usuario dueño del token.

### HTTP Request

`PUT https://api.stg.keynua.com/send-otp/v1`

### Request body

Atributo | Tipo | Opcional | Descripción
--------- | ----------- | ----------- | -----------
auth | [Auth](#propiedades-de-auth) | No | Permite identificar el OTP del usuario y contrato

### Response body

Atributo | Tipo | Descripción
--------- | ----------- | -----------
sent | boolean | Si el código OTP se envió o no.
channel | string | Por qué medio se realizó el envío del código.
resendInSec | number | En cuantos segundos se podrá solicitar el envío de un nuevo código.
availableAttempts | number | Cuantos intentos aún quedan disponibles.

## Verificar OTP

```ruby
require "uri"
require "net/http"

url = URI("https://api.stg.keynua.com/send-otp/v1")

https = Net::HTTP.new(url.host, url.port)
https.use_ssl = true

request = Net::HTTP::Post.new(url)
request["Content-Type"] = 'application/json'
request.body = "{\n\t\"auth\": {\n\t\t\"type\": \"contract\",\n\t\t\"token\": \"eyJ0eXA...2iDq_5VSiZo\"\n\t},\n\t\"code\": \"GtEyfgCb\"\n}"

response = https.request(request)
puts response.read_body
```

```python
import http.client
import json

conn = http.client.HTTPSConnection("api.stg.keynua.com")

payload = json.dumps({
	{
		"auth": {
			"type": "contract",
			"token": "eyJ0eXAiOiJ...Dq_5VSiZo"
		},
		"code": "as34Er"
	}
})

headers = {
  'Content-Type': 'application/json'
}

conn.request("POST", "/send-otp/v1", payload, headers)
res = conn.getresponse()
data = res.read()
print(data.decode("utf-8"))
```

```shell
curl --request POST \
  --url https://api.stg.keynua.com/send-otp/v1 \
  --header 'Content-Type: application/json' \
  --data '{
	"auth": {
		"type": "contract",
		"token": "eyJ0..._5VSiZo"
	},
	"code": "GtEyfgCb"
}'
```

```javascript
const data = JSON.stringify({
  "auth": {
    "type": "contract",
    "token": "eyJ0eXA..._2iDq_5VSiZo"
  },
  "code": "GtEyfgCb"
});

const xhr = new XMLHttpRequest();
xhr.withCredentials = true;

xhr.addEventListener("readystatechange", function () {
  if (this.readyState === this.DONE) {
    console.log(this.responseText);
  }
});

xhr.open("POST", "https://api.stg.keynua.com/send-otp/v1");
xhr.setRequestHeader("Content-Type", "application/json");

xhr.send(data);
```

> Success response

```json
{
  "otpToken": "eyJ0eXA...qivpYpLm9R7LHid3w"
}
```

Permite verificar si el código OTP ingresado es correcto o no.

### HTTP Request

`POST https://api.stg.keynua.com/send-otp/v1`

### Request body

Atributo | Tipo | Opcional | Descripción
--------- | ----------- | ----------- | -----------
auth | [Auth](#propiedades-de-auth) | No | Permite identificar el OTP del usuario y contrato
code | string | No | Código OTP ingresado por el firmante.

### Response body

Atributo | Tipo | Descripción
--------- | ----------- | -----------
otpToken | string | Token que debe ser enviado en el _Paso 3_ al [actualizar el valor de un item](#actualizar-el-valor-de-un-item) para demostrar que se ingresó correctamente el código OTP.

## Obtener información

```ruby
require "uri"
require "net/http"

url = URI("https://api.stg.keynua.com/send-otp/v1/details")

https = Net::HTTP.new(url.host, url.port)
https.use_ssl = true

request = Net::HTTP::Post.new(url)
request["Content-Type"] = 'application/json'
request.body = "{\n\t\"auth\": {\n\t\t\"type\": \"contract\",\n\t\t\"token\": \"eyJ0eXAiOiJ...Dq_5VSiZo\"\n\t}\n}"

response = https.request(request)
puts response.read_body
```

```python
import http.client
import json

conn = http.client.HTTPSConnection("api.stg.keynua.com")

payload = json.dumps({
	{
		"auth": {
			"type": "contract",
			"token": "eyJ0eXAiOiJ...Dq_5VSiZo"
		}
	}
})

headers = {
  'Content-Type': 'application/json'
}

conn.request("POST", "/send-otp/v1/details", payload, headers)
res = conn.getresponse()
data = res.read()
print(data.decode("utf-8"))
```

```shell
curl --request POST \
  --url https://api.stg.keynua.com/send-otp/v1/details \
  --header 'Content-Type: application/json' \
  --data '{
	"auth": {
		"type": "contract",
		"token": "eyJ0eXAiOiJKV1Q...Dq_5VSiZo"
	}
}'
```

```javascript
const data = JSON.stringify({
  "auth": {
    "type": "contract",
    "token": "eyJ0eXA..._2iDq_5VSiZo"
  }
});

const xhr = new XMLHttpRequest();
xhr.withCredentials = true;

xhr.addEventListener("readystatechange", function () {
  if (this.readyState === this.DONE) {
    console.log(this.responseText);
  }
});

xhr.open("POST", "https://api.stg.keynua.com/send-otp/v1/details");
xhr.setRequestHeader("Content-Type", "application/json");

xhr.send(data);
```

> Success response

```json
{
  "sent": true,
  "channel": "sms",
  "resendInSec": 55,
  "availableAttempts": 3
}
```

Permite revisar el estado del código actual.

### HTTP Request

`POST https://api.stg.keynua.com/send-otp/v1/details`

### Request body

Atributo | Tipo | Opcional | Descripción
--------- | ----------- | ----------- | -----------
auth | [Auth](#propiedades-de-auth) | No | Permite identificar el OTP del usuario y contrato

### Response body

Atributo | Tipo | Descripción
--------- | ----------- | -----------
sent | boolean | Si el código OTP se envió o no.
channel | string | Por qué medio se realizó el envío del código.
resendInSec | number | En cuantos segundos se podrá solicitar el envío de un nuevo código.
availableAttempts | number | Cuantos intentos aún quedan disponibles.

## Errores

Code | Descripción
--------- | -----------
`OUT_OF_DELIVERIES` | Se ha excedido la cantidad permitida de notificaciones.
`CODE_SENT_RECENTLY` | El código ya se envió y necesario esperar el tiempo definido en la configuración para poder solicitar uno nuevo.
`INVALID_AUTH` | Los valores enviados en `auth` no son correctos.
`USED_CODE` | Ya se verificó correctamente el código enviado. Es necesario solicitar uno nuevo para realizar otra verificación.
`EXPIRED_CODE` | El último código enviado ya venció. Es necesario solicitar uno nuevo para realizar otra verificación.
`OUT_OF_ATTEMPTS` | La configuración de OTP ya no permite más intentos.
`WRONG_CODE` | El código enviado no es el correcto.
`INVALID_TOKEN` | El token enviado dentro de `auth` no es el correcto.
