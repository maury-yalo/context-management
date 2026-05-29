# Unilever Ecuador — Knowledge Base

> **Documento maestro consolidado** del handover Tiziana → Alejandra
> **Versión:** v13 (2026-05-22) — incorpora call overlap Tisi-Ale (53 min) + CR2 SOW IR Trax Loyalty + Base Customers CSV + Calendario PWST + screenshot config tiered Gravty
> **Fuentes:** 3 JSON exports, 3 HTMLs arquitectura, Slack `#b2b_unilever_ec_icecream` (16+ threads), `#loyalty-lji-yalo`, `#delivery-engineering-support`, 10 Notion docs técnicos (~150k chars), 2 repos clonados, ~25 archivos de código leídos, 2 PDFs (CR Task 3 + CR2 Fase 2), 1 email priorización, 1 CSV Base Customers (38,962 rows), 1 XLSX Calendario PWST, 1 transcript call overlap
> **Owner saliente:** Tiziana Rassow (tiziana@yalo.com) — último día 22/5/2026
> **Owner entrante:** Alejandra López (maria.lopez@yalo.com)

---

## 📧 Email final Tisi — Priorización & Definición (22/5/2026 10:12 -03)

**De:** Tiziana Rassow · **A:** Kevia Lopez + Pedro Tejena (Magnum), Daniel Mosquera, Ale, Adán Muguiro

### Cosas EN PROD esta semana (5)

