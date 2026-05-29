# Audiencias CDP — Unilever Ecuador HPC

> **CDP UUID:** `63ceb48fadbef7545f69f806` · **Entity type:** `stores` · **CDP Version:** V1 (assumed — verificar)
>
> **Última actualización:** 2026-05-27

---

<!-- DYNAMIC:audiencias_resumen -->
## 📊 Resumen de segmentos
> _🔄 Actualizado: 2026-05-27_
>
> Para lista completa y actualizada:
> ```bash
> cdp segments list --account-id 63ceb48fadbef7545f69f806
> ```

| Tipo de segmento | Ejemplos observados |
|-----------------|---------------------|
| Promo por ciclo + día | `Promo_Ciclo{n}_{mes}{año}: DIA_VISITA2 {op} {día}` |
| Pedido digital / utility | `PedidoDigital_VideoInstructivo_Utility` |
| No-visita YI | tiendas sin visita ese día (trigger automático YI) |
| Engagedprospects | subgrupo activo de prospectos comprometidos |

> Total segmentos activos: pendiente — ejecutar `cdp segments list`

---

## 🗂️ Días de visita disponibles

El atributo `DIA_VISITA2` contiene el día de visita del vendedor a cada tienda.

| Valor | Usado en campañas |
|-------|------------------|
| `Lunes` | ✅ Sí (No_visita_lunes YI) |
| `Martes` | verificar |
| `Miercoles` | ✅ Sí (top revenue) |
| `Jueves` | verificar |
| `Viernes` | ✅ Sí (top revenue) |

> ⚠️ Capitalización: verificar el case exacto con `cdp stores describe --account-id 63ceb48fadbef7545f69f806 --sample` — puede ser `Lunes` o `lunes`.

Valores observados en nombres de campaña (fuente BQ): `Lunes`, `Miercoles`, `Viernes` (Title Case con tilde omitida en Miércoles).

---

## 🔧 Campos CDP — referencia

| Campo | Tipo | Descripción | Notas |
|-------|------|-------------|-------|
| `DIA_VISITA2` | string | Día de visita del vendedor | ⚠️ CRÍTICO — con "2" |
| `store_id` | string | ID de la tienda | clave primaria |
| `phone` | string | WhatsApp de la tienda | formato internacional |
| `name` | string | Nombre de la tienda | para personalización HSM |
| `is_active` | boolean | Si la tienda está activa | filtrar `true` en segmentos |
| `last_order_date` | date | Fecha del último pedido | para segmentos recencia |
| `channel` | string | Canal de contacto | típicamente `whatsapp` |

> Lista completa: `cdp stores describe --account-id 63ceb48fadbef7545f69f806`

---

## 📐 Convención de nombres

### Promo Ciclo
```
Promo_Ciclo{N}_{Mes}{Año}[_Engagedprospects]: DIA_VISITA2 {operador} {Día}
```
Ejemplos:
- `Promo_Ciclo4_Mar26: DIA_VISITA2 == Viernes`
- `Promo_Ciclo2_Q1_Feb26: DIA_VISITA2 in Miercoles`
- `Promo_Ciclo4_Mar26_Engagedprospects: DIA_VISITA2 == Viernes`

### Pedido Digital
```
PedidoDigital_{descripción}[_Utility]
```
Ejemplo: `PedidoDigital_VideoInstructivo_Utility`

### No-visita (YI)
Segmento generado automáticamente por Yalo Intelligence — tiendas sin visita ese día.
Naming de campaña asociada: `Caso_de_uso_No_visita_{día}_Yalo_Intelligence`

---

## 🧱 JSONLogic base

### Segmento por DIA_VISITA2 — un día
```json
{
  "==": [
    { "var": "DIA_VISITA2" },
    "Viernes"
  ]
}
```

### Segmento por DIA_VISITA2 — múltiples días
```json
{
  "in": [
    { "var": "DIA_VISITA2" },
    ["Miercoles", "Jueves"]
  ]
}
```

### Segmento Engagedprospects (base + día)
```json
{
  "and": [
    {
      "==": [
        { "var": "DIA_VISITA2" },
        "Viernes"
      ]
    },
    {
      "==": [
        { "var": "is_active" },
        true
      ]
    }
  ]
}
```

> ⚠️ Verificar el JSONLogic exacto para Engagedprospects con el equipo de datos — puede incluir criterios adicionales de recencia o frecuencia de pedidos.

---

## 🆕 Cómo solicitar un segmento nuevo

1. **Verificar que no existe** → `cdp segments list --account-id 63ceb48fadbef7545f69f806`
2. **Definir la lógica** usando los campos CDP disponibles
3. **Usar el bloque de solicitud** abajo → enviar a Marcela Colorado por `#b2b-unilever-hpc-ec`
4. **Confirmar nombre** sigue la convención `Promo_Ciclo{n}_{mes}{año}: DIA_VISITA2 {op} {día}`

### Bloque de solicitud — nuevo segmento

```
SOLICITUD DE SEGMENTO CDP — Unilever Ecuador HPC
Cuenta: Unilever Ecuador HPC
CDP UUID: 63ceb48fadbef7545f69f806
Entity type: stores

Nombre propuesto: [seguir convención — ej. Promo_Ciclo2_Jun26: DIA_VISITA2 == Lunes]
Lógica:
  - DIA_VISITA2 == "{Día}"
  [+ condiciones adicionales si aplica]

JSONLogic:
{
  "==": [{ "var": "DIA_VISITA2" }, "{Día}"]
}

Campaña asociada: [nombre de la campaña que usará este segmento]
Fecha necesaria: [cuándo debe estar listo]
Notas: [contexto adicional]
```

---

## ⚠️ Gotchas críticos

### 🔴 `DIA_VISITA2` — NO `DIA_VISITA`
```
✅ CORRECTO:   { "var": "DIA_VISITA2" }
❌ INCORRECTO: { "var": "DIA_VISITA" }
```
Usar `DIA_VISITA` (sin "2") produce segmentos vacíos o con datos incorrectos. Este es el error más frecuente.

### 🔴 Case de los valores de días — verificar
Los valores de `DIA_VISITA2` probablemente son Title Case (`Lunes`, `Miercoles`) pero **verificar con**:
```bash
cdp stores list --account-id 63ceb48fadbef7545f69f806 --limit 10 | grep DIA_VISITA2
```
Si son minúsculas (`lunes`, `miercoles`), el JSONLogic debe usar minúsculas exactas.

### 🟡 Miércoles sin tilde
En los nombres de campaña y posiblemente en los valores del atributo, se usa `Miercoles` (sin tilde). Verificar el valor real en CDP antes de construir el JSONLogic.

### 🟡 Entity type = `stores`
Este bot usa entidades tipo `stores`, no `contacts`. Los comandos CDP deben usar:
```bash
cdp stores list --account-id 63ceb48fadbef7545f69f806
cdp stores describe --account-id 63ceb48fadbef7545f69f806
```

### 🟡 CDP Version V1 (assumed)
La versión CDP está asumida como V1. Verificar con:
```bash
cdp accounts get --account-id 63ceb48fadbef7545f69f806
```
Si es V2, la estructura de campos puede variar.

### 🟡 Engagedprospects — criterios no confirmados
El subgrupo `Engagedprospects` aparece en campañas con alta conversión pero los criterios exactos de la lógica CDP no están documentados. Consultar con el equipo de datos antes de replicar.
