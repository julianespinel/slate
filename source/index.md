---
title: Facturala API Reference

language_tabs:
  - http

toc_footers:
  - <a href='http://github.com/tripit/slate'>Documentation Powered by Slate</a>

includes:
  - errores
  - soporte

search: false
---

# Facturala API

Facturala es un sistema que permite generar facturas por computador on demand: [facturala.co](http://facturala.co)

Para utilizar el API de Facturala es necesario tener un token de autorización. Para obtener uno contactanos en `ìnfo@facturala.co`

Todos los llamados al API deben enviar este token de autorización como valor del header `Authorization` (ver ejemplos en la documentación de cada uno de los servicios).

El API de Facturala tiene 4 servicios:

1. Crear factura
2. Actualizar el estado de una factura
3. Consultar una factura
4. Consultar múltiples facturas

# Factura

## Crear factura

```http
POST /facturala/api/companies/56491cd4e4b0c19ae3b74ed3/invoices HTTP/1.1
Accept: application/json
Content-Type: application/json;charset=UTF-8
Authorization: 66e58291f7cbcd5157f3

{
    "creationDate": "2015-11-16T00:03:53.846Z",
    "dueDate": "2015-12-31T05:00:00.000Z",
    "state": "PENDING",
    "customer": {
        "idType": "CC",
        "country": "Colombia",
        "name": "Carlos Rodriguez",
        "idNumber": "304204672683",
        "address": "Cl 34 # 45 - 10",
        "city": "Medellín",
        "state": "Antioquia",
        "email": "crodriguez@gmail.com"
    },
    "items": [
        {
            "name": "Computador",
            "description": "Asus K45V",
            "unitValue": 1600000,
            "quantity": 1
        }
    ],
    "thanks": "Gracias",
    "note": "Se aplicará una multa equivalente al 1.5% despues de 30 días de mora.",
    "footer": "Factura creada en computador y es válida sin firma y sello."
}
```

> El llamado al API fue exitoso si retorna la siguiente respuesta:

```http
HTTP/1.1 201 Created
Content-Type: application/json

{
    "creationDate": "2015-11-16T00:03:53.846Z",
    "dueDate": "2015-12-31T05:00:00.000Z",
    "state": "PENDING",
    "invoicePDFUrl": "",
    "invoiceTemplate": {
        "companyId": "56491cd4e4b0c19ae3b74ed3",
        "header": null,
        "companyData": {
            "name": "Facturala",
            "address": "Cll 123 # 12 - 23",
            "phone": "1234567",
            "email": "info@facturala.co",
            "logoUrl": "1da7993d-931a-414b-b494-82c658965d8c.png"
        },
        "thanks": "Gracias",
        "note": "Se aplicará una multa equivalente al 1.5% despues de 30 días de mora.",
        "footer": "Factura creada en computador y es válida sin firma y sello.",
        "templateName": "template.ftl"
    },
    "number": "1",
    "customer": {
        "name": "Carlos Rodriguez",
        "idType": "CC",
        "idNumber": "304204672683",
        "address": "Cl 34 # 45 - 10",
        "city": "Medellín",
        "state": "Antioquia",
        "country": "Colombia",
        "email": "crodriguez@gmail.com"
    },
    "items": [
        {
            "name": "Computador",
            "description": "Asus K45V",
            "quantity": 1,
            "unitValue": 1600000,
            "discount": null,
            "total": 1600000
        }
    ],
    "totals": {
        "subTotalNoDiscounts": 1379310.34,
        "discounts": 0,
        "total": 1600000,
        "taxes": 220689.65
    }
}
```

Servicio para generar una factura.

### HTTP Request

`POST /facturala/api/companies/:companyId/invoices`<br>
`Accept: application/json`<br>
`Content-Type: application/json`<br>
`Authorization: :authToken`<br>

Se debe enviar una factura en el body del llamado (ver ejemplo a la derecha).

### HTTP Response

Las posibles respuestas que puede generar este servicio son:

1. `Status Code: 201`<br>
Indica que la factura ha sido creada correctamente.<br>
Adicionalmente retorna un mensaje JSON (ver ejemplo a la derecha).<br><br>
1. `Status code: 400`<br>
Indica que el companyId o la factura enviada en el body del request no son válidos.<br><br>
1. `Status code: 401`<br>
Indica que el token de autorización enviado en el header 'Authorization' no es un token válido.<br><br>
1. `Status code: 403`<br>
Indica que el usuario no tiene permisos para ejecutar el request.<br><br>
1. `Status code: 500`<br>
Indica que hubo un error en los servidores de Factúrala, por favor contacte al equipo de soporte.<br>

## Actualizar estado de una factura

```http
PUT /facturala/api/companies/56491cd4e4b0c19ae3b74ed3/invoices/1/state HTTP/1.1
Accept: application/json
Content-Type: application/json
Authorization: 66e58291f7cbcd5157f3

{
    "state": "PAID"
}
```

> El llamado al API fue exitoso si retorna la siguiente respuesta:

```http
HTTP/1.1 200 OK
Content-Type: application/json
```

Servicio para actualizar el estado de una factura.<br>
Una factura tiene 3 posibles estados: PAID, PENDING, CANCELED.

### HTTP Request

`PUT /facturala/api/companies/:companyId/invoices/:invoiceNumber/state`<br>
`Accept: application/json`<br>
`Content-Type: application/json`<br>
`Authorization: :authToken`<br>

Se debe enviar un mensaje JSON en el body del llamado (ver ejemplo a la derecha).

### HTTP Response

Las posibles respuestas que puede generar este servicio son:

1. `Status Code: 200`<br>
Indica que el estado de la factura ha sido actualizado correctamente.<br><br>
1. `Status code: 404`<br>
Indica que el invoiceNumber enviado en la URL no pertenece a ninguna factura creada.<br><br>
1. `Status code: 400`<br>
Indica que el companyId, el invoiceNumber o el JSON enviado en el body del request no son válidos.<br><br>
1. `Status code: 401`<br>
Indica que el token de autorización enviado en el header 'Authorization' no es un token válido.<br><br>
1. `Status code: 403`<br>
Indica que el usuario no tiene permisos para ejecutar el request.<br><br>
1. `Status code: 500`<br>
Indica que hubo un error en los servidores de Factúrala, por favor contacte al equipo de soporte.<br>

## Consultar una factura

```http
GET /facturala/api/companies/56491cd4e4b0c19ae3b74ed3/invoices/1 HTTP/1.1
Accept: application/json
Content-Type: application/json
Authorization: 66e58291f7cbcd5157f3
```
> El llamado al API fue exitoso si retorna la siguiente respuesta:

```http
HTTP/1.1 200 OK
Content-Type: application/json

{
    "creationDate": "2015-11-16T00:03:53.846Z",
    "dueDate": "2015-12-31T05:00:00.000Z",
    "state": "PAID",
    "invoicePDFUrl": "",
    "invoiceTemplate": {
        "companyId": "56491cd4e4b0c19ae3b74ed3",
        "header": null,
        "companyData": {
            "name": "Facturala",
            "address": "Cll 123 # 12 - 23",
            "phone": "1234567",
            "email": "info@facturala.co",
            "logoUrl": "1da7993d-931a-414b-b494-82c658965d8c.png"
        },
        "thanks": "Gracias",
        "note": "Se aplicará una multa equivalente al 1.5% despues de 30 días de mora.",
        "footer": "Factura creada en computador y es válida sin firma y sello.",
        "templateName": "template.ftl"
    },
    "number": "1",
    "customer": {
        "name": "Carlos Rodriguez",
        "idType": "CC",
        "idNumber": "304204672683",
        "address": "Cl 34 # 45 - 10",
        "city": "Medellín",
        "state": "Antioquia",
        "country": "Colombia",
        "email": "crodriguez@gmail.com"
    },
    "items": [
        {
            "name": "Computador",
            "description": "Asus K45V",
            "quantity": 1,
            "unitValue": 1600000,
            "discount": null,
            "total": 1600000
        }
    ],
    "totals": {
        "subTotalNoDiscounts": 1379310.34,
        "discounts": 0,
        "total": 1600000,
        "taxes": 220689.65
    }
}
```

Servicio para consultar una factura específica de una compañía.

### HTTP Request

`GET /facturala/api/companies/:companyId/invoices/:invoiceNumber`<br>
`Accept: application/json`<br>
`Content-Type: application/json`<br>
`Authorization: :authToken`<br>

No se debe enviar nada en el body del llamado.

### HTTP Response 

1. `Status Code: 200`<br>
Indica que se ha retornado el resultado esperado.<br><br>
1. `Status code: 400`<br>
Indica que el companyId enviado en la URL no es válido.<br><br>
1. `Status code: 401`<br>
Indica que el token de autorización enviado en el header 'Authorization' no es un token válido.<br><br>
1. `Status code: 403`<br>
Indica que el usuario no tiene permisos para ejecutar el request.<br><br>
1. `Status code: 500`<br>
Indica que hubo un error en los servidores de Factúrala, por favor contacte al equipo de soporte.<br>

## Consultar múltiples facturas

```http
GET /facturala/api/companies/56491cd4e4b0c19ae3b74ed3/invoices HTTP/1.1
Accept: application/json
Content-Type: application/json
Authorization: 66e58291f7cbcd5157f3
```
> El llamado al API fue exitoso si retorna la siguiente respuesta:

```http
HTTP/1.1 200 OK
Content-Type: application/json

[
    {
        "creationDate": "2015-11-16T00:03:53.846Z",
        "dueDate": "2015-12-31T05:00:00.000Z",
        "state": "PAID",
        "invoicePDFUrl": "",
        "invoiceTemplate": {
            "companyId": "56491cd4e4b0c19ae3b74ed3",
            "header": null,
            "companyData": {
                "name": "Facturala",
                "address": "Cll 123 # 12 - 23",
                "phone": "1234567",
                "email": "info@facturala.co",
                "logoUrl": "1da7993d-931a-414b-b494-82c658965d8c.png"
            },
            "thanks": "Gracias",
            "note": "Se aplicará una multa equivalente al 1.5% despues de 30 días de mora.",
            "footer": "Factura creada en computador y es válida sin firma y sello.",
            "templateName": "template.ftl"
        },
        "number": "1",
        "customer": {
            "name": "Carlos Rodriguez",
            "idType": "CC",
            "idNumber": "304204672683",
            "address": "Cl 34 # 45 - 10",
            "city": "Medellín",
            "state": "Antioquia",
            "country": "Colombia",
            "email": "crodriguez@gmail.com"
        },
        "items": [
            {
                "name": "Computador",
                "description": "Asus K45V",
                "quantity": 1,
                "unitValue": 1600000,
                "discount": null,
                "total": 1600000
            }
        ],
        "totals": {
            "subTotalNoDiscounts": 1379310.34,
            "discounts": 0,
            "total": 1600000,
            "taxes": 220689.65
        }
    },
    {
        "creationDate": "2015-11-16T00:57:19.275Z",
        "dueDate": "2015-11-21T05:00:00.000Z",
        "state": "PAID",
        "invoicePDFUrl": "",
        "invoiceTemplate": {
            "companyId": "56491cd4e4b0c19ae3b74ed3",
            "header": null,
            "companyData": {
                "name": "Facturala",
                "address": "Cll 123 # 12 - 23",
                "phone": "1234567",
                "email": "info@facturala.co",
                "logoUrl": "1da7993d-931a-414b-b494-82c658965d8c.png"
            },
            "thanks": "Gracias",
            "note": "Se aplicará una multa equivalente al 1.5% despues de 30 días de mora.",
            "footer": "Factura creada en computador y es válida sin firma y sello.",
            "templateName": "template.ftl"
        },
        "number": "2",
        "customer": {
            "name": "Felipe Restrepo",
            "idType": "CC",
            "idNumber": "40750374503",
            "address": "Cl 145 # 13 - 25",
            "city": "Bogota",
            "state": "Cundinamarca",
            "country": "Colombia",
            "email": "frestrepo@gmail.com"
        },
        "items": [
            {
                "name": "Mouse",
                "description": "Wireless mobile mouse 1000",
                "quantity": 1,
                "unitValue": 50000,
                "discount": null,
                "total": 50000
            }
        ],
        "totals": {
            "subTotalNoDiscounts": 43103.45,
            "discounts": 0,
            "total": 50000,
            "taxes": 6896.55
        }
    }
]
```

Servicio para consultar las facturas de una compañía.

### HTTP Request

`GET /facturala/api/companies/:companyId/invoices`<br>
`Accept: application/json`<br>
`Content-Type: application/json`<br>
`Authorization: :authToken`<br>

No se debe enviar nada en el body del llamado.

### HTTP Response

Las posibles respuestas que puede generar este servicio son:

1. `Status Code: 200`<br>
Indica que se ha retornado el resultado esperado.<br><br>
1. `Status code: 400`<br>
Indica que el companyId enviado en la URL no es válido.<br><br>
1. `Status code: 401`<br>
Indica que el token de autorización enviado en el header 'Authorization' no es un token válido.<br><br>
1. `Status code: 403`<br>
Indica que el usuario no tiene permisos para ejecutar el request.<br><br>
1. `Status code: 500`<br>
Indica que hubo un error en los servidores de Factúrala, por favor contacte al equipo de soporte.<br>