1. ✅ Nota de Crédito Inyección Canjes a PWST (UEICE-871)
2. ✅ Consolidación reportes Loyalty — Dashboard Gravty (Newton #79299) — **validar**
3. ✅ Enviar fecha de pedido a Gravty no de facturación (UEICE-879)
4. ✅ Nuevo Message Template con imagen para puntos loyalty — **pendiente revisar costo notificación Marketing**
5. ✅ Solución bug Napolita Pro

### Siguiente semana

- 🔄 Continuar Digitalización Canal Móvil FRIGOS
- 🔵 Iniciar Copy clientes primer pago Yalo Pago en flujo

### Pendiente release

- ⛔️ **Antifraude:** Aprobado ✅ — Paso a PROD **1/6/2026** (queda el **2/6**)
- ⛔️ **Mecánicas promocionales:** Sigue en QA — paso a PROD tentativamente **Agosto 2026**

### Junio: archivo de siempre

⚠️ **Metas segmentadas Target Club Pingüino** — Junio: enviar archivo "de siempre" sin ajustes (aún no configurado por Gravty)

### Adjunto al email: CR Task 3 POP v2.0 — $11,440 USD / 11 días

| Item | Precio (USD) | Tiempo |
|---|---|---|
| Implementación nueva lógica Task 3 | $9,360 | 9 días |
| Pruebas e2e | $1,040 | 1 día |
| Pase a prod & Hypercare | $1,040 | 1 día |
| **TOTAL** | **$11,440** | **11 días** |

✅ El CR es para Ice-Cream (Napolita Pro / Don Pingüino). La etiqueta "HPC" en la tabla de precios fue un typo — ignorar.

**Files:**
- 📎 `Magnum EC _ CR A-4262 - Task 3_ Imagenes POP.pdf` (v2.0, 21/5/2026)
- 📎 `Magnum EC _ Priorización & definición.eml` (22/5/2026)

---

## Tabla de contenidos

1. [Overview de la cuenta](#1-overview-de-la-cuenta)
2. [Bots y workflows (mapa completo)](#2-bots-y-workflows-mapa-completo)
3. [Stakeholders](#3-stakeholders)
4. [Activities por bot (de JSON exports)](#4-activities-por-bot)
5. [Cloud Functions y Lambdas (31 totales)](#5-cloud-functions-y-lambdas-31-totales)
6. [Integraciones externas](#6-integraciones-externas)
7. [Calendario PWST Unilever (descifrado)](#7-calendario-pwst-unilever-descifrado)
8. [AWS EventBridge schedulers](#8-aws-eventbridge-schedulers)
9. [Repos y código](#9-repos-y-código)
10. [Status items del proyecto](#10-status-items-del-proyecto)
11. [Tickets Jira UEICE](#11-tickets-jira-ueice)
12. [Banderas técnicas](#12-banderas-técnicas)
13. [Credenciales y variables clave](#13-credenciales-y-variables-clave)
14. [Stack tecnológico Yalo](#14-stack-tecnológico-yalo)
15. [LangSmith debugging](#15-langsmith-debugging)
16. [Patrones API y código](#16-patrones-api-y-código)
17. [Oris (AI agent — 6 skills)](#17-oris-ai-agent--6-skills)
18. [Recursos y links útiles](#18-recursos-y-links-útiles)
19. [Glosario](#19-glosario)
20. [Preguntas abiertas para Tisi](#20-preguntas-abiertas-para-tisi)
21. [Tribal knowledge gaps](#21-tribal-knowledge-gaps)
22. [Lista accionable para Ale](#22-lista-accionable-para-ale)
- [Apéndice A — Diagrama del ecosistema](#apéndice-a--diagrama-del-ecosistema)
- [Apéndice B — Quick reference de comandos](#apéndice-b--quick-reference-de-comandos)
- [Apéndice C — Crisis Napolita Pro (post-mortem)](#apéndice-c--crisis-napolita-pro-post-mortem)
- [Apéndice D — Threads Slack largos a leer](#apéndice-d--threads-slack-largos-a-leer)

---

## 1. Overview de la cuenta

**Cliente:** Magnum Ice Cream / Unilever Ecuador (correos `@magnumicecream.com`).

**Marcas cubiertas en Ecuador:**

- **Helados Pingüino** (principal — productivo `unilever-ecu-ic-prd`)
- **Magnum** (premium)
- **Napolita** (interno + clientes)
- **Marcas HPC** (Home & Personal Care: Dove, Suave, Rexona, etc.) — `unilever-ecu-hpc-prd`

**Bot principal coloquial:** "Don Pingüino" (`unilever-ecu-ic-prd`).
**Programa Loyalty:** Club Pingüino — partner **Gravty**.

### Scope de Ale (corregido)

> ⚠️ **Corrección crítica:** el bot `unilever-convencion` en la carpeta de Ale **es México (Napolita MX / Heladio Holanda)**, NO Ecuador. La HTML de arquitectura lo confirma. Napolita Ecuador es un bot distinto (`wa-un1912-napolita-ecuador`) cuyo JSON no está en la carpeta.

Ale toma **ambos países** (handover MX paralelo con Tisi):

| Bot | País | Coloquial | En carpeta Ale |
|---|---|---|---|
| `unilever-convencion` | 🇲🇽 México | Napolita MX | ✅ JSON + HTML |
| `unilever_b2b` (Heladio MX) | 🇲🇽 México | Heladio | ❌ (no exportado) |
| `wa-un1912-napolita-ecuador` | 🇪🇨 Ecuador | Napolita EC | ❌ (no exportado) |
| `unilever-ecu-hpc-prd` | 🇪🇨 Ecuador | Don Lever HPC | ✅ JSON + HTML |
| `unilever-ecu-ic-prd` | 🇪🇨 Ecuador | Don Pingüino | ✅ JSON + HTML |
| `Unilever ECU ICE B2B2C` | 🇪🇨 Ecuador | B2B2C cupones | ❌ (no exportado) |

Tisi también le entrega Magnum (LATAM) cross-país.

---

## 2. Bots y workflows (mapa completo)

### Mapa de storefronts MX vs Ecuador

| Concepto | MX | Ecuador | Botslug EC | Presales/Otros |
|---|---|---|---|---|
| **Napolita** (empleados) | `unilever-mx-napolita` | `unilever-ec-napolita` | `wa-un1912-napolita-ecuador` | `napolita-ec-presales`, `napolita-ec-clients`(commerce) |
| **Don Pingüino** (B2B tiendas) | `unilever-b2b-ng` | `unilever-ecu-b2b` | `unilever-ecu-ic-prd` | `unilever-ec-ice-presale` |
| **B2B2C** (consumer final) | — | `unilever-ice-ec-coupons` | *(workflow separado)* | — |
| **Rewards** (catálogo Loyalty) | — | `unilever-ec-ice-rewards` | *(addon en storefront)* | — |
| **Napolita QA test** | — | — | — | `unilever-ec-napolita-test`, `napolita-ec-clients-test` |
| **HPC** | — | `unilever-ecu-hpc` | `unilever-ecu-hpc-prd` | — |

### Workflow URLs (Studio)

| Bot | URL |
|---|---|
| Don Pingüino prod | [`unilever-ecu-ic-prd`](https://platform.yalochat.com/en/c/63ceb43a85a6789106c048b5/b/63cec1bbcdf69d22dd9a7e6e/studio/workflows/63ceb572f81767f745288ec6) |
| Napolita EC prod | [`wa-un1912-napolita-ecuador`](https://platform.yalochat.com/es/c/66956c7c937501141c76b131/b/66956dc4937501225e76b393/studio/workflows/66956dc3e430bb3aedab04fe/activity/66956dc7004bb3c05eebcbe3) · WA: [+593968379276](https://wa.me/593968379276) |
| Napolita EC test | [`unilever-ec-napolita-test`](https://platform.yalochat.com/en/c/63ffa1604e7023200eaa183f/b/665798ed135e953e0ea59870/studio/workflows/665798ecb88a6a8428ad147f/activity/6657992d2b09e8677ba8cd61) |
| B2B2C | [`Unilever ECU ICE B2B2C`](https://platform.yalochat.com/es/c/6792a8285eeecc1536cf8047/b/6792a8861714534b839e9efa/studio/workflows/6792a885c9be7624300269fd/activity/6793ea0b38623e8b26cae352) |

### IDs internos Don Pingüino

- **botId:** `63ceb43a85a6789106c048b5`
- **branch:** `63cec1bbcdf69d22dd9a7e6e`
- **workflow:** `63ceb572f81767f745288ec6`
- **webview:** `https://commerce.yalochat.com/unilever-ecu-b2b/{{sessionId}}`

### Tipos de empleado en Napolita

| Tipo | Acceso menú |
|---|---|
| **WHITE COLLAR** | HR · SSHE · Trade Visit · Innovaciones · Guía ejecución · Hacer pregunta (**SIN Mis tareas**) |
| **WC/COMERCIAL** | Todo WC + **Mis tareas** |
| **BLUE COLLAR** | Igual que WC (sin Mis tareas) |
| **FURIA ROJA** | BC + **Mis tareas** — son los vendedores que toman fotos de cabinets |

**Código empleado:** regex `^\w{4,6}$` (Furia Roja usa letras, otros numérico).

---

## 3. Stakeholders

### Cliente (Magnum Ice Cream / Unilever Ecuador)

| Persona | Rol | Email | Notas |
|---|---|---|---|
| **Pedro Tejena** | Validador / contacto técnico-funcional Magnum | pedro.tejena@magnumicecream.com | Validó Napolita Pro el 13/05. Administra el SharePoint `Proyecto ALEXA -YALO - ICE_BOT`. Aparece en reuniones técnicas con Kevia. |
| **Kevia** *(apellido pendiente)* | PM Unilever — contacto día a día | *(pedir a Tisi)* | **Stakeholder #1 operativo cliente.** Levanta requerimientos, agenda reuniones, hace push comercial en SOWs. |
| **Nicolle** *(apellido pendiente)* | Contacto cliente | *(pedir a Tisi)* | En cadena de correos sobre notificación cumpleaños Loyalty. |
| **Magnum (LATAM)** | Aprobador paso a PROD | *(pedir)* | Aprueba items críticos (ej. Antifraude paso a PROD 1/6). |
| **Gian Franco / Data Analytics (?)** | Data Unilever | *(pedir)* | Posible analista data Ecuador (en MX existe un Gian Franco que arma Power BI). Confirmar. |
| **Contacto IT Unilever EC / Azure** | — | *(pedir)* | Jaider confirmó acceso a Azure pero cliente aún no creó carpetas. Confirmar quién entrega credenciales SAS cada 3 meses. |

### Yalo (equipo de la cuenta)

| Persona | Rol | Email | Notas |
|---|---|---|---|
| **Tiziana Rassow (Tisi)** | PM saliente | tiziana@yalo.com | Último día 22/5/2026. |
| **Alejandra López (Ale)** | PM entrante | maria.lopez@yalo.com | Toma cuenta desde 25/5/2026. |
| **Juan José Ulloa (Juanjo)** | Key Account Manager (KAM/AE) | juan.ulloa@yalo.com | Cierra SOWs (Canal Móvil 27k USD), maneja descuentos con Kevia. Mismo AE que MX. |
| **Daniel Mosquera Maya** | Customer Success Manager II | daniel.mosquera@yalo.com | Lead operativo. Crea PM Assists, sigue tickets, maneja escalaciones Magnum. |
| **Ernesto De La Espriella** | Technical Project Manager | ernesto.espriella@yalo.com | PM técnico — figura en discoveries (Canal Móvil). |
| **Adán Muguiro** | Associate Manager, Engineering (Tech Lead) | adan@yalo.com | Tech lead. Decide arquitectura, gestiona crisis, valida SOWs. **OoO al momento del handover.** |
| **Jaider Panqueva** | Software Engineer (Dev principal) | jaider.panqueva@yalo.com | Dev principal del bot. Implementó Antifraude, hace hypercare Napolita Pro. |
| **Daniel Capelasso Hoffmann** | Solutions Consultant (SC) — SOWs | *(pedir)* | Construye SOWs (Canal Móvil A-3781 27k USD, SFTP→Azure A0LNs00000MjhoyMAB). |
| **JJ Hermosillo (Juan Núñez)** | Solutions Engineer | juan.nunez@yalo.com | Llevó cuenta hasta 3/3, hizo handover a Daniel Capelasso. Hizo discovery inicial Canal Móvil (18/2). |
| **Efrén Ávila** | GTM Innovation Manager | efren.avila@yalo.com | Levanta requerimientos (bandera pago tarjeta, puntos primera compra). |
| **Andrés OG** | Head of FDE | andres@yalo.com | Feedback técnico, mete data a BigQuery. |
| **Erick Martinez García** | UX | *(pedir)* | UX de la cuenta. Co-construye CR Agente Loyalty con Tisi. |
| **Fausto Cortez** | UX (Yalo Pago) | *(pedir)* | Documentó sección Yalo Pago en Notion Tech. |
| **Andrey Cifuentes** | Data Yalo | *(pedir)* | Gestiona integración Gravty para reportería Loyalty. |
| **Pablo Bribiesca** | Data Yalo | *(pedir)* | Data team — procesa data IR + Oris en GCP/BQ. |
| **Aldo Galeotti** | L2 Support | *(pedir)* | Atiende tickets Don Pingüino. |
| **Alfredo Covarrubias** | L2 Support | *(pedir)* | Escala incidentes a Delivery Engineering. |
| **Jair Menezes Vidal** | Soporte / Zendesk | jair@yalo.com | `ZendeskUser=jair@yalo.com` en variables del bot. |
| **Walter Moorlag** | Engineering support | *(pedir)* | Sugirió add logs en debugging Napolita Pro. |
| **Juan Zuluaga (Juanzu)** | Leadership / escalaciones | *(pedir)* | Push de claridad/cumplimiento durante crisis Napolita Pro. |
| **Annette** | SC routing | *(pedir)* | Redirige requests a SC. |
| **Ale Garibotti** | *(rol a confirmar)* | *(pedir)* | cc en SFTP migration thread. |
| **Juli Guerrero** | Data/Billing | *(pedir)* | Reporta costos de notificaciones Meta. |
| **Andrés Alvarez** | Data/Billing | *(pedir)* | cc en threads Loyalty/billing. |
| **Alexander Baquiax** | Voice / VAPI engineering | *(pedir)* | Debug Voice Oris. |
| **Carlos Marin** | Dev | *(pedir)* | Implementó Mis Tareas SOW Furia Roja. |
| **Mau Guillen** | GAM | *(pedir)* | Creador del canal Slack (2023). |

### Partners externos

| Partner | Rol | Contacto Yalo |
|---|---|---|
| **Gravty / LJI** | Plataforma Loyalty (Club Pingüino) | Andrey Cifuentes (gestiona) · Sankalp Mohanty (LJI POC) |
| **Trax** | Image Recognition (cabinets) | Adán Muguiro + Pablo Bribiesca (data) |
| **Magnum LATAM (Bárbara?)** | Aprobador paso a prod | *(confirmar nombre)* |
| **Proveedor APIs Canal Móvil** | Bloqueante UEICE-846 | *(pedir nombre + ETA)* |
| **PWST (PowerStreet)** | Sistema inyección pedidos | — |
| **Kushki** | Payment processor (Yalo Pago) | — |

### Contactos Gravty / LJI (confirmados Slack `#loyalty-lji-yalo`)

| Nombre | Rol |
|---|---|
| **Sankalp Mohanty** | **POC operativo PROD** (el más importante para Ale) |
| **Praneeth Chiravuri** | PM/Lead LJI |
| **Rohit Saxena** | Manager LJI |
| **Om Prakash Sahoo** | Reports / data lead |
| **eashwar** | Tech LJI (APIs, simulation) |
| **Bhanu Prakash** | Tech LJI |
| **Drona Gummalla** | LJI (joined feb 2026) |
| **Nikhil Arepu** | PM LJI |
| **Anwar** | LJI BI |
| **Shyam Shah** | Enablement / pre-sales LJI |
| **Dinesh Kulhari** | SDK / pre-sales |

**Cliente Unilever Ecuador en canal Loyalty:**
- **Ernesto De La Espriella** — PM Unilever EC / Magnum (predominante 2025)

---

## 4. Activities por bot

### 4.1 Don Pingüino (`unilever-ecu-ic-prd`) — 80 activities

**Customer ID:** `unilever-ecuador-ice-cream` · **Componentes totales:** 1,219 · **AI Agents:** 8 · **Integrations:** 16

**Activities críticas (>20 componentes):**

| Activity | Componentes | Flow | Función |
|---|---|---|---|
| Home | 48 | det | Menú principal + delete-profile + Napolita welcome noti |
| Loyalty | 76 | **hybrid** | Club Pingüino completo (enrollment, balance, statement, redenciones, birthday) |
| Make order Preventistas | 34 | det | Pedido para preventa |
| make-order | 35 | det | Pedido para canal regular |
| customer-validation | 36 | det | Validación inicial cliente |
| Commerce Conversional v2 | 31 | det | Suggested order + intelligence triggers |
| YP-Registration v3 | 55 | det | Registro Yalo Pago + KYC |
| YP-Payment v3 | 38 | det | Pagos Yalo Pago |
| YP-Notification v3 | 25 | det | Notificaciones pagos (9 triggers diferentes) |
| YP-PaymentLink v1 | 27 | det | Pago por link |
| YP-Init v3 | 11 | det | Init Yalo Pago |
| send-order | 27 | det | Envío de orden a PWST |
| send-order Preventistas | 28 | det | Envío preventa |
| YF register cabinets | 28 | det | Censo cabinets |
| YF new cab register | 21 | det | Alta nuevo cabinet |
| YF retired cab register | 19 | det | Cabinet retirado |
| YF cab maintenance | 20 | det | Mantenimiento cabinet |
| YF update cab data | 22 | det | Actualizar info cabinet |
| ICE Release Single Order 11/02/2025 | 21 | det | Release individual |
| Sales Agent Preventa | 29 | **hybrid** | LLM agents Preventa (R1 + P1) |
| Sales Agent Regular | 30 | **hybrid** | LLM agents Regular (R1 + P1) |
| Voice Oris Final | 31 | **hybrid** | Voice agent Oris |
| Voice Agent Template | 15 | **hybrid** | Template voice |

**AI Custom Agents en Don Pingüino:**

- **Don Pingüino** (FAQs) — activity `GenAI N-Interactions template` — KB "FAQs Club Pingüino"
- **Sales Agent R1/P1 Preventa** — canal preventistas
- **Sales Agent R1/P1 Regular** — tenderos regulares
- **Sales Agent in Voice 01** (orisVoice) — llamadas salientes, estados: Success/Not Answer/Schedule/Error
- **Support Agent** (Loyalty)
- **Knowledge Genie** (3 variantes: FAQs, FAQs KG 19 sep, KG Faqs 13 sep)

**Triggers (campaigns):**

- `Commerce Conversional v2`: `intelligence_suggested_order_ATC`, `intelligence_personalized_skus`, `Validate Store-Contact Campaign`
- `Loyalty`: `loyalty-campaign-starting`, `loyalty-birthday-points`
- `Pinguino Force`: `Campaign Pinguino Force`
- `Voice Oris Final`: `Voice Campaing Trigger`, `oris-voice-notification-return`
- `YP-Notification v3`: 9 triggers (Approved/Fix Required/Rejected Customer Approvals, Customer Rep Payment Request, Customer Payment Reminders Due/Today/Advance, Manual Customer Approvals, Propaga KYC Completed)
- `NPS Tiendas NG`: `CSAT`

### 4.2 Napolita MX (`unilever-convencion`) — 40 activities

**Confirmado MX** por HTML de arquitectura ("México", "Helados Holanda®"). Componentes totales: 937.

| Activity | Comps | Función |
|---|---|---|
| HSM Trivias | 10 | Trivias |
| Home | 34 | Menú (webhooks `reset-webhook`, `store-analysis-result`) |
| White Collar HR / SSHE / Trade visit | 10/6/5 | Empleados WC |
| Blue collar HR / SSHE | 7/6 | Empleados BC |
| BC Innovations / BC Mis tareas | 4/40 | BC features |
| heladio-support | 61 | Soporte Heladio |
| invoicing-support | 51 | Soporte facturación |
| fridge-support | 107 | **Soporte cabinets (el más grande)** |
| order-issue-support | 114 | **Soporte issues con pedidos** |
| Share fridge photo | 51 | **Foto cabinet + Trax + SharePoint** |
| Make order | 19 | Pedido |
| Genie Trainer Office/Field v1.2.0 | 14/15 | Genie Trainer |
| Napolita White Genie | 9 | **Hybrid** — FAQs custom agent (White) |
| Napolita Blue Genie | 9 | **Hybrid** — FAQs custom agent (Blue) |
| Prospect clients | 78 | **Prospección clientes (la más grande)** |
| Update phone number | 30 | Actualizar teléfono |
| GenAI Intentions | 14 | Clasificador intenciones |
| Business Transformation | 4 | Trigger `BT - Answer` |
| 7-11-survey | 31 | Encuesta hardcoded (archivable?) |
| encuesta 11AGO | 8 | Encuesta hardcoded (archivable?) |

**KB Napolita White:** `knowledge_base_step_1191_-_General_Knowledge`.
**Storefronts:** `unilever-mx-napolita`, `unilever-mx-napolita-clients`, `unilever_b2b`.

### 4.3 HPC (`unilever-ecu-hpc-prd`) — 31 activities

**Brand persona:** "Don Lever". Componentes ~407.

- **Límite:** 5 customer codes por phone (vs 10 en ic-prd)
- **Orders split:** por businessUnit 30/31 antes de PWST
- **Report frequency days:** L/M/X/J/V/S
- **SFTP sync window:** 6:15-17:15 Ecuador Time
- **Catalog AddOn timeout:** 180s

**Activities clave:** `Validacion Cliente`, `Home`, `Bienvenida Trigger`, `TyC`, `send-order` (13), `FAQs (ES)` (hybrid), `make-order` (40), `Pickable promotions`, `Pre Custom Promos`, `Display Custom Promotions`, `CSAT GTM Tiendas Español` (20), `Yalo Force` (17), `MultiSKU`, `Multi ATC`, `Carrusel ATC` (8), `Jorge Bienvenida`, `Oris R1 - P1` (28 comps, hybrid), `Bulk Release Order` (18).

**AI Agent:** `FAQs custom agent HPC Ecuador` ("Don Lever") + `Oris R1 / Oris P1` (Sales Agent).

### 4.4 B2B2C (bot separado)

Bot SEPARADO (NO es Don Pingüino). Storefront: `unilever-ice-ec-coupons`.

**Mecánica:**
1. Cliente final habla con el bot
2. Validación: ¿es tendero? Si sí → redirige a chat tendero
3. Si es consumer válido → menú: Hacer pregunta · Obtener cupón · Localizar tiendas
4. **Obtener cupón:** primera vez pide nombre → crea entry BSNG `prize` con `{code}_{phone}` → lambda `unilever-ecu-ice-generate-qr-code` genera URL QR expirable
5. QR dirige al chat del tendero con mensaje `"Cupón: {coupon code}"`

**Catálogo cupones (CSV en storefront):** `sku` formato `{sequential}_{code}`, `name`, `prizeSku`, `prizeQty`, `startAt`, `endAt`.

**Profile vars:** `qrImage`, `userRealName`, `activePrizeCode`, `activePrizeNumber`, `activePrize`.

**Geolocation lookup:** hasta 5 tiendas en radio de **10 km**.

---

## 5. Cloud Functions y Lambdas (31 totales)

Inventario consolidado de toda la sesión. **~22 con source visible + 9 deployed sin source findable.**

### 5.1 Gravty (10 CFs en `delivery-services/projects/gravty/`)

| CF | Función |
|---|---|
| `gravty-orders-consumer` | Trigger Pub/Sub. CSVs PWST → accruals a Gravty. **UEICE-879 ya mergeado en main** (PR #2809). Retries 3 prod / 5 staging |
| `gravty-points-redeem` | Ejecuta redención (step 1079) |
| `gravty-member-info` | Balance (step 1049) |
| `gravty-member-statement` | Last transactions (step 1051) |
| `gravty-member-enrollment` | Enroll + update `gravtyMemberId` (step 1056) |
| `gravty-member-monthly-spense-status` 🆕 | **Metas segmentadas (item P0 #7) — ya implementado del lado Yalo** |
| `gravty-create-rewards-session-addon` | CREATE_SESSION addon rewards (step 1086) |
| `gravty-display-balance-addon` 🆕 | Inyecta balance como **promo TOTAL ficticia** (ID `000000000000000000000001`) |
| `gravty-catalog-points-filter-addon` 🆕 | Filtra catálogo a productos canjeables con balance |
| `gravty-tenant-reset` 🆕 | Admin tool — invalida cache del tenant en Redis |

**Stack Gravty:**
- Runtime: nodejs24
- Auth: JWT con `x-api-key` + login → token cacheado en Redis
- VPC connector dedicado (`vpc-connector-gravty`), egress `PRIVATE_RANGES_ONLY`

### 5.2 Napolita (8 CFs en `delivery-services/projects/napolita/`)

Todas con nodejs24, service account `tf-cf-napolita-sa@prod-yalo-main.iam.gserviceaccount.com`.

| CF | Función |
|---|---|
| `napolita-task-management` | Lookup customer por code + `diaVisita = getDayLetter()`, devuelve pending tasks del día |
| `napolita-update-task-done` | Marca task done en BSNG (`executedTasksAtStoreInPeriod` + `executedTaskBySeller`) |
| `napolita-update-executed-task-with-trax-kpi` | Update KPI Trax en `executedTaskBySeller.detail.traxKpi` |
| `napolita-pending-task-customers` | Lista furias con tareas pendientes hoy |
| `napolita-generate-seller-goals-progress-report` | Suma daily/monthly progress por seller |
| `napolita-notify-seller-goals-report` | Cron — notifica progreso (param `isDaily`) |
| `napolita-notify-sellers-route-report` | Cron — ruta tiendas pending |
| `napolita-process-seller-goals-from-csv` | Lee CSV goals → BSNG `sellerMonthlyGoals` |

**Trigger external:**
- `YALO_BASE_URL: https://api2-ww-us-001.yalochat.com/` (Notifications API)
- `NOTIFICATIONS_SECRET: napolita-ec-notifications:latest`

### 5.3 Antifraude (1 CF en branch — NO mergeado a main)

| CF | Estado |
|---|---|
| `napolita-generate-ir-webapp-url` | ⚠️ Branch `feature/napolita-generate-ir-webapp-url`, 4 commits Jaider, 21 días sin update |

**Detalle del código (76 líneas):**

```javascript
const ALGORITHM = 'aes-256-gcm'
const IV_BYTES = 12

const encryptParams = (params, baseKeyHex) => {
  const plaintext = new URLSearchParams(stringParams).toString()
  const key = Buffer.from(baseKeyHex, 'hex')
  const iv = crypto.randomBytes(IV_BYTES)
  const cipher = crypto.createCipheriv(ALGORITHM, key, iv)
  const ciphertext = Buffer.concat([cipher.update(plaintext, 'utf8'), cipher.final()])
  const authTag = cipher.getAuthTag()
  // Packed: iv (12B) | authTag (16B) | ciphertext, base64url
  return Buffer.concat([iv, authTag, ciphertext]).toString('base64url')
}
```

**Schema Joi:**

```javascript
const COORDS_REGEX = /^-?\d+(\.\d+)?,-?\d+(\.\d+)?$/

joi.object({
  userId: joi.string().required(),
  channelId: joi.string().required(),
  lang: joi.string().valid('es', 'en', 'pt').required(),
  webhookId: joi.string().required(),
  token: joi.string().required(),
  validateLocation: joi.boolean().default(false),
  storeLocation: joi.string().pattern(COORDS_REGEX)
    .when('validateLocation', { is: true, then: joi.required() }),
  distanceLimit: joi.number().integer().positive()
    .when('validateLocation', { is: true, then: joi.required() }),
})
```

**Env vars y secrets:**

```yaml
# Staging
IR_WEBAPP_BASE_URL: 'https://foto-logv-3-1.replit.app/'
IR_WEBAPP_BASE_KEY: 'napolita-ir-webapp-base-key:latest' (GCP Secret Manager)

# Production
IR_WEBAPP_BASE_URL: 'https://ir-webapp-yalo.replit.app'
IR_WEBAPP_BASE_KEY: 'napolita-ir-webapp-base-key:latest'
```

**Tests:** 10 casos (5 validation + 3 success + 2 error handling).

**Commits del branch:**

| SHA | Fecha | Mensaje |
|---|---|---|
| `926be8e0` | 29/4 18:21 | feat: Napolita \| Add generate-ir-webapp-url CF (**🤖 Co-Authored-By: Claude Opus 4.7**) |
| `231614dc` | 30/4 16:38 | Update YALO_BASE_URL to new Replit app URL |
| `71e16cad` | 30/4 16:40 | feat: add debug logs |
| `535203ab` | 1/5 20:31 | Update YALO_BASE_URL + comment out token in logs |

**🟡 Riesgos pre-merge a prod 1/6:**

1. **¿Webapp prod `ir-webapp-yalo.replit.app` ya tiene descifrado AES-256-GCM?** Si no, el merge no funcionará e2e
2. Doble slash en staging URL (`https://foto-logv-3-1.replit.app//?_c=...`)
3. Co-Authored-By Claude → recomendar code review humano
4. **Branch 21 días sin update** — posibles conflicts con main
5. Confirmar secret `napolita-ir-webapp-base-key` existe en GCP `prod-yalo-main`
6. Backwards compatibility con query strings planos (lado webapp)

### 5.4 Trax (2 CFs en `delivery-services/projects/trax-integrations/`)

- `trax-analize-image` — Envía imagen a Trax (auth OAuth)
- `trax-webhook-analize-image` — Recibe resultados y los persiste/dispara eventos

**Latencia ~2 min** → async (Furia Roja no queda bloqueada, ve análisis en menú aparte). **KPI usado:** `traxResults.kpis.availability.result` → guarda como `traxKpi`. **Event:** `traxImageAnalysis`.

### 5.5 unilever-ecu-ice (4 lambdas en `delivery-integrations/projects/unilever-ecu-ice/`)

| Lambda | Función |
|---|---|
| `catalog-add-on` | PerfectStore filter (3 modos: CATALOG/SEARCH/PRODUCT). Custom fields: `perfectStore`, `distribuidorUL`, `canastillasCabinet` |
| `bulk-upload-cabinets` | SharePoint CSV → BSNG. 2 modos: full-replace (default, peligroso) y `upsertOnly: true` (incremental). Code = `{noplaca}-{IDCLIENTE}` |
| `generate-qr-code` | B2B2C — QR 400px + texto multilinea SVG → S3 `ecommerce-data-files` → signed URL |
| `restart-napolita-tasks` | **Automatiza el runbook manual del Notion.** Reset masivo `goal1=0`, `goal3=0` en Napolita + `currentMonthExecution=0` en clientes |

### 5.6 unilever-ecu-hpc (1 lambda)

- `catalog-add-on` — versión simple (custom fields `sellers` + `businessUnits` 30/31, sin PerfectStore)

### 5.7 Compartidas CPG (deployed sin source findable)

Estas lambdas existen en infra pero su source code NO es findable en `yalochat`:

| Lambda | Función | Compartida con |
|---|---|---|
| `cpg-generics-submit-orders-report-clone` | Reportes diarios/mensuales SharePoint (orders) | Nestlé Venezuela |
| `cpg-generics-submit-events-report` | Reportes diarios cabinets → SharePoint | — |
| `cpg-generics-upload-image-to-source` | Sube imagen a SharePoint | — |
| `cpg-generics-save-event` | Crea evento BSNG (`cabinetsEvent`) | — |
| `cpg-generics-get-delivery-date` | Cálculo fecha entrega | — |
| `pwst-integration-orders-dispatcher` | Inyección PWST | Unilever HPC + ICE |
| `commons-br-sync-multiple-csv-to-headless-merged` | Loader multi-CSV (Prices+Customers+Stock) | — |
| `commons-br-sync-csv-to-headless` | Loader single-CSV (Prices/Promotions) | — |
| `unilever-ice-ecu-generate-distributor-cabinets-report` | Reporte cabinets por distribuidor | — |

**Cómo verlas en runtime:**

```bash
aws lambda get-function --function-name cpg-generics-submit-orders-report-clone \
  --query 'Code.Location' --output text
# Luego curl esa URL para descargar el zip
```

---

## 6. Integraciones externas

### 6.1 PWST (PowerStreet)

Sistema externo del cliente para inyección de pedidos.

- **Activities relacionadas:** `send-order`, `send-order Preventistas`, `Bulk Release Order`, `ICE Release Single Order`
- **Items vivos:** UEICE-871 (Nota de Crédito en Canjes a PWST), bandera pago tarjeta (Efrén Ávila)
- **Calendario:** PWST_CYCLE_ANCHOR = `2025-12-29T00:00:00-05:00` (ver sección 7)

### 6.2 Gravty (Loyalty)

Plataforma de Club Pingüino.

**5 use cases:**
1. **Enrollment** → asigna `gravtyMemberId`
2. **Member Overview** → balance + transacciones
3. **Accrual Simulation** → cálculo antes de fulfillment
4. **Points Accrual** → puntos post-fulfillment
5. **Points Redemption** → uso en webview

**Custom fields del Customer:**
- `isLoyaltyMember = "true"` — flag de elegibilidad
- `gravtyMemberId` — identificador único del miembro

**Tiers (`loyaltyTierGoals`):**
- UN01: goal=1000, reward=10000
- UN02: goal=500, reward=4000
- UN03: *(definido en variable)*

> ✅ **Mapeo de tiers COMPLETO (confirmado en `Base total Customers 2026.csv` + tabla CVA Magnum + call overlap Tisi-Ale 22/5):**
>
> | Código | Nombre Gravty / CVA Magnum | # clientes en Base (May 2026) |
> |---|---|---|
> | **UN01** | ICE_TOP ORO / `1. TOP ORO` | 566 |
> | **UN02** | ICE_ORO / `2. ORO` | 991 |
> | **UN03** | ICE_PLATA+ / `3. PLATA +` | 4,334 |
> | **UN04** | ICE_PLATA- / `4. PLATA -` | 5,910 |
> | **UN06** | ICE_BRONCE / `5. BRONCE` | 23,106 |
> | **UN07** | ICE_LOW BRONCE / `6. LOW BRONCE` | 3,614 |
> | **UN99** | SIN COMPRA / `7. SIN COMPRA` | 441 |
> | | **Total Base Customers** | **38,962** |
>
> *(UN05 no existe — código saltado)*
>
> **Identificación en files de Gravty:** usar el **código** (`UN01`, `UN02`), NO el nombre. Multi-tier: separado por **coma** sin espacio (`UN01,UN02`).
>
> **Ojo:** los 38,962 customers son la base activa (CSV PowerStreet). De estos, ~30,333 están enrolados en Gravty (Loyalty Members). Daniel da seguimiento a desenrolar inactivos para no superar 30k (costo Gravty se eleva > 30k).

**Birthday reward elegibilidad:** solo `ICE_PLATA+`, `ICE_ORO`, `ICE_TOP_ORO` (decisión 9/3/2026).

### Bulk member cancellation (Newton #78604)

**4,244 members cancelados** previo a 19/5/2026. Implicaciones operativas importantes:

**Caveats de la membership stage "Cancelled" en GRAVTY:**

- ❌ Member inactivo en el programa
- ❌ Points balance = 0
- ❌ No puede participar en accrual/redemption activities
- ⚠️ **Member ID retenido en el sistema — NO reutilizable**
- ⚠️ Historical BITs/transactions NO se restauran si re-enrollar

**Re-enrollment (caso por caso):**

- Sí se puede re-enrollar pero:
  1. Se genera **NUEVO Member ID** (External ID puede reusarse)
  2. Historial perdido
  3. Balance arranca en 0
  4. Después se acreditan los puntos requeridos

**Nueva regla operativa (Unilever 21/5):**

> *"Cuando se envíen puntos manualmente y el cliente no esté enrollado, ustedes (LJI) pueden re-enrollarlos y asignar puntos directamente — sin pedir aprobación."*

Ejemplo: Newton #79381 — cliente `ESS4753272_1` re-enrollado + 400 pts (22/5/2026).

### 🚨 CRÍTICO — Disagreement sobre cálculo de puntos (SKU goals)

**Thread:** `#loyalty-lji-yalo` "SKU goals" (30 replies, 10/4 → 20/5/2026, **ACTIVO**)

**El problema:** Gravty calcula los puntos **proporcionalmente** cuando debería ser **threshold-based**.

| Concepto | Cómo lo hace Gravty HOY | Cómo debe ser según Yalo/Unilever |
|---|---|---|
| Lógica | Proporcional: `(qty_recibida / qty_configurada) × reward` | Threshold: solo otorgar reward si qty_recibida ≥ qty_configurada |
| Ejemplo (qty=18, threshold=54, reward=350) | `(18/54) × 350 = 116.67 pts` | **0 pts** (no alcanzó el threshold) |
| Ejemplo (qty=162, threshold=54, reward=350) | `(162/54) × 350 = 1050 pts` (3× reward) | 1050 pts (igual, después del threshold se otorga proporcionalmente) |

**Definición original de Ernesto (cliente):**
> *"Award C points when a user purchases **AT LEAST** D units. After the initial award at D, award C points again for every additional E units purchased"*

**⚠️ ACTUALIZACIÓN (call overlap Tisi-Ale 22/5):** El bug del cliente 30847784 **NO era causado por este disagreement de cálculo**.

- **Root cause REAL según Tisi:** **PowerStreet NUNCA mandó la transacción** que debía generar los 500 pts. Los 13.89 pts venían de otra orden previa. Hasta 28/4 Yalo no había recibido la transacción. Tisi no hizo follow-up con PowerStreet.
- **Reporte original:** Jorge Velasquez en [`#delivery-engineering-support` 14/4/2026 18:35](https://yalochat.slack.com/archives/C07CDFEQJ3T/p1776213315423239) (escaló a Tisi + Jaider)
- **Texto:** *"Se encuentran asignando mal los puntos de Club Pinguino a los clientes por ejemplo el cliente de preventa 30847784 debe de ganar 500 por sanduche TRI sin embargo le dan 13.89 cuando no es la puntuación que debería de asignársele."*
- **Conversación afectada:** [`593994594860`](https://platform.yalochat.com/es/c/63ceb43a85a6789106c048b5/b/63cec1bbcdf69d22dd9a7e6e/conversation-explorer?conversation=593994594860)
- **Acción Ale:** preguntar a Adán si Yalo eventualmente recibió la transacción del cliente

**El disagreement de cálculo de puntos** (proporcional vs threshold) sigue siendo un tema legítimo, pero es **independiente** de este bug. Tisi lo discutió con Sankalp 21/5 y Sankalp aceptó la lógica threshold-based — Gravty está configurando en TEST.

**Status al 20/5/2026:**
- Sankalp pidió call para resolver, Tisi aceptó
- **Sin resolución todavía** — pendiente la llamada

**Sheet de ejemplo de Unilever:** [SKU goals example](https://docs.google.com/spreadsheets/d/17Bf21kusdRw50t2E9VibmsY5YbfaA-dx/edit?gid=1115177452)

**Formato confirmado del nuevo file (28/4):**
- Nueva columna: `Tier`
- Identificación: por código (`UN01`, `UN02`) NO por nombre
- Multi-tier: separado por coma (ej. `UN01,UN02`)

**🎯 ACCIÓN URGENTE PARA ALE:** agendar/retomar call con Sankalp para resolver lógica antes de configurar offers segmentadas en prod.

**Tenants Gravty:**
- UAT: `https://unilever.uso24.gravty.io/`
- PROD: `https://unilever.uso15.gravty.io/`

**Flujo crítico de acumulación:**

```
PowerStreet SFTP (Omnichannel Orders CSVs)
  ↓ [scheduler: unilever-ec-ice-omni-orders]
Integrations SFTP
  ↓ [gravty-orders-consumer CF]
GRAVTY platform (acreditación)
```

**Importante:** **GRAVTY controla** cómo se valoran las orders y se transforman en puntos. Yalo es passthrough.

### Contactos Gravty / LJI (confirmados de Slack `#loyalty-lji-yalo`)

| Nombre | Rol |
|---|---|
| **Sankalp Mohanty** | POC operativo PROD (más mencionado en 2026) |
| **Praneeth Chiravuri** | PM/Lead LJI |
| **Rohit Saxena** | Manager LJI |
| **Om Prakash Sahoo** | Reports / data lead |
| **eashwar** | Tech LJI (APIs, simulation) |
| **Bhanu Prakash** | Tech LJI |
| **Drona Gummalla** | LJI (joined feb 2026) |
| **Nikhil Arepu** | PM LJI |
| **Anwar** | LJI BI |

### Newton (sistema oficial de tickets Gravty)

Desde **18/11/2025** todas las solicitudes a Gravty deben entrar por **Newton** (`newton.gravty.io`). Slack solo para urgencias y comunicación informal.

**Tickets recientes referenciados:** 73137 (ene), 77458 (8/4), **79381** (19/5 — Pinguino Pago acreditación 400 pts, ACTIVO).

**Acceso a Newton:** ✅ Ya concedido a Ale (newton.gravty.io).

### Cadencia mensual establecida

- LJI pide ~día **21-25 del mes anterior** los detalles del **Individualized Offer** + **SKU-Based Offer** del mes siguiente
- Unilever consistentemente entrega tarde (Ernesto y Tisi se disculparon múltiples veces)
- Offer IDs se publican alrededor del último día del mes anterior

### Birthday Reward — config completa

- **Offer ID UAT:** `308749270`
- **Program ID:** `3`, **Sponsor ID:** `2`, **Event ID:** `1` (`birthday_event`), **Subscription ID:** `1`
- **Channel:** PUSH · **Communication:** SYNC
- **Trigger:** diario en GRAVTY → llama YaloChat Notifications API:
  ```
  POST https://api2-ww-us-001.yalochat.com/notifications/api/v1/accounts/unilever-ecuador-ice-cream/bots/unilever-ecu-ic-prd/notifications
  ```
- **Payload:** solo `phone` (formato +593...)
- **Template type:** `puntos_diadecumpleanos_mayo_2026` ✅ **(actualizado 22/5 6:07 CST por Sankalp para incluir imagen)** — anterior: `puntos_diadecumpleanos_enero_2026_v_1`
  - **CURL config:** `{"type": "puntos_diadecumpleanos_mayo_2026", "users": [{"priority": "", "phone": "{{to_number}}", "params": {}}]}`
  - **Categoría Meta (aclaración call 22/5):** ⚠️ **SIEMPRE fue MARKETING** (no UTILITY). Tisi intentó pasar a UTILITY pero Meta no permitió por fines de fidelización. Quedó en Marketing con imagen
  - **Costo actual:** **$1.50 USD por 17 notis** este mes (base ~$0.074/noti). **Pendiente Ale:** calcular costo escalado para meses con más cumpleaños (ej: 20k clientes en marzo) y responder a Ale Magnum con análisis
- **Recompensa:** 1000 puntos
- **Formato DoB:** `DD-MM-YYYY`
- **Tiers elegibles:** ICE_PLATA+, ICE_ORO, ICE_TOP_ORO
- **Pendiente abierto:** bulk update DoB para members existentes (32,805 sin DoB en prod desde 5/3/2026, sin confirmación de cierre)

### GRAVTY 2.12.0 — Migración API individualization

**Cambio breaking (dic 2025 — ene 2026):**
- ❌ DEPRECATED: `GET /v1/members/{member_id}/personalized/{?segments}` (segment-level)
- ✅ NEW: `GET /v1/members/{member_id}/individualized/{?offer_id}` (offer-level)

**Solución elegida (S2 — implementada por Yalo, confirmar con Adán):**
- LJI crea custom attribute `type_of_offer=INDIVIDUALIZED`
- Yalo hace `GET /v1/f/offers/?type_of_offer=INDIVIDUALIZED` para descubrir offer IDs dinámicamente
- PROD upgraded 7/4/2026
- UAT upgraded 21/5/2026 (último release)

**Patrón release Gravty:** martes/miércoles 1:00 AM CDMX, 30 min downtime. UAT primero → sign-off Yalo → PROD ~2-4 semanas después.

### Sheets críticos en Google Drive (configuración mensual)

- **`Configurations Unilever Gravty: pinguipuntos`** — SKUs mensuales que dan puntos
- **`Configurations Unilever Gravty: Individual purchase goals – [MES]`** — metas mensuales (formato Member_ID + purchaseGoal1/2/3 + purchaseReward1/2/3)
- **`Relacion SKUs <> Plataformas`** — mapeo de SKUs

**Input file requisitos:**
- Usar `Member_ID` de Gravty (no External_ID)
- Goals numérico SIN "$" SIN comas
- Members deben estar enrolados antes del cutoff o caen a configuración tier-based

#### 🆕 Cambio en curso: columna `perfectStore` (config tiered SKU goals)

Hasta ahora todos los clientes tenían el mismo threshold para ganar puntos. Unilever pidió diferenciar por tier: que TOP ORO compre más cajas que BRONCE para ganar los mismos puntos.

**Estado (call overlap 22/5):** Gravty configurando en TEST. **Junio: subir archivo "de siempre" SIN columna tier** (config tiered aún no lista). Cuando Gravty avise TEST listo, se prueba.

**Email Gravty pide a Yalo confirmar 3 cosas:**

| Pregunta | Respuesta Yalo |
|---|---|
| Nombre de la nueva columna | A confirmar (screenshot dice `perfectStore`) |
| Separator múltiples targets | **Coma sin espacio**: `UN01,UN02` |
| Identificación por código o nombre | **Código** (`UN01`, no `ICE_TOP ORO`) |

**Ejemplo nuevo formato:**

| MONTH | PLATAFORMA | PUNTOS | A PARTIR | UNITS/PKG | SKU | CATEGORÍA | NAME | VIGENCIA | **perfectStore** |
|---|---|---|---|---|---|---|---|---|---|
| MAYO | MAGNUM | 350 | 5 cajas | 5 | CJS226 | MAGNUM | Magnum Almendras | 1-17 MAYO | `UN01,UN02` |
| MAYO | MAGNUM | 350 | 3 cajas | 3 | CJS226 | MAGNUM | Magnum Almendras | 1-17 MAYO | `UN03,UN04` |
| MAYO | MAGNUM | 350 | 1 caja | 1 | CJS226 | MAGNUM | Magnum Almendras | 1-17 MAYO | `UN06,UN07,UN99` |

**Lógica:** todos ganan los mismos 350 pts, pero tier alto necesita comprar 5 cajas vs tier bajo solo 1.

⚠️ **Importante (call 22/5):** Gravty solo recibía hoy la base "desde 1 unidad". Sankalp inicialmente pensaba dar puntos proporcionales si compra menos del threshold. Tisi explicó la lógica threshold-based correcta: si compra `54 unidades` pero el threshold es `54` → 350 pts; si compra `30 unidades` → 0 pts (no proporcional). Sankalp aceptó la lógica.

**Base Customers actual (CSV PowerStreet 22/5):** 38,962 customers activos, con CVA distribuido así:
- 566 TOP ORO · 991 ORO · 4,334 PLATA+ · 5,910 PLATA- · 23,106 BRONCE · 3,614 LOW BRONCE · 441 SIN COMPRA
- La base se sube continuamente a SharePoint vía integración con PowerStreet

#### Junio 2026 — Misiones plataforma (recibido 26/5 22:09 de Nicolle)

**Sheet master:** [Configurations Unilever Gravty](https://docs.google.com/spreadsheets/d/1jV3zHZpGpyjK0dXH64zg06eXp2Lt8OQ-3wtCoY_DepM/edit?gid=905518213)

**Archivo cliente:** `Rutero Club Pinguino DTs-PVs JUNIO 2026.xlsx` (con pestañas `pinguipuntos` + `Individual purchase goals - JUNIO`)

| Plataforma | Puntos | Threshold | Vigencia | SKUs |
|---|---|---|---|---|
| **CORNETTO** | 350 | 1 caja (22 unid/pkg) | 1-14 junio | Oreo (64934560) · Snickers (62728273) · Clásico (62728263) · Choco Vainilla (62728266) + 3 surtidas (CJS200/204/209) |
| **GEMELO** | 800 | 1 caja (50 unid/pkg) | 1-14 junio | Limón Naranja (69570377) · Chocolate Leche (69570378) + Surtida Limón+Leche (CJS267) |
| **GEMELO Medias Cajas** | 400 | 1 caja (1 unid/pkg) | 1-14 junio | Media caja Limón Naranja (MCJ111) · Chocolate Leche (MCJ112) |
| **POTES CREMOSSITOS** | 100 | 1 Unidad | **15-30 junio** | Chicle (62738915) · Chocolate (68683242) · Frutilla (68683255) · Napolitano (68683254) · Ron Pasas (68683050) · Vainilla (68683250) · Napolitano 750ml (65427095) |

**Particularidad junio:**
- **Primera quincena (1-14):** Cornetto + Gemelo + Medias Cajas
- **Segunda quincena (15-30):** Cremossitos (rotación por categoría diferente)

**Formato:** mismo que mayo (sin columna `perfectStore`/tier — la config tiered arranca julio post-feasibility check de Gravty).

**Pedido extra de Magmun:** actualizar imágenes catálogo + captions en flow Don Pingüino. Desactivar el SKU 1ero (vigente mayo) y arrancar con los nuevos del adjunto.

#### Calendario operativo Gravty (recurrente)

- **Día 21-25 del mes anterior:** LJI pide SKUs + Individualization del mes siguiente
- **Día 25-26:** Magnum (Nicolle/Pedro) envía el rutero formal por email a Yalo + LJI
- **Día 27-31:** Pedro sube ticket Newton con Individualization
- **Día 1 del mes:** activación de nuevas misiones en Gravty

**Junio 2026 desfase:** Sankalp pidió 25/5, Magmun respondió 26/5 22:09 (~1 día tarde). Pedro Tejena debe subir Newton esta semana.

### Blueprint técnico

**SharePoint LJI:** `ljiio.sharepoint.com/.../Unilever GRAVTY Implementation Blueprint_v1.0.docx` (compartido 18/12/2025).

**Support Process Deck:** `LJI GRAVTY Support Processes Overview.pdf` (2/12/2025).

### Cronología de eventos clave Loyalty (Oct 2025 → May 2026)

| Fecha | Evento |
|---|---|
| 27/10/2025 | Set-up Individualization Offer kickoff |
| 29/10/2025 | Bloqueante: 16,029 members sin External_ID mapeado |
| 3/11/2025 | Push to PROD Individual goals |
| 15/11/2025 | Apagar promos Magnum & Cornetto |
| 18/11/2025 | Crisis "URG Accrual Error". LJI mueve soporte a Newton |
| 25/11/2025 | Root cause: ERP PowerStreet no enviaba N órdenes |
| 1/12/2025 | 101 duplicated members encontrados (resuelto 4/12) |
| 11/12/2025 | GRAVTY 2.12.0 UAT — breaking API change |
| 7/4/2026 | GRAVTY 2.12.0 PROD upgrade |
| 14/4/2026 | Bug crítico: cliente 30847784 recibió 13.89 pts vs 500 esperados ([reportado en #delivery-engineering-support](https://yalochat.slack.com/archives/C07CDFEQJ3T/p1776213315423239) por Jorge Velasquez, NO en #loyalty-lji-yalo) |
| 18/5/2026 | Order EGM5301164_1 marzo no localizable (ACTIVO) |
| 19/5/2026 | Newton #79381 — PINGUINO PAGO acreditación 400 pts (ACTIVO) |
| 20/5/2026 | Tisi anuncia handover formal a Ale en canal |
| 21/5/2026 | GRAVTY 2.12.0 UAT upgrade ejecutado |
| **22/5/2026** | **Último día Tisi.** En prod: Fecha Gravty + Nota Crédito Canjes + Bug Napolita Pro + Birthday template imagen + Consolidated Report. SOW Task 3 POP enviado ($11,440). SOW CR2 IR Trax Club Pingüino aprobado. Bulk DoB confirmado hecho. SKU goals disagreement resuelto (threshold-based). **Newton creados:** #79596 YP First TX Reward, #79597 SKU offers by tier. **Épica creada:** UEICE-885 Agente Loyalty |
| **26/5/2026** | **Primera reunión Ale ↔ Trax (Esteban + David) + Magnum (Kevia).** Aclaraciones críticas: (1) Kevia objeta SOW POP como deuda Yalo (no desarrollo adicional), pidió revisar con Juanjo. (2) Trax NO estaba en loop del SOW POP, Esteban quiere alinear con Juanjo antes que se firme. (3) Pedido sugerido movido de Backlog a Discovery porque requiere mejoras en Oris (rotación + frecuencia de compra que hoy no usa). Pendiente: Kevia manda a David el estimado real de imágenes POP por trimestre (~10,670 clientes con tareas, cambio cada 3 meses, no mensual). Confirmado: esquinero SÍ se reconoce, estado del caballete NO. Planimetrías Traditional Trade actualizadas, Modern Trade sin claridad |
| **26/5/2026 (noche)** | **Adán armó doc Notion `Yalo Pago - Magnum Ecuador Integrations`** con decisiones técnicas: (1) Flag PWST = `metodoPago` cash/credit header, default cash, reminder 5 min, persiste en `customFields.paymentMethod`. (2) BIT Gravty YP First TX independiente, dispara desde flow post-Kushki webhook, detección primera TX via endpoint YP + Lua (count `paid` == 0). Estructura BIT diferida a Gravty. (3) Re-enrollment scenario descartado como overkill. **Hallazgo Efrén:** no hay forma de cruzar TX YP con pedidos previos (factura es input manual sin validación + DTs usan flow para cartera vencida). Desbloquea UEICE-893 + UEICE-883 |
| **26/5/2026 22:09 (noche)** | **Nicolle (Magnum) envía rutero junio + SKU goals + Individualization** (respuesta a GR-1 / ping Sankalp 25/5). Misiones plataforma: CORNETTO + GEMELO + GEMELO Medias Cajas (1-14 junio) y POTES CREMOSSITOS (15-30 junio). Formato mayo, sin columna tier (tiered config arranca julio). Pide actualización imágenes catálogo + captions en flow |
| **27/5/2026 mañana** | **Pruebas QA FRIGOS** documentadas por Jaider en Notion. Make Order ✅, Send Order ✅, getOrders ✅. 4 bugs abiertos: catálogo desactivado por default desde SharePoint, precio vacío, error enrolamiento Gravty (permiso GCP secretmanager — ALTA), Invalid content filtro storeCode |
| **27/5/2026 tarde** | **Reunión semanal Magnum-Yalo (Kevia + Nicolle + Carlos + Luis + Daniel M + Ale).** Confirmaciones: Daniel armará reporte Oris desde febrero. Lucas (data Yalo) probando nuevo modelo ML pedido sugerido en preventa. IR en Pedido Sugerido — Daniel asume análisis. SOW POP — Kevia reconfirma "deuda Yalo". Yalo Pago + Flag PWST se unifican en mismo JSON. Tiers Gravty confirmado julio (no junio). Feedback Carlos sobre Consolidated Report: 4 ajustes (doble columna puntos, logic abril/mayo, mil500 vs cero, target debe ser fila no columna). Napolita Perfil Empleados — Capelasso retoma esta semana |

### Threads críticos para Ale (Slack `#loyalty-lji-yalo`)

| Thread | Replies | Latest | Tema |
|---|---|---|---|
| Birthday offer configuration | 58 | 14/4 | Setup completo Birthday Reward |
| Reports (Tisi + Om Prakash) | 50 | 23/4 | Reporting strategy + BI requirements |
| SKU goals | 30 | **20/5** | ACTIVO — SKU goals mensuales |
| Customer 30847784 points bug | 29 | 28/4 | Bug 500 vs 13.89 (probable resuelto) — **thread original en #delivery-engineering-support, no Loyalty** |
| Newton #79381 PINGUINO PAGO | 21 | **21/5** | ACTIVO — Yalo Pago reward |
| Order EGM5301164_1 marzo | 19 | **20/5** | ACTIVO — reconciliación order |
| Birthday Reward (Tisi) | 16 | 10/5 | Update message + remove members |

**CVA tiers Napolita** (probablemente correlacionado con ICE_* tiers — confirmar):
- 1. TOP ORO 🥇
- 2. ORO 🥇
- 3. PLATA + 🥈
- 4. PLATA - 🥈
- 5. BRONCE 🥉
- 6. LOW BRONCE 🥉
- 7. SIN COMPRA 🚫

### 6.3 Trax (Image Recognition)

Análisis de fotos de cabinets vs planograma.

- **Latencia ~2 min** → async (menú aparte)
- Config por customer: `customFields` con planograma correcto
- Resultados → CDP/BSNG/Event `traxImageAnalysis`
- **KPI usado en Napolita Pro:** `traxResults.kpis.availability.result`
- **Incidente histórico crítico:** _incident-5843 (4-5/5/2026, durante crisis Napolita Pro)

#### Tasks Napolita Pro (aclaración Tisi call 22/5)

El flujo Napolita Pro define 4 "Tasks" que el vendedor (Furia Roja) puede ejecutar en cada visita a tienda. Solo **Task 2 está activa hoy**:

| Task # | Qué hace | Status | Notas |
|---|---|---|---|
| **Task 1** | Registrar ubicación tienda (GPS) | ❌ Desactivada | Estuvo activa hasta el censo. Ya tienen coordenadas en sistema |
| **Task 2** | Foto cabinet + análisis IR Trax + planimetría | ✅ **ÚNICA ACTIVA** | El vendedor toma 1 foto. KPI `traxResults.kpis.availability.result` |
| **Task 3** | Foto material POP (afiche caballete + foto esquinero) | 🟡 SOW $11,440 enviado 22/5 | Requiere 2 fotos. Task code Trax: 66. Mejora middleware necesaria (N escenas/sesión) |
| **Task 4** | (uso desconocido) | ❌ No activa | Adán pasó info a Notion. Tisi no recuerda qué hace |

Cómo se asignan las tareas a tiendas: cada cliente tiene un `idPlaneacion` (Task #) en el CSV `Base total Customers 2026`, y una columna `periodicity` (frecuencia visita). El calendario PWST de semanas (`CALENDARIO 2026 SEMANAS NAPOLITA.xlsx`) define qué semana del año aplica cada tarea.

**Bug Napolita Pro May 2026 (resuelto):** el calendario PWST tiene 2 semanas con el mismo número (Mayo arrancó S2 y terminó S2). Jaider lo mapeó por "semana del mes" para evitar confusión. **Prueba de fuego:** junio.

**IR POP Task 3 (UEICE-877 / PM Assist A-4262):**
- Task code: `66` (confirmado por Trax)
- 1 imagen/escena, **2 escenas/sesión** (Task 3 son 2 imágenes POP), 1 sesión/visita
- Trax envía 2 JSON respuestas (live + offline-processed); para POP son iguales (proyecto 100% live)
- Trax NO tiene KPIs para POP — devolverán datos crudos de `recognized items` (igual que SKUs hoy)
- **Estimación final Adán: 8.5d** (subió de 4.5d al descubrir N escenas/sesión) → Tisi dejó **9 días + 1 soporte + paso a prod & hypercare** en el SOW
- **SOW se envía mañana 22/5** (último día Tisi) — [Doc Google](https://docs.google.com/document/d/1jdX4Ylue94-cCABIt2-PKp-eijkgc8xIsPoirT6oF-Y/edit)
- **Mejora middleware necesaria:** middleware actualmente solo soporta 1 escena/sesión → hay que soportar N escenas/sesión (Yalo → Trax)
- **Actividades contempladas:** (1) Config envío imagen + recepción Trax · (2) Ajuste capa intermedia extracción/mapeo · (3) Reporte independiente diario
- **Fuera de scope:** cambios contrato envío, soporte stitching, lógica dinámica Task Code
- **Bloqueante Trax:** Aún NO están listos para recibir imágenes — necesitan flujo estimado para training. Tisi pidió "no envíen mientras no estemos listos"
- **Pendiente Unilever:** definir reporte esperado + (eventualmente) KPIs
- PM Assist creado por Daniel Mosquera (13/5)
- Sources: [Slack thread PM Assist Task 3](https://yalochat.slack.com/archives/C0508K27XHV/p1778686729354329)

#### Update reunión Magnum-Trax-Yalo (26/5/2026)

**Asistentes:** Esteban Ortega + David (Trax) · Kevia López (Magnum) · Alejandra López (Yalo)

**Capacidades técnicas confirmadas por Trax en la reunión:**
- ✅ **Esquinero:** reconocible — tanto `skinner` como `display`
- ❌ **Estado del caballete** (oxidado, dañado): NO está en las capacidades de Trax
- ✅ **Planimetrías Traditional Trade:** actualizadas (sesión David ↔ Pedro Tejena, post-revisión de planimetrías obsoletas detectadas a principios del año)
- ⚠️ **Planimetrías Modern Trade:** pendiente confirmar si están al día (David: "no me queda claro si se cambió o no")

**Estimado de imágenes POP (Kevia calculó en vivo):**
- 42,000 clientes totales en Ecuador
- ~10,670 clientes con tareas activas asignadas
- **Frecuencia de cambio: cada 3 meses (trimestral)** — NO mensual como cabinets
- Excepciones: cada 2 meses si hay innovaciones, ajustes zonales por estacionalidad
- **Sin este estimado, Trax NO arranca training del motor POP** (David tomó como referencia provisional el mismo número de imágenes de productos cabinets)

**Conflicto contractual abierto (descubierto en reunión):**

| Lectura | Argumento |
|---|---|
| **Kevia (Magnum)** | POP es "deuda Yalo" — el alcance original de IR (cuando aceptaron arrancar) incluía POP, no solo cabinets. No debería ser desarrollo adicional facturable |
| **Esteban (Trax)** | "Ahorita acabamos de renovar contrato y NO venía lo del POP" — el contrato renovado Trax↔Yalo no contempla POP |

→ Si ambas partes tienen razón → Yalo queda en el medio (Magnum no paga, Trax sí cobra). **TR-3** en tracking: Ale + Juanjo Ulloa deben revisar ambos contratos antes de cerrar SOW con cliente.

**Aclaración clave de Kevia (reunión 27/5):**
> *"Cuando POP se active en Napolita, automáticamente entra como source para Club Pingüino — todo lo que tengamos de reconocimiento en Napolita también se puede usar como tarea para darle puntos a los clientes. Eso ya quedó establecido con Capelasso."*

→ POP no se duplica desarrollo: una sola vez activado, se reutiliza para Loyalty.

**3 temas activos con Trax (estado post-reunión):**

| # | Tema | Status |
|---|---|---|
| 1 | IR POP Task 3 (UEICE-877) | 🟡 SOW enviado 22/5 — esperando aprobación + resolución contractual |
| 2 | CR2 IR Trax en Loyalty Club Pingüino (A-4043) | ✅ Aprobado por Kevia 22/5 — sin arrancar, ~3 meses ejecución |
| 3 | IR en Pedido Sugerido | 🔍 Discovery (movido de Backlog) — requiere mejoras a producto Yalo (Oris no usa rotación/frecuencia) |

**Personas Trax (contactos):**
- **Esteban Ortega** — comercial/lead (POC principal Magnum)
- **David / Davi** — técnico (training motor, planimetrías)
- **Esteban Paiz** (`esteban.paiz@traxretail.com`) — posiblemente otro comercial para tema contractual

**BiWeekly meeting:** martes con cliente (Kevia + equipo Magmun + Daniel Mosquera + Ale).

#### Test environment FRIGOS (`unilever-ec-ice-frigos-test`)

**URL Commerce QA:** `https://commerce.yalochat.com/unilever-ec-ice-frigos-test/6a15e700c91919ce27d5ccda`

**Pruebas QA documentadas por Jaider en Notion (26-27/5):**

| Funcionalidad | Estado | Notas |
|---|---|---|
| Catálogo productos | ⚠️ Productos llegan desactivados desde SharePoint | Solo "Caja: Crema Real Gigante" activado |
| Precio en tarjeta | ⚠️ Campo `USD$` vacío en vista del catálogo | Bug |
| Make Order (WhatsApp) | ✅ OK | Flow conversacional funciona |
| Send Order (Confirmación) | ✅ OK | Mensaje confirmación "¡Tu pedido fue registrado con éxito!" |
| Registro en API (`getOrders`) | ✅ OK | Pedido queda en backend correctamente |
| Enrolamiento Gravty | ❌ ERROR | Permission `secretmanager.versions.access` denied |

**Test pedido exitoso:** ID `6a15f04bc91919ce27d87941` · customer `FRI30551042_1` · store `FRI_123` · 12 × Caja Crema Real Gigante = $364.32 USD · fecha entrega miércoles 27/5

**Bug crítico (severidad ALTA) — Enrolamiento Gravty:**

```
Error enrolling member: Unable to access secret:
Permission 'secretmanager.versions.access' denied on resource 
(or it may not exist).
```

- **CF afectado:** `gravty-member-enrollment`
- **Region:** `us-east1`
- **Proyecto GCP:** `sta-yalo-main`
- **Logs:** [GCP Cloud Run Logs](https://console.cloud.google.com/logs/query;query=resource.type%20%3D%20%22cloud_run_revision%22%0Aresource.labels.service_name%20%3D%20%22gravty-member-enrollment%22%0Aresource.labels.location%20%3D%20%22us-east1%22?project=sta-yalo-main)
- **Causa probable:** Service account del Cloud Run sin rol `Secret Manager Secret Accessor`
- **Acción:** otorgar `roles/secretmanager.secretAccessor` a la service account en GCP IAM (responsable: Adán)

**Otros bugs abiertos:**
- `Invalid content in variables` al filtrar por `storeCode: FRI_123` en la API (severidad baja)

**Doc completo:** [🧪 Pruebas Frigos — Yalo Test Unilever Ecuador B2B](https://www.notion.so/yalo/Pruebas-Frigos-Yalo-Test-Unilever-Ecuador-B2B-36cd53382b238178958afd38b6a22aac)

### 6.4 Zendesk

- Activity `createZendesk TICKET` (13 comps, en ic-prd)
- Usuario: `jair@yalo.com` (var `ZendeskUser`)
- **Tickets clave históricos:** 52050, 52462, 53004, 53244, 53290, 53369, 54581, 54691, 55091

### 6.5 Folkvangr

- Endpoint: `https://api-staging2.yalochat.com/folkvangr/` (⚠️ apunta a staging en bots prod — bandera #1)
- Token: variable `folkvangr_token`

### 6.6 SharePoint (Magnum)

Path real:

- **Site:** `sites/ProyectoALEXA-YALO`
- **Carpetas:**
  - `/Documentos compartidos/ICE_BOT/Napolita/` — cabinets CSVs
  - `/Documentos compartidos/ICE_BOT/Image_Recognition/` — output Trax
  - `/Documentos compartidos/ICE_BOT/Oris_Report/` — output Oris
  - `/Documentos compartidos/ICE_BOT/Reportes mensuales/`
- **Path local cliente:** `C:\Users\Pedro.Tejena\Unilever\Proyecto ALEXA -YALO - ICE_BOT\`
- **Admin cliente:** Pedro Tejena
- **Auth:** Azure AD OAuth con `clientId`, `clientSecret`, `tenantId`, `resourceId` en GCP Secret Manager

> El path `sites/TelesalesHolanda` en variables del bot convencion es **leftover de MX**, no aplica a EC.

### 6.7 Azure Blob Storage (migración pendiente)

| Item | Valor |
|---|---|
| Storage account | `dbstorageda09p930091adls` |
| Container | `unilever` |
| Folder | `IC_EC/YALO` |
| Key Vault | `bieno-da09-p-930091-kv01` |
| Secret | `SASURL-930091-unilever-racwdl-13` |
| Expiry | 2026-06-30 (rota cada 3 meses) |
| Permisos | `racwdl` (read, add, create, write, delete, list) |
| PM Assist Yalo | [A0LNs00000MjhoyMAB](https://yalo.lightning.force.com/lightning/r/Tech_Assist__c/a0LNs00000MjhoyMAB/view) deadline 21/5 |
| DRI Yalo | Daniel Capelasso |

**SAS Token URI:** `https://dbstorageda09p930091adls.blob.core.windows.net/unilever/IC_EC/YALO?sp=racwdl&sv=2020-12-06&sdd=2&spr=https&sig=5dm9sZcgDI28PtxE4KhzM0QbNdpE%2BGq6VI5CpHa69mE%3D&si=2026-Q2&sr=d`

**Plan Adán:** período de transición duplicando publicación (Azure + SharePoint), migrar progresivamente porque librerías Yalo no soportan Azure nativo. **Magnum aún no creó las carpetas** — sin fecha objetivo cliente.

### 6.8 Big Storage NG (BSNG)

- URL prod: `https://api-global.yalochat.com/big-storage-ng`
- URL staging: `https://api-staging2.yalochat.com/big-storage-ng`
- Token: `bsngToken` (en variables del bot) / `BSNG_TOKEN` (en GCP Secret Manager `bsng:latest`)
- Slug ic-prd + Cabinets: `unilever-ecu-b2b`
- Slug convencion: `unilever` (genérico)

**Records BSNG clave:**

```javascript
// executedTasksAtStoreInPeriod (Napolita Pro)
{ resource: customerCode, resourceId: `${year}-${month}-W${week}`,
  detail: { executedTasks: { [taskId]: {taskId, doneBy, doneAt, frequency} } } }

// executedTaskBySeller (Napolita Pro)
{ resource: sellerCode, resourceId: `${taskId}|${customerCode}|${periodCode}`,
  detail: { storeCode, periodCode, referenceTaskId, traxKpi } }

// sellerMonthlyGoals (mes calendario natural)
{ resource: sellerCode, resourceId: `${year}-${month}`, detail: monthlyGoals }

// cabinetsEvent (Yalo Force)
{ name: 'cabinetsEvent', detail: { idCliente, idVendedor, noPlaca, marca, modelo, ts, ... } }

// prize (B2B2C cupones)
{ resource: customerCode, resourceId: `${code}_{phone}` }
```

### 6.9 Replit IR webapp (Antifraude)

- URL prod: `https://ir-webapp-yalo.replit.app/`
- URL staging: `https://foto-logv-3-1.replit.app/`
- Owner Yalo: Adán Muguiro
- **Compartida Napolita MX y EC** — cambios impactan ambas operaciones

### 6.10 Yalo Pago / Kushki (Payment Links)

**Doc maestro de decisiones técnicas:** [💸 Yalo Pago - Magnum Ecuador Integrations](https://www.notion.so/yalo/Yalo-Pago-Magnum-Ecuador-Integrations-36dd53382b2380888dc9ea3d3c45b61d) (Adán, 26/5/2026)

#### Arquitectura general

- **Procesador de pago:** Kushki (vía Yalo Pago payment links)
- **Activities en bot:** `69696ca25082dd866dc084a9` (Init v3) · `69667b42ca43176c9abe4030` (Payment Link Generation)
- **Steps clave:** #1226 `DEV | Create PL Kushki` · #1230 `WA | Payment Flow` (usa WhatsApp Flows)
- **Equipo Yalo Pago:** Efrén Ávila (lead), Fausto Cortez (dev), Andrés Cano

#### Contexto de uso (Magnum Ecuador)

Lanzamiento piloto **mayo 2026** en 3 distribuidores: **Babahoyo, Distrilópez, Cascal**. Lanzamiento nacional propuesto **junio 2026**.

- **Para qué sirve:** tendero paga la factura con tarjeta crédito/débito vía link en WhatsApp cuando llega el despachador
- **Cuándo pasa el pago:** Día 2 (cuando despachador llega), NO Día 1 (cuando se arma pedido)
- **Para Pedro Tejena (administrador SharePoint Magnum):** asigna manualmente 400 pts a clientes que pagan con YP, mientras se automatiza
- **Para el distribuidor:** el pago YP se contabiliza como "efectivo" en PWST (no es crédito) — para él funciona igual que transferencia

#### Hallazgo crítico (Efrén — 27/5)

**La factura que el cliente paga en YP NO se puede relacionar técnicamente con pedidos previos:**
- Usuario tipea número de factura + monto **manualmente** sin validación (puede escribir 000000 o equivocarse)
- DTs (distribuidores) usan el flow YP también para **recuperación de cartera** (facturas viejas de muchos pedidos atrás)

→ Cualquier reward o reconciliación basada en "primera TX YP" debe ser **independiente** del Spend BIT regular.

#### Decisión técnica — Flag PWST `metodoPago` (UEICE-893)

| Campo | Decisión |
|---|---|
| **Atributo** | `metodoPago` a nivel **header** del payload PWST |
| **Valores** | `"cash"` / `"credit"` |
| **Default** | `"cash"` si usuario no responde en 5 min |
| **Persistencia interna** | `Commerce Order's customFields.paymentMethod` |
| **Flow** | Step nuevo en final confirmation con reminder 5 min |
| **Reference code** | [`delivery-integrations/projects/pwst-integration/scripts/modules/transformers.js#L269`](https://github.com/yalochat/delivery-integrations/blob/main/projects/pwst-integration/scripts/modules/transformers.js#L269) |
| **Step ref (Studio)** | [step #104 actividad `63e349759b28759bfc679838`](https://platform.yalochat.com/en/c/63ceb43a85a6789106c048b5/b/63cec1bbcdf69d22dd9a7e6e/studio/workflows/63ceb572f81767f745288ec6/activity/63e349759b28759bfc679838/step/104) |

**Payload ejemplo (inyección a PWST):**

```json
{
  "tipoDocumento":"FOY",
  "yaloOrder":"EUD_YA01_6a163c8b43da59abba3d892a_UD03",
  "distributorCode":"eud01",
  "clientCode":"4550295",
  "clientSucursal":1,
  "orderDate":"26/05/2026",
  "orderVencimiento":"03/06/2026",
  "vendedor":"UD03",
  "bodega":9101,
  "metodoPago": "cash",    
  "articulos":[
    {"sku":"64777207","cantidad":75,"desc_porc":"0","price_unit":"0.1851"}
  ]
}
```

**Acciones técnicas (Jaider):**
1. Introducir step de selección método pago en flow con reminder 5 min default `cash`
2. Mejorar Lua code para setear `customFields.paymentMethod`
3. Mejorar componente de inyección para mapear `customFields.paymentMethod` → `metodoPago` en payload PWST

#### Decisión técnica — BIT Gravty First TX Reward (Newton #79596)

| Campo | Decisión |
|---|---|
| **Tipo BIT** | Independiente (NO relacionado al Spend BIT) |
| **Razón** | Factura no se cruza con pedidos (ver hallazgo Efrén arriba) |
| **Trigger** | Desde flow al recibir webhook Kushki, solo si "primera TX YP" validada |
| **Estructura BIT** | **Diferida a Gravty** (Sankalp debe proponer) |
| **Detección "primera TX"** | Endpoint Yalo Pago + Lua filter: count transactions con status `paid` == 0 → es primera |
| **Reward** | 400 PingüiPuntos en la primera TX exitosa |

**Endpoint Yalo Pago (Fausto):** existe endpoint que retorna listado de pagos con paginado. Spec a compartir post-ajuste (1 día de trabajo Fausto). Yalo filtra del lado nuestro con Lua + CF.

**Approach genérico:** Adán sugiere considerar estructura reutilizable para futuros rewards YP (no solo el 400 pts inicial).

#### Edge case — Re-enrollment cancelled members (Bulk Newton #78604)

**Descartado como overkill.** Cuando un member cancelado paga YP:
1. En Yalo: `isLoyaltyMember=false` se valida primero → no se hace accrual
2. Aunque forzáramos `isLoyaltyMember=true`, el BIT iría con `gravtyMemberId` antiguo → Gravty rechaza
3. Para "re-enrollar correctamente": wipe `gravtyMemberId` + nuevo enrollment → Gravty daría los 100 pts welcome (no sabe que es re-enrollment)

**Veredicto Adán:** *"Further discussions would be needed with Gravty to figure out the scenario, but feels like overkill"* → cancelled members que paguen YP simplemente NO reciben los 400 pts del reward.

#### Tickets relacionados

| Ticket | Tema | Status |
|---|---|---|
| UEICE-893 | DEV Validar envío a PWST (flag pago) | 🟢 Approach definido (`metodoPago` header) |
| UEICE-883 | DEV Copy YP en flow Don Pingüino | 🟢 Detección "primera TX" definida |
| Newton #79596 | First Transaction Reward (Gravty) | 🟡 Ale respondió 27/5 con propuesta a Sankalp |

#### Step de copy "ganaste 400 pts" (UEICE-883)

**Trigger:** post-Kushki confirmation, antes de regresar a menú principal

**Copy propuesto:**
```
¡Felicidades {{customerName}}! 🎉

Ganaste *400 PingüiPuntos* en Club Pingüino por estrenar Yalo Pago 🎉

Tus puntos se acreditarán en las próximas horas. Puedes ver tu saldo en la opción Club Pingüino del menú.
```

Pendiente: copy oficial del cliente (Efrén/Nicolle).

### 6.11 SAP (Pre-Sales)

Referenciado vía `unilever-ec-presales-sap-orders-report` con SALES_ORG=EC03, DOC_TYPE=ZOR.

### 6.12 Otros

- **CDP / Segment:** `cdp_api_key`, `segmentKey` (en ic-prd)
- **Frontapp:** activity `Frontapp` (5 comps), soporte alternativo a Zendesk
- **OpenAI:** `openAiKey` (Don Pingüino), `openAiKey` + `openAiKeyuni` (convencion — **dos keys, función a confirmar**)
- **S3 base64:** `https://aws.limas.dev/yalo-base64-s3`

---

## 7. Calendario PWST Unilever (descifrado)

Bug UEICE-881 ("Mis clientes con tareas no muestra") tiene causa raíz en este calendario custom.

**Fuente:** `delivery-services/projects/napolita/libs/utils.js`:

```javascript
// PWST_CYCLE_ANCHOR: Monday 2025-12-29 in America/Guayaquil — base Monday
// of the continuous 4-week PWST cycle shared with Napolita.
const PWST_CYCLE_ANCHOR = new Date(process.env.PWST_CYCLE_ANCHOR || '2025-12-29T00:00:00-05:00')

function getWeekOfMonth(dateInput) {
  const diffDays = Math.floor((date - PWST_CYCLE_ANCHOR) / MS_PER_DAY)
  const cycleIndex = Math.floor(diffDays / 28) + 1
  const month = String(cycleIndex).padStart(2, '0')
  const weekIndex = ((Math.floor(diffDays / 7) % 4) + 4) % 4
  const weekOfMonth = weekIndex + 1
  return [year, month, weekOfMonth]
}
```

**Lo que esto significa:**

- **Anchor:** Lunes **29 diciembre 2025**, hora Ecuador (UTC-5)
- **Ciclo:** continuo, **28 días = 4 semanas** exactas
- **"month"** en código y BSNG **NO es mes calendario** — es un **cycleIndex consecutivo** (1, 2, 3, ...)
- **ResourceId BSNG:** `${year}-${month}-W${week}`
- **Configurable:** vía env var `PWST_CYCLE_ANCHOR`

### Tabla de equivalencias

| Fecha real | diffDays | cycleIndex (month) | weekOfMonth | ResourceId BSNG |
|---|---|---|---|---|
| 29 dic 2025 (lun) | 0 | 1 | 1 | `2025-01-W1` |
| 5 ene 2026 (lun) | 7 | 1 | 2 | `2026-01-W2` |
| 26 ene 2026 (lun) | 28 | 2 | 1 | `2026-02-W1` |
| 18 may 2026 (lun) | 140 | 6 | 1 | `2026-06-W1` ← **"S1 de mayo"** según Tisi |
| 25 may 2026 (lun) | 147 | 6 | 2 | `2026-06-W2` |

### Frecuencia format

Cada cliente tiene un campo `customFields.frecuencia`:

```
"S1:2|S2:2|S3:2|S4:2"
```

Parse:

```javascript
// S{week}:{taskId1,taskId2,...}
{ S1: ['2'], S2: ['2'], S3: ['2'], S4: ['2'] }
```

### Para debuggear UEICE-881

El bug del FT01 que no ve sus clientes con tareas en S1 está en uno de:

1. La CF `pending-task-customers` no calcula `getWeekOfMonth` bien para esa hora EC
2. Desync entre `frecuencia` del customer y el cycle real
3. Timezone issue (server vs Ecuador) al calcular `diffDays`

Usar **LangSmith** (sección 15) para ver el trace exacto.

---

## 8. AWS EventBridge schedulers

6 rules identificadas:

| Rule | Propósito |
|---|---|
| `send-csat-notification-unilever-ec` | CSAT notification post-visita |
| `send-customers-data-unilever-ec` | Reporte diario updates customers |
| `unilever-napolita-ec-clients-commerce-sync` | Sync info empleados (customers) |
| `unilever-ice-rewards-catalog-sync` | Sync catálogo rewards |
| `unilever-ec-ice-rewards-report` | Reporte redenciones a Unilever |
| `unilever-ec-ice-omni-orders` | PowerStreet SFTP → Integrations SFTP (Loyalty accrual) |

**Plus** (mencionadas en Tech root pero no verificadas):
- `unilever-ecu-ice-catalogs-update`
- `unilever-ecu-ice-stock-sync` (cada 30min, 5am-5pm EC desde 5/5/2025)
- `unilever-ecu-ice-daily-orders-report`
- `unilever-ecu-ice-weekend-orders`
- `unilever-ecu-ice-daily-order-batches`
- `unilever-ecu-ice-monthly-batches`
- `unilever-ecu-ice-cabinets-report`
- `unilever-ecu-hpc-hourly-batches`
- `unilever-ec-presales-sap-orders-report`
- `unilever-ec-presales-commerce-sync(-staging)`
- `sync-employees-unilever-ec-napolita-staging`
- `sync-goals-unilever-ec-napolita`

**IaC:** [`devops_iac` repo](https://github.com/yalochat/devops_iac/) → path `prod/yalo-main/scheduler/`

---

## 9. Repos y código

### Repos confirmados (4)

| Repo | Contenido |
|---|---|
| `yalochat/delivery-services` | Don Pingüino + Napolita + Trax + Gravty CFs. **Slugs Unilever EC:** `unilever-ecu-ice`, `unilever-convencion`. Plus 76+ projects más cross-cliente |
| `yalochat/delivery-integrations` | Cabinets + B2B2C + Catalog add-ons. **Slug Unilever EC:** `unilever-ecu-hpc`, `unilever-ecu-ice` |
| `yalochat/devops_iac` | Schedulers EventBridge / Pub/Sub triggers / Terraform infra |
| `yalochat/yalocode-skills` | Docs para account managers (referencias docs, **menos actualizado que este .md**) |

### Repos descartados

| Repo | Por qué no |
|---|---|
| `yalochat/trax-integration` (singular) | Middleware nuevo flowbuilder↔Trax — distinto del `trax-integrations` dentro de delivery-services. Útil para futuras mejoras IR POP |
| `yalochat/commercelite-csv` / `csvtool-cli` | Genéricos no-Unilever |
| `yalochat/yalo-pago-report-builder` | Backend YP reports en general |
| `cpg-generics` / `pwst-integration` | **NO existen** como repos. Source code no findable (deployed sin source en yalochat) |

### Branches relevantes

- `feature/napolita-generate-ir-webapp-url` — Antifraude CF (no mergeado, ver sección 5.3)

### Comandos para clonar (sparse-checkout recomendado)

```bash
cd "/Users/maria.lopez@yalo.com/Documents/Unilever Ecuador/repos"

# delivery-integrations
gh repo clone yalochat/delivery-integrations -- --filter=blob:none --sparse
cd delivery-integrations
git sparse-checkout set projects/unilever-ecu-hpc projects/unilever-ecu-ice
cd ..

# delivery-services
gh repo clone yalochat/delivery-services -- --filter=blob:none --sparse
cd delivery-services
git sparse-checkout set \
  projects/gravty projects/napolita projects/trax-integrations \
  docs/Yalo-Context/Commerce docs/Yalo-Context/Delivery \
  docs/Yalo-Context/Oris docs/Yalo-Context/Projects \
  snippets/unilever
```

---

## 10. Status items del proyecto

### 🟢 Recientemente cerrados

| Item | Estado | Notas |
|---|---|---|
| Mejoras Loyalty (parte 1) | 🟢 En prod | Aprobado 14/5, subido a prod 15/5 |
| Napolita Pro | 🟢 En prod | Crisis 1-5/5 (ver Apéndice C). Validado por Pedro 13/5. Estable 14/5. [UEICE-661](https://yalochat.atlassian.net/browse/UEICE-661) |

### ⛔️ P0 — Listo en QA, pausado por cliente

**1. Antifraude — `UEICE-771`**

- **Qué hace:** geolocalización + validación proximidad ≤250m a la tienda
- **Reglas:** GPS obligatorio + distancia tienda-usuario ≤ 250m
- **Dev:** Jaider Panqueva
- **QA listo:** 7/5/2026
- **Paso a PROD:** 1/6/2026 (anunciado por Tisi 18/5)
- **⚠️ Status real del código:** branch `feature/napolita-generate-ir-webapp-url` **NO mergeado a main** todavía. Ver sección 5.3 para riesgos pre-merge
- **Quién lo pausa:** Magnum (espera fecha definida)

**2. Mecánicas promocionales**

- **Estado Slack:** pase a prod estaba programado para **lunes 4/5/2026 7pm**
- **Estado actual cliente:** Unilever pospuso ~Agosto
- *(Pregunta para Tisi: ¿se ejecutó el paso del 4/5 o se canceló antes?)*
- **Caso relacionado (Kevia 30/4):** PSWT puede enviar distribuidor nuevo SIN mecánicas promocionales → quieren operar con CSV como preventa. Pregunta abierta a Adán: ¿nuevo storefront?

### 🔴 P0 — Bloqueado

**3. Digitalización Canal Móvil — COMERCIANTES MÓVILES — `UEICE-846`**

- **Qué es:** digitalizar Canal Móvil Magnum Ecuador (45 frigos + ~800 comerciantes móviles)
- **Hoy:** 100% manual (papel)
- **Discovery:** 18/2/2026, JJ Hermosillo, [GONG](https://us-67466.app.gong.io/call?id=5750547068445393657)
- **Objetivos cliente:** Q2 implementado, H2 operando
- **Scope:** sobre Don Pingüino (NO bot nuevo). Único duro: nuevo storefront (Canal Móvil vende en unidades, no cajas)
- **SOW:** [A-3781 ~27k USD](https://yalo.lightning.force.com/lightning/r/Tech_Assist__c/a0LNs00000Kra9tMAB/view) entregado por Daniel Capelasso
- **Objeción Kevia:** comparó con preventa (14k USD / 120h del año pasado)
- **Extensión Kevia (19/3):** sumar B2B2C (cupones QR para clientes finales) → 2 fases
- **Estado:** Bloqueado hasta APIs del nuevo proveedor
- **Pendiente cierre comercial:** Juan José Ulloa con Kevia

### 🔄 P0 — Work in progress

**4. Envío Nota de Crédito en Canjes a PWST — `UEICE-871`**

- **Qué es:** mandar documento de Nota de Crédito con la inyección a PWST cuando hay un canje
- **Hipótesis técnica:** toca [step 1079 script-confirmacion-canje](https://platform.yalochat.com/en/c/63ceb43a85a6789106c048b5/b/63cec1bbcdf69d22dd9a7e6e/studio/workflows/63ceb572f81767f745288ec6/activity/68d6fad804f5809c984e9f1a/steps/1079) + `gravty-points-redeem` CF + integración PWST

**5. Canal Móvil — FRIGOS — `UEICE-810`**

- **Qué es:** parte del proyecto Canal Móvil para los 45 frigos
- **Activities:** las 7 YF (register, new, retired, maintenance, update, see all, store register)
- **Bug abierto:** `UEICE-816` cliente con múltiples cabinets (Jaider, P3, Backlog)

**6. Consolidación reportes Loyalty — Newton #79299** ✅ **ENTREGADO 22/5/2026**

- **Status (22/5 6:31 CST):** ✅ **DASHBOARD ENTREGADO Y PUBLICADO** — Sankalp Mohanty notificó a Tisi + Ale. Pendiente validación
- **Nombre oficial:** "Consolidated Report" → vista principal: **"Member Transaction & Target Report"**
- **Ticket Newton:** [#79299 — Unilever EC | New dashboard request](https://newton.gravty.io) (creado 15/5)
- **DRI Gravty:** Sankalp Mohanty (LJI) — ya construido y publicado
- **ETA cumplido:** **22/5/2026** (Sankalp lo prometió el 18/5)
- **Members en dashboard:** 30,333 · Última refresh: 21/5 8:34am

**Campos requeridos (12 totales):**

| Campo | Origen |
|---|---|
| External ID | Transactions Report |
| Member Name | Transactions Report |
| BIT Date | Transactions Report |
| BIT Type | Transactions Report |
| Points Earned | Transactions Report |
| Points Redeemed | Transactions Report |
| SKU Code | SKU Level Transaction Report |
| SKU Name | SKU Level Transaction Report |
| Line Quantity | SKU Level Transaction Report |
| Points Earned (SKU) | SKU Level Transaction Report |
| **Goal SO Month** (nuevo) | Monthly Spend Individualized Offer per Client — qué target lograron (Target 1/2/3) |
| **Goal Won Points** (nuevo) | Puntos ganados por ese target |

**Campo extra agregado por Sankalp (no estaba en lista original):**
- **Line Amount** — para mejor visibilidad/contexto a nivel SKU, especialmente para transacciones `PAYMENT_WITH_POINTS`

**Filtros del dashboard (implementados):**
- Member Id · External Id · BIT Date · BIT Id · BIT Type · SKU Code · SKU Name · Goal SO Month

**Estructura/estilo:** Sigue similar al SKU Level Transaction Report existente para consistencia.

**Acción pendiente Ale:**
1. Validar dashboard en Gravty (campos correctos + datos)
2. Compartir con equipo Unilever para UAT
3. Confirmar a Sankalp OK o cambios

**Contexto previo (deprecado):** Originalmente se discutió G-DataConnect con fee (3 opciones planteadas). **La decisión final fue opción 2: Gravty crea el reporte dentro de su plataforma sin fee adicional.**

- **Slack:** [thread New report en #loyalty-lji-yalo](https://yalochat.slack.com/archives/C08LL2PT3QA/p1778678437759669)

**7. Metas segmentadas Target Club Pingüino**

- **Yalo:** ✅ **YA implementado** — CF `gravty-member-monthly-spense-status` soporta personalized offers + segments (modo `offerId + segmentId`)
- **Pendiente:** Gravty configure las personalized offers + segments por tipo de cliente
- **Variable:** `loyaltyTierGoals` (UN01/UN02/UN03)
- **Slack thread:** [hilo Gravty](https://yalochat.slack.com/archives/C08LL2PT3QA/p1775823708495889)

### 🔵 P1 — Backlog

| Proyecto | Status | Notas |
|---|---|---|
| **Image Recognition POP Task 3 (UEICE-877 / A-4262)** | 🟡 SOW se envía 22/5 | Task code 66. **Estimación final 9d + 1 soporte + paso a prod**. Mejora middleware N escenas/sesión. Trax aún sin training |
| **Integrar IR en Pedido Sugerido** | 🔵 Pendiente análisis | Toca `Commerce Conversional v2` + triggers intelligence |
| **Migración SharePoint → Azure Blob** | 🔵 PM Assist Received | DRI: Daniel Capelasso. Plan Adán: período transición. Magnum aún no creó carpetas |

### 🔄 Items adicionales detectados en Slack (no estaban en status original)

| # | Item | Origen | Estado |
|---|---|---|---|
| 8 | Bandera pago tarjeta PWST | Efrén Ávila 19/5 | Solicitud levantada |
| 9 | Confirmación puntos Club Pingüino primera compra con tarjeta (400 pts) | Efrén Ávila 19/5 | Solicitud abierta |
| 10 | Distribuidor nuevo PSWT sin promos | Kevia 30/4 | Pendiente decisión técnica |
| 11 | Cupón B2B2C - Don Pingüino no reconoce | Slack 11/5 (40 replies) | Bug en investigación |
| 12 | Censo Cabinets + Reporte Censo | 18/2 + 11/5 | En curso |
| 13 | Pedido sugerido | thread 24/2 (activo 14/5) | En análisis |
| 14 | Birthday Reward Loyalty | thread 19/5 | Pendiente Meta UTILITY |
| 15 | Generación facturas antes 15/05 | 12/5 (29 replies) | Proceso operativo |
| 16 | Agente Omnipresente alto nivel | 8/5 (26 replies) | Activo |
| 17 | UEICE-879 fecha Gravty | 19/5 | **YA mergeado en main** (PR #2809) |
| 18 | UEICE-881 Mis clientes con tareas | 20/5 | Escalada a Jaider |

### 🆕 Items ACTIVOS en Loyalty (de canal Slack `#loyalty-lji-yalo`)

| # | Item | Status | Detalle |
|---|---|---|---|
| 19 | **Newton #79381 — Cliente ESS4753272_1 400 pts** | ✅ Sankalp re-enrolla 22/5 | Cliente debía 400 pts pero estaba en bulk cancellation Newton #78604 (4,244 members cancelados). Nueva regla Unilever: re-enrollar + asignar sin pedir aprobación |
| 20 | **Yalo Pago first transaction Reward** (NEW request) | 🆕 NUEVO 20/5 | Unilever pide 400 pts por primera tx Yalo Pago. Pendiente: crear Newton ticket + definir flow + estructura BIT + UAT + launch date |
| 21 | **Order EGM5301164_1 marzo** | 🔴 ACTIVO (19 replies, 20/5) | No localizable, reconciliación pipeline PowerStreet → Gravty |
| 22 | **Birthday reward message update** | ✅ EN PROD (22/5 6:07) | Sankalp aplicó: `puntos_diadecumpleanos_enero_2026_v_1` → `puntos_diadecumpleanos_mayo_2026` (con imagen). Pendiente: revisar costo Marketing |
| 23 | **SKU goals — DISAGREEMENT cálculo puntos** | 🚨 CRÍTICO (30 replies, 20/5) | Bug abierto: Gravty calcula proporcional vs threshold-based esperado. Es la causa raíz del bug 30847784 |
| 24 | **Bulk DoB update existing members** | 🟡 Sin confirmación | 32,805 members sin DoB → Birthday Reward no funciona para existentes. Última mención 10/3, sin cierre |
| 25 | **Consolidated Report — Newton #79299** | ✅ Sankalp entrega 22/5 | Dashboard con 12 campos + filtros (Date/Member/SKU/Offer). Decisión final: opción 2 (Gravty interno, sin fee) |
| 26 | **GRAVTY 2.12.0 implementación S2** | 🟡 Verificar | API change segment-level → offer-level. Confirmar con Adán si está completa |
| 27 | **Bulk cancellation Newton #78604** | ✅ HECHO | 4,244 members cancelados. Membership stage = Cancelled. Balance 0. **No reactivables** (excepto re-enroll con nuevo Member ID, sin historial) |

---

## 11. Tickets Jira UEICE

**Tickets vivos (10):**

| Ticket | Tema | Status |
|---|---|---|
| [UEICE-661](https://yalochat.atlassian.net/browse/UEICE-661) | Napolita Pro | 🟢 En prod |
| [UEICE-766](https://yalochat.atlassian.net/browse/UEICE-766) | Metas del mes (referenciado en historia) | — |
| [UEICE-771](https://yalochat.atlassian.net/browse/UEICE-771) | Antifraude | ⛔️ Branch sin merge, paso a PROD 1/6 |
| [UEICE-810](https://yalochat.atlassian.net/browse/UEICE-810) | Frigos (épica) | 🔄 WIP |
| [UEICE-816](https://yalochat.atlassian.net/browse/UEICE-816) | Múltiples cabinets bug | 🔵 P3 Backlog (Jaider) |
| [UEICE-846](https://yalochat.atlassian.net/browse/UEICE-846) | Canal Móvil Comerciantes | 🔴 Bloqueado |
| [UEICE-871](https://yalochat.atlassian.net/browse/UEICE-871) | Nota de Crédito Canjes | 🔄 En desarrollo |
| [UEICE-877](https://yalochat.atlassian.net/browse/UEICE-877) | Trax POP Task 3 | 🟡 SOW enviado 22/5 — $11,440 / 11d |
| [UEICE-885](https://yalochat.atlassian.net/browse/UEICE-885) | Agente Loyalty (épica) | 🔄 Tisi creó épica EOD 22/5. Pulir CR + alinear Magnum |
| [UEICE-879](https://yalochat.atlassian.net/browse/UEICE-879) | Fecha createdAt a Gravty | ✅ **Mergeado main** (verificar deploy) |
| [UEICE-881](https://yalochat.atlassian.net/browse/UEICE-881) | Mis clientes con tareas (S1) | 🔴 Escalada a Jaider |

**PM Assists / Salesforce:**

- [A0LNs00000MjhoyMAB](https://yalo.lightning.force.com/lightning/r/Tech_Assist__c/a0LNs00000MjhoyMAB/view) — SFTP→Azure (deadline 21/5, DRI Daniel Capelasso)
- [A0LNs00000Kra9tMAB](https://yalo.lightning.force.com/lightning/r/Tech_Assist__c/a0LNs00000Kra9tMAB/view) — SOW Canal Móvil A-3781
- [A0LNs00000KklTRMAZ](https://yalo.lightning.force.com/lightning/r/Tech_Assist__c/a0LNs00000KklTRMAZ/view) — A-3760 Canal Móvil intake
- [A0LNs00000MmAurMAF](https://yalo.lightning.force.com/lightning/r/Tech_Assist__c/a0LNs00000MmAurMAF/view) — A-4262 IR POP

---

## 11.5 Tickets Newton (Gravty / LJI)

**Newton** es el sistema oficial de tickets de Gravty/LJI (`newton.gravty.io`). Desde 18/11/2025 es el canal oficial, NO Slack. Ale tiene acceso desde 26/5/2026.

### Newton tickets vivos (mayo 2026)

| Newton # | Tema | Status | Owner LJI | Detalle |
|---|---|---|---|---|
| **#78604** | Bulk member cancellation | ✅ HECHO | LJI ops | 4,244 members cancelados previo a 19/5. Stage `Cancelled`, balance 0, no reactivables |
| **#79299** | Consolidated Report (Dashboard "Member Transaction & Target Report") | ✅ Entregado 22/5 + 4 ajustes pendientes | Sankalp Mohanty | Decisión final: opción 2 (Gravty interno, sin fee). 30,333 members. Feedback Carlos: doble columna puntos, logic abril/mayo, mil500 vs cero, target debe ser fila no columna |
| **#79381** | Cliente ESS4753272_1 (400 pts) | ✅ Hecho 22/5 | Sankalp | Cliente bulk cancelled re-enrollado + acreditados 400 pts. Nueva regla Unilever: re-enrollar manual sin pedir aprobación |
| **#79596** | Yalo Pago First Transaction Reward (400 pts) | 🟡 Ale respondió 27/5 con propuesta Adán | Sankalp | Approach: BIT independiente, dispara desde flow post-Kushki webhook, detección "primera TX" via endpoint YP + Lua. Estructura BIT diferida a Gravty |
| **#79597** | SKU offers by tier | 🟡 Gravty config TEST | Sankalp | Feasibility from July onwards. Junio en formato mayo (sin columna tier). Julio = primer mes tiered con `perfectStore` |

### Cadencia operativa con Newton

- LJI prefiere Newton sobre Slack desde nov 2025
- Yalo levanta tickets cuando hay un requerimiento técnico/operativo
- Sankalp es el POC principal de operación, Praneeth (PM Lead LJI), Rohit (Manager), Om Prakash (BI/Reports)

---

## 11.6 Trade Visit Backlog — minuta 20/5 (recibida 27/5)

**Origen:** Reunión interna Magmun del 20/5/2026 (Digital Trade Visit). Kevia envió formalmente la minuta el 27/5 vía email a Ale + Daniel Mosquera + Pedro + Nicolle + equipos Magmun + Promostock.

**Deadline propuesto para todos los items:** próximo QBR (septiembre 2026, aunque el documento dice "2025-09-26" — typo).

**Total: 33 action items** divididos en 3 áreas.

### Don Pingüino / Oris (12 items)

| # | Item | Owner | Categoría |
|---|---|---|---|
| 1 | Oris omnipresente en todos los flujos (sesiones inter-agénticas) | Daniel M | Mejora |
| 2 | Enviar factura el día de distribución / previo a entrega / consulta cliente | Pedro T | Feature |
| 3 | Entrenar Oris con sinonimias: "Polito fresa = Polito frutilla" | Daniel M | Training |
| 4 | Bug: Oris respondió en portugués en ruta a cliente | Daniel M | Bug |
| 5 | Notificación cuando cliente confirma pedido SIN acceder a promo dropsize | Daniel M | UX |
| 6 | Alerta webview con popup "cuánto falta para escala dropsize" | Daniel M | UX |
| 7 | Análisis de aceptación pedido sugerido **por CVA** | Pedro T | Análisis |
| 8 | Mejoras pedido sugerido: foto napolita, rotación SKU, sugerido por tamaño cliente | Daniel M + Kevia | Mejora *(= P3 + IR Pedido Sugerido)* |
| 9 | Compartir benchmark efectividad sugerido | Daniel M | Reporte |
| 10 | Definir target efectividad pedido sugerido | Kevia L | Definición |
| 11 | Reporte consolidado clientes agénticos para medir adopción IA | Daniel M + Kevia | Reporte *(= KV-1)* |
| 12 | Incentivar aceptación pedido sugerido otorgando puntos Club Pingüino | Nico M | Feature |

### Club Pingüino (10 items)

| # | Item | Owner | Notas |
|---|---|---|---|
| 13 | **Agentizar Club Pingüino** | Daniel M | ✅ Ya en marcha como `UEICE-885` (Custom Agent Loyalty CR v2.3 en Notion) |
| 14 | Analizar rutas de ventas por DT con oportunidad en canjes | Nico M | |
| 15 | Mejoras webview canjes + link directo + comunicación top canjes con botón directo | Nico M + Daniel M | |
| 16 | Mejorar flujo dentro del menú Club Pingüino | Daniel M + Kevia | |
| 17 | Campañas trimestrales para motivar canje | Nico M | |
| 18 | Consultar estatus entrega de premios canjeados | Kevia L | |
| 19 | Encuesta cliente post-entrega premio (validar PWST estatus) | Nico M + Kevia | |
| 20 | Reforzar comunicación cumplimiento cuota mensual | Nico M | |
| 21 | Incluir en forms trade visit "medio por el que aprendió Club Pingüino" | Pedro T | |
| 22 | Opción reportar novedad con canje + reporte diario | Daniel M + Pedro T | |

### Napolita (11 items)

| # | Item | Owner | Notas |
|---|---|---|---|
| 23 | Selección automática clientes con tareas en flow | Daniel M | |
| 24 | Optimizar flujo para hacer la tarea | Daniel M + Pedro T | |
| 25 | Bug: Imagen negra al tomar foto (furia debe abrir desde navegador) | Daniel M | 🐛 Severidad alta |
| 26 | Bug: No muestra foto guía ejecución congelador antes del resultado | Daniel M + Pedro T | 🐛 Severidad media |
| 27 | Bug rutero dinámico: semana visita con más clientes pero no salían todos | Daniel M + Pedro T | 🐛 *Posible mismo bug del calendario PWST cerrado 22/5 — confirmar con Jaider* |
| 28 | Validar tiempo promedio ejecución (caso 25 min) y clientes/día (70 Distrilópez) | Jordan M | |
| 29 | Conectar foto levantada en tarea con Don Pingüino y sugerido | Daniel M | *(= IR en Pedido Sugerido / TR-2)* |
| 30 | Evaluación objetivos cumplimiento tareas del mes (brechas) | Jenniffer T | |
| 31 | Retos de ejecución a clientes en Club Pingüino para auto-ejecución | Nico M + Kevia | |
| 32 | Viabilidad permitir furia hacer tarea fuera de semana (casos excepcionales) | Pedro T + Kevia | |
| 33 | Revisar medición evaluación foto (novedades en evaluación) | Pedro T | |

### Distribución de owners

| Persona | # items |
|---|---|
| Daniel Mosquera (Yalo CSM) | 16 items |
| Pedro Tejena (Magmun) | 7 items |
| Kevia López (Magmun) | 7 items |
| Nicolle Medina (Magmun) | 7 items |
| Jenniffer T. (Magmun) | 1 item |
| Jordan M. (Magmun) | 1 item |

**Acción Ale:** sesión con Daniel Mosquera para revisar los 16 items que tiene asignados y priorizar/delegar antes del QBR.

---

## 11.7 Gap contractual POP (descubierto 26/5)

### Contexto

En la reunión Magmun-Trax-Yalo del 26/5, se descubrió un **gap contractual** entre los 3 contratos:

| Lectura | Posición |
|---|---|
| **Kevia (Magmun)** | "POP estaba implícito en el contrato original de IR con Yalo, no es desarrollo adicional" |
| **Esteban (Trax)** | "Nuestra renovación reciente Trax↔Yalo NO contempla POP" |
| **Yalo (Ale)** | Necesita revisar ambos contratos antes de cerrar |

### 4 escenarios posibles

| Escenario | Yalo↔Magmun incluye POP | Yalo↔Trax incluye POP | Implicación |
|---|---|---|---|
| 1 | ✅ Sí | ✅ Sí | No hay SOW. Yalo paga a Trax internamente |
| 2 | ✅ Sí | ❌ No | Yalo necesita addendum con Trax. Yalo come costo Trax. **⚠️ Worst case para Yalo** |
| 3 | ❌ No | ✅ Sí | SOW válido para Magmun ($11,440). Trax cubierto |
| 4 | ❌ No | ❌ No | SOW válido para Magmun + Yalo necesita addendum Trax |

### Acción

**TR-3 en tracking:** Ale + Juanjo Ulloa deben revisar **ambos contratos** (Yalo↔Magmun original + Yalo↔Trax renovación reciente) antes de regresar a Kevia con respuesta sobre el SOW POP.

**Equipo involucrado:**
- Juanjo Ulloa (KAM Yalo) — contratos comerciales
- Daniel Capelasso (SC) — alcance SOWs
- MT (Yalo partnerships) — relación con Trax
- Santiago Copiano (mencionado por Ale en alineamiento interno)

---

## 12. Banderas técnicas

| # | Bandera | Resolución |
|---|---|---|
| 1 | `folkvangr_url_prod` apunta a `api-staging2.yalochat.com` en bots prod | ⚠️ Abierta — preguntar a Adán si Folkvangr nunca movió a prod o es bug |
| 2 | Webhook Mondelez Perú en convencion (`mo1561-mondelez-peru-b2b`) | ⚠️ Abierta — leftover de plantilla o uso real |
| 3 | Storefronts MX en convencion (`unilever-mx-napolita`) | ✅ **Resuelta:** convencion ES MX, NO Ecuador |
| 4 | Sharepoint `sites/TelesalesHolanda` | ✅ **Resuelta:** path real es `sites/ProyectoALEXA-YALO`. TelesalesHolanda es leftover MX |
| 5 | Activities con nombre genérico en ic-prd (`new-activity-71/72/82`, `second test`, `Step-test`, `NO ACTIVITY`, `Thank You`) | ⚠️ Abierta — preguntar archivables |
| 6 | Encuestas con fecha hardcoded (`Encuesta Julio 2024`, `CSAT 2024`, `Encuesta 15 de mayo`) | ⚠️ Abierta — preguntar archivables |
| 7 | `CSat Napolita` en ic-prd (¿por qué Napolita está en bot helados?) | ⚠️ Abierta |
| 8 | 3 activities Yalo Force similares (`Yalo Force March 2024`, `Yalo force & cabinets`, `Pinguino Force`) | 🟡 Cabinets viven en Don Pingüino, no Napolita. Probable evolución histórica |
| 9 | Webhook `Update-catalogues-ice-presale` URL vacía | ⚠️ Abierta |
| 10 | Dos variables OpenAI (`openAiKey` vs `openAiKeyuni`) en convencion | ⚠️ Abierta |
| 11 | CF calls = 0 en JSON summary | ✅ Falso — hay 31 CFs reales (Anexo J inventario) |
| 12 | **SharePoint clientSecret en cleartext en Notion Tech root** | ⚠️ **SECURITY** — debería removerse del Notion (el código ya maneja vía GCP Secret Manager) |

---

## 13. Credenciales y variables clave

### Don Pingüino (`unilever-ecu-ic-prd`)

```
hourDeadLine=18:00
folkvangr_url_prod=https://api-staging2.yalochat.com/folkvangr/  ⚠️ STAGING
storefrontNamePresale=unilever-ec-ice-presale
cdp_api_key=***
mtoName=Monthly Amount Spent
currencyCode=USD
workflowName=unilever-ecu-ic-prd
folkvangr_token=***
yfRegistryTemplate=hsmdcyaloforce8july2024v3
yfjNotificationToken=***
ZendeskUser=jair@yalo.com
loyaltyTierGoals={"UN01":{"goal":1000,"reward":10000},"UN02":{"goal":500,"reward":4000},"UN03":{...}}
yfjRegistryTemplate=hsminvitacion24july2024v1
bsngToken=***
openAiKey=***
storefrontNameLoyalty=unilever-ec-ice-rewards
webviewUrl=https://commerce.yalochat.com
segmentKey=***
```

### Napolita MX / convencion (`unilever-convencion`)

```
agendaURL=https://drive.google.com/file/d/1QoVc2p1oMtCJCKMwx7WSQD4NKdhQ5Sd0/view
storefrontName=unilever-mx-napolita  ⚠️ MX
botSlug=unilever-convencion
bsngBotSlug=unilever
botSlugUnileverB2B=unilever_b2b
storefrontNameUnileverB2BNapolita=unilever-mx-napolita-clients
sharepointPath={"site":"sites/TelesalesHolanda","upload":"Documentos compartidos/Yalo"}  ⚠️ Leftover
openAiKeyuni=***
openAiKey=***
sendPromoToHeladioCustomerURL=https://api2-ww-us-001.yalochat.com/notifications/api/v1/accounts/unilever_b2b/b
```

### HPC (`unilever-ecu-hpc-prd`)

```
storefrontName=unilever-ecu-hpc
workflowName=unilever-ecu-hpc-prd
timeZone=America/Guayaquil
maximumRetry=3
clientLimitPerUser=5  ← límite de customer codes
orisUp=true
folkvangr_url_prod=https://api-staging2.yalochat.com/folkvangr/  ⚠️ STAGING
CommonsCloudFunctionsBaseUrl=https://us-east1-prod-yalo-main.cloudfunctions.net
```

### Antifraude / Replit webapp

```
ALGORITHM=aes-256-gcm
IV_BYTES=12 + AuthTag 16B + ciphertext, base64url
IR_WEBAPP_BASE_KEY=napolita-ir-webapp-base-key:latest (GCP Secret Manager prod)
IR_WEBAPP_BASE_URL prod=https://ir-webapp-yalo.replit.app
IR_WEBAPP_BASE_URL staging=https://foto-logv-3-1.replit.app/
distanceLimit=250 (metros, default)
```

### Napolita Pro (delivery-services config.yaml)

```
GCP_BUCKET_NAME=prod-yalo-main-us-delivery-commerce-data
GCF_SELLER_GOALS_PROGRESS_REPORT_URL=https://us-east1-prod-yalo-main.cloudfunctions.net/napolita-generate-seller-goals-progress-report
GCF_PENDING_TASK_CUSTOMERS_URL=https://us-east1-prod-yalo-main.cloudfunctions.net/napolita-pending-task-customers
YALO_BASE_URL=https://api2-ww-us-001.yalochat.com/
PWST_CYCLE_ANCHOR=2025-12-29T00:00:00-05:00
NOTIFICATIONS_SECRET=napolita-ec-notifications:latest
```

### Delivery-integrations (`unilever-ecu-ice`)

```
BSNG_URL=https://api-global.yalochat.com/big-storage-ng/api
SHAREPOINT_BASE_URL=unilever.sharepoint.com
SHAREPOINT_SITE_PATH=sites/ProyectoALEXA-YALO
HEADLESS_PAGE_SIZE=100
STOREFRONT_NAME_NAPOLITA=unilever-ec-napolita
STOREFRONT_NAME_NAPOLITA_CLIENT=napolita-ec-clients
AWS_BACKUP_BUCKET=ecommerce-data-files
REGION=us-east-1
```

**Secretos SSM:** `SHAREPOINT_CLIENT_ID`, `SHAREPOINT_CLIENT_SECRET`, `SHAREPOINT_RESOURCE_ID`, `SHAREPOINT_TENANT_ID`, `BSNG_TOKEN`, `HEADLESS_ADMIN_URL`, `HEADLESS_USER_URL`.

### Azure Blob (migración pendiente)

Ver sección 6.7.

---

## 14. Stack tecnológico Yalo

### delivery-services (monorepo principal)

- **76+ client projects** + 49 shared en `delivery-common`
- **Languages:** JS primary (CommonJS), TS growing (esbuild + tsc), Go selective
- **Runtime:** `nodejs24`, `go123`
- **GCP projects:** `sta-yalo-main` (staging), `prod-yalo-main` o `yalo-production-env`
- **Region:** us-east1
- **Build:** esbuild para JS/TS, `go mod tidy` para Go
- **Frameworks:** GCP Cloud Functions (Gen 2)
- **CI/CD:** GitHub Actions, **1 project per PR** (constraint importante), staging on PR, prod on merge to main
- **npm internos:** `@engyalo/delivery-integrations-libs`, `@engyalo/headless-sdk`, `@engyalo/commerce-delivery-addons`
- **Naming:** project = `<client>-<country>`, function = kebab-case, deployed = `<project>-<function>`
- **Schema fields:** camelCase (incluso si API externa usa snake_case)

### delivery-integrations

- Lambdas AWS (no GCP)
- Region: us-east-1
- Backup bucket: `ecommerce-data-files`
- Runtime: nodejs (no específico nodejs24)
- Build: esbuild + serverless framework
- Pattern: SFTP / SharePoint → Commerce Admin bulk

### Patrón canónico de handler

```js
exports.handler = async (req, res) => {
  try {
    const { error, value } = schema.validate(req.body)
    if (error) return res.status(422).send({ success: false, message: error.message })
    const result = await doSomething(value)
    return res.status(200).send({ success: true, data: result })
  } catch (error) {
    logger.error(error)
    return res.status(error.statusCode || 500).send({ success: false })
  }
}
```

### Config hierarchy (YAML)

`Root config.yaml → Project config.yaml → Function config.yaml` (merged con `yq`).

### Comandos locales

```bash
make run project=<p> functionName=<f>       # local run
make watch project=<p> functionName=<f>     # hot-reload
npm run check                               # lint TS
npm test                                    # jest
npm run test:<project>                      # scoped tests
```

### delivery-common (49 CFs cross-cliente)

Útiles para futuras necesidades: `sync-commerce-entity` · `send-order-producer` · `alert-producer` · `catalog-build-producer` · `csv-to-cdp-contacts` · `validate-commerce-entities` · `email-notification` · `handle-pubsub` · `handle-redis` · `handle-sftp` · `claude-log-reader` · `claude-oncaller` (AI-powered on-call).

### Petco MX como referencia gold-standard

CLAUDE.md de delivery-services lo menciona como modelo a copiar para nuevas integraciones. TypeScript primary, 6 CFs proxy entre Studio Tool Steps y APIs cliente.

---

## 15. LangSmith debugging

Plataforma Yalo para debuggear traces de Oris/agents.

### Projects (por env)

| Env | LangSmith Project |
|---|---|
| Production | `ml-gai-service-production` |
| Staging | `ml-gai-service-staging` |
| Dev | `ml-gai-service-dev` |

### Timezone

UTC stored. MX = UTC-6 (no DST since 2022). **Ecuador = UTC-5.**

### Filter DSL (ejemplo Don Pingüino)

```
and(
  eq(is_root, true),
  eq(name, "order_taking_skill"),
  and(
    and(eq(metadata_key, "workflow_name"), eq(metadata_value, "wa-un1912-napolita-ecuador")),
    and(eq(metadata_key, "workflow_user_id"), like(metadata_value, "%593969630279%"))
  )
)
```

### Comando CLI

```bash
langsmith trace list \
  --project ml-gai-service-production \
  --filter '<DSL filter>' \
  --since "2026-05-20T00:00:00Z" \
  --limit 100 \
  --full \
  --format pretty
```

### Para debuggear UEICE-881

Filtrar por:
- `workflow_name=wa-un1912-napolita-ecuador`
- `workflow_user_id=<phone del FT01>` (ej. `%593969630279%`)
- `name=order_taking_skill`

Eso te muestra el trace exacto que decidió no había clientes con tareas. **Útil para Jaider.**

### Workaround para límite de 100 resultados

Filtrar por `--name order_taking_skill` (ignora intent_classifier, skill_classifier, guardrails, locale_identifier, silence_filler).

### Categorías de `search_match_products_tool`

| Category | Significado |
|---|---|
| `single_option_products` | Exact match, 1 producto |
| `multiple_options_products` | Exact match, varios productos |
| `unavailable_products` | En catálogo pero **out of stock** |
| `alternatives_products` | No exact — alternativas low-confidence (brand-level match) |

---

## 16. Patrones API y código

### 3 patrones de auth para wrappers a APIs externas

| Pattern | Cuándo usar | Secret JSON shape |
|---|---|---|
| **API Key en headers** | Auth fijo via header | `{ apiKey }` |
| **Login Bearer Token** | Token short-lived | `{ baseUrl, email, password }` |
| **Basic Auth** | HTTP Basic | `{ username, password }` |

### Convenciones

- Todas las creds en GCP Secret Manager como JSON string
- `HttpError` class personalizada por proyecto
- Timeouts: 45s GET / 15s login / 30-120s writes
- **CF timeout > axios timeout** (default repo 3600s)
- Response validation siempre
- Reuse: misma base URL → add method, distinto API → crear nuevo wrapper

### Frecuencia format Napolita

```
"S1:2|S2:2|S3:2|S4:2"
// → { S1: ['2'], S2: ['2'], S3: ['2'], S4: ['2'] }
```

### PerfectStore segmentation grammar (ICE catalog add-on)

```
EHU-ELR-EFP-EGI-EDU-EVA-EPV:0-2-3-4-5-6-7-8-9|EQR-EPR-EFM-EPM:4-5-6-7-8-9
```

Format: `distrib1-distrib2-...:canastilla1-canastilla2-...|...`. Ambos (`distribuidorUL` Y `canastillasCabinet`) deben matchear en mismo segmento.

### Coupon code format (B2B2C)

`{folio}_{userId}` (ej. `60914_34678113002`).

### Cabinet code format (delivery-integrations)

`{noplaca}-{IDCLIENTE}` (uppercase).

### PWST Order Injection contract

`tipoDocumento: COY`, `yaloOrder: EIA_YA01_{orderId}_{vendedor}`, distributorCode, clientCode, clientSucursal, orderDate, orderVencimiento, vendedor, bodega, articulos[{sku, cantidad, desc_porc, price_unit}].

### Pre-Sale SAP Report schema

`PURCH_NO: YALO-{seq}`, `PARTN_NUMB`, `PARTN_ROLE=AG`, `SALES_ORG=EC03`, `DISTR_CHAN=01`, `DIVISION=11`, `DOC_TYPE=ZOR`, `TARGET_QU=CS`.

---

## 17. Oris (AI agent — 6 skills)

Oris es un AI agent que se alimenta de Commerce vía Headless APIs. **Multimodal:** text + images + audio.

| Skill | Función |
|---|---|
| **Order taking** | Add to cart, update quantities, clear cart, confirm order, search products |
| **Suggested order** | Genera pedido sugerido (ML recommendations + previous orders) |
| **Upsell** | Ofrece upsell post-confirmación |
| **Followup** | Mensajes de followup si user abandona conversación |
| **Previous orders** | Muestra historial + permite agregar al carrito |
| **Show promotions** | Lee Commerce promotions y las muestra; permite agregar promo al carrito |

**En Unilever EC:**
- Sales Agent R1/P1 Preventa (Don Pingüino)
- Sales Agent R1/P1 Regular (Don Pingüino)
- Sales Agent in Voice 01 (Voice Oris Final)
- Oris R1/P1 (HPC)

**CR en construcción:** "Agente Loyalty" — Daniel Mosquera + Erick Martinez. Concept: Oris del Club que guíe a redimir sin entrar al catálogo (acumulado + redimibles + cómo acumular/redimir + misión del mes).

---

## 18. Recursos y links útiles

### Notion (5 páginas conectadas)

- [Handover principal](https://www.notion.so/yalo/366d53382b23804c8ec0c9a81aab00ec)
- [Anexo Técnico P1](https://www.notion.so/366d53382b2381ef954af92d41619c73) · [P2](https://www.notion.so/366d53382b23816f82d5d15dc60aa637)
- [Anexo C — v2](https://www.notion.so/367d53382b2381899d8de29ef32587f3) · [E — delivery-integrations](https://www.notion.so/367d53382b238197aa8dcf8a67c71994) · [F — delivery-services](https://www.notion.so/367d53382b2381cbbbedd2ce8c2d6453) · [G — Antifraude](https://www.notion.so/367d53382b2381cc9527e06a4a157d25)

### Notion técnicos (de Yalo)

- [Unilever EC ICE Tech (root, 94k chars)](https://www.notion.so/yalo/Unilever-EC-ICE-Tech-02e9df19ef684425b4033dce9db38830)
- [Unilever Napolita Ecuador](https://www.notion.so/yalo/Unilever-Napolita-Ecuador-558900c652914cf0985b01964bca05d1)
- [Napolita Pro](https://www.notion.so/yalo/Napolita-Pro-321d53382b23804c9944d9188ead23a8)
- [Napolita Anti-Fraud Validation](https://www.notion.so/yalo/Napolita-Anti-Fraud-Validation-34ad53382b2380bbacccc8dcd300e12c)
- [Unilever EC ICE Cabinets Tech](https://www.notion.so/yalo/Unilever-EC-ICE-Cabinets-Tech-1e694f440cb74c8aae4458022e6e5c63)
- [Trax Integration](https://www.notion.so/yalo/Trax-Integration-21bd53382b2380e5b049cfcf45b5fca7)
- [Unilever ICE Loyalty Implementation](https://www.notion.so/yalo/Unilever-ICE-Loyalty-Implementation-28dd53382b2380cabd49c20841352429)
- [Unilever ICE B2B2C](https://www.notion.so/Unilever-ICE-B2B2C-1aed53382b2380b88677caac8026fe0f)

### Google Drive / Docs

- [CR Image Recognition POP](https://docs.google.com/document/d/1jdX4Ylue94-cCABIt2-PKp-eijkgc8xIsPoirT6oF-Y/edit)
- [SOW Canal Móvil](https://docs.google.com/document/d/1OMONPVkpTEcdPqPwq6WgCtJHCmtcWbXd/edit)
- [Archivo Metas Mensuales para Gravty](https://docs.google.com/spreadsheets/d/1jV3zHZpGpyjK0dXH64zg06eXp2Lt8OQ-3wtCoY_DepM/edit?gid=905518213)
- [Diagrama general Don Pingüino](https://drive.google.com/file/d/1I-q0dUv6xBleisFR3Qnn8x1WDp3nHVDJ/view)
- [Videos walkthroughs Cabinets](https://drive.google.com/drive/folders/1nbqAkf2VF-21p9PT10brKVHtDpHBxKTJ)

### Slack canales

- [#b2b_unilever_ec_icecream](https://yalochat.slack.com/archives/C0508K27XHV) — canal principal (created 2023-03-28)
- [#loyalty-lji-yalo](https://yalochat.slack.com/archives/C08LL2PT3QA) — Loyalty con Gravty/LJI (private, created 2025-04-02 por MT Chamorro)
- `#delivery-engineering-support` — reportar incidentes
- `_incident-5843` — referencia histórica crisis Napolita Pro

### Gravty / Loyalty (URLs)

- **Newton tickets (oficial):** `newton.gravty.io` — Ale necesita pedir acceso propio
- **Gravty UAT tenant:** `https://unilever.uso24.gravty.io/`
- **Gravty PROD tenant:** `https://unilever.uso15.gravty.io/`
- **Blueprint técnico:** SharePoint LJI `ljiio.sharepoint.com/.../Unilever GRAVTY Implementation Blueprint_v1.0.docx`
- **Support Process Deck:** `LJI GRAVTY Support Processes Overview.pdf` (compartido 2/12/2025)

### Sheets Google Drive críticos (config Gravty mensual)

- `Configurations Unilever Gravty: pinguipuntos` — SKUs mensuales que dan puntos
- `Configurations Unilever Gravty: Individual purchase goals – [MES]` — metas mensuales (Member_ID + purchaseGoal1/2/3 + purchaseReward1/2/3)
- `Relacion SKUs <> Plataformas` — mapeo SKUs cross-platform
- [Archivo Metas Mensuales (ENERO)](https://docs.google.com/spreadsheets/d/1jV3zHZpGpyjK0dXH64zg06eXp2Lt8OQ-3wtCoY_DepM/edit?gid=905518213)

### Yalo Studio / Builder

Ver workflow URLs en sección 2.

### Otros

- [Monday board](https://ayalo.monday.com/boards/4359132620) (en topic del canal Slack)
- [Replit IR webapp](https://ir-webapp-yalo.replit.app/)
- [Whimsical Yalo Force diagram](https://whimsical.com/yalo-force-and-cabinets-2023-8C7aL9dqzBwwutnJxWzT9z)
- WhatsApp Napolita EC: [+593968379276](https://wa.me/593968379276)

---

## 18.5 Eventos clave post-handover (26-27 mayo 2026)

### Reunión Magnum-Trax-Yalo (26/5)

**Asistentes:** Esteban Ortega, David (Trax) · Kevia López (Magnum) · Alejandra López (Yalo)

**3 temas abiertos con Trax confirmados:**
1. **IR POP Task 3** (UEICE-877) — SOW enviado 22/5 ($11,440 / 11d). Kevia objeta como "deuda Yalo" — pidió revisar con Juanjo Ulloa. Esteban (Trax) confirma que POP NO está en contrato Trax↔Yalo renovado recientemente.
2. **IR Trax en Loyalty Club Pingüino** (CR2 A-4043) — Aprobado 22/5. Yalo no ha arrancado. ETA ejecución ~3 meses.
3. **IR en Pedido Sugerido** — Movido de Backlog a Discovery. Requiere mejoras a producto Yalo (Oris no usa rotación/frecuencia hoy).

**Capacidades Trax confirmadas:** esquinero SÍ se reconoce (skinner + display). Estado caballete (oxidado/dañado) NO. Planimetrías Traditional Trade actualizadas. Modern Trade sin claridad.

**Estimado de imágenes POP:** Kevia calculó en vivo ~10,670 clientes con tareas, cambio cada 3 meses (trimestral). Se compromete a mandar el cálculo formal a David por correo. Sin eso Trax no arranca training POP.

**Personas Trax para contactos:** Esteban Ortega (comercial), David (técnico), Esteban Paiz (parece otro contacto comercial para tema contractual).

### Decisión técnica Yalo Pago (Adán — 26/5)

**Master doc:** [💸 Yalo Pago - Magnum Ecuador Integrations](https://www.notion.so/yalo/Yalo-Pago-Magnum-Ecuador-Integrations-36dd53382b2380888dc9ea3d3c45b61d)

#### Flag PWST (UEICE-893)

- **Campo:** `metodoPago` a nivel **header** del payload PWST
- **Valores:** `"cash"` / `"credit"`
- **Default:** `"cash"` si usuario no responde en 5 min
- **Persistencia:** `Commerce Order's customFields.paymentMethod`
- **Reference code:** `delivery-integrations/projects/pwst-integration/scripts/modules/transformers.js#L269`
- **Action items:** introducir step de selección método pago + reminder 5 min · Lua para customFields · ajustar inyección PWST

#### BIT Gravty YP First TX (Newton #79596)

- **Tipo:** BIT independiente (no relacionado al Spend BIT)
- **Razón:** no hay forma de cruzar TX YP con pedidos previos
- **Trigger:** desde flow al recibir webhook Kushki, solo si primera TX validada
- **Estructura del BIT:** diferida a Gravty (Sankalp debe proponer)
- **Detección primera TX:** endpoint Yalo Pago + Lua (count transactions con status `paid` == 0)
- **Approach genérico:** Adán sugiere reutilizable para futuros rewards YP

#### Hallazgo crítico — factura no se cruza con pedidos

Efrén explicó:
- Usuario tipea número factura + monto **manualmente** sin validación
- DTs usan el flow YP para **recuperación de cartera** (facturas viejas de muchos pedidos atrás)
- Por eso TX YP NO se puede relacionar técnicamente con pedidos previos

#### Re-enrollment scenario (Bulk Newton #78604) — descartado

Adán determinó que el caso edge no vale la pena atacarlo. Cancelled members con `isLoyaltyMember=false` bloqueado upstream, no reciben puntos. "Feels like overkill" reactivarlos.

### Pruebas QA FRIGOS (Jaider — 26-27/5)

**Notion:** [🧪 Pruebas Frigos — Yalo Test Unilever Ecuador B2B](https://www.notion.so/yalo/Pruebas-Frigos-Yalo-Test-Unilever-Ecuador-B2B-36cd53382b238178958afd38b6a22aac)
**URL QA:** `https://commerce.yalochat.com/unilever-ec-ice-frigos-test/6a15e700c91919ce27d5ccda`
**Entorno:** `unilever-ec-ice-frigos-test`

**✅ Funcionando:** Make Order, Send Order, getOrders. Test pedido `6a15f04bc91919ce27d87941` · customer `FRI30551042_1` · store `FRI_123` · 12×Caja Crema Real Gigante = $364.32

**🐛 4 bugs abiertos:**
1. Productos llegan desactivados por defecto desde SharePoint (cliente debe confirmar si esperado)
2. Precio vacío en tarjeta del catálogo (USD$ vacío)
3. **ALTA:** Error enrolamiento Gravty — `Permission secretmanager.versions.access denied` en CF `gravty-member-enrollment` (us-east1, sta-yalo-main). Falta rol `roles/secretmanager.secretAccessor` a service account → Adán
4. `Invalid content in variables` filtro `storeCode: FRI_123` en API

### Reunión semanal Magnum-Yalo (27/5)

**Asistentes:** Kevia + Nicolle + Carlos + Luis + Deivy (Magnum) · Daniel Mosquera + Ale (Yalo)

**Highlights:**
- Daniel: 8,985 clientes en pedido sugerido + 6,886 otras skills (mayo). Kevia pide histórico desde febrero
- Lucas (data Yalo) probará nueva capa ML para pedido sugerido en preventa (hubo alucinación reciente de $3,000 USD)
- Promo escalas ✅ implementado por Daniel — funcionando antes del sugerido
- IR en Pedido Sugerido — Daniel asume análisis técnico
- SOW POP — Kevia reconfirma "deuda Yalo" + aclaración: cuando POP se active en Napolita, automáticamente entra como source para Club Pingüino (Capelasso ya alineado)
- Yalo Pago + Flag PWST: se unifican en mismo JSON. Efrén ya tuvo call con PWST que aceptó
- **Tiers Gravty: JULIO** confirmed (no junio — corregido por Ale)
- Feedback Carlos sobre Consolidated Report: 4 ajustes (doble columna puntos, logic abril/mayo, mil500 vs cero, target debe ser fila no columna)
- Napolita Perfil Empleados — Capelasso retoma esta semana (estuvo OoO mar-vie pasada)
- Bulk DoB / desenrollar: Kevia manda hoy EOD lista de clientes a desenrollar

### Minuta Digital Trade Visit (20/5 — enviada por Kevia el 27/5)

**Reunión interna Magnum** del 20/5 (Tisi probablemente asistió en su última semana pero no pasó la minuta). Kevia la envió formalmente el 27/5 a Ale, Daniel, Pedro, Nicolle, equipos Magnum + Promostock.

**33 action items** divididos en 3 áreas, deadline propuesto para próximo QBR (sept 2026):

- **Don Pingüino / Oris (12 items):** omnipresencia Oris, envío factura, training sinonimias, fix bug idioma portugués, alertas dropsize, análisis aceptación por CVA, mejoras sugerido (foto + rotación), benchmark efectividad, target efectividad, reporte clientes agénticos, incentivar aceptación con puntos
- **Club Pingüino (10 items):** **Agentizar Club Pingüino** (= UEICE-885), análisis rutas DT con oportunidad canjes, mejoras webview canjes, mejorar flujo menú, campañas trimestrales canje, estatus entrega premios, encuesta post-entrega, comunicación cuota mensual, forms trade visit con medio aprendizaje, opción reportar novedad canje
- **Napolita (11 items):** selección automática clientes con tareas, optimizar flujo tarea, **3 bugs** (imagen negra al tomar foto, no muestra guía ejecución, rutero dinámico con clientes faltantes), validar tiempos ejecución, conectar foto con sugerido (= IR en Pedido Sugerido), evaluación cumplimiento mensual, retos auto-ejecución, viabilidad tarea fuera de semana, revisar medición evaluación foto

**Owners según minuta:** Daniel Mosquera (16 items), Pedro Tejena (7), Kevia (7), Nicolle (7), Jenniffer (1), Jordan (1).

**Importante:** El item #13 "Agentizar Club Pingüino" ya está en marcha como UEICE-885 (Custom Agent Loyalty CR v2.3 en Notion).

### SKU goals + Individualization JUNIO (Nicolle — 26/5 22:09)

**Email cliente:** Nicolle Medina envió rutero junio + escalas SO + misiones plataforma (respuesta al ping de Sankalp 25/5 = GR-1).

**Sheet master:** [Configurations Unilever Gravty](https://docs.google.com/spreadsheets/d/1jV3zHZpGpyjK0dXH64zg06eXp2Lt8OQ-3wtCoY_DepM/edit?gid=905518213)

**Misiones plataforma junio (pestaña `pinguipuntos`):**

| Plataforma | Puntos | Threshold | Vigencia | SKUs |
|---|---|---|---|---|
| CORNETTO | 350 | 1 caja (22 unid/pkg) | 1-14 junio | Oreo · Snickers · Clásico · Choco Vainilla + 3 surtidas |
| GEMELO | 800 | 1 caja (50 unid/pkg) | 1-14 junio | Limón Naranja · Chocolate Leche + 1 surtida |
| GEMELO Medias Cajas | 400 | 1 caja (1 unid/pkg) | 1-14 junio | Media caja Limón Naranja · Chocolate Leche |
| POTES CREMOSSITOS | 100 | 1 unidad | **15-30 junio** | 8 sabores: Chicle, Chocolate, Frutilla, Napolitano, Ron Pasas, Vainilla, Napolitano 750ml |

**Particularidad junio:** primera quincena (1-14) = Cornetto + Gemelo. Segunda quincena (15-30) = Cremossitos.

**Formato:** mismo que mayo (sin columna tier — tiered config arranca julio).

**Pedido extra de Magnum a Yalo:**
- Actualizar imágenes catálogo + captions en flow
- Desactivar el SKU 1ero (lo vigente este mes)
- Arrancar con los nuevos del adjunto

**Individualization (pestaña `Individual purchase goals - JUNIO`):** detalle de targets por cliente en el archivo `Rutero Club Pinguino DTs-PVs JUNIO 2026.xlsx`.

### Newton tickets vivos

| Newton # | Tema | Status |
|---|---|---|
| #79299 | Consolidated Report | ✅ Entregado 22/5 + ajustes pendientes (4 feedback de Carlos) |
| #79381 | Cliente ESS4753272_1 (re-enroll + 400 pts) | ✅ Hecho 22/5 |
| #79596 | YP First Transaction Reward | 🟡 Ale respondió 27/5 a Sankalp con propuesta de Adán |
| #79597 | SKU offers by tier | 🟡 En config TEST por Gravty, productivo desde julio |

### Tickets Jira UEICE vivos (post-handover)

| UEICE | Tema | Status |
|---|---|---|
| 877 | Trax POP Task 3 | 🟡 SOW enviado, Kevia objeta "deuda Yalo" — pendiente revisión interna Juanjo |
| 883 | DEV Copy YP en flow Don Pingüino | 🟢 Desbloqueado por decisión Adán, Jaider implementa |
| 885 | Épica Custom Agent Loyalty | 🟡 CR v2.3 en Notion, asignada Adán, 8 decisiones técnicas pendientes |
| 893 | DEV Validar envío a PWST (flag pago) | 🟢 Approach definido por Adán (`metodoPago` header) |
| 846 | Canal Móvil COMERCIANTES (~800 móviles) | 🔴 Bloqueado por APIs proveedor nuevo |
| 810 | Canal Móvil FRIGOS (45 frigos) | 🔄 QA en curso, 4 bugs abiertos (1 ALTA) |

### Contactos nuevos / actualizados (mayo 2026)

| Persona | Rol | Email | Notas |
|---|---|---|---|
| Esteban Ortega | Trax comercial/lead | (Slack/email) | Lleva relación cuenta Magnum, alinea con Juanjo Ulloa |
| David | Trax técnico | (Slack/email) | Entrenamiento motor IR |
| Esteban Paiz | Trax (otro contacto comercial) | `esteban.paiz@traxretail.com` | Posiblemente para tema contractual |
| MT | Yalo partnerships | (pedir) | Maneja partnership con Trax — mencionada por Ale en reunión 26/5 |
| Santiago Copiano | Yalo | (pedir) | Mencionado en alineamiento interno con Juanjo |
| Lucas | Yalo data | (pedir) | Construye modelos de pedido sugerido (ML layer) |
| Nicolle Medina | Magnum Trade Marketing TT | `Nicolle.Medina@magnumicecream.com` · +593 98 093 0219 | Envía rutero mensual a Ale + Daniel |
| Sammantha Armendaris | Magnum | `Sammantha.Armendaris@magnumicecream.com` | Trade marketing |
| Yamilet Benavides | Magnum | `Yamilet.Benavides@magnumicecream.com` | (en CC del rutero) |
| Jenniffer Trivino | Magnum | `Jenniffer.Trivino@magnumicecream.com` | Trade visit, evaluación cumplimiento tareas |
| Carlos Negrete | Magnum data | `Carlos.Negrete@magnumicecream.com` | Reporta issues PowerBI del Consolidated Report |

---

## 19. Glosario

| Término | Definición |
|---|---|
| **Don Pingüino** | Nombre coloquial del bot productivo de Helados (`unilever-ecu-ic-prd`) |
| **Don Lever** | Persona brand del bot HPC Ecuador |
| **Heladio** | Storefront/bot del lado MX (NO confundir con Don Pingüino EC) |
| **Napolita** | Programa interno Unilever (empleados WC/BC/Furia Roja) y clientes B2B |
| **Furia Roja** | Vendedores Unilever que visitan tiendas y toman fotos de cabinets |
| **PWST / PSWT** | PowerStreet — sistema externo del cliente para inyección de pedidos |
| **Gravty** | Partner externo que opera la plataforma de Loyalty Club Pingüino |
| **Trax** | Partner externo de Image Recognition (análisis fotos cabinets) |
| **YF / Yalo Force** | Sistema de gestión de cabinets / fuerza de ventas |
| **BSNG** | Big Storage NG — sistema de almacenamiento Yalo |
| **Folkvangr** | API interna Yalo (en bots prod apunta a staging — bandera) |
| **Kushki** | Payment processor (usado por Yalo Pago) |
| **B2B2C** | Bot separado para consumer final (cupones QR) |
| **Club Pingüino** | Programa de Loyalty |
| **Magnum** | Marca premium Unilever EC (también nombre de entidad cliente) |
| **Cabinet / Frigo** | Congelador en tiendas (45 frigos activos) |
| **Censar** | Registrar un cabinet existente |
| **Comerciantes móviles** | ~800 vendedores en triciclos/carros |
| **Antifraude** | Validación GPS + distancia ≤250m para fotos de cabinets |
| **PerfectStore** | Sistema de segmentación de catálogo por customer (ICE) |
| **CVA** | Sistema de tiers de cliente Napolita (TOP ORO, ORO, PLATA+, PLATA-, BRONCE, LOW BRONCE, SIN COMPRA) |
| **Oris** | AI agent multimodal de Yalo (6 skills) |
| **PWST cycle anchor** | 29 dic 2025 — fecha base del calendario continuo 4-semanas que usa Napolita |
| **LJI** | Partner que gestiona Gravty para Yalo. Sankalp Mohanty es POC operativo |
| **Newton** | Sistema oficial de tickets de Gravty/LJI (`newton.gravty.io`). Desde 18/11/2025 es el canal oficial, NO Slack |
| **ICE_PLATA / ICE_PLATA+ / ICE_ORO / ICE_TOP_ORO** | Tiers reales del Club Pingüino en Gravty (vs la nomenclatura `UN01/UN02/UN03` que aparece en el bot — confirmar correspondencia) |
| **Birthday Reward** | 1000 puntos al cumpleañero. Template `puntos_diadecumpleanos_mayo_2026` (actualizado 22/5 con imagen). Solo tiers ICE_PLATA+, ICE_ORO, ICE_TOP_ORO. $0.074/noti (revisar si cambia con imagen) |
| **GRAVTY 2.12.0** | Upgrade con API breaking change (dic 2025): segment-level → offer-level. PROD upgraded 7/4/2026 |
| **Tenant Gravty UAT/PROD** | UAT `unilever.uso24.gravty.io` · PROD `unilever.uso15.gravty.io` |
| **Member_ID vs External_ID** | En input files de Gravty usar SIEMPRE `Member_ID` (no External_ID). Numérico sin "$" ni comas |

---

## 20. Preguntas abiertas para Tisi

### Críticas (preguntar sí o sí esta semana)

1. **Apellidos** de Kevia y Nicolle del lado cliente
2. **Contraparte Magnum LATAM** que aprueba paso a prod Antifraude (¿es Bárbara como en MX?)
3. ~~**Weekly recurrente Unilever-Yalo** — día/hora/link/asistentes~~ ✅ Respondida ("ya coordinado en la reunión de hoy" + BiWeekly con Trax martes)
4. ~~**Estado real Mecánicas Promocionales** — ¿se ejecutó el paso del 4/5 7pm o se canceló?~~ ✅ Respondida ("se canceló — PWST debe hacer cambios")
5. **Proveedor APIs Canal Móvil Comerciantes** — quién es, ETA APIs
6. ~~**¿Webapp prod `ir-webapp-yalo.replit.app` ya tiene descifrado AES?**~~ ✅ Aclarado por Ale: Antifraude es solo validación GPS para foto en Napolita, scope simple

### Loyalty (críticas — basadas en Slack `#loyalty-lji-yalo`)

7. **¿El bulk DoB update se hizo?** 32,805 members sin DoB → Birthday Reward NO funciona para ellos. Última mención 10/3, sin confirmación de cierre
8. **¿Implementación S2 del API 2.12.0 está completa en Yalo?** (`GET /v1/f/offers/?type_of_offer=INDIVIDUALIZED`) — confirmar con Adán
9. **Correlación `UN01/UN02/UN03` ↔ `ICE_PLATA*/ICE_ORO/ICE_TOP_ORO`** — los tiers en `loyaltyTierGoals` del bot vs los reales de Gravty. Confirmar mapping con Andrey
10. **Status Newton #79381** (Yalo Pago 400 pts no se acreditan) — el ticket más urgente con Gravty
11. **Order EGM5301164_1 marzo** — confirmar resolución con LJI
12. **Quién es Sankalp Mohanty y cómo es trabajar con él** (el POC LJI más importante)

### Técnicas

7. **Folkvangr staging en prod** — ¿intencional o bug?
8. **Webhook Mondelez Perú en convencion** — leftover o uso real?
9. **Activities con nombre genérico en ic-prd** (`new-activity-71/72/82`, `second test`, `Step-test`, `NO ACTIVITY`, `Thank You`) — archivables?
10. **Encuestas hardcoded** (Julio 2024, CSAT 2024, 15 de mayo) — archivables?
11. **Dos variables OpenAI** (`openAiKey` vs `openAiKeyuni`) — uso de cada una?
12. **`CSat Napolita` en ic-prd** — ¿por qué Napolita en bot de helados?
13. **Correlación CVA tiers ↔ loyaltyTierGoals UN01-UN03** — confirmar mapeo
14. **¿Tres activities Yalo Force similares son duplicación?** (March 2024, force & cabinets, Pinguino Force)

### Stakeholders / comercial

15. **Contrato Unilever Ecuador:** fechas renovación, hitos, modelo
16. **Petit comité interno:** ¿qué se habla con AE/CSM sobre Magnum políticamente?
17. **Cruce con MX:** convencion usa storefronts MX — ¿cómo se reparten responsabilidades con handover MX paralelo?
18. **Gravty:** relación contractual Unilever-Yalo-Gravty, quién paga, quién escala
19. **¿Hay sync cruzado EC-MX-Yalo** cuando hay cambios que afectan ambos?

### Roadmap

20. **Q3 / H2 2026:** qué se acordó con Unilever EC
21. **KPIs / OKRs del cliente:** GMV Helados, GMV HPC, NPS, contención IA, adopción Loyalty
22. **¿Dónde están los dashboards** de métricas mensuales que se reportan?
23. **Decisión fee Gravty Loyalty** — quién decide entre las 3 opciones

### De proceso

24. **Sales Desk / handoff humano:** a quién va, SLA por bot
25. **Aprobación de cambios:** quién aprueba del lado cliente y Yalo
26. **Release process:** staging separado, ventanas, CI/CD
27. **HSM templates Meta:** quién solicita aprobaciones
28. **On-call / runbook 3am:** quién levanta si Don Pingüino o Napolita se cae

---

## 21. Tribal knowledge gaps

Lo que **NO** se puede reconstruir de docs — solo Tisi puede transmitir oralmente:

1. **Personalidad real de Kevia, Pedro, Nicolle** — cómo hablarles, qué evitar
2. **¿Hay equivalente a "Caro" (jefa difícil del lado MX) en Ecuador?**
3. **Historial de escalaciones** estilo "Citronella" que no estén en Jira
4. **Decisiones de producto importantes** de los últimos 6-12 meses (por qué X, intentaron Y y no funcionó)
5. **Workarounds temporales** activos en el bot (hardcodes, ifs con nombres, sleeps arbitrarios)
6. **Quién en Yalo conoce los bots EC mejor** excluyendo CSM/TL/Dev — para segundo punto de contacto

---

## 22. Lista accionable para Ale

### Antes del viernes 22/5

- [ ] Sesión con Tisi → llevarle las preguntas arriba + tribal knowledge
- [x] ~~Pedir add a Slack canal `#loyalty-lji-yalo`~~ ✅ Acceso ya activo
- [ ] Confirmar transferencia del Calendar invite Weekly Unilever-Yalo
- [ ] **Pedir acceso a Newton** (`newton.gravty.io`) — sistema oficial de tickets Gravty/LJI
- [ ] Leer thread "Birthday offer configuration" (58 replies) en `#loyalty-lji-yalo` para entender el feature completo
- [ ] Coordinar con Tisi para que te presente a **Sankalp Mohanty** (POC operativo LJI más importante)

### Próxima semana (25-29/5)

- [ ] Confirmar con **Jaider** si UEICE-879 ya está deployed a prod
- [ ] Confirmar con **Jaider/Adán** estado real del Antifraude (paso a PROD 1/6):
  - ¿Las pruebas de QA van bien?
  - Code review del PR del branch antes del merge
- [ ] Para debuggear UEICE-881: usar **LangSmith** (sección 15) con filtro `workflow_name=wa-un1912-napolita-ecuador` + name=`order_taking_skill`
- [ ] Reunión inicial con **Kevia** del cliente
- [ ] Seguimiento con **Juanjo** sobre cierre comercial Canal Móvil

### Próxima semana — específicamente para Loyalty

- [ ] **Newton #79381 (Yalo Pago 400 pts) — ticket más urgente con Gravty.** Revisar status con Sankalp
- [ ] **Order EGM5301164_1 marzo** — confirmar resolución con LJI (Tisi pidió 18/5, sin cierre)
- [ ] **Birthday Reward** — actualizar message template + confirmar bulk DoB update está hecho
- [ ] **SKU goals mensuales** — para junio, LJI pedirá ~día 21-25 mayo (esta semana). Coordinar con Unilever
- [ ] **GRAVTY 2.12.0 implementación S2** — verificar con Adán que está completa
- [ ] **Confirmar tier mapping** UN01/UN02/UN03 ↔ ICE_PLATA*/ICE_ORO/ICE_TOP_ORO

### En el día a día

- [ ] Verificar accesos del checklist (Jira UEICE, Studio, GitHub repos, Salesforce, Trax, Gravty, Azure SAS, PWST, SharePoint)
- [ ] Suscribirse a notificaciones de los 10 tickets UEICE vivos
- [ ] Familiarizarse con el `.md` (este doc) — está organizado para que sea referencia rápida

### Cuando llegue 1/6 (paso a PROD Antifraude)

- [ ] Validar deploy exitoso
- [ ] Monitorear logs en GCP Cloud Logging (proyecto `prod-yalo-main`)
- [ ] Confirmar con Magnum que la validación GPS funciona en campo

### Migración SFTP→Azure (deadline indefinido del cliente)

- [ ] Seguimiento con **Magnum/Pedro Tejena** sobre creación de carpetas Azure
- [ ] Coordinar con **Daniel Capelasso** (DRI Yalo)
- [ ] Recordar: SAS token rota cada 3 meses (próximo cambio ~30/6/2026)

---

## Apéndice A — Diagrama del ecosistema

```
                    ┌──────────────────────────────┐
                    │   CLIENTE: Magnum / Unilever │
                    │   Ecuador                    │
                    │   Kevia (PM) · Pedro Tejena  │
                    │   Nicolle                    │
                    └────────────┬─────────────────┘
                                 │
                ┌────────────────┼────────────────┐
                │                │                │
        ┌───────▼──────┐  ┌──────▼─────┐  ┌──────▼──────┐
        │   PWST       │  │ SharePoint │  │   Gravty    │
        │ (PowerStreet)│  │ Proyecto   │  │  Loyalty    │
        │ inyección    │  │ ALEXA-YALO │  │  (partner)  │
        │ pedidos      │  │ (Pedro     │  │             │
        │ + Kushki     │  │  Tejena)   │  │             │
        └──────┬───────┘  └─────┬──────┘  └──────┬──────┘
               │                │                │
               │ CSVs orders    │ Imágenes IR    │ CSV accruals
               │                │ + Oris report  │ (via scheduler)
               ↓                ↓                ↓
   ┌────────────────────────────────────────────────────┐
   │              YALO ECUADOR ICE                      │
   │  ┌────────────────────────────────────────────┐   │
   │  │  Don Pingüino (unilever-ecu-ic-prd)        │   │
   │  │  · 80 activities · Loyalty · YF Cabinets   │   │
   │  │  · Sales Agents · Voice Oris · YP-* v3     │   │
   │  └────────────────────────────────────────────┘   │
   │  ┌────────────────────────────────────────────┐   │
   │  │  Napolita EC (wa-un1912-napolita-ecuador)  │   │
   │  │  · Furia Roja · Mis tareas · Genie Trainer │   │
   │  │  · Antifraude (en branch) · Share fridge   │   │
   │  └────────────────────────────────────────────┘   │
   │  ┌────────────────────────────────────────────┐   │
   │  │  HPC (unilever-ecu-hpc-prd) "Don Lever"    │   │
   │  │  · 31 activities · Oris R1/P1              │   │
   │  └────────────────────────────────────────────┘   │
   │  ┌────────────────────────────────────────────┐   │
   │  │  B2B2C (Unilever ECU ICE B2B2C)            │   │
   │  │  · Consumer final · Cupones QR             │   │
   │  └────────────────────────────────────────────┘   │
   └────────┬───────────────────┬────────────────┬─────┘
            │                   │                │
            ↓                   ↓                ↓
      ┌──────────┐        ┌──────────┐    ┌──────────┐
      │   BSNG   │        │  Trax    │    │ Zendesk  │
      │ (events, │        │  (Image  │    │ (Jair)   │
      │  tasks,  │        │   Reco)  │    │          │
      │  prizes) │        │          │    │          │
      └──────────┘        └──────────┘    └──────────┘
            │
            ↓
      ┌──────────────┐
      │ AWS Lambdas  │
      │ unilever-ecu │ → reporte diario CSV → SharePoint
      │ -ice + cpg-* │
      └──────────────┘
            ↓
      ┌──────────────┐
      │   Azure Blob │ ← migración pendiente (deadline cliente indefinido)
      │ IC_EC/YALO   │   SAS expira 30/6/2026
      └──────────────┘

CFs deployed en GCP (us-east1-prod-yalo-main):
  · Gravty (10)     · Napolita (8 + 1 branch)
  · Trax (2)        · unilever-ice-ecu-* (1+)
```

---

## Apéndice B — Quick reference de comandos

### Resetear tasks mensuales de una tienda (Napolita)

**Manual (Admin API):**
1. `getCustomers` storefront `napolita-ec-clients` → obtener `id` + `customFields`
2. Cambiar `customFields.currentMonthExecution` de `1` a `0` (mandar TODOS los customFields)
3. `updateCustomer` con mismo storefront

**Automatizado (Lambda):**
```bash
aws lambda invoke --function-name unilever-ecu-ice-restart-napolita-tasks \
  --payload '{}' /tmp/out.json
```

### Añadir / quitar sellers autorizados Yalo Force

1. Cliente envía CSV con `phoneNumber`, `name`, `sellerCode`
2. Transformar a código Lua
3. Editar [step 430 del flow](https://platform.yalochat.com/en/c/63ceb43a85a6789106c048b5/b/63cec1bbcdf69d22dd9a7e6e/studio/workflows/63ceb572f81767f745288ec6/activity/63e349759b28759bfc6797c9/step/430)
4. Publicar flow

### Rotar SAS token Azure (cada 3 meses)

1. Magnum EC notifica nueva SAS URL
2. Support actualiza variable global en builder
3. Validar acceso post-rotación

### Buscar accrual transaction de un member en Loyalty

[Cloud Logging query](https://console.cloud.google.com/logs/query;query=resource.type%20%3D%20%22cloud_run_revision%22%0Aresource.labels.service_name%20%3D%20%22gravty-orders-consumer%22%0Aresource.labels.location%20%3D%20%22us-east1%22%0Aseverity%3E%3DDEFAULT%0A%22WYJ3LT4%22) — reemplazar el Gravty member id en la query.

### Generar reporte cabinets por distribuidor

```bash
curl -X POST "https://us-east1-prod-yalo-main.cloudfunctions.net/unilever-ice-ecu-generate-distributor-cabinets-report" \
  -H "Authorization: bearer $(gcloud auth print-identity-token)" \
  -H "Content-Type: application/json" \
  -d '{"storefrontName": "napolita-ec-clients"}'
```

### Release Orders "On Hold" (PWST manual)

```bash
cd ~/Documents/.../delivery-integrations/snippets/support-unilever-ec-ice-release-orders
# Seguir README de ese folder
```

### Local dev de una CF

```bash
cd ~/Documents/.../delivery-services
make run project=napolita functionName=task-management
```

### Volver a main después de checkout antifraude branch

```bash
cd "/Users/maria.lopez@yalo.com/Documents/Unilever Ecuador/repos/delivery-services"
git checkout main
```

---

## Apéndice C — Crisis Napolita Pro (post-mortem)

**Cronología 1-5 mayo 2026:**

| Fecha | Evento |
|---|---|
| 1/5 6:08 AM | Tisi detecta CFs `napolita-generate-seller-goals-progress-report` (step 775) y `napolita-pending-task-customers` (step 777) dando error 500 |
| 1/5 mañana | Aldo investiga, hipótesis inicial: archivos CSV (base Customers, base goals Napolita) recién actualizados ese día |
| 1/5 4:20 PM | Daniel reporta: negocio sigue sin ver tareas. Pide volver a Napolita normal |
| 1/5 4:30 PM | Adán: "no vamos a regresar". Jaider monitorea Napolita Pro hasta estabilizar |
| 4/5 | Reporte de foto del cabinet falla en algunos teléfonos. Replit webapp issue. Adán dice usar Chrome/Safari en lugar del browser embedded de WhatsApp |
| 4/5 11:59 AM | Adán: steps no quedaron bien conectados al nuevo orquestrador de tareas. Trabajo en curso |
| 4/5 4:20 PM | Daniel pide ETA o apagar Napolita Pro. Adán: "afinar la experiencia hoy" + "incidente Trax en investigación" |
| 5/5 7:30 AM | Daniel: tareas no están sumando. Caos total en Magnum |
| 5/5 7:43 AM | Juan Zuluaga escala: "el compromiso era jueves listo, viernes problemas, ayer reportaron, hoy tampoco. Necesitamos claridad y cumplimiento." |
| 5/5 8:34 AM | Adán: "no regresamos a Napolita normal. Jaider monitorea. Trax incidente _incident-5843" |
| 5/5 8:48 AM | Adán confirma: análisis Trax regresando. Jaider revisando tareas no contadas |
| 13/5 | Pedro Tejena valida funcionalidad |
| 14/5 | Quedó estable en prod |

**Lecciones:**

1. **Magnum estresa el desarrollo desde el minuto 1.** Si algo se lanza con bugs, escala rápido. Cero margen.
2. **Hacer pruebas exhaustivas previas** antes de lanzar nuevo flujo a clientes así de exigentes.
3. **Replit webapp** falla en algunos browsers embedded de WhatsApp — recomendar Chrome/Safari.
4. **Magnum usa el desarrollo el primer día** sin advertencia, así que no se puede confiar en hypercare gradual.

---

## Apéndice D — Threads Slack largos a leer

Si tienes tiempo, vale la pena leer estos threads completos:

| Thread | Replies | Tema | Link |
|---|---|---|---|
| Discovery Canal Móvil (18/2) | 53 | Discovery + SOW + B2B2C extension | [link](https://yalochat.slack.com/archives/C0508K27XHV/p1771519581610689) |
| Napolita Pro (1/5) | 67 | Crisis lanzamiento + estabilización | [link](https://yalochat.slack.com/archives/C0508K27XHV/p1777636968170139) |
| Cupón B2B2C - Don Pingüino no reconoce (11/5) | 40 | Bug B2B2C | [link](https://yalochat.slack.com/archives/C0508K27XHV/p1778533427188289) |
| Pruebas SOW Furia Roja (27/2) | 79 | Mis tareas implementation | [link](https://yalochat.slack.com/archives/C0508K27XHV/p1772220580182379) |
| No se dispara llamada Don Pingüino (18/4) | 189 | Issue Voice Oris (números MX) | [link](https://yalochat.slack.com/archives/C0508K27XHV/p1776514939685209) |
| Reporte por correo (24/4) | 30 | Reportería | [link](https://yalochat.slack.com/archives/C0508K27XHV/p1777037737476249) |
| Sharepoint reportes (28/4) | 36 | Path SharePoint + pipeline Databricks | [link](https://yalochat.slack.com/archives/C0508K27XHV/p1777386642385759) |
| Promociones ambiente QA (10/3) | 36 | Mecánicas promocionales | [link](https://yalochat.slack.com/archives/C0508K27XHV/p1773143354293359) |
| Censo de Cabinets (1/4) | 35 | SOW Censo | [link](https://yalochat.slack.com/archives/C0508K27XHV/p1775043744052549) |
| Fechas de Cumpleaños Loyalty (26/3) | 39 | Birthday reward | [link](https://yalochat.slack.com/archives/C0508K27XHV/p1774548047962849) |
| Mis clientes con tareas no se muestra (20/5) | 25 | Bug UEICE-881 (último activo) | [link](https://yalochat.slack.com/archives/C0508K27XHV/p1779288089563369) |
| PM Assist Task 3 (13/5) | 23 | Trax IR POP (UEICE-877) | [link](https://yalochat.slack.com/archives/C0508K27XHV/p1778686729354329) |
| Envío de ordenes hacia Gravty (19/5) | 14 | UEICE-879 (fecha) | [link](https://yalochat.slack.com/archives/C0508K27XHV/p1779200102830099) |
| Noti Reward por cumpleaños (19/5) | 9 | Birthday + Meta UTILITY + costos | [link](https://yalochat.slack.com/archives/C0508K27XHV/p1779194006748939) |

---

## Apéndice E — Historia operativa de Loyalty (Oct 2025 → May 2026)

Resumen de eventos clave del canal Slack `#loyalty-lji-yalo`. Útil para entender por qué las cosas son como son.

| Fecha | Evento |
|---|---|
| **Oct 2025** | |
| 27/10 | Set-up Individualization Offer kickoff |
| 29/10 | **Bloqueante crítico:** Unilever PROD tenía 20,879 members enrolados pero input file traía 36,866 records → 16,029 members sin External_ID mapeado |
| 29/10 | Acuerdo con LJI: usar `Member_ID` (no External_ID), formato numérico sin "$" ni comas |
| **Nov 2025** | |
| 3/11 | Push to PROD de Individual goals |
| 4/11 | 4 open items críticos abiertos. Bug: NO se ven redemption BITs en GRAVTY |
| 15/11 | Apagar promos Magnum & Cornetto (acción manual) |
| 18/11 | Crisis "URG Accrual Error". 4 threads simultáneos en un día. **LJI mueve todo el soporte a Newton** |
| 25/11 | **Root cause identificada:** ERP PowerStreet no enviaba N órdenes (bug ERP → accrual) |
| **Dic 2025** | |
| 1/12 | 101 duplicated members encontrados |
| 4/12 | DataFix completo: 15,928 External IDs restaurados + 95 deletes + 6 merges |
| 11/12 | **GRAVTY 2.12.0 en UAT** — breaking API change segment-level → offer-level. Solución elegida: S2 (`type_of_offer=INDIVIDUALIZED` + GET filter) |
| 26/12 | Ernesto: "Unilever team created service ticket last minute due to holidays" — patrón cultural recurrente |
| **Ene 2026** | |
| 12/1 | GRAVTY PROD upgrade 2.12.0 propuesto para 13/1 o 19/1 |
| 21/1 | **Birthday offer kickoff** — thread más largo del canal (58 replies). Acción Yalo: enviar DoB de nuevos members. Acción LJI: bulk update DoB de existing members |
| **Feb 2026** | |
| 23/2 | **Tisi se une al canal** (toma cuenta de Ernesto) |
| 24/2 | 32,417 members en GRAVTY. Teléfonos duplicados aceptados como expected |
| **Mar 2026** | |
| 5/3 | LJI configura Birthday Offer (ID `308749270`). Test exitoso: 1000 pts. **32,805 members en PROD, ninguno tiene DoB configurado** |
| 9/3 | Definición final: tiers elegibles Birthday = ICE_PLATA+, ICE_ORO, ICE_TOP_ORO |
| 10/3 | Member_Report compartido para bulk DoB update (**sin confirmación de cierre todavía**) |
| **Abr 2026** | |
| 7/4 | GRAVTY 2.12.0 PROD upgrade ejecutado |
| 14/4 | **Bug crítico:** cliente `30847784` debía recibir 500 pts por TRI sandwich pero recibió **13.89 pts** ([reporte original](https://yalochat.slack.com/archives/C07CDFEQJ3T/p1776213315423239) de Jorge Velasquez en `#delivery-engineering-support`) |
| **May 2026** | |
| 10/4-20/5 | Thread SKU goals activo (sin cerrar mayo) |
| 18/5 | Tisi: order EGM5301164_1 marzo no localizable (ACTIVO) |
| 19/5 | **Newton #79381** abierto — PINGUINO PAGO acreditación 400 pts no funciona |
| 20/5 | Tisi anuncia handover formal a Ale en canal Loyalty |
| 21/5 | GRAVTY 2.12.0 UAT upgrade ejecutado |
| **22/5** | **Último día Tisi.** Email Priorización & definición a Magnum. Call overlap Tisi-Ale (53 min). Sankalp entrega Consolidated Report. Birthday template con imagen aplicado. Sankalp acepta lógica threshold-based SKU goals. **Tisi crea Newton #79596 (YP First TX Reward) + Newton #79597 (SKU offers by tier) + épica UEICE-885 (Agente Loyalty)**. Quedaron sin cerrar: cálculo costo Birthday y follow-up PowerStreet (a Ale) |

### Patrones culturales aprendidos

1. **Magnum estresa el desarrollo desde el día 1.** Si algo se lanza con bugs, escala rápido (caso Napolita Pro + Loyalty Accrual Error). Cero margen.
2. **Unilever entrega configuración mensual a último momento** — múltiples disculpas por holidays. Cadencia LJI pide día 21-25 del mes anterior.
3. **Para que algo se priorice con LJI, tagear a Sankalp Mohanty directamente.** El resto FYI a Praneeth + Rohit.
4. **eashwar** = Simulation API + Bits API endpoint specialist.
5. **Om Prakash Sahoo** = Reports/BI lead — todo lo de reportes va con él.
6. **Releases Gravty:** martes/miércoles 1AM CDMX, 30 min downtime. UAT primero → sign-off Yalo → PROD ~2-4 semanas después.

---

**FIN DEL DOCUMENTO** · v9 con conocimiento Loyalty consolidado · 2026-05-21 · Owner: Alejandra López

**Stats:** ~1900 líneas · 22 secciones + 5 apéndices · Compila 3 JSONs + 3 HTMLs + 17 threads Slack (`#b2b_unilever_ec_icecream` + `#loyalty-lji-yalo`) + 10 docs Notion + 2 repos clonados + 25+ archivos código
