---
layout: recipe
title: When to use PUT or POST
category: HTTP Methods
author: joshua thijssen
author_email: jthijssen@noxlogic.nl
---

## ¿Cuándo deberíamos usar usamos PUT y cuando deberíamos usar POST?

Los métodos HTTP [POST] y [PUT] no son los equivalentes a la creación y actualización del CRUD. Ambos sirven para un propósito distinto. Es posible, válido e incluso preferido en algunas ocaciones usar [PUT] para crear recursos y [POST] para actualizar los recursos.

Usamos [PUT] cuando puedes actualizar un recurso completamente a través de un recurso específico. Por ejemplo si sabes que un artículo reside en http://example.org/article/123, puedes colocar una nueva representación de este artículo directamente a través de un [PUT] a esta URL.

Si no sabes cual es la ubicación de un recurso, por ejemplo cuando vas a agregar un nuevo artículo, pero no tienes idea de donde almacenarlo, puedes usar [POST] a una URL y el servidor decidirá la URL.

```
PUT /article/1234 HTTP/1.1
<article>
    <title>red stapler</title>
    <price currency="eur">12.50</price>
</article>
```

```
POST /articles HTTP/1.1
<article>
    <title>blue stapler</title>
    <price currency="eur">7.50</price>
</article>

HTTP/1.1 201 Created
Location: /articles/63636
```

En tanto sabes la ubicación del recurso, puedes usar [POST] para crear el artículo `blue stapler`. Pero como dije antes: tu PUEDES agregar recursos através de [PUT] también. El siguiente ejemplo muestra perfectamente esto en una API:

```
PUT /articles/green-stapler HTTP/1.1
<article>
    <title>green stapler</title>
    <price currency="eur">9.95</price>
</article>

HTTP/1.1 201 Created
Location: /articles/green-stapler
```

Aquí el cliente decide la URL actual del recurso.

### Advertencias
- [PUT] y [POST] son métodos inseguros, sim embargo [PUT] es idempotente, mientras que [POST] no lo es

### Ver también
- [Idempotencia y Métodos seguros](/HTTP%20Methods/idempotency)
