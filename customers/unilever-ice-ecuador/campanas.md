# Campañas — Unilever Ecuador ICE

> **Account:** Unilever Ecuador ICE · bot_id: `unilever-ecu-ic-prd`
> **Última actualización:** 2026-05-27

---

<!-- DYNAMIC:campanas_ytd -->
## 📊 Métricas YTD 2026 (BQ)
> _🔄 Actualizado: 2026-05-27_

| Métrica | Valor |
|---------|-------|
| Campañas únicas | 492 |
| Notificaciones / Enviados | 2,877,612 |
| Entregados | 2,871,023 |
| Delivery rate | **99.8%** |
| Respondidos | 2,222,452 |
| Response rate | 77.2% ⚠️ _incluye auto-responses del bot — no es tasa de respuesta humana real_ |
| Órdenes | 266,534 |
| Revenue reportado | $314,677,488 ⚠️ _verificar unidades — posiblemente no USD directo_ |

---

## 🗂️ Tipos de campaña

| Tipo | Patrón naming | Descripción |
|------|--------------|-------------|
| Digital Transformation promo | `Digital_{producto}_DT_{hora?}_W{n}{mes}{año}: DIA_VISITA == {DÍA}` | Promos semanales por día de visita de promotor |
| Loyalty DT | `Digital_Loyalty_DT_W{n}{mes}{año}: DIA_VISITA == {DÍA}` | Fidelización recurrente semanal |
| Innovación producto | `Digital_Innov_{producto}_DT_W{n}{mes}{año}: DIA_VISITA == {DÍA}` | Lanzamiento o activación de nuevos SKUs |
| Seguimiento / Retención | `Digital_Seguimiento_{marca}_W{n}{mes}{año}` | Post-compra, club de marca (ej. Club Pinguino) |
| Descuento producto | `Digital_Dcto{producto}_Visita_DT_{audiencia}{mes}{año}` | Descuentos puntuales por producto |

---

## 🏆 Top 10 campañas por revenue (YTD 2026)

| # | Campaña | Enviados | Órdenes | Revenue | Conv% |
|---|---------|---------|---------|---------|-------|
| 1 | Digital_Innov_PiñaColada_DT_W3Jan25: DIA_VISITA == MARTES | 41,177 | 5,072 | $7,397,010 | 12.3% |
| 2 | Digital_Innov_PiñaColada_DT_W3Jan25: DIA_VISITA == LUNES | 41,707 | 5,092 | $7,229,933 | 12.2% |
| 3 | Digital_Innov_PiñaColada_DT_W3Jan25: DIA_VISITA == VIERNES | 35,934 | 4,673 | $6,327,037 | 13.0% |
| 4 | Digital_SanducheTri_DT_8AM_W1Abr26: DIA_VISITA == LUNES | 40,336 | 3,637 | $5,140,386 | 9.0% |
| 5 | Digital_Loyalty_DT_W4Mar26: DIA_VISITA == LUNES | 41,390 | 3,945 | $4,932,503 | 9.5% |
| 6 | Digital_SanducheTri_DT_8AM_W1Abr26: DIA_VISITA == MARTES | 36,314 | 3,534 | $4,898,901 | 9.7% |
| 7 | Digital_Loyalty_DT_W1Jan25: DIA_VISITA == JUEVES | 24,561 | 2,612 | $4,500,903 | 10.6% |
| 8 | Digital_Loyalty_DT_W4Mar26: DIA_VISITA == MARTES | 34,313 | 3,542 | $4,498,397 | 10.3% |
| 9 | Digital_SanducheTri_DT_8AM_W1Abr26: DIA_VISITA == JUEVES | 32,181 | 3,022 | $4,114,753 | 9.4% |
| 10 | Digital_Loyalty_DT_W4Mar26: DIA_VISITA == MIERCOLES | 33,262 | 3,404 | $4,081,777 | 10.2% |

---

<!-- DYNAMIC:hsm_templates -->
## 📋 Top 10 HSM Templates por volumen (YTD 2026)
> _🔄 Actualizado: 2026-05-27_

