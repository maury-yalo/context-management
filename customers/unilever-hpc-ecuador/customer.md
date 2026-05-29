# Customer: Unilever Ecuador HPC

> **Última actualización:** 2026-05-27

---

## 📂 Kit Operativo
> _Abre este archivo primero. Navega a los MDs específicos según lo que necesites._

| # | Archivo | Para qué sirve |
|---|---------|----------------|
| 1 | `campanas.md` | Rendimiento YTD · tipos · HSM templates · cómo solicitar campaña · cómo crear HSM |
| 2 | `audiencias.md` | Segmentos CDP activos · campos · JSONLogic · cómo solicitar nuevo segmento |

---

<!-- DYNAMIC:live_metrics -->
## 📊 Live Metrics
> _🔄 Actualizado: 2026-05-27 — `refresh_live_fields.py --account unilever-hpc`_

*ARR = SF `Current_ARR__c` · Health = RCC (Agency) · NPS = Health Model dashboard*

| Métrica | Valor |
|---------|-------|
| ARR | $172,000 USD |
| Health | 🟢 Green (RCC) |
| NPS | pendiente |
| Renewal | pendiente — verificar SF |
| País / Región | Ecuador · SSLA |

---

## 👥 Account Team

| Rol | Nombre |
|-----|--------|
| CSM | Daniel Mosquera |
| Ops | Marcela Colorado |
| AE | Juan Ulloa |

---

## 🤝 Champions & Stakeholders
*Fuente: Agency CRM — contactos con actividad reciente*

| Nombre | Rol | Email |
|--------|-----|-------|
| Alexander Nazareno | Commercial Contact / RTM Specialist | alexander.nazareno@unilever.com |
| Alejandro Tobar | Budget Approval Authority (aprobación PS) | alejandro.tobar@unilever.com |
| Eloy González Jara | IT Tools Specialist — BI & CD Transformation | _no email en CRM_ |

---

## 🧱 Identidad técnica

| Campo | Valor |
|-------|-------|
| `bot_id` | `unilever-ecu-hpc-prd` |
| Storefront | `unilever-ecuador-hpc` |
| CDP UUID | `63ceb48fadbef7545f69f806` |
| CDP Version | V1 (assumed — verificar) |
| Entity type | `stores` |
| SF ID | `001Ns00000JFCsOIAX` |
| Slack | `#b2b-unilever-hpc-ec` (ID pendiente) |
| Timezone | `America/Guayaquil (UTC-5)` |

---

## 🏢 Modelo de negocio
Unilever HPC Ecuador — productos de cuidado personal y hogar (Home & Personal Care). Canal B2B: distribuidores hacen pedidos vía WhatsApp.

- Segmentación principal: `DIA_VISITA2` ⚠️ (con "2") — días de visita del vendedor
- Campañas organizadas por **ciclos** (Ciclo1–Ciclo4 por mes)
- Cuenta con **Yalo Intelligence (YI)** para casos de no-visita automáticos
- Revenue en USD (Ecuador dolarizado)

---

## 🤖 Use Cases live

| Use Case | Estado |
|----------|--------|
| Promo por ciclo + `DIA_VISITA2` | ✅ Live |
| No-visita YI (rescate automático) | ✅ Live |
| Pedido digital | ✅ Live |
| Comunicaciones informativas (utility) | ✅ Live |

---

## 📁 Audience footprint

| Métrica | Valor |
|---------|-------|
| Campañas 2026 YTD | 551 únicas |
| Revenue YTD | $4,210,016 USD |
| Notificaciones enviadas | 670,798 |
| Delivery rate | 98.5% |
| Response rate | 28.3% |
| Órdenes generadas | 26,311 |
| Atributo clave | `DIA_VISITA2` (Lunes–Viernes) |

---

## 🔗 Links

| Recurso | URL / Referencia |
|---------|-----------------|
| Salesforce | [Account Unilever HPC](https://yalo.lightning.force.com/lightning/r/Account/001Ns00000JFCsOIAX/view) |
| Revenue Command Center | [RCC — Unilever HPC](https://revenue-command-center-yalo.vercel.app/dashboard) |
| Performance Standards | [Commerce unilever-ecu-hpc-prd](https://performancestandardsyalo.replit.app/commerce/unilever-ecu-hpc-prd) |
| OKR Dashboard | [CS OKRs](https://okr-cs-dashboard.replit.app/) |
| Slack | [#b2b-unilever-hpc-ec]() · ID: `_pendiente_` |
| Agency CRM | [Unilever HPC en Agency](https://app.agency.inc) |
