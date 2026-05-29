# Audiencias CDP — Unilever Ecuador ICE

> **Account:** Unilever Ecuador ICE · bot_id: `unilever-ecu-ic-prd`
> **Última actualización:** 2026-05-27

---

## 🔑 Identidad CDP

| Campo | Valor |
|-------|-------|
| CDP UUID | `63ceb43a85a6789106c048b5` |
| CDP Version | V1 (assumed) |
| Entity type | `stores` |
| Storefront | `unilever-ecuador-ice-cream` |

---

<!-- DYNAMIC:audiencias_resumen -->
## 📊 Resumen audiencias
> _🔄 Actualizado: 2026-05-27 — `refresh_live_fields.py --account unilever-ice`_

_pendiente — ejecutar CDP CLI para obtener conteo de stores activos y breakdown por DIA_VISITA_

```bash
# Comando para obtener resumen de audiencia
cdp stores list --uuid 63ceb43a85a6789106c048b5
```

---

## 📅 Segmentación DIA_VISITA

> ⚠️ **CRÍTICO: Los valores de `DIA_VISITA` son SIEMPRE EN MAYÚSCULAS**

| Día | Valor en CDP | ❌ Incorrecto |
|-----|-------------|--------------|
| Lunes | `LUNES` | ~~`Lunes`~~ |
| Martes | `MARTES` | ~~`Martes`~~ |
| Miércoles | `MIERCOLES` | ~~`Miercoles`~~ ~~`Miércoles`~~ |
| Jueves | `JUEVES` | ~~`Jueves`~~ |
| Viernes | `VIERNES` | ~~`Viernes`~~ |

**Nota:** No hay `SABADO` ni `DOMINGO` en el modelo de visitas de Unilever ICE Ecuador.

---

## 🧩 JSONLogic — Ejemplos de segmentos

### Por día de visita único

```json
{
  "==": [
    { "var": "DIA_VISITA" },
    "LUNES"
  ]
}
```

### Múltiples días (OR)

```json
{
  "or": [
    { "==": [{ "var": "DIA_VISITA" }, "LUNES"] },
    { "==": [{ "var": "DIA_VISITA" }, "MARTES"] }
  ]
}
```

### Día de visita + marca activa

```json
{
  "and": [
    { "==": [{ "var": "DIA_VISITA" }, "MIERCOLES"] },
    { "==": [{ "var": "MARCA_ACTIVA" }, "MAGNUM"] }
  ]
}
```

> _Nota: confirmar nombre exacto de campos adicionales con Juliana Guerrero antes de usar en producción._

---

## 📝 Solicitud de audiencia nueva

```
Cuenta: Unilever Ecuador ICE
UUID: 63ceb43a85a6789106c048b5
Entity type: stores

Segmentación solicitada:
  DIA_VISITA: [LUNES | MARTES | MIERCOLES | JUEVES | VIERNES]  ← MAYÚSCULAS OBLIGATORIO
  Otros filtros (si aplica): 

Estimado de audiencia (stores): _pendiente — consultar CDP_
Campaña destino: 
Semana de uso: W{n}{Mes}{año}
Responsable Ops: Juliana Guerrero
```

---

## 🔎 Comandos CDP útiles

```bash
# Lookup store por UUID de cuenta
cdp stores list --uuid 63ceb43a85a6789106c048b5

# Filtrar por DIA_VISITA (recordar MAYÚSCULAS)
cdp stores list --uuid 63ceb43a85a6789106c048b5 --filter "DIA_VISITA=LUNES"

# Ver perfil de store individual
cdp stores get --uuid 63ceb43a85a6789106c048b5 --store-id {store_id}

# Contar stores por día de visita
cdp stores count --uuid 63ceb43a85a6789106c048b5 --group-by DIA_VISITA
```

---

## ⚠️ Gotchas críticos

| # | Gotcha | Impacto |
|---|--------|---------|
| 1 | **`DIA_VISITA` SIEMPRE MAYÚSCULAS** — `LUNES`, `MARTES`, `MIERCOLES`, `JUEVES`, `VIERNES` | Filtro retorna 0 stores si se usa title case o minúsculas |
| 2 | **`MIERCOLES` sin tilde** — NO `MIÉRCOLES` | Error de encoding — usar sin tilde en el valor |
| 3 | **CDP Version V1 assumed** — no confirmado | Si hay errores de schema, verificar si la cuenta migró a V2 |
| 4 | **Entity type `stores`** — no `contacts` | Unilever ICE opera sobre puntos de venta (stores), no contactos individuales |
| 5 | **UUID `63ceb43a85a6789106c048b5`** — verificar antes de campañas nuevas | Nunca asumir; confirmar que el UUID sea el correcto con `cdp stores list` |