| # | Template | Enviados |
|---|---------|---------|
| 1 | IRIS_hsm_Digital_PromoMagnum_Visita_DT_W1Abr26_v1 | 37,990 |
| 2 | IRIS_hsm_Digital_SanducheTri_W1Abr26_v1 | 36,914 |
| 3 | IRIS_hsm_Digital_Innovacion_SanducheTri_All_W4May26_v1 | 35,471 |
| 4 | IRIS_hsm_Digital_Loyalty_DT_W4Mar25_v1 | 35,429 |
| 5 | IRIS_hsm_Digital_DctoSanduche_Visita_DT_AllMay26_v1 | 35,340 |
| 6 | IRIS_hsm_Digital_Promo_Tortas_3Potes_DT_W1y2May26_v1 | 27,129 |
| 7 | IRIS_hsm_Digital_Carrusel_SeguimientoPinguino_W3May26_v1 | 22,203 |
| 8 | IRIS_hsm_Digital_Seguimiento_ClubPinguino_W3Abr26 | 20,764 |
| 9 | IRIS_hsm_Digital_Innov_PinaColada_DT_W3Jan25_v1 | 19,882 |
| 10 | IRIS_hsm_Digital_Loyalty_All_W1Feb25 | 19,027 |

**Naming convention HSM:**
```
IRIS_hsm_Digital_{descripcion}_{audiencia}_{semana}_v{n}
```
- `{descripcion}`: producto o tipo (PromoMagnum, SanducheTri, Loyalty, DctoSanduche, Innovacion...)
- `{audiencia}`: DT (Digital Transformation), All, Visita
- `{semana}`: W{n}{Mes}{año} (ej. W1Abr26)
- `_v{n}`: versión (v1, v2...)

---

## 🧙 Cómo solicitar una campaña nueva

### Paso a paso (5 pasos)

1. **Definir tipo** — ¿Es promo semanal, loyalty, innovación de producto o seguimiento?
2. **Identificar audiencia** — ¿Qué `DIA_VISITA` aplica? (`LUNES`, `MARTES`, `MIERCOLES`, `JUEVES`, `VIERNES`) ⚠️ SIEMPRE EN MAYÚSCULAS
3. **Semana de envío** — Confirmar `W{n}{Mes}{año}` (ej. W2Jun26)
4. **¿HSM nuevo o reusar?** — Si es nuevo, solicitar aprobación Meta con naming `IRIS_hsm_Digital_{desc}_{audiencia}_{semana}_v1`
5. **Llenar form y enviar a Ops** — Usar el bloque de solicitud correspondiente abajo

---

### 📝 Form: Digital_Promo_semanal

```
Tipo: Digital_Promo_semanal
Campaña nombre: Digital_{producto}_DT_{hora?}_W{n}{mes}{año}
Segmento DIA_VISITA: [LUNES | MARTES | MIERCOLES | JUEVES | VIERNES]  ← MAYÚSCULAS
Producto/SKU: 
Semana: W{n}{Mes}{año}
Hora de envío (opcional): (ej. 8AM)
HSM template: IRIS_hsm_Digital_{desc}_Visita_DT_{semana}_v1
Copies del mensaje: 
CTA: 
Fecha límite de aprobación Meta: 
Responsable Ops: Juliana Guerrero
```

---

### 📝 Form: Digital_Loyalty

```
Tipo: Digital_Loyalty
Campaña nombre: Digital_Loyalty_DT_W{n}{mes}{año}
Segmento DIA_VISITA: [LUNES | MARTES | MIERCOLES | JUEVES | VIERNES]  ← MAYÚSCULAS
Semana: W{n}{Mes}{año}
HSM template: IRIS_hsm_Digital_Loyalty_DT_{semana}_v1
Beneficio/oferta loyalty: 
Copies del mensaje: 
CTA: 
Fecha límite de aprobación Meta: 
Responsable Ops: Juliana Guerrero
```

---

## ⚠️ Gotchas críticos

| # | Gotcha | Impacto |
|---|--------|---------|
| 1 | **`DIA_VISITA` valores son MAYÚSCULAS** — `LUNES`, `MARTES`, `MIERCOLES`, `JUEVES`, `VIERNES` | Audiencia vacía si se usa title case (`Lunes`) |
| 2 | **Revenue $314.7M necesita verificación de unidades** | No asumir USD directos — confirmar con Daniel o Sara si son centavos o unidades locales |
| 3 | **Response rate 77.2% incluye auto-responses del bot** | No reportar como tasa de respuesta humana — el número real es menor |
| 4 | **Campañas DT siempre incluyen DIA_VISITA en el nombre** después de los dos puntos (`: DIA_VISITA == LUNES`) | El nombre de campaña en BQ incluye el segmento como sufijo |
| 5 | **Marcas distintas tienen naming diferente** — `SanducheTri` (sin tilde), `PiñaColada` (con tilde en nombre pero `PinaColada` en HSM) | Revisar naming exacto en BQ antes de crear nuevos HSM |
