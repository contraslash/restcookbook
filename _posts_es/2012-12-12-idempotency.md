---
layout: recipe
title: What are idempotent and/or safe methods?
category: HTTP Methods
author: joshua thijssen
author_email: jthijssen@noxlogic.nl
---

## ¿Qué son los métodos idempotentes o seguros?

Los metodos seguros en HTTP son los métodos que no modifican los recursos, por ejemplo, usar un [GET] o un [HEAD] en la URL de un recurso NUNCA cambia su contenido. Sin embargo, esto no es completamente cierto. Significa: No debería cambiar la representacion del recurso. Todavía es posible que los métodos seguros cambie cosas en el servidor o en el recurso, pero estos cambios no se reflejarán en la representación del servidor.

Esto significa que lo siguiente sería incorrecto, si lo que queremos e eliminar un post:

```
GET /blog/1234/delete HTTP/1.1
```

Los métodos seguros pueden ser almacenados en cache, preobtenidos  y esto no tendrá ninguna repercusión en los recursos.

### Métodos idempotentes:
Un método idempotente es un método HTTP que puede se llamado diferentes veces sin que su salida sea modificada. No importa si el método es llamado una vez o diez veces. El resultado será el mismo. De nuevo, esto solo aplica para el resultado, no el recurso por si mismo. De nuevo, esto solo aplica al resultado, no al recurso en si mismo. Este puede ser manipulado (como actualizar un tiempo, proveido por la información que no es compartida en la actual representación del recurso).

Considera el siguiente ejemplo:

```
  a = 4;
```
```
  a++;
```
El primer ejemplo es idempotente: no importa cuantas veces ejecutes este código, a siempre será 4. El segundo ejemplo no es idempotente. Ejecutar este código dará un resultado distinto cada vez, así como cuando se ejecute 5 veces. Como ambos métodos están cambiando el valor de a, ninguno de estos es un método seguro.

La idempotencia es importante para contruir APIs tolerantes a fallas. Sopón que un cliente actualiza un recurso a través de [POST]. Como [POST] no es un método idempotente, llamarlo múltiples veces puede resultar en actualizaciones equivocadas. Que pasaría si envías un [POST] al servidor, pero obtienes un error por límite de tiempo. ¿El recurso fue actualizado? ¿El error por límite de tiempo ocurrió enviando la petición al servidor o en la respuesta al cliente? ¿Podemos reintentar seguramente de nuevo o neesitamos averiguar que sucedión con el recurso? Al usar métodos idempotentes, no tenemos que preguntarnos este tipo de cosas, y podemos reenviar la petición tranquilamente y recibiremos la respuesta de parte del servidor sin alterar nada.

Se cuidadoso, cuando se trata de métodos seguros también: Puede ser posible que un método seguro como [GET] cambie un recurso, puede ser posible que algún cliente middleware  o un proxy entre tu y el servidor cambie la respuesta. Otros clientes pueden cambiar este recurso a través de la misma url como http://example.org/api/article/1234/delete, puede no llamar al servidor, pero  retornar la información directamente desde el cache. Los métodos No seguros (y no idempotentes)nunca deben ser almacenados en cache por ningín middleware o proxy.

### Resumen de (algunos) métodos HTTP
Método HTTP|Idempotente|Seguro
-|-
OPTIONS|SI|SI
GET|SI|SI
HEAD|SI|SI
PUT|SI|NO
POST|NO|NO
DELETE|SI|NO
PATCH|NO|NO

### Ver también

- [RFC 7231 - Especificación HTTP-1.1](http://tools.ietf.org/html/rfc7231#section-4.2)
- [¿Cuándo usar PUT o POST](/HTTP%20Methods/put-vs-post/)
