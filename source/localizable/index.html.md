---
title: API Reference

language_tabs: # must be one of https://git.io/vQNgJ
  - shell: cURL
  - ruby: Ruby
  - python: Python
  - javascript: Node.js

toc_footers:
  - <a href='https://www.keynua.com/account-request'>Sign Up for a Developer Key</a>
  - <a href="https://github.com/tripit/slate">Documentation Powered by Slate</a>

includes:
  - errors

search: true

code_clipboard: true
---

# Introduction

> La URL base de nuestra API es distintia por ambiente:

```shell
Pruebas: https://api.stg.keynua.com
Producción: https://api.keynua.com
```

```ruby
Pruebas: https://api.stg.keynua.com
Producción: https://api.keynua.com
```

```python
Pruebas: https://api.stg.keynua.com
Producción: https://api.keynua.com
```

```javascript
Pruebas: https://api.stg.keynua.com
Producción: https://api.keynua.com
```

Bienvenid@ a la documentación del API de Keynua! Puedes usar nuestro API para poder crear contratos, obtener el detalle del mismo o eliminar contratos.

Algunas características de nuestra API:

* Está basada en REST: los payload para los `request` y `response` deben estar en formato `JSON`
* Solo soporta `HTTPS`

# Authentication

```shell
curl 'https://api.stg.keynua.com' \
  -H 'x-api-key: YOUR-APIKEY-HERE'
  -H 'authorization: YOUR-API-TOKEN-HERE' \
```

