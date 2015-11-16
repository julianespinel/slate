# Errores

El API de Facturala maneja los siguientes errores:

Status Code | Significado
---------- | -------
400 | Bad Request -- Los parámetros de la URL o el body del request no son válidos.
401 | Unauthorized -- El token de autorización no es válido.
403 | Forbidden -- El usuario no tiene permisos para ejecutar ese llamado. 
404 | Not Found -- No se encuentra el recurso.
405 | Method Not Allowed -- La URL del llamado no soporta el método HTTP. 
500 | Internal Server Error -- Ha ocurrido un proglema en los servidores de Factúrala, por favor contacte al equipo de soporte.
