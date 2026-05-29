# Campañas — Unilever Ecuador HPC

> **bot_id:** `unilever-ecu-hpc-prd` · **Storefront:** `unilever-ecuador-hpc` · **Timezone:** `America/Guayaquil (UTC-5)`
>
> **Última actualización:** 2026-05-27

---

<!-- DYNAMIC:campanas_ytd -->
## 📊 Rendimiento YTD (2026)
> _🔄 Actualizado: 2026-05-27_

| Métrica | Valor |
|---------|-------|
| Campañas únicas | 551 |
| Notificaciones | 670,798 |
| Enviados | 670,798 |
| Entregados | 660,842 |
| Delivery rate | 98.5% |
| Respondidos | 190,008 |
| Response rate | 28.3% |
| Órdenes | 26,311 |
| Revenue | $4,210,016 USD |

### Top 10 campañas por revenue

| # | Campaña | Enviados | Órdenes | Revenue | Conv% |
|---|---------|---------|---------|---------|-------|
| 1 | Promo_Ciclo4_Mar26_Engagedprospects: DIA_VISITA2 == Viernes | 2,985 | 86 | $154,955 | 2.9% |
| 2 | Caso_de_uso_No_visita_lunes_Yalo_Intelligence | 10,843 | 496 | $119,551 | 4.6% |
| 3 | Promo_Ciclo4_Feb26: DIA_VISITA2 == Viernes | 6,114 | 458 | $102,638 | 7.5% |
| 4 | Promo_Ciclo2_Q1_Feb26: DIA_VISITA2 in Miercoles | 5,878 | 251 | $101,223 | 4.3% |
| 5 | Promo_Ciclo1_Ene26: DIA_VISITA2 in Miercoles | 6,643 | 344 | $94,933 | 5.2% |
| 6 | Promo_Ciclo3_Feb26: DIA_VISITA2 in Miercoles | 5,982 | 334 | $90,918 | 5.6% |
| 7 | Caso_de_uso_No_visita_miercoles_Yalo_Intelligence | 4,808 | 242 | $83,074 | 5.0% |
| 8 | Promo_Ciclo3_Feb26: DIA_VISITA2 in Viernes | 5,560 | 309 | $81,031 | 5.6% |
| 9 | Caso_de_uso_No_visita_viernes_Yalo_Intelligence | 6,253 | 293 | $65,940 | 4.7% |
| 10 | Promo_Ciclo4_Feb26: DIA_VISITA2 == Miercoles | 6,136 | 301 | $64,573 | 4.9% |

**Observaciones:**
- El top de revenue lo lideran campañas `Promo_Ciclo` segmentadas por `DIA_VISITA2`
- No-visita YI tiene conversión superior (4.6–5.0%) vs promos masivas
- Viernes y Miércoles son los días de mayor impacto en revenue

---

## 🗂️ Tipos de campaña

### 1. Promo Ciclo (principal)
Campañas de promoción organizadas por ciclo (1–4 por mes). Siempre segmentadas por `DIA_VISITA2`.

- **Naming:** `Promo_Ciclo{n}_{mes}{año}[_Engagedprospects]: DIA_VISITA2 {op} {día}`
- **Frecuencia:** ~4 ciclos por mes
- **HSM:** `IRIS_hsm_Promo_Ciclo{n}_{mes}{año}_v{n}`
- **Segmento:** `DIA_VISITA2 == {día}` o `DIA_VISITA2 in {día}`
- **Revenue:** Principal driver (~70% del revenue YTD)

### 2. No-visita YI (Yalo Intelligence)
Rescate automático para tiendas que no tuvieron visita del vendedor ese día.

- **Naming:** `Caso_de_uso_No_visita_{día}_Yalo_Intelligence`
- **Días:** lunes, miercoles, viernes (confirmados en top 10)
- **HSM:** `IRIS_utilidad_aviso_no_visita_img_variable_v2`
- **Tipo:** Utility (no marketing)
- **Conversión:** Alta (4.6–5.0%) — audiencia calificada (tiendas sin visita ese día)

### 3. Pedido Digital
Comunicación instructiva sobre canal digital. Posiblemente onboarding o reactivación.

- **Naming:** campaña con HSM `IRIS_hsm_PedidoDigital_{mes}{año}_v{n}`
- **HSM más reciente:** `IRIS_hsm_PedidoDigital_May2026_v1`
- **Objetivo:** Adopción del canal digital

### 4. Comunicación / Utility
Avisos operativos, notificaciones informativas.

- **HSM tipo:** `IRIS_utilidad_*`
- **Ejemplo:** `IRIS_utilidad_aviso_no_visita_img_variable_v2`

---

<!-- DYNAMIC:hsm_templates -->
## 📱 HSM Templates activos
> _🔄 Actualizado: 2026-05-27_

| # | Template | Volumen YTD |
|---|---------|------------|
| 1 | `IRIS_hsm_suggestedorder_eprospects` | 26,803 |
| 2 | `IRIS_utilidad_aviso_no_visita_img_variable_v2` | 25,653 |
| 3 | `IRIS_hsm_PedidoDigital_May2026_v1` | 22,097 |
| 4 | `IRIS_hsm_Promo_Ciclo4_Feb26_v1` | 21,929 |
| 5 | `IRIS_hsm_Promo_Ciclo3_Feb26_v1` | 21,776 |
| 6 | `IRIS_hsm_Promo_Ciclo2_Mar26_v2` | 21,762 |
| 7 | `IRIS_hsm_Promo_Ciclo2_Feb26_v4` | 21,534 |
| 8 | `IRIS_hsm_Promo_Ciclo1_Ene26_v3` | 21,132 |
| 9 | `IRIS_hsm_Promo_Ciclo3_Ene26_v2` | 21,004 |
| 10 | `IRIS_hsm_Promo_Ciclo4_Abr26_Engagedprospects_v3` | 20,774 |

