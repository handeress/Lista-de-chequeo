# Aprobaciones

App para aprobar las listas largas que Claude le propone a Andrés en el chat: en vez de contestar ítem por ítem escribiendo, él marca cada uno **Aprobar**, **Revisar** o **Descartar** (con nota opcional) y toca **Terminé**. Claude lee las respuestas y sigue. Solo para listas largas; las cortas se responden en el chat con números.

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

### Listas de porcentaje (avance)

Con `"modo": "porcentaje"` en la lista, cada ítem muestra el porcentaje con botones −5, +5 y 100 y una casilla para escribirlo; si el ítem trae `valor`, muestra el ejecutado (valor × %). Cada ítem lleva `"pct"` con el porcentaje de partida. Arriba de cada grupo y abajo en el pie sale el avance ponderado por valor. La respuesta guarda `"pct"` en cada ítem cambiado.

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

## Archivo

Al abrir, la app muestra directamente la lista vigente (la más reciente sin terminar). Cuando Claude lee una lista terminada, la mueve de `listas/` a `archivo/` (con `git mv`); la app la muestra en "Ver archivadas", solo para consulta. Las respuestas se quedan en `respuestas/`.

## Versiones

Subir `CACHE` en `sw.js` y `VERSION` en `index.html` en cada cambio. Para actualizar en el celular: cerrar y abrir dos veces con internet.

- 1.0 (22/11/2026): listas por grupos, tres estados con nota, aprobar grupo, filtros, búsqueda, Terminé, sin señal y sincronización.
- 1.2 (22/11/2026): listas de porcentaje (`modo: porcentaje`) para dar avances de obra.
- 1.1 (22/11/2026): abre la lista vigente al entrar; listas archivadas por Claude se ven aparte, solo consulta.

- 23-nov-2026: atrás por niveles (subpágina → elemento → lista o inicio → cierra la app); botón Instalar en el celular.
