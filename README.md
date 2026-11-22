# Aprobaciones

App para aprobar listas largas desde el celular: cada ítem se marca **Aprobar**, **Revisar** o **Descartar**, con nota opcional. Claude arma la lista y lee las respuestas; Andrés no copia ni pega nada.

- App: https://handeress.github.io/Lista-de-chequeo/ (código público, sin datos).
- Datos: repo privado `handeress/Bitacora-datos`, carpeta `aprobaciones/`.
- Llave: la misma de la Bitácora ("Bitacora celular"). La app la toma sola si la Bitácora ya se abrió en el mismo navegador; si no, se pega en Ajustes (⚙).
- Prueba sin datos: `?demo` al final de la dirección.

## Lo que escribe Claude: `aprobaciones/listas/<id>.json`

```json
{
  "id": "suesca-actividades-2026-11-22",
  "titulo": "Actividades Suesca",
  "proyecto": "Suesca",
  "descripcion": "Texto corto arriba de la lista (opcional).",
  "items": [
    {"id": "1", "grupo": "Cimentación", "titulo": "Placa de tanques", "detalle": "Texto largo opcional", "valor": 1850000}
  ]
}
```

- `id` de la lista sin espacios; `id` de cada ítem único dentro de la lista y estable (si se reemplaza la lista, las respuestas se conservan por id).
- `grupo`, `detalle` y `valor` son opcionales. `valor` numérico se muestra en pesos; texto se muestra tal cual.

## Lo que escribe la app: `aprobaciones/respuestas/<id>.json`

```json
{
  "lista": "suesca-actividades-2026-11-22",
  "titulo": "Actividades Suesca",
  "terminada": true,
  "actualizado": "2026-11-22T14:00:00.000Z",
  "items": {"1": {"estado": "aprobado", "nota": "", "fecha": "…"}}
}
```

- `estado`: `aprobado`, `revisar`, `descartado` o vacío (sin marcar).
- `terminada` = Andrés tocó **Terminé**. Claude lee la lista cuando está terminada, o antes si Andrés lo pide.
- Claude no escribe en `respuestas/`.

## Versiones

Subir `CACHE` en `sw.js` y `VERSION` en `index.html` en cada cambio. Para actualizar en el celular: cerrar y abrir dos veces con internet.

- 1.0 (22/11/2026): listas por grupos, tres estados con nota, aprobar grupo, filtros, búsqueda, Terminé, sin señal y sincronización.
