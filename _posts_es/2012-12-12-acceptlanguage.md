---
layout: recipe
title: Unsatisfied Accept-Language header
category: HTTP Headers
author: joshua thijssen
author_email: jthijssen@noxlogic.nl
---
## Si un encabezado HTTP no puede ser satisfecho, retornamos un [406], pero si un encabezado Accept-Language no puede ser satisfecho, ¿Cual es la respuesta que debo retornar?

### Respuesta
Deberías retornar un [406]. RFC_2626 10.4.7 dice "aceptar encabezados", el plural. Estu sugiera que cualquier encabezado de tipo Accept-* que no pueda ser satisfecho, debería retornar una respuesta [406].

Ver también :
- [RFR_7231 -406 No aceptable](http://tools.ietf.org/html/rfc7231#section-6.5.6)
