# Errors

El API de Keynua usa los siguientes códigos de errores.

Error Code | Meaning
---------- | -------
400 | Bad Request -- Tu request no es válido.
401 | Unauthorized -- Las credenciales no son válidas o pueden haber expirado.
403 | Forbidden -- Acceso denegado, es probable que no estés enviando las credenciales o sean incorrectas.
404 | Not Found -- El Item que estás buscando no existe..
429 | Too Many Requests -- Tus request son muy consecutivos, baja la velocidad de los request!
500 | Internal Server Error -- Tenemos problemas en nuestro servidores, intenta más tarde por favor.
503 | Service Unavailable -- Estamos temporalmente fuera de servicio, intenta más tarde por favor.
