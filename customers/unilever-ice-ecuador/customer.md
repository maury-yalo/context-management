# Customer: Unilever Ecuador ICE

> **Última actualización:** 2026-05-27

---

## 📂 Context Map
> _Este archivo es el punto de entrada. Carga los MDs específicos según la tarea._

### customers/ — contexto del cliente final

| Archivo | Qué contiene | Cuándo cargarlo |
|---------|-------------|-----------------|
| **`customer.md`** ← estás aquí | Overview, equipo, IDs técnicos, métricas, links | Siempre — es el índice |
| [`campanas.md`](campanas.md) | Rendimiento YTD · tipos · HSM templates · cómo solicitar campaña | Campañas, HSM, audiencias de envío |
| [`audiencias.md`](audiencias.md) | Segmentos CDP activos · campos · JSONLogic · cómo crear segmentos | CDP, segmentación, audiencias |
| [`analytics.md`](analytics.md) | BQ storefronts · experimento Oris P1 · queries Q1/Q2 · GMV canónico · datasharing mensual | Análisis de datos, reportes, datasharing |

### accounts-and-flows/ — contexto técnico y de delivery

| Archivo | Qué contiene | Cuándo cargarlo |
|---------|-------------|-----------------|
| [`01-magnum-ec-tech.md`](../../accounts-and-flows/unilever-ice-ecuador/01-magnum-ec-tech.md) | Bots, Cloud Functions, integraciones (Gravty/PWST/Trax), infra, credenciales, stack, LangSmith | Desarrollo, debugging, infra |
| [`02-magnum-ec-pm.md`](../../accounts-and-flows/unilever-ice-ecuador/02-magnum-ec-pm.md) | Stakeholders, status proyecto, tickets Jira/Newton, roadmap, accionables | PM, delivery, seguimiento de tickets |
| [`03-magnum-ec-historia.md`](../../accounts-and-flows/unilever-ice-ecuador/03-magnum-ec-historia.md) | Glosario, crisis Napolita Pro (post-mortem), historia operativa Loyalty | Onboarding, contexto institucional |

---

<!-- DYNAMIC:live_metrics -->
## 📊 Live Metrics
> _🔄 Actualizado: 2026-05-27 — `refresh_live_fields.py --account unilever-ice`_

*ARR = SF `Current_ARR__c` · Health = RCC (Agency) · NPS = Health Model dashboard*

| Campo | Valor |
|---------|-------|
| ARR | **$421,788 USD** |
| Health | 🟢 Green (RCC) |
| NPS | _pendiente_ |
| Renewal | _pendiente_ |
| Región | Ecuador · SSLA |
| Timezone | America/Guayaquil (UTC-5) |

---

## 👥 Account Team

| Rol | Persona |
|-----|---------|
| CSM | Daniel Mosquera |
| Ops | Juliana Guerrero |
| AE | _pendiente — verificar SF_ |

---

## 🤝 Champions & Stakeholders
*Fuente: Agency CRM — contactos con actividad reciente*

| Nombre | Rol | Email |
|--------|-----|-------|
| Kevia Lopez | Digital Operations Chief · ⭐ contacto principal | kevia.lopez@unilever.com |
| Johnny Manrique | System / data notifications contact | johnny.manrique2@unilever.com |
| Alejandro Tobar | Regional Business Manager (también en HPC) | alejandro.tobar@unilever.com |
| Keyla Ramirez | Finance / Compliance | keyla-stefania.ramirez@unilever.com |

---

## 🧱 Identidad técnica

| Campo | Valor |
|-------|-------|
| bot_id | `unilever-ecu-ic-prd` |
| Storefront | `unilever-ecuador-ice-cream` |
| CDP UUID | `63ceb43a85a6789106c048b5` |
| CDP Version | V1 (assumed) |
| Entity type | `stores` |
| SF ID | `0013g00000aNhu2AAC` |
| Slack | `#b2b-unilever-ice-ec` (ID pendiente) |

---

## 🏢 Modelo de negocio

Unilever ICE Ecuador — helados y productos de frío (marcas: Magnum, Pinguino, Sándwich Tri). Canal B2B.

- **Segmentación clave:** `DIA_VISITA` ⚠️ valores en **MAYÚSCULAS** (`LUNES`, `MARTES`, `MIERCOLES`, `JUEVES`, `VIERNES`)
- **Cadencia:** Campañas semanales por semana (`W{n}`) — Digital Transformation model
- **Marcas principales:** Magnum · Pinguino · Sándwich Tri · Club Pinguino · Tortas

---

## 🤖 Use Cases live

| Use Case | Descripción |
|----------|-------------|
| Promo semanal DIA_VISITA | Promos por día de visita del promotor (Digital Transformation) |
| Loyalty DT | Campañas de fidelización recurrentes (`Digital_Loyalty_DT_W{n}`) |
| Innovación / nuevos productos | Lanzamiento de SKUs (Pina Colada, SanducheTri, etc.) |
| Seguimiento Pinguino | Retención club y marca Pinguino post-compra |

---

## 📁 Audience footprint

- Campañas 2026: **492 únicas** · **2.87M enviados** · **99.8% delivery** · **266K órdenes**
- ⚠️ Revenue $314.7M — **verificar unidades con Daniel/Sara** (posiblemente no USD directo — podría ser centavos o unidades de contabilidad local)

---

## 🔗 Links

| Recurso | URL / Referencia |
|---------|-----------------|
| Salesforce | [Account Unilever ICE](https://yalo.lightning.force.com/lightning/r/Account/0013g00000aNhu2AAC/view) |
| Revenue Command Center | [RCC — Unilever ICE](https://revenue-command-center-yalo.vercel.app/dashboard) |
| Performance Standards | [Commerce unilever-ecu-ic-prd](https://performancestandardsyalo.replit.app/commerce/unilever-ecu-ic-prd) |
| OKR Dashboard | [CS OKRs](https://okr-cs-dashboard.replit.app/) |
| Slack | [#b2b-unilever-ice-ec]() · ID: `_pendiente_` |
| Agency CRM | [Unilever ICE en Agency](https://app.agency.inc) |
