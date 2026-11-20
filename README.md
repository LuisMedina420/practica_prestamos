# Práctica: Préstamo de equipo en Odoo 20.0 (UNICAH)

- **Nombre:** Luis Medina
- **Número de cuenta:** 0806200300590

## Respuestas

### Paso 1. Catálogo de equipos
1. **¿Qué pasa con un equipo cuando se desmarca el campo Activo? ¿Por qué es mejor que borrarlo?**
   - En Odoo, `active` es un campo reservado del ORM: cuando se desmarca (`active = False`), el registro queda **archivado** y deja de aparecer en las listas, búsquedas y selectores por defecto, pero sigue existiendo en la base de datos (y puede consultarse filtrando por *Archivados*).
   - Es mucho mejor que borrarlo físicamente porque si ese equipo ya formó parte de préstamos históricos (`prestamo.linea`), eliminarlo rompería la integridad referencial o borraría el historial de qué equipo se prestó en el pasado; archivarlo conserva todo el historial intacto e impide que se siga prestando.
2. **¿Para qué sirve `copy=False` en el campo `codigo`?**
   - Indica a Odoo que cuando el usuario utilice la acción **Duplicate (Duplicar)** sobre un equipo existente, el valor de `codigo` (Código de inventario) **no se copie** al nuevo registro. Como cada equipo físico tiene un número de inventario único (por ejemplo `LAP-001`), evita que se creen dos equipos con el mismo código por accidente y obliga a escribir el código nuevo.

### Paso 2. Registro de préstamos
1. **Este modelo usa `_name` y `_inherit` al mismo tiempo. ¿Qué hace cada uno aquí?**
   - `_name = "prestamo.prestamo"` crea un **modelo nuevo** en Odoo con su propia tabla en PostgreSQL (`prestamo_prestamo`).
   - Al combinarlo con `_inherit = ["mail.thread"]` (herencia por mezcla o *mixin*), el modelo nuevo `prestamo.prestamo` incorpora todos los campos y métodos del modelo abstracto `mail.thread` (como `message_ids`, `message_follower_ids` y `message_post()`), lo que permite usar la etiqueta `<chatter/>` en el formulario para registrar mensajes, notas y cambios de estado sin tener que programarlos desde cero.
2. **¿Qué hace `tracking=True` en el campo `state`?**
   - Como el modelo hereda de `mail.thread`, el atributo `tracking=True` hace que Odoo **registre automáticamente en el chatter cada vez que cambia el valor del campo `state`** (por ejemplo: *Estado: Borrador → Prestado*), indicando qué usuario realizó el cambio y en qué fecha y hora ocurrió.
