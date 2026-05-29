# Unilever ICE Ecuador — PM & Delivery Context

> **Fuente:** extraído del knowledge-base v13 (2026-05-22)
> **Scope:** stakeholders, status proyecto, tickets, roadmap, accionables

---

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