Keynua utiliza **API Keys** y **Authorization tokens** para acceder al API. Una vez ya tengas una cuenta en el ambiente de pruebas de Keynua, puedes acceder a la sección [Developers](https://app.stg.keynua.com/developers) y generar tu APIKey y APIToken. Estos valores deberán ser enviados en la cabecera de la siguiente manera:

Header | Description
--------- | -----------
x-api-key | API Key para acceder al servicio.
Authorization | Token único de autorización.

<aside class="notice">Si ya estás listo para integrar en producción, por favor contacta al equipo de <code>Keynua</code></aside>

# Contratos
El API de Contratos permite crear, ver y eliminar un contrato
## Propiedades de un Contrato

```json
{
	"id":"some:id",
	"accountId":"someone",
	"templateId":"template-id",
	"createdAt":"2021-01-24T02:52:41.426Z",
	"startedAt":"2021-01-24T02:52:42.347Z",
	"title":"Contract title",
	"description":"Description",
	"finishedAt":null,
	"deletedAt":null,
	"language":"es",
	"metadata":{
		"something":"any-value"
	},
	"reference":"reference",
	"shortCode":"12345",
	"expirationInHours":0,
	"users":User[],
	"groups":Group[],
	"documents":Document[],
	"items":Item[],
	"status":"pending_input"
}
```

Atributo | Tipo | Descripción
--------- | ----------- | -----------
id | string | Identificador único del contrato
accountId | string | Identificador único de la cuenta con la que se creó el contrato
templateId | string | Identificador del Template a usar
createdAt | string | Fecha de creación del contrato
startedAt | string | Fecha de inicio del contrato
title | string | Título del contrato
description | string | Descripción del contrato
finishedAt | string | Fecha de finalización del proceso de firma del contrato
deletedAt | string | Fecha de eliminación del contrato
language | string | Lenguaje del contrato
metadata | object | Metadata del contrato. Puedes enviar información en este campo como key-value para poder identificar el contrato creado por Keynua con algún Identificador interno de tu sistema.
reference | string | Referencia del contrato. Este campo es usado como clave de búsqueda en la plataforma web de Keynua.
shortCode | string | Código corto del contrato para facilitar su identificación
expirationInHours | integer | Expiración del contrato en horas, a partir de la fecha de inicio del contrato
status | string | Estado actual del contrato
users | array | [Usuarios](#propiedades-de-usuarios-de-un-contrato) del contrato
groups | array | [Grupos](#propiedades-de-grupos-de-un-contrato) del contrato
documents | array | [Documentos](#propiedades-de-documentos-de-un-contrato) del contrato
items | array | [Items](#propiedades-de-items-de-un-contrato) del contrato

### Propiedades de Usuarios de un Contrato

```json
{
	"id":0,
	"name":"Manuel Silva",
	"email":"msilva@keynua.com",
	"phone":null,
	"groups":[
		"signers"
	],
	"token":"token-value"
}
```

Usuarios que firman el contrato.

Atributo | Tipo | Descripción
--------- | ----------- | -----------
id | integer | Identificador único del usuario en el contrato
name | string | El nombre del usuario
email | string | El correo electrónico del usuario
phone | string | El teléfono del usuario
groups | array | Nombre de los grupos a los que pertenece el usuario, normalmente siempre pertenece a un sólo grupo. El identificador del grupo será asignado por el equipo de Keynua
token | string | El token del usuario que se utilizará para realizar la firma (actualizar los valores de cada item)

### Propiedades de Grupos de un Contrato

```json
{
	"id":"signers",
	"name":"Firmantes",
	"description":"",
	"digitalSignature":false,
	"bulk":false
}
```

Grupos que existen en el contrato.

Atributo | Tipo | Descripción
--------- | ----------- | -----------
id | string | Identificador del grupo
name | string | El nombre del grupo
description | string | Descripción del grupo
digitalSignature | boolean | Flag que indica si el grupo realizará firma digital o no
bulk | boolean | Flag que indica si el grupo pertenece a Firma Múltiple o no

### Propiedades de Documentos de un Contrato

```json
{
	"id":0,
	"name":"DocumentPdf.pdf",
	"ext":"pdf",
	"sha":"pdf-hash",
	"size":3708,
	"type":"application/pdf",
	"url":"signed-url"
}
```

Los documentos que son firmados en el contrato.

Atributo | Tipo | Descripción
--------- | ----------- | -----------
id | integer | Identificador único del documento
name | string | Nombre del documento incluyendo su extensión
ext | string | Extensión del documento
sha | string | Código sha256 del documento
size | integer | Tamaño en bytes del documento
type | string | Tipo del documento
url | string | La URL para poder visualizar el documento

### Propiedades de Items de un Contrato

```json
{
	"id": 4,
	"version": 1,
	"state": "success",
	"userId": null,
	"reference": "pdf",
	"title": "Certificado y documentos",
	"type": "pdf",
	"stageIndex": 1,
	"value": {
		"url": "signed-url"
	}
}
```

Para llevar a cabo el flujo de firma de un contrato, cada uno de los usuarios deberá ingresar la información necesaria para firmar. Por ejemplo: la foto de su DNI, el video diciendo el código corto o el número de su DNI. También hace referencia a los procesos internos llevados a cabo por Keynua, por ejemplo la generación del PDF final o validación biométrica. A cada uno de estos elementos los llamamos **Item**

Atributo | Tipo | Descripción
--------- | ----------- | -----------
id | integer | Identificador del item
version | integer | Versión del item. Siempre se mostrará la última versión disponible
state | string | El estado del item. Puede tener los siguientes valores: `success`, `pending`, `working`, `error`
userId | integer | El identificador del usuario al que está relacionado este item
reference | string | La referencia del item (está relacionado con la plantilla del contrato)
title | string | Un título referente al item
type | string | El tipo del item
stageIndex | integer | El índice del nivel al que pertenece el item
value | object | El valor del item. La estructura varía de acuerdo al tipo del item. Cuando se trata de un Item que contiene un Archivo, habrá un key **url** el cual contiene la URL firmada para poder descargar el archivo. **Esta URL firmada tiene una duración máxima de 12 horas**

## Crear un Contrato

```ruby
require 'uri'
require 'net/http'
require 'openssl'

url = URI("https://api.stg.keynua.com/contracts/v1")

http = Net::HTTP.new(url.host, url.port)
http.use_ssl = true
http.verify_mode = OpenSSL::SSL::VERIFY_NONE

request = Net::HTTP::Put.new(url)
request["authorization"] = 'YOUR-API-TOKEN-HERE'
request["x-api-key"] = 'YOUR-API-KEY-HERE'
request["content-type"] = 'application/json'
request.body = "{\n  \"title\": \"Contract created by API\",\n  \"language\": \"es\",\n  \"userEmailNotifications\": true,\n\t\"templateId\": \"keynua-peru-default\",\n  \"documents\": [\n    {\n      \"name\": \"DocumentPdf.pdf\",\n      \"base64\": \"YOUR-BASE64-PDF-HERE\"\n    }\n  ],\n  \"users\": [\n    {\n      \"name\": \"Manuel Silva\",\n      \"email\": \"msilva@keynua.com\",\n\t\t\"groups\": [ \"signers\" ]\n    }\n  ]\n}\n"

response = http.request(request)
puts response.read_body
```

```python
import http.client

conn = http.client.HTTPSConnection("api.stg.keynua.com")

payload = "{\n  \"title\": \"Contract created by API\",\n  \"language\": \"es\",\n  \"userEmailNotifications\": false,\n\t\"templateId\": \"keynua-peru-default\",\n  \"documents\": [\n    {\n      \"name\": \"DocumentPdf.pdf\",\n      \"base64\": \"YOUR-BASE64-PDF-HERE\"\n    }\n  ],\n  \"users\": [\n    {\n      \"name\": \"Manuel Silva\",\n      \"email\": \"msilva@keynua.com\",\n\t\t\"groups\": [ \"signers\" ]\n    }\n  ]\n}\n"

headers = {
    'x-api-key': "YOUR-API-KEY-HERE",
    'authorization': "YOUR-API-TOKEN-HERE",
    'content-type': "application/json"
    }

conn.request("PUT", "/contracts/v1", payload, headers)

res = conn.getresponse()
data = res.read()

print(data.decode("utf-8"))
```

```shell
curl --request PUT \
  --url https://api.stg.keynua.com/contracts/v1 \
  --header 'x-api-key: YOUR-API-KEY-HERE' \
  --header 'authorization: YOUR-API-TOKEN-HERE' \
  --header 'content-type: application/json' \
  --data '{
  "title": "Contract created by API",
  "language": "es",
  "userEmailNotifications": true,
  "templateId": "keynua-peru-default",
  "documents": [
    {
      "name": "DocumentPdf.pdf",
      "base64": "YOUR-BASE64-PDF-HERE"
    }
  ],
  "users": [
    {
      "name": "Manuel Silva",
      "email": "msilva@keynua.com",
	  "groups": [ "signers" ]
    }
  ]
}'
```

```javascript
const https = require("https");

const data = JSON.stringify({
  title: 'Contract created by API',
  language: 'es',
  userEmailNotifications: true,
  templateId: 'keynua-peru-default',
  documents: [
    {
      name: 'DocumentPdf.pdf',
      base64: 'YOUR-BASE64-PDF-HERE'
    },
  ],
  users: [
    {name: 'Manuel Silva', email: 'msilva@keynua.com', groups: ['signers']}
  ]
});

const options = {
  method: "PUT",
  hostname: "api.stg.keynua.com",
  path: "/contracts/v1",
  headers: {
    "x-api-key": "YOUR-API-KEY-HERE",
    "authorization": "YOUR-API-TOKEN-HERE",
    "content-type": "application/json",
    "content-length": data.length
  }
};

const req = https.request(options, function (res) {
  const chunks = [];

  res.on("data", function (chunk) {
    chunks.push(chunk);
  });

  res.on("end", function () {
    const body = Buffer.concat(chunks);
    console.log(body.toString());
  });
});


req.on('error', (error) => {
  console.error(error)
});

req.write(data);
req.end();
```

> Si el contrato fue creado satisfactoramente, el API retorna un Json estructurado como aparece en la sección de [Contratos](#contratos)

### HTTP Request

`PUT /contracts/v1`

### Headers

Key | Value
--------- | -----------
x-api-key | your-api-key
Authorization | your-api-token
Content-Type | application/json

### Body

Para crear el contrato, se tiene que enviar la data como un solo objeto JSON

Atributo | Tipo | Descripción
--------- | ----------- | -----------
title | string | Título del contrato
language | string | Idioma del contrato. Default `es`
userEmailNotifications | boolea | Indica si los usuarios serán notificados por email cuando hay un error o finaliza un contrato. Default `false`
templateId | string | Id del template a usar. Puedes usar uno de los template públicos de Keynua como `keynua-peru-default`. Si es un proceso customizado, el equipo de Keynua te enviará este valor
documents | array | Arreglo de los documentos PDFs encodificados en base64 que van a ser firmados. Mínimo 1 y máximo 10. El peso máximo en total no debe ser mayor a 4.5 MB
users | array | Arreglo de los usuarios que firmarán el contrato. El email es opcional y el valor a enviar en **groups** depende del templateId a usar. Para el caso de `keynua-peru-default`, el valor en groups debe ser `signers`. Si utilizan un template customizado en el que hay más de un grupo, por ejemplo firmas con DNI + Firma múltiple, el valor del grupo representará al grupo que pertenece dicho usuario
## Obtener un Contrato

```ruby
require 'uri'
require 'net/http'
require 'openssl'

url = URI("https://api.stg.keynua.com/contracts/v1/{contractId}")

http = Net::HTTP.new(url.host, url.port)
http.use_ssl = true
http.verify_mode = OpenSSL::SSL::VERIFY_NONE

request = Net::HTTP::Get.new(url)
request["x-api-key"] = 'YOUR-API-KEY-HERE'
request["authorization"] = 'YOUR-API-TOKEN-HERE'

response = http.request(request)
puts response.read_body
```

```python
import http.client

conn = http.client.HTTPSConnection("api.stg.keynua.com")

headers = {
    'x-api-key': "YOUR-API-KEY-HERE",
    'authorization': "YOUR-API-TOKEN-HERE"
    }

conn.request("GET", "/contracts/v1/{contractId}", "", headers)

res = conn.getresponse()
data = res.read()

print(data.decode("utf-8"))
```

```shell
curl --request GET \
  --url 'https://api.stg.keynua.com/contracts/v1/{contractId}' \
  --header 'x-api-key: YOUR-API-KEY-HERE' \
  --header 'authorization: YOUR-API-TOKEN-HERE' \
```

```javascript
const http = require("https");

const options = {
  "method": "GET",
  "hostname": "api.stg.keynua.com",
  "path": "/contracts/v1/{contractId}",
  "headers": {
    "x-api-key": "YOUR-API-KEY-HERE",
    "authorization": "YOUR-API-TOKEN-HERE",
    "content-length": "0"
  }
};

const req = http.request(options, function (res) {
  const chunks = [];

  res.on("data", function (chunk) {
    chunks.push(chunk);
  });

  res.on("end", function () {
    const body = Buffer.concat(chunks);
    console.log(body.toString());
  });
});

req.end();
```

> Si el contrato fue obtenido satisfactoramente, el API retorna un Json estructurado como aparece en la sección de [Contratos](#contratos)

Este API obtiene un contrato específico

### HTTP Request

`GET /contracts/v1/{contractId}`

### URL Parameters

Parámetro | Descripción
--------- | -----------
contractId | El ID del Contrato a obtener

## Eliminar un Contrato

```ruby
require 'uri'
require 'net/http'
require 'openssl'

url = URI("https://api.stg.keynua.com/contracts/v1/{contractId}")

http = Net::HTTP.new(url.host, url.port)
http.use_ssl = true
http.verify_mode = OpenSSL::SSL::VERIFY_NONE

request = Net::HTTP::Delete.new(url)
request["authorization"] =
request["x-api-key"] = 'YOUR-API-KEY-HERE'
request["authorization"] = 'YOUR-API-TOKEN-HERE'

response = http.request(request)
puts response.read_body
```

```python
import http.client

conn = http.client.HTTPSConnection("api.stg.keynua.com")

headers = {
    'x-api-key': "YOUR-API-KEY-HERE",
    'authorization': "YOUR-API-TOKEN-HERE"
    }

conn.request("DELETE", "/contracts/v1/{contractId}", "", headers)

res = conn.getresponse()
data = res.read()

print(data.decode("utf-8"))
```

```shell
curl --request DELETE \
  --url https://api.dev.keynua.com/contracts/v1/{contractId} \
  --header 'x-api-key: YOUR-API-KEY-HERE' \
  --header 'authorization: YOUR-API-TOKEN-HERE' \
```

```javascript
const https = require("https");

const options = {
  method: "DELETE",
  hostname: "api.stg.keynua.com",
  path: "/contracts/v1/{contractId}",
  headers: {
    "x-api-key": "YOUR-API-KEY-HERE",
    "authorization": "YOUR-API-TOKEN-HERE"
  }
};

const req = https.request(options, function (res) {
  const chunks = [];

  res.on("data", function (chunk) {
    chunks.push(chunk);
  });

  res.on("end", function () {
    const body = Buffer.concat(chunks);
    console.log(body.toString());
  });
});

req.end();
```

> Si el contrato fue eliminado, el API retornará un JSON como el siguiente:

```json
{
  "deletedAt": "2021-01-25T21:38:50.055Z"
}
```

Este API elimina un Contrato

### HTTP Request

`DELETE /contracts/v1/{contractId}`

### URL Parameters

Parámetro | Descripción
--------- | -----------
contractId | El ID del Contrato a eliminar

<aside class="warning">No se puede eliminar un contrato que su estado es <code>done</code>. Si eliminas un contrato que aún no ha sido iniciado por el firmante, la transacción utilizada será reintegrada a tu saldo. Si el contrato ya fue iniciado por el firmante, se tomará como una transacción utilizada.</aside>
