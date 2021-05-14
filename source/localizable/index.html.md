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

# Introducción

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

# Autenticación

```shell
curl 'https://api.stg.keynua.com' \
  -H 'x-api-key: YOUR-APIKEY-HERE'
  -H 'authorization: YOUR-API-TOKEN-HERE' \
```

Keynua utiliza **API Keys** y **Authorization tokens** para acceder al API. Una vez ya tengas una cuenta en el ambiente de pruebas de Keynua, puedes acceder a la sección [Developers](https://app.stg.keynua.com/developers) y generar tu APIKey y APIToken. Estos valores deberán ser enviados en la cabecera de la siguiente manera:

Header | Description
--------- | -----------
x-api-key | API Key para acceder al servicio.
authorization | Token único de autorización.

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
metadata | object | Metadata del contrato
reference | string | Referencia del contrato. Este campo es usado como clave de búsqueda en la plataforma web de Keynua.
shortCode | string | Código corto del contrato para facilitar su identificación
expirationInHours | integer | Expiración del contrato en horas, a partir de la fecha de inicio del contrato
users | array | [Usuarios](#propiedades-de-un-usuario) del contrato
groups | array | [Grupos](#propiedades-de-un-grupo) del contrato
documents | array | [Documentos](#propiedades-de-un-documento) del contrato
items | array | [Items](#items-del-contrato) del contrato
status | string | [Estado del contrato](#estados-del-contrato) actual

### Estados del Contrato

Valor | Descripción
--------- | -----------
pending_input | Estado inicial del contrato, cuando ha sido creado pero aún nadie ha comenzado el proceso de firma
working | Cuando los Items del contrato están en proceso de ejecución
pending_approval | Cuando existe un Item pendiente a ser aprobado manualmente por el dueño del contrato
error | Cuando existe un error sin resolver en uno de los Items del contrato
deleted | Cuando el contrato ha sido eliminado
done | Cuando el contrato ha sido firmado por todos y ha finalizado correctamente

### Propiedades de un Usuario

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
token | string | El token del usuario que se utilizará para realizar la firma. Por ejemplo para [actualizar los valores de cada item](#actualizar-el-valor-de-un-item)

### Propiedades de un Grupo

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

### Propiedades de un Documento

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

## Cavali

```json
{
	"banking": 6,
	"product": 3,
	"uniqueCode": 5478700,
	"issuedDate": "2020-09-20",
	"issuedPlace": "Lima",
	"client": {
	  "userId": 0,
	  "civilStatus": 2,
	  "domicile": "Lima"
	},
	"representatives": [
	  {
		"userId": 1
	  }
	]
}
```

Cavali permite crear un Pagaré y asociarlo a la firma de un contrato. Para crear un contrato, el proceso debe tener un Item Cavali y Item de tipo text con el id `documentNumber`

<aside class="warning">Si quieres crear contratos con Pagarés electrónicos, debes contactar al equipo de soporte de Keynua</aside>

El Pagaré esta compuesto por lo siguiente:

Atributo | Tipo | Descripción
--------- | ----------- | -----------
banking | integer | Código de banca
product | integer | Código de producto
uniqueCode | integer | Código único del cliente
issuedDate | string | Fecha de emisión. Formato: YYYY-MM-dd
conditionJustSign | integer | Condición del pagaré. Puede tener los siguientes valores: 1 (si) | 2 (no)
special | integer | Indicador de pagaré especial. Puede tener los siguientes valores: 1 (si) | 2 (no)
issuedPlace | string | Lugar de emisión
expirationDate | string | Fecha de caducidad. Formato: YYYY-MM-dd
amount | double | Monto del pagaré
currency | integer | Moneda del pagaré. Puede tener los siguientes valores: 1 (S/) | 2 (US$)
compensatoryInterestAmount | double | El interés compensatorio sobre el monto
periodOne | integer | El período de capitalización 1
compensatoryInterestArrears | double | El interés compensatorio por el período de morosidad
periodTwo | integer | El período de capitalización 2
interestArrears | double | Interés moratorio del periodo
periodTwo | integer | El período de capitalización 3
specialClauses | string | Las Cláusulas especiales que pueda contener el Pagaré
token | string | El token de validación de firmas para proveedores
additionalField1 | string | Campo adicional 1
additionalField2 | string | Campo adicional 2
client | object | [Cliente](#cliente-cavali) del Pagaré
representatives | array | Arreglo de [Representantes](#representantes-cavali) del cliente
guarantees | array | Arreglo de [Garantías](#garantías-cavali) del cliente

### Cliente Cavali
El cliente del pagaré. El elemento está compuesto por:

Atributo | Tipo | Descripción
--------- | ----------- | -----------
userId | integer | Id del firmante
civilStatus | integer |  Estado civil del cliente. Puede tener los siguientes valores: `1 (SOLTERO)`, `2 (CASADO)`, `3 (DIVORCIADO)`, `4 (VIUDO)`
domicile | string | Domicilio del cliente `Máximo 100 de longitud`

### Representantes Cavali
Representante del cliente. El elemento está compuesto por:

Atributo | Tipo | Descripción
--------- | ----------- | -----------
userId | integer | Id del firmante

### Garantías Cavali
Garantía del cliente. El elemento está compuesto por:

Atributo | Tipo | Descripción
--------- | ----------- | -----------
userId | integer | Id del firmante
civilStatus | integer | Estado civil. Puede tener los siguientes valores: `1 (SOLTERO)`, `2 (CASADO)`, `3 (DIVORCIADO)`, `4 (VIUDO)`
domicile | string | Domicilio `Máximo 100 de longitud`
representative | array | Arreglo de [Representantes](#representantes-cavali)

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
authorization | your-api-token
Content-Type | application/json

### Body

Para crear el contrato, se tiene que enviar la data como un solo objeto JSON

Atributo | Tipo | Descripción
--------- | ----------- | -----------
title | string | Título del contrato
description | string | `optional` Descripción del contrato
reference | string | `optional` Referencia del contrato. Este campo es usado como clave de búsqueda en la plataforma web de Keynua
language | string |  `Default "es"` Idioma del contrato. Puede ser `en` o `es`
userEmailNotifications | boolean | `Default "false"` Indica si los usuarios serán notificados por email cuando hay un error o finaliza un contrato
expirationInHours | integer | `optional` Expiración del contrato en horas, a partir de la fecha de inicio del contrato. Mínimo uno (1)
templateId | string | Id del template a usar. Puedes usar uno de los template públicos de Keynua como `keynua-peru-default`. Si es un proceso customizado, el equipo de Keynua te enviará este valor
documents | array | Arreglo de los documentos PDFs encodificados en base64 que van a ser firmados. Mínimo 1 y máximo 10. El peso máximo en total no debe ser mayor a 4.5 MB. En lugar de `base64` también se puede enviar `storageId`, como por ejemplo se obtiene de [este](#generar-documentos-rellenados) API.
users | array | Arreglo de los usuarios que firmarán el contrato. El email es opcional y el valor a enviar en **groups** depende del templateId a usar. Para el caso de `keynua-peru-default`, el valor en groups debe ser `signers`. Si utilizan un template customizado en el que hay más de un grupo, por ejemplo firmas con DNI + Firma múltiple, el valor del grupo representará al grupo que pertenece dicho usuario
metadata | object | `optional` Metadata del contrato. Puedes enviar información en este campo como key-value para poder identificar el contrato creado por Keynua con algún Identificador interno de tu sistema.
flags | object | `optional` Se podrá enviar información adicional para crear un contrato. Por ejemplo la información de [Cavali](#cavali) para crear un contrato con Pagaré Electrónico se enviará con el key `cavaliData`
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
    "authorization": "YOUR-API-TOKEN-HERE"
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
  --url https://api.stg.keynua.com/contracts/v1/{contractId} \
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

# Items del Contrato
Para llevar a cabo el flujo de firma de un contrato, cada uno de los usuarios deberá ingresar la información necesaria para firmar. Por ejemplo: la foto de su DNI, el video diciendo el código corto o el número de su DNI. También hace referencia a los procesos internos llevados a cabo por Keynua, por ejemplo la generación del PDF final o validación biométrica. A cada uno de estos elementos los llamamos **Item**
## Propiedades de un Item

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

Atributo | Tipo | Descripción
--------- | ----------- | -----------
id | integer | Identificador del item
version | integer | Versión del item. Siempre se mostrará la última versión disponible
state | string | El estado del item. Puede tener los siguientes valores: `success`, `pending`, `working`, `error`
userId | integer | El identificador del usuario al que está relacionado este item
reference | string | La referencia del item (está relacionado con la plantilla del contrato)
title | string | Un título referente al item
type | string | El ID del [tipo del Item](#tipos-de-item) al que pertenece
stageIndex | integer | El índice del nivel al que pertenece el item
value | object | El valor del item. La estructura varía de acuerdo al tipo del item. Cuando se trata de un Item que contiene un Archivo, habrá un key **url** el cual contiene la URL firmada para poder descargar el archivo. **Las URLs firmadas tienen una duración máxima de 12 horas**

## Tipos de Item
En la siguiente tabla podrás ver los Tipos de Items que existen en Keynua

Item | Id | User Input | Descripción
--------- | ----------- | ----------- | -----------
Términos y condiciones | terms | `si` | Hace  referencia a si el usuario ha aceptado o no los términos y condiciones
Verificación de documentos | documents | `si` | Indica que se le han mostrado los documentos a firmar al usuario
Campo de Texto | text | `si` | Campo de texto ingresado por el usuario o por el creador del contrato, por ejemplo el número de Documento de Identidad
Imagen | image | `si` | Hace referencia a una imagen subida por un usuario, por ejemplo, foto del documento de identidad
Firma Ológrafa | imagesign | `si` | Firma ológrafa realizada por el usuario en el proceso de firma
Video | video | `si` | Video realizado por el usuario, en la mayoría de los casos hace referencia a la videofirma.
Email | email | `no` | Hace referencia al envío del email a un usuario
Reconocimiento de Documento | detectlabels | `no` | Proceso de Reconocimiento del formato de un Documento. Por ejemplo, si se le pide subir a un usuario la foto de su DNI del país y sube la foto de su pasaporte, este proceso detectará que no es el documento solicitado y arrojará un error
Reconocimiento de textos | checklabels | `no` | Este proceso realiza la verificación de textos entre 2 elementos, por ejemplo la búsqueda del documento de identidad ingresado y los textos extraídos de la imagen del documento
Conversión de Video | convertvideo | `no` | Proceso que indica que el video del firmante tiene un formato correcto para ser procesado
Reniec | reniec | `no` | Proceso que indica el resultado de la búsqueda del DNI ingresado en Reniec
Validación de Biometría Facial | facematch | `no` | Proceso que indica la validación Biométrica entre 2 elementos que pueden ser por ejemplo la foto obtenida de RENIEC y la videofirma realizada por el usuario
Reconocimiento de texto hablado | transcribe | `no` | Indica si el usuario dijo como texto hablado el código que corresponde con los documentos que está firmando
Prueba de vida | livenessdetection | `no` | Indica si se detectó vida en el video del usuario
Aprobación Manual| manualapproval | `no` | Indica si hubo una aprobación manual de algún Item como `detectlabels`, `facematch` o `transcribe`
Generación de imágenes para insertar en el PDF final | generatethumbnails | `no` | Proceso que indica si se pudieron obtener los thumbnails necesarios antes de  poder crear el PDF de documento de firma final (Certificado)
Documento de Firma Electrónica/Digital | pdf | `no` | Indica si el archivo PDF de la firma electrónica se generó satisfactoriamente. **En este Item puedes encontrar el PDF final de firma electrónica generado por Keynua**
Firma Digital | dsignature | `no` | Proceso de firma digital aplicado a un archivo de firma electrónica. En este Item puedes encontrar el archivo digital, en caso de que tu proceso incluya firma digital
Cavali - Perú | cavali | `no` | Proceso que indica el registro de un Pagaré electrónico en Cavali para Perú
Blockchain | blockchain | `no` | Proceso que indica si se registró correctamente el hash del documento de firma electrónica final(certificado) en Blockchain
Normativa NOM151 - México | knom151 | `no` | Proceso que indica si se generó correctamente la constancia de conservación según la norma Méxicana NOM151
Firma Múltiple | bulksignature | `no` | Indica si se solicitó firmar al usuario de firma múltiple o si firmó satsifactoriamente. El estado "error" no está implementado por el momento en este Item.
## Actualizar el valor de un item

<aside class="notice">Este procedimiento sólo será realizado en caso quieran construir su propio proceso de firma. Keynua ofrece el proceso de firmar sin costo adicional bajo el propio dominio de Keynua o la posibilidad de incrustar el proceso de firma bajo tu dominio o App móvil mediante nuestro <code><a href="https://github.com/Keynua/public-docs/wiki/Widget">Keynua Widget</a></code></aside>

Para poder actualizar el valor de cada item, se debe utilizar la información que se obtiene luego de crear un certificado o de obtener un certificado por id. Así, podemos saber los tipos (type) de cada item y seguir los siguientes pasos para realizar la actualización.

<aside class="warning">Este procedimiento solo aplica para los items del tipo <code>image</code> y <code>video</code></aside>

### Paso 1: solicitar la información antes de subir un archivo

> Body payload

```json
{
  "token": "some:user:token",
  "name": "some_file.pdf",
  "md5": "9a333cae630cf48165d18a9b1d33f5dd"
}
```

> Si la solicitud fue satisfactoria, la respuesta tendrá la información para que puedas subir el contenido del archivo

```json
{
  "linkId": "some:id",
  "url": "https://some:url:for:upload",
  "method": "PUT",
  "headers": {
    "Content-Type": "image/jpeg",
    "Content-MD5": "solo si se envió md5"
  }
}
```

### HTTP Request

`POST /contracts/v1/sign/upload`

### Headers

Key | Value
--------- | -----------
Content-Type | application/json

### Body

Atributo | Tipo | Descripción
--------- | ----------- | -----------
token | string | El token del usuario del cual se actualizará el item
name | string | El nombre del archivo que se va a subir. Debe incluir su extensión
md5 | string | `opcional` El valor md5 del archivo

### Paso 2: subir el contenido del archivo

```
curl --request PUT \
  --url 'https://some:url:for:upload' \
  --header 'content-type: image/jpeg' \
  --header 'Content-MD5: contentMD5' \
  --data some_file.jpeg
```

> Si no hubo errores al subir el archivo, se retornará el código de estado 200

Con la información obtenida en el paso 1, se subirá el archivo.

### HTTP Request

`POST {URL-STEP-1}`

### Headers

Key | Value
--------- | -----------
Content-Type | {Content-Type-Step-1}
Content-MD5 | contentMD5
### Body

El archivo en Base64

### Paso 3: actualizar el valor del item

```shell
curl --request PUT \
  --url https://api.stg.keynua.com/contracts/v1/sign \
  --header 'content-type: application/json' \
  --data '{
	"token": "USER-TOKEN-HERE",
	"itemId": 2,
	"version": 8,
	"value": {
		"linkId": "LINK-ID-HERE"
	}
}'
```

```javascript
const http = require("https");

const data = JSON.stringify({
  token: 'USER-TOKEN-HERE',
  itemId: 2,
  version: 8,
  value: {
    linkId: 'LINK-ID-HERE'
  }
});

const options = {
  method: "PUT",
  hostname: "api.stg.keynua.com",
  path: "/contracts/v1/sign",
  headers: {
    "content-type": "application/json",
    "content-length": data.length
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

req.on('error', (error) => {
  console.error(error)
});

req.write(data);
req.end();
```

```python
import http.client

conn = http.client.HTTPSConnection("api.stg.keynua.com")

payload = "{\n\t\"token\": \"USER-TOKEN-HERE\",\n\t\"itemId\": 2,\n\t\"version\": 8,\n\t\"value\": {\n\t\t\"linkId\": \"LINK-ID-HERE\"\n\t}\n}"

headers = { 'content-type': "application/json" }

conn.request("PUT", "/contracts/v1/sign", payload, headers)

res = conn.getresponse()
data = res.read()

print(data.decode("utf-8"))
```

```ruby
require 'uri'
require 'net/http'
require 'openssl'

url = URI("https://api.stg.keynua.com/contracts/v1/sign")

http = Net::HTTP.new(url.host, url.port)
http.use_ssl = true
http.verify_mode = OpenSSL::SSL::VERIFY_NONE

request = Net::HTTP::Put.new(url)
request["content-type"] = 'application/json'
request.body = "{\n\t\"token\": \"USER-TOKEN-HERE\",\n\t\"itemId\": 2,\n\t\"version\": 8,\n\t\"value\": {\n\t\t\"linkId\": \"LINK-ID-HERE\"\n\t}\n}"

response = http.request(request)
puts response.read_body
```

> Si la actualización fue exitosa, se devolverá el cuerpo del [item](#items-del-contrato) actualizado

En este paso se realizará la actualización de la imagen o el video del item.
### HTTP Request

`PUT /contracts/v1/sign`

### Headers

Key | Value
--------- | -----------
Content-Type | application/json

### Body

Atributo | Tipo | Descripción
--------- | ----------- | -----------
token | string | El token del usuario del cual se actualizará el item
itemId | integer | El identificador del item del que se actualizará el valor
version | integer | La versión del item del que se actualizará el valor
value | object | Dentro del objeto value debes enviar el linkId obtenido en el paso 1 con el key `linkId`

<aside class="notice">Los valores <code>itemId</code> y <code>version</code> del Item se obtienen de la respuesta de crear un Contrato o de obtener un Contrato por id</aside>

# Plantillas de documento

Permite listar, obtener y rellenar plantillas de documentos PDF.

<aside class="notice">Estas plantillas son creadas únicamente desde el Web App. Si quieres usar esta funcionalidad ponte en contacto con el equipo de soporte de Keynua.</aside>

## Propiedades de una plantilla de documento

```json
{
  "templateId": "2021-05-07T22:10:14.935Z8f8bc671-626d-4212-9c8c-c5d5f9464fa6",
  "accountId": "48101c38-f770-4ea8-88ab-248e672d88bd",
  "name": "A document template",
  "creatorEmail": "creator@keynua.com",
  "fields": DocumentTemplateField[],
  "files": DocumentTemplateFile[],
  "updatedAt": "2021-05-07T22:10:14.935Z",
  "createdAt": "2021-05-07T22:10:14.935Z"
}
```

Atributo | Tipo | Opcional | Descripción
--------- | ----------- | ----------- | -----------
templateId | string | No | El id de la plantilla de documento
accountId | string | No | El id del usuario que creó la plantilla de documento
name | string | No | El nombre de la plantilla de documento
creatorEmail | string | No | El email del usuario que creó la plantilla de documento
fields | [DocumentTemplateField](#propiedades-de-documenttemplatefield)[] | No | Contiene las reglas para cada campo de la plantilla de documento. Los valores de estos campos serán utilizados para generar los archivos finales.
files | [DocumentTemplateFile](#propiedades-de-documenttemplatefile)[] | No | Los archivos pdf que serán utilizados como base para generar los archivos que incluyan los valores de los campos
updatedAt | string | No | La fecha y hora de la última modificación en formato ISO
createdAt | string | No | La fecha y hora de creación en formato ISO

### Propiedades de DocumentTemplateField

```json
{
  "id": "email-0",
  "name": "Email",
  "type": "string",
  "required": true,
  "default": "default value",
  "minLength": 2,
  "maxLength": 10,
  "regex": "^\\d?$",
  "placeholder": "A placeholder"
}
```

Atributo | Tipo  | Opcional | Descripción
--------- | ----------- | ----------- | -----------
id | string | No | El id del campo
name | string | No | El nombre del campo
type | string | No | El tipo del campo. `string`, `date` o `number`.
required | boolean | No | Si el campo es obligatorio
default | string | Sí | Un valor por defecto para el campo
minLength | number | Sí | La longitud mínima para campos de tipo `string`
maxLength | number | Sí | La longitud máxima para campos de tipo `string`
regex | string | Sí | Regex para validar campos de tipo `string`
placeholder | string | Sí | Texto adicional


### Propiedades de DocumentTemplateFile

```json
{
  "url": "https://s3.amazonaws.com/48101c38-f770-4ea8-88ab-25",
  "id": "b5d1b359-3736-424b-b83d-f0d0da910a89",
  "name": "isotype_stamp.pdf",
  "size": 3708
}
```

Atributo | Tipo  | Opcional | Descripción
--------- | ----------- | ----------- | -----------
url | string | No | Url que contiene el documento original
id | string | No | Id del archivo
name | boolean | No | Nombre del archivo
size | number | No | Tamaño del archivo


## Listar plantillas de documento

```ruby
require "uri"
require "net/http"

url = URI("https://api.stg.keynua.com/contract-manager-document-templates/api")

https = Net::HTTP.new(url.host, url.port)
https.use_ssl = true

request = Net::HTTP::Get.new(url)
request["x-api-key"] = 'YOUR-API-KEY-HERE'
request["authorization"] = 'YOUR-API-TOKEN-HERE'

response = https.request(request)
puts response.read_body
```

```python
import http.client

conn = http.client.HTTPSConnection("api.stg.keynua.com")
headers = {
  'x-api-key': "YOUR-API-KEY-HERE",
  'authorization': "YOUR-API-TOKEN-HERE",
}
conn.request("GET", "/contract-manager-document-templates/api", payload, headers)
res = conn.getresponse()
data = res.read()
print(data.decode("utf-8"))
```

```shell
curl --location --request GET 'https://api.stg.keynua.com/contract-manager-document-templates/api' \
  --header 'x-api-key: YOUR-API-KEY-HERE' \
  --header 'authorization: YOUR-API-TOKEN-HERE'
```

```javascript
const https = require("https");

var options = {
  'method': 'GET',
  'hostname': 'api.stg.keynua.com',
  'path': '/contract-manager-document-templates/api',
  'headers': {
    'x-api-key': 'YOUR-API-KEY-HERE',
    'authorization': 'YOUR-API-TOKEN-HERE'
  },
};

var req = https.request(options, function (res) {
  var chunks = [];

  res.on("data", function (chunk) {
    chunks.push(chunk);
  });

  res.on("end", function (chunk) {
    var body = Buffer.concat(chunks);
    console.log(body.toString());
  });

  res.on("error", function (error) {
    console.error(error);
  });
});

req.end();
```

> Success response

```json
{
  "templates": [
    {
      "templateId": "2021-05-07T22:10:14.935Z8f8bc671-626d-4212-9c8c-c5d5f9464fa6",
      "accountId": "48101c38-f570-4ea8-88ab-258e672d88bd",
      "name": "test 1",
      "creatorEmail": "creator@keynua.com",
      "fileCount": 5,
      "fieldCount": 2,
      "updatedAt": "2021-05-07T22:10:14.935Z",
      "createdAt": "2021-05-07T22:10:14.935Z"
    },
    {
      "templateId": "2021-02-11T21:18:18.463Z962ce87f-d3b7-4cbb-afa0-04e85e2791b2",
      "accountId": "48101c38-f770-4e28-88ab-258e672d88bd",
      "name": "test 2",
      "creatorEmail": "creator@keynua.com",
      "fileCount": 1,
      "fieldCount": 1,
      "updatedAt": "2021-02-11T21:18:18.463Z",
      "createdAt": "2021-02-11T21:18:18.463Z"
    }
  ],
  "next": "eyJwayI6IjQ4MTAxYzM4LWY3NzAtNGVhOC04OGFiLTI1OGU2NzJkODhiZDp0ZW1wbGF0ZSIsInJrIjoiMjAyMS0wMi0wMlQyMToxMzozNy42NDhaNGE3YWZiZDEtMTQ0Yi00YmRmLWI1NTUtOWFlZTQ2MDMxZGE1In0="
}
```

Permite obtener una lista de las plantillas de documento que pertenecen al usuario o a su organización.

### HTTP Request

`GET /contract-manager-document-templates/api`

### Headers

Key | Value
--------- | -----------
x-api-key | your-api-key
authorization | your-api-token

### Search params

Nombre | Tipo | Opcional | Descripción
--------- | ----------- | ----------- | -----------
limit | string | Sí | La cantidad máxima de items que el api debe devolver. Si no se envía, el api devuelve todos.
start | string | Sí | Sirve para obtener la siguiente porción de items en la lista. Recibe el valor de `next` que se obtiene en la respuesta.
organizational | boolean | Sí | `true` si se quiere las plantillas de documento de la organización


### Response body

Atributo | Tipo | Descripción
--------- | ----------- | -----------
templates | [TemplateItem](#response-body-templateitem) | Lista de plantillas de documento
next | string | `opcional` Envía este valor en `start` para obtener la siguiente porción de la lista. Si no se encuetra en la respuesta, entonces no quedan más items en la lista.


### Response body TemplateItem

Atributo | Tipo | Descripción
--------- | ----------- | -----------
templateId | string | El id de la plantilla de documento
accountId | string | El id del usuario que creó la plantilla de documento
name | string | El nombre de la plantilla de documento
creatorEmail | string | El email del usuario que creó la plantilla de documento
fileCount | number | Cantidad de archivos en la plantilla
fieldCount | number | Cantidad de campos en la plantilla
updatedAt | string | La fecha y hora de la última modificación en formato ISO
createdAt | string | La fecha y hora de creación en formato ISO

## Obtener plantilla de documento

```ruby
require "uri"
require "net/http"

url = URI("https://api.stg.keynua.com/contract-manager-document-templates/api/{templateId}")

https = Net::HTTP.new(url.host, url.port)
https.use_ssl = true

request = Net::HTTP::Get.new(url)
request["x-api-key"] = 'YOUR-API-KEY-HERE'
request["authorization"] = 'YOUR-API-TOKEN-HERE'

response = https.request(request)
puts response.read_body
```

```python
import http.client

conn = http.client.HTTPSConnection("api.stg.keynua.com")
headers = {
  'x-api-key': "YOUR-API-KEY-HERE",
  'authorization': "YOUR-API-TOKEN-HERE",
}
conn.request("GET", "/contract-manager-document-templates/api/{templateId}", payload, headers)
res = conn.getresponse()
data = res.read()
print(data.decode("utf-8"))
```

```shell
curl --location --request GET 'https://api.stg.keynua.com/contract-manager-document-templates/api/{templateId}' \
  --header 'x-api-key: YOUR-API-KEY-HERE' \
  --header 'authorization: YOUR-API-TOKEN-HERE'
```

```javascript
const https = require("https");

var options = {
  'method': 'GET',
  'hostname': 'api.dev.keynua.com',
  'path': '/contract-manager-document-templates/api/{templateId}',
  'headers': {
    'x-api-key': 'YOUR-API-KEY-HERE',
    'authorization': 'YOUR-API-TOKEN-HERE'
  },
};

var req = https.request(options, function (res) {
  var chunks = [];

  res.on("data", function (chunk) {
    chunks.push(chunk);
  });

  res.on("end", function (chunk) {
    var body = Buffer.concat(chunks);
    console.log(body.toString());
  });

  res.on("error", function (error) {
    console.error(error);
  });
});

req.end();
```

> Si la plantilla fue obtenida satisfactoramente, el API retorna un Json estructurado como aparece en la sección de [Plantilla de documento](#propiedades-de-una-plantilla-de-documento)

Permite obtener una plantilla de documento.

### HTTP Request

`GET /contract-manager-document-templates/api/{templateId}`

### Headers

Key | Value
--------- | -----------
x-api-key | your-api-key
authorization | your-api-token

## Generar documentos rellenados

```ruby
require 'uri'
require 'net/http'
require 'openssl'

url = URI("https://api.stg.keynua.com/contract-manager-document-templates/api/create-filled-files/{templateId}")

http = Net::HTTP.new(url.host, url.port)
http.use_ssl = true
http.verify_mode = OpenSSL::SSL::VERIFY_NONE

request = Net::HTTP::Post.new(url)
request["authorization"] =
request["x-api-key"] = 'YOUR-API-KEY-HERE'
request["authorization"] = 'YOUR-API-TOKEN-HERE'
request["Content-Type"] = "application/json"
request.body = "{\n  \"fieldValues\": [\n    {\n      \"name\": \"email-0\",\n      \"value\": \"user@mail.com\"\n    },\n    {\n      \"name\": \"date-0\",\n      \"value\": \"2020-10-09\"\n    }\n  ]\n}"

response = http.request(request)
puts response.read_body
```

```python
import http.client
import json

conn = http.client.HTTPSConnection("api.stg.keynua.com")

payload = json.dumps({
  "fieldValues": [
    {
      "name": "email-0",
      "value": "user@mail.com"
    },
    {
      "name": "date-0",
      "value": "2020-10-09"
    }
  ]
})

headers = {
  'x-api-key': "YOUR-API-KEY-HERE",
  'authorization': "YOUR-API-TOKEN-HERE",
  'Content-Type': 'application/json'
}

conn.request("POST", "/contract-manager-document-templates/api/create-filled-files/{templateId}", payload, headers)

res = conn.getresponse()
data = res.read()

print(data.decode("utf-8"))
```

```shell
curl --location --request POST 'https://api.stg.keynua.com/contract-manager-document-templates/api/create-filled-files/{templateId} \
  --header 'x-api-key: YOUR-API-KEY-HERE' \
  --header 'authorization: YOUR-API-TOKEN-HERE'
  --header 'Content-Type: application/json' \
  --data-raw '{
    "fieldValues": [
      {
        "name": "email-0",
        "value": "user@mail.com"
      },
      {
        "name": "date-0",
        "value": "2020-10-09"
      }
    ]
  }'
```

```javascript
const https = require("https");

var options = {
  method: 'POST',
  hostname: 'api.stg.keynua.com',
  path: '/contract-manager-document-templates/api/create-filled-files/{templateId',
  headers: {
    "x-api-key": "YOUR-API-KEY-HERE",
    "authorization": "YOUR-API-TOKEN-HERE"
    "Content-Type": "application/json"
  },
};

var req = https.request(options, function (res) {
  var chunks = [];

  res.on("data", function (chunk) {
    chunks.push(chunk);
  });

  res.on("end", function (chunk) {
    var body = Buffer.concat(chunks);
    console.log(body.toString());
  });

  res.on("error", function (error) {
    console.error(error);
  });
});

var postData = JSON.stringify({
  "fieldValues": [
    {
      "name": "email-0",
      "value": "user@mail.com"
    },
    {
      "name": "date-0",
      "value": "2020-10-09"
    }
  ]
});

req.write(postData);

req.end();
```

> Success response:

```json
{
  "files": [
    {
      "id": "b5d1b359-3736-424b-b83d-f0d0da910a89",
      "storageId": "eyJidWNrZXQiOiJjb250cmFjdC1tYW5hZ2VyLWRvY3Vt",
      "url": "https://s3.amazonaws.com/id-del-documento",
      "size": 8568,
      "sha256": "edd34e53e6b709fc4344f6799a8c228320e879fd7f92c95ed6ad8d7c4e0cd812"
    },
    {
      "id": "428c25aa-6d9e-405e-9c19-9fafc9f1a3ec",
      "storageId": "eyJidWNrZXQiOiJjb250cmFjdC1tYW5hZ2VyLWRvY3Vt",
      "url": "https://s3.amazonaws.com/id-del-documento",
      "size": 56628,
      "sha256": "b4b488d71896341ff6520881e56da7e5cfe41a603de5f16eef68114c2c9acda3"
    }
  ]
}
```

En base a cada documento de una plantilla, este API genera otros documentos pdf que incluyen el valor de cada campo enviado.

### HTTP Request

`POST /contract-manager-document-templates/api/create-filled-files/{templateId}`

### Headers

Key | Value
--------- | -----------
x-api-key | your-api-key
authorization | your-api-token
Content-Type | application/json

### Body

Atributo | Tipo | Opcional | Descripción
--------- | ----------- | ----------- | -----------
fieldValues | [FieldValues](#propiedades-de-fieldvalues)[] | No | Lista de items que contienen el id y valor de cada campo

### Propiedades de FieldValues

Atributo | Tipo | Descripción
--------- | ----------- | -----------
name | string | Id que indica a qué [DocumentTemplateField](#propiedades-de-documenttemplatefield) de la plantilla corresponde `value`
value | string | Valor del campo que es validado con las reglas de [DocumentTemplateField](#propiedades-de-documenttemplatefield)

### Response body item

Atributo | Tipo | Descripción
--------- | ----------- | -----------
id | string | Id del archivo dentro de la plantilla
storageId | string | Id de almacenamiento. Se puede utilizar para [crear contratos](#crear-un-contrato).
url | string | Url para descargar el documento generado
size | number | Tamaño en bytes del documento generado
sha256 | string | Hash sha256 del documento generado

# Webhooks

Los Webhooks permiten suscribirte a ciertos eventos del Contrato, por ejemplo cuando un contrato es creado, cuando ocurre un error en el proceso de firma o el contrato ha sido firmado satisfactoriamente. Cuando ocurre uno de estos eventos, enviaremos un request **HTTP POST** a la API configurada en el webhook.

## Configuración del Webhook

<aside class="warning">Para poder configurar tu Webhook debes contactar al equipo de Keynua</aside>

Una vez ya tengas una cuenta en el ambiente de pruebas de Keynua, puedes acceder a la sección [Developers](https://app.stg.keynua.com/developers) y configurar tu Webhook.

Si tu API necesita unos headers específicos, puedes agregarlo en la configuración del Webhook y nosotros lo enviaremos en cada request.

Entre los eventos que puedes escoger están:

Evento | Descripción
--------- | -----------
Created | Serás notificado cuando un contrato fue creado satisfactoriamente
Started | Serás notificado cuando un contrato fue creado satisfactoriamente y está listo para ser firmado. **Recomendamos usar este evento en lugar del evento Created ya que este evento te notificará cuando el contrato está listo para comenzar el proceso de firma**
Finished | Serás notificado cuando el contrato ha sido firmado por todos y finalizó correctamente
ItemWorking | Serás notificado cada vez que se comienza a procesar un [Item](#propiedades-de-un-item)
ItemSuccess | Serás notificado cada vez que un [Item](#propiedades-de-un-item) ha concluído satisactoriamente
ItemError | Serás notificado cada vez que ocurre un error en un [Item](#propiedades-de-un-item)

Una vez configurado el Webhook, te llegará un email de confirmación al correo que ingresaste. Luego, debes seleccionar la opción **Habilitar** para poder activar el Webhook. Keynua enviará un HTTP POST al API configurado y será activado si cumple estos requisitos:

* El API debe ser de acceso público
* El API debe ser HTTPS
* El API debe retornar status code 200 y el body del response debe ser `KEYNUA`

Una vez tengas habilitado tu Webhook, puedes comenzar a escuchar las notificaciones de tus contratos siguiendo la siguiente [Especificación del Webhook](#especificación-del-webhook).

<aside class="notice">Si ya estás listo para integrar en producción, por favor contacta al equipo de <code>Keynua</code></aside>

## Especificación del Webhook

El request tendrá la siguiente estructura

### Headers

```json
{
	"headers": {
		"Content-Type": "application/json",
		"X-Keynua-Webhook-Type": "ContractFinished",
		"X-Keynua-Webhook-TS": "1604957455847",
		"X-Keynua-Webhook-Sig": "ec7584cb..."
	}
}
```

Key                 | Value
------------------ | -----------
Content-Type | application/json
x-keynua-webhook-sig | Firma del request para el control de la manipulacion de la peticion. Para más información revisa la sección de [Verificar Keynua Signature Webhook](#verificar-keynua-signature-webhook)
x-keynua-webhook-ts | Numero de millisegundos desde UNIX Epoch (01/01/1970) cuando se envio el evento
x-keynua-webhook-type | Nombre del evento emitido

### Body

```json
{
	"type": "ContractFinished",
	"accountId": "d6c5ce27-fa63-4fb8-8f0f-83278c6b7985",
	"payload": { ... }
}
```

Atributo | Tipo | Descripción
--------- | ----------- | -----------
type | string | Nombre del [tipo de evento](#tipos-de-eventos-webhook) emitido. Puede ser `ContractCreated`, `ContractStarted`, `ContractItemUpdated`, `ContractFinished`
accountId | string | Codigo de la cuenta propietaria del webhook
payload | object | Datos del evento que varian segun el valor de [type](#tipos-de-eventos-webhook)

## Tipos de Eventos Webhook
Keynua enviará un request a tu API configurado por cada Evento registrado. En esta tabla podrás ver la relación que existe entre cada evento registrado y los tipos de eventos disponibles

Evento del Webhook | Tipo de Evento
--------| ----------
Created | [ContractCreated](#propiedades-de-contractcreated)
Started | [ContractStarted](#propiedades-de-contractstarted)
Finished | [ContractFinished](#propiedades-de-contractfinished)
ItemWorking | [ContractItemUpdated](#propiedades-de-contractitemupdated). Item.state `working`
ItemSuccess | [ContractItemUpdated](#propiedades-de-contractitemupdated). Item.state `success`
ItemError | [ContractItemUpdated](#propiedades-de-contractitemupdated). Item.state `error`

### Propiedades de ContractCreated

> Ejemplo de Body

```json
{
	"type": "ContractCreated",
	"accountId": "00000000-0000-0000-0000-000000000000",
	"payload": {
		"contractId": "00000000-0000-0000-0000-000000000001",
		"reference": "",
		"title": "Mi primer webhook",
		"description": "Ejemplo de mi primer webhook",
		"language": "es",
		"createdAt": "2020-01-01T00:00:00.000Z",
		"docCount": 0,
		"itemCount": 0,
		"userCount": 1,
		"metadata": {}
	}
}
```

Se emite cuando el contrato ha sido creado.

Atributo | Tipo | Descripción
--------- | ----------- | -----------
contractId | string | Codigo del contrato
reference | string | Referencia del contrato
title | string | Titulo del contrato
templateId | string | Identificador del Template
description | string | Descripcion del contrato
language | string | Idima del contrato
createdAt | string | Fecha de creacion del contrato
docCount | integer | Cantidad de documentos que tiene el contrato
itemCount | integer | Cantidad de items que tiene el contrato
userCount | integer | Cantidad de usuarios que tiene el contrato
metadata | integer | Metada del contrato. Esta Metadata será la misma que se envió al [crear un Contrato](#crear-un-contrato)

### Propiedades de ContractStarted

> Ejemplo de Body

```json
{
	"type": "ContractStarted",
	"accountId": "00000000-0000-0000-0000-000000000000",
	"payload": {
		"contractId": "00000000-0000-0000-0000-000000000001",
		"reference": null,
		"title": "Mi primer webhook",
		"language": "es",
		"createdAt": "2020-01-01T00:00:00.000Z",
		"startedAt": "2020-01-01T01:00:00.000Z",
		"docCount": 1,
		"itemCount": 14,
		"userCount": 1,
		"longCode": "1a2b3c4d5e6f7a8b9c0d1e2f3a4b5c6d7e8f9a0b1c2d3e4f5a6b7c8d9e0f1a2b",
		"shortCode": "123456",
		"metadata": {},
		"users": [
			{
				"id": 0,
				"name": "John Doe",
				"email": "example@keynua.com",
				"phone": null,
				"ref": null,
				"groups": [
					"signers"
				]
			}
		],
		"documents": [
			{
				"name": "sample.pdf",
				"type": "application/pdf",
				"size": 3028,
				"url": "https..."
			}
		]
	}
}
```

Se emite cuando el contrato está listo para ser firmado.

Atributo | Tipo | Descripción
--------- | ----------- | -----------
contractId | string | Codigo del contrato
reference | string | Referencia del contrato
title | string | Titulo del contrato
templateId | string | Identificador del Template
description | string | Descripcion del contrato
language | string | Idima del contrato
createdAt | string | Fecha de creacion del contrato
startedAt | string | Fecha de inicio del proceso de firma del contrato
docCount | integer | Cantidad de documentos que tiene el contrato
itemCount | integer | Cantidad de items que tiene el contrato
userCount | integer | Cantidad de usuarios que tiene el contrato
metadata | integer | Metada del contrato. Esta Metadata será la misma que se envió al [crear un Contrato](#crear-un-contrato)
longCode | string | Código largo del contrato
shortCode | string | Código corto del contrato para facilitar su identificación
users | array | Arreglo de [Usuarios](#propiedades-de-un-usuario-webhook) del Contrato
documents | array | Arreglo de [Documentos](#propiedades-de-un-documento-webhook) del contrato

### Propiedades de ContractFinished

> Ejemplo de Body

```json
{
	"type": "ContractFinished",
	"accountId": "00000000-0000-0000-0000-000000000000",
	"payload": {
		"contractId": "00000000-0000-0000-0000-000000000001",
		"reference": null,
		"title": "Testing WH CC",
		"language": "es",
		"createdAt": "2020-01-01T00:00:00.000Z",
		"startedAt": "2020-01-01T01:00:00.000Z",
		"finishedAt": "2020-01-01T02:00:00.000Z",
		"docCount": 1,
		"itemCount": 14,
		"userCount": 1,
		"longCode": "1a2b3c4d5e6f7a8b9c0d1e2f3a4b5c6d7e8f9a0b1c2d3e4f5a6b7c8d9e0f1a2b",
		"shortCode": "123456",
		"metadata": {},
		"users": [
			{
				"id": 0,
				"name": "John Doe",
				"email": "example@keynua.com",
				"phone": null,
				"ref": null,
				"groups": [
					"signers"
				]
			}
		],
		"documents": [
			{
				"name": "sample.pdf",
				"type": "application/pdf",
				"size": 3028,
				"url": "https..."
			}
		]
	}
}
```

Se emite cuando el contrato ha sido firmado por todos y ha finalizado correctamente.

Atributo | Tipo | Descripción
--------- | ----------- | -----------
contractId | string | Codigo del contrato
reference | string | Referencia del contrato
title | string | Titulo del contrato
templateId | string | Identificador del Template
description | string | Descripcion del contrato
language | string | Idima del contrato
createdAt | string | Fecha de creacion del contrato
startedAt | string | Fecha de inicio del proceso de firma del contrato
finishedAt | string | Fecha de finalización del proceso de firma del contrato
docCount | integer | Cantidad de documentos que tiene el contrato
itemCount | integer | Cantidad de items que tiene el contrato
userCount | integer | Cantidad de usuarios que tiene el contrato
metadata | integer | Metada del contrato. Esta Metadata será la misma que se envió al [crear un Contrato](#crear-un-contrato)
longCode | string | Código largo del contrato
shortCode | string | Código corto del contrato para facilitar su identificación
users | array | Arreglo de [Usuarios](#propiedades-de-un-usuario-webhook) del Contrato
documents | array | Arreglo de [Documentos](#propiedades-de-un-documento-webhook) del contrato. Acá encontrarás los **documentos originales del contrato**, NO el documento final de firma. Si lo que quieres es el documento final de firma, podrás obtenerlo desde el [Item](#propiedades-de-un-item) **PDF** en [ContractItemUpdated](#propiedades-de-contractitemupdated)

### Propiedades de ContractItemUpdated

> Ejemplo de Body

```json
{
	"type": "ContractItemUpdated",
	"accountId": "00000000-0000-0000-0000-000000000000",
	"payload": {
		"contractId": "00000000-0000-0000-0000-000000000001",
		"reference": null,
		"title": "Testing WH CC",
		"language": "es",
		"createdAt": "2020-01-01T00:00:00.000Z",
		"startedAt": "2020-01-01T01:00:00.000Z",
		"docCount": 1,
		"itemCount": 14,
		"userCount": 1,
		"longCode": "1a2b3c4d5e6f7a8b9c0d1e2f3a4b5c6d7e8f9a0b1c2d3e4f5a6b7c8d9e0f1a2b",
		"shortCode": "123456",
		"metadata": {},
		"item": {
			"id": 0,
			"refId": "signersemail",
			"state": "working",
			"version": 1,
			"stageIndex": 0,
			"type": "email",
			"title": "Mail de inicio de firma",
			"value": null
		},
		"user": {
			"id": 0,
			"name": "John Doe",
			"email": "example@keynua.com",
			"phone": null,
			"ref": null,
			"groups": [
				"signers"
			]
		}
	}
}
```

Se emite cuando un elemento del contrato ha cambiado de estado. Los estados pueden ser `working`, `success`, `error`

Atributo | Tipo | Descripción
--------- | ----------- | -----------
contractId | string | Codigo del contrato
reference | string | Referencia del contrato
title | string | Titulo del contrato
templateId | string | Identificador del Template
description | string | Descripcion del contrato
language | string | Idima del contrato
createdAt | string | Fecha de creacion del contrato
startedAt | string | Fecha de inicio del proceso de firma del contrato
docCount | integer | Cantidad de documentos que tiene el contrato
itemCount | integer | Cantidad de items que tiene el contrato
userCount | integer | Cantidad de usuarios que tiene el contrato
metadata | integer | Metada del contrato. Esta Metadata será la misma que se envió al [crear un Contrato](#crear-un-contrato)
longCode | string | Código largo del contrato
shortCode | string | Código corto del contrato para facilitar su identificación
item | object | [Item](#propiedades-de-un-item) del contrato que ha cambiado de estado
user | object | [Usuario](#propiedades-de-un-usuario-webhook) al que pertence el elemento modificado. Si el valor es null el elemento le pertenece a todos los usuarios

### Propiedades de un Usuario Webhook

```json
{
	"id": 0,
	"name": "John Doe",
	"email": "example@keynua.com",
	"phone": null,
	"ref": null,
	"groups": [
		"signers"
	]
}
```

Atributo | Tipo | Descripción
--------- | ----------- | -----------
id | integer | Identificador único del usuario en el contrato
name | string | El nombre del usuario
email | string | El correo electrónico del usuario
phone | string | El teléfono del usuario
groups | array | Nombre de los grupos a los que pertenece el usuario

### Propiedades de un Documento Webhook

```json
{
	"name": "sample.pdf",
	"type": "application/pdf",
	"size": 3028,
	"url": "https..."
}
```

Atributo | Tipo | Descripción
--------- | ----------- | -----------
name | string | Nombre del documento
type | string | Tipo del documento en formato MIME
size | integer | Peso del documento en bytes
url | integer | Link del documento. **Expira en 12 horas**

## Verificar Keynua Signature Webhook

```
SHA_256(requestPayloadAsString + ts + webHookSecret)
```

Si quieres verificar la integridad del payload y asegurarte de que Keynua es quién está golpeando tu API Webhook, debes verificar el Signature que envía Keynua en el header `X-Keynua-Webhook-Sig`

Para verificar este valor necesitas:

* El object payload del body `body.payload` como string
* El valor del header `X-Keynua-Webhook-TS`
* El valor del `secret` que obtuviste al crear el webhook

Luego de ello debes concatenar todos estos valores y aplicarle SHA256, el resultado debe ser el mismo que el valor del header `X-Keynua-Webhook-Sig`

<aside class="notice">Si el resultado de tu hash coincide con el que te enviamos, puedes tener la seguridad de que es Keynua quién envió la información a tu API y que la información no ha sido alterada en tránsito.</aside>
