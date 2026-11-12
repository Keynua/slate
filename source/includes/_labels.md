# Etiquetas

Las etiquetas clasifican un contrato o una verificación de identidad. El catálogo se administra en el portal de Keynua. El API **no crea, lista, edita ni archiva etiquetas**: solo recibe sus ids y devuelve el snapshot que quedó guardado.

No las confundas con los items `detectlabels` y `checklabels`. Esos validan el documento de identidad. Una etiqueta es un dato de clasificación que eliges tú.

## Cómo obtener el id

No existe un listado público de etiquetas con API Key.

1. Entra a [Organización → Etiquetas](https://app.stg.keynua.com/profile/organization/labels).
2. En la fila de la etiqueta, usa **Copiar id**.

Ese valor es el que envías dentro de `labelIds`. Los ids pertenecen al catálogo de la organización de la API key. Si creas el contrato o la identificación con `onBehalfOf`, el catálogo sigue siendo el de la organización: no cambia al de la cuenta hija.

## Qué enviar

`labelIds` es opcional. Es un arreglo de ids activos de esa organización.

- El campo es un arreglo para poder aceptar más de una más adelante. Hoy solo se acepta un id.
- Sin duplicados y sin strings vacíos.
- Si lo omites o envías `[]`, el recurso queda sin etiqueta.

```json
"labelIds": ["11111111-1111-1111-1111-111111111111"]
```

Un id que no existe o que ya fue archivado responde `400` con código `InvalidLabels`. Un arreglo inválido (dos ids, duplicados o un id vacío) responde `400` con código `InvalidInput`.

## Qué recibes

La respuesta no repite `labelIds`. Guarda un snapshot congelado al asignar:

```json
"labels": [
  {
    "id": "11111111-1111-1111-1111-111111111111",
    "name": "Legal",
    "color": "#E5534B"
  }
]
```

| Atributo | Tipo | Descripción |
| --- | --- | --- |
| id | string | Id de la etiqueta en el catálogo |
| name | string | Nombre al momento de asignarla |
| color | string | Color `#RRGGBB` al momento de asignarla |

<aside class="notice">
Renombrar o archivar la etiqueta en el portal no cambia lo ya asignado. Si no hay etiquetas, el campo no aparece.
</aside>
