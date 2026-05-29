# Unilever ICE Ecuador — Analytics & Datasharing

> **Fuente:** `YLandrey/yalo-client-docs` — Andrey Cifuentes (Data Yalo)
> **Última actualización:** 2026-05-29

---

## Bot IDs y storefronts en BigQuery

| País | bot_id | Storefronts |
|------|--------|-------------|
| Ecuador | `unilever-ecu-ic-prd` | `unilever-ecu-b2b`, `unilever-ec-ice-presale` |

> ⚠️ Unilever ICE tiene **2 storefronts** en `commerce_dim_products`. Siempre filtrar con `IN ('unilever-ecu-b2b', 'unilever-ec-ice-presale')` — usar solo uno subestima el universo.

---

## Experimento Oris P1

Las tiendas del experimento se identifican con el atributo `P1_ENABLE` en `dim_attributes_customer`.

- Valores válidos: `'test'` o `'control'` (case insensitive)
- Cada tienda tiene un único valor — no hay tiendas en ambos grupos

```sql
SELECT DISTINCT store_id, value
FROM arched-photon-194421.DWH2.dim_attributes_customer
WHERE bot_id = 'unilever-ecu-ic-prd'
  AND property IN ('P1_ENABLE')
  AND LOWER(value) IN ('test', 'control')
```

---

## Datasharing a SharePoint — Oris Resultados (cron diario)

| Campo | Valor |
|-------|-------|
| Notebook | `Analytics/Unilever ICE/Unilever IC EC - Datasharing Oris Resultados.ipynb` |
| Plataforma | Colab Enterprise — cron diario |
| Secret GCP | `unilever-ec-sharepoint` en `prod-yalo-main` (JSON: tenantId, clientId, clientSecret, resourceId) |
| Carpeta SharePoint | `{resourceId}/Shared Documents/Proyecto ALEXA -YALO - ICE_BOT/Reportes Oris` |
| Archivo output | `Oris_DonPinguino_Resultados_{YYYY-MM-DD}.csv` |

---

## Q1 — Oris P1 Pedido Sugerido (sugerido vs comprado)

- **Grain:** SKU × sesión × order_id
- **Universo:** tiendas con `P1_ENABLE IN ('test','control')` + sesiones con `suggested_order_skill`
- **GMV:** `SUM(p.subtotal - p.discount_given)` desde `commerce_fct_orders` con `status IN ('CONFIRMED','PROCESSED')`

---

## Q2 — Sesiones Agénticas (todo el bot)

Grain: `commerce_session_id`. Cubre **todas las tiendas** del bot (no solo P1_ENABLE). Se reporta junto con Q1 al cliente.

### Patrón canónico `tipo_agentic` (4 buckets)

```sql
CASE
  WHEN oris = 'P1' AND 'suggested_order_skill' IN UNNEST(genie_facets_concat) THEN 'Suggested Order P1'  -- == universo Q1
  WHEN oris = 'P1' AND ARRAY_LENGTH(genie_facets_concat) > 0                  THEN 'P1 otras skills'
  WHEN oris IN ('R1', 'R1 Fallback')                                          THEN 'R1'
  ELSE                                                                              'No agentic'
END
```

### GMV — antipatrón documentado

| | Query | Problema |
|-|-------|---------|
| ❌ | `MAX(saf.total)` desde `sales_agent_fct_source_v2` | No filtra status, agrupa mal con multi-orden por sesión |
| ✅ | JOIN con `commerce_fct_orders` + `UNNEST(products)` + `SUM(subtotal - discount_given)` + `status IN ('CONFIRMED','PROCESSED')` | Consistente con Q1 |

### Validación Q1 ↔ Q2 (benchmark Abr 2026, 14 días)

| Métrica | Q1 | Q2 | Δ |
|---------|----|----|---|
| Sesiones | 13,507 | 13,492 | -0.11% |
| Órdenes | 12,759 | 12,762 | +0.02% |
| GMV | $1,065,300 | $1,065,702 | +0.04% |

**Match 99.96%.** Gap residual: ~15 sesiones desfasadas por timezone (Q1 usa UTC, Q2 usa `America/Guayaquil`).

### Composición típica del universo agéntico

| Bucket | % sesiones agénticas |
|--------|----------------------|
| Suggested Order P1 | ~78% |
| P1 otras skills (up_sell, cross_sell) | ~17% |
| R1 | ~5% |

### Archivos SQL en el repo de Andrey

| Archivo | Descripción |
|---------|-------------|
| `Analytics/Unilever ICE/Unilever IC EC - Q2 Sesiones Agenticas v2.sql` | Q2 lista para producción |
| `Analytics/Unilever ICE/Unilever IC EC - Q2 v2 Validacion.sql` | Agregado por `tipo_agentic` para validar vs Q1 |
| `Analytics/Unilever ICE/Unilever IC EC - Diagnostico Q1 vs Q2.sql` | Diagnóstico cruzado (divergencias, sesiones ∩) |

---

## Datasharing mensual al equipo Ice

### Convención de archivos y entrega

| Archivo | Contenido |
|---------|-----------|
| `Datos_Resultados_Pedido_Sugerido_{Mes}_{YYYY}.csv` | Q1: SKU × sesión × order_id |
| `Sesions_{Mes}_{YYYY}.csv` | Q2: sesiones agénticas completas |
| `Resultados Yalo {Mes} {YYYY}.zip` | ZIP con contraseña que contiene ambos CSVs |

- **Contraseña ZIP:** `DataYalo{YYYY}` (ej: `DataYalo2026`)
- **Corte de fechas:** último día del mes exacto — validar que no haya filas del mes siguiente

### Template correo mensual

**Asunto:** `Resultados Don Pingüino – {Mes} {YYYY}`

```
Buenas tardes,

Les comparto los resultados del 1 al 30 de {mes}. El archivo comprimido contiene 2 CSVs:

1. Datos_Resultados_Pedido_Sugerido_{Mes}_{YYYY}.csv — {N} líneas
   Pedido Sugerido Oris P1: sugerido vs comprado.

2. Sesions_{Mes}_{YYYY}.csv — {N} líneas
   Detalle de sesiones de Don Pingüino:
   • session_id: ID único de la sesión.
   • fecha: Día en hora local Ecuador (Guayaquil).
   • store_id: ID de la tienda.
   • oris: Versión del agente (P1, R1, o vacío).
   • tipo_agentic: Tipo de interacción agéntica (4 valores).
   • session_agentic: TRUE si Oris se activó con al menos una skill.
   • n_ordenes: Órdenes confirmadas en la sesión.
   • skus_comprados: SKUs distintos comprados.
   • gmv: Venta total (subtotal menos descuentos), en dólares.
   • skills_usadas: Skills ejecutadas (suggested_order, up_sell, etc.).

Contraseña: DataYalo{YYYY}

¿Podrían confirmar que recibieron la información correctamente?

Saludos,
Andrey
```