**Patrón de naming HSM:**
- Promo ciclo: `IRIS_hsm_Promo_Ciclo{n}_{mes}{año}_v{n}`
- Pedido digital: `IRIS_hsm_PedidoDigital_{mes}{año}_v{n}`
- No-visita: `IRIS_utilidad_aviso_no_visita_img_variable_v{n}`
- Suggested order: `IRIS_hsm_suggestedorder_eprospects`

---

## 🚀 Cómo solicitar una campaña

### Wizard de 5 pasos

**Paso 1 — Identificar el tipo**
¿Es promo de ciclo, no-visita YI, pedido digital, o comunicación?

**Paso 2 — Definir el segmento**
- Para promos: ¿Qué valor de `DIA_VISITA2`? (Lunes / Martes / Miercoles / Jueves / Viernes)
- ¿Es Engagedprospects (subgrupo activo) o universo completo?
- Verificar que el segmento CDP existe → ver `audiencias.md`

**Paso 3 — Confirmar el HSM**
- ¿Existe el template en Iris? Si no, crear primero (ver sección abajo)
- Naming del template: `IRIS_hsm_Promo_Ciclo{n}_{mes}{año}_v{n}`

**Paso 4 — Definir timing**
- Timezone: `America/Guayaquil (UTC-5)`
- ¿Qué día y hora de envío? (Típicamente el mismo día del ciclo)
- No-visita YI: enviar a primera hora del día correspondiente

**Paso 5 — Solicitar a Ops (Marcela Colorado)**
Llenar el bloque de solicitud correspondiente y enviar por Slack `#b2b-unilever-hpc-ec`.

---

### Bloque de solicitud — Promo Ciclo

```
SOLICITUD DE CAMPAÑA — Promo Ciclo
Cuenta: Unilever Ecuador HPC
bot_id: unilever-ecu-hpc-prd

Nombre campaña: Promo_Ciclo{N}_{Mes}{Año}: DIA_VISITA2 == {Día}
Segmento: [nombre del segmento CDP]
HSM template: IRIS_hsm_Promo_Ciclo{N}_{Mes}{Año}_v{n}
Fecha/hora envío: YYYY-MM-DD HH:MM (America/Guayaquil UTC-5)
Variables: [lista de variables del HSM si aplica]
Notas: [contexto adicional, ej. si es Engagedprospects]
```

---

### Bloque de solicitud — No-visita YI

```
SOLICITUD DE CAMPAÑA — No-visita Yalo Intelligence
Cuenta: Unilever Ecuador HPC
bot_id: unilever-ecu-hpc-prd

Nombre campaña: Caso_de_uso_No_visita_{día}_Yalo_Intelligence
Segmento: [tiendas sin visita día {día}]
HSM template: IRIS_utilidad_aviso_no_visita_img_variable_v2
Fecha/hora envío: YYYY-MM-DD HH:MM (America/Guayaquil UTC-5)
Día afectado: [lunes / miercoles / viernes]
Notas: [contexto del trigger YI]
```

---

## 🆕 Cómo crear un HSM en Iris

> Aplica cuando el template no existe aún en Iris para el ciclo/mes actual.

1. **Acceder a Iris** → sección Templates / HSM
2. **Naming convention obligatorio:**
   - Promo ciclo: `IRIS_hsm_Promo_Ciclo{N}_{Mes}{Año}_v1`
     - Ejemplo: `IRIS_hsm_Promo_Ciclo2_Jun26_v1`
   - Pedido digital: `IRIS_hsm_PedidoDigital_{Mes}{Año}_v1`
   - No-visita: `IRIS_utilidad_aviso_no_visita_img_variable_v{n}`
3. **Seleccionar cuenta:** `unilever-ecu-hpc-prd`
4. **Categoría:** Marketing (promos) / Utility (no-visita, avisos)
5. **Idioma:** Español (es)
6. **Variables:** Verificar que coinciden con las del segmento CDP
   - Común: nombre tienda, producto, precio, imagen del ciclo
7. **Enviar a aprobación Meta** — tiempo estimado: 24–48h
8. **Confirmar aprobación** antes de programar campaña
9. **Versionar:** si hay rechazo, crear `_v2`, `_v3`, etc.

---

## ⚠️ Bugs y gotchas

### 🔴 CRÍTICO: `DIA_VISITA2` — con "2"
El atributo de segmentación es **`DIA_VISITA2`** (con el número 2 al final).  
`DIA_VISITA` (sin 2) es un campo diferente o inexistente — usar el incorrecto produce audiencias vacías o incorrectas.

```
✅ CORRECTO:   DIA_VISITA2 == "Viernes"
❌ INCORRECTO: DIA_VISITA == "Viernes"
```

### 🔴 Ciclos — numeración reinicia cada mes
Los ciclos van del 1 al 4 (o 5) dentro de cada mes. No son continuos entre meses.
- Ene: Ciclo1–Ciclo4
- Feb: Ciclo1–Ciclo4
- etc.
- Naming: `Promo_Ciclo{N}_{Mes}{Año}` — incluir mes y año siempre

### 🟡 Ecuador = USD
Revenue y precios siempre en USD. No hay conversión de moneda.

### 🟡 Engagedprospects = subgrupo
`_Engagedprospects` en el nombre de campaña indica un segmento más pequeño (tiendas activas/prospectos comprometidos). Tienen menor volumen pero mejor conversión potencial.

### 🟡 Versiones de HSM
Si un template es rechazado por Meta, se crea `_v2`, `_v3`. Verificar siempre cuál versión está aprobada antes de programar.
