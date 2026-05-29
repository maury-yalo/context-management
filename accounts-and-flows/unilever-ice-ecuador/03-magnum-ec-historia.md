# Unilever ICE Ecuador — Historia & Contexto Institucional

> **Fuente:** extraído del knowledge-base v13 (2026-05-22)
> **Scope:** glosario, crisis Napolita Pro, historia Loyalty, patrones culturales

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
