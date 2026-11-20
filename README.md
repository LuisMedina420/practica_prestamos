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
