[README.md](https://github.com/user-attachments/files/25403585/README.md)
# 📦 Shipper Perú · Sistema Operativo v3.3

Sistema web para gestión operativa semanal de paquetería USA → Perú. Genera facturas Zoho, mensajes WhatsApp y valida inventarios — todo desde un solo archivo HTML, sin instalación.

---

## ¿Qué hace?

| Módulo | Función |
|--------|---------|
| 🔍 **Validación** | Compara Warehouse vs Inventario, detecta faltantes e inconsistencias |
| 🚛 **Facturación** | Genera facturas Zoho + mensajes WhatsApp por cliente |
| 📊 **Análisis** | Dashboard con métricas del lote semanal |

---

## Uso rápido

1. Descarga `index.html`
2. Ábrelo en Chrome, Firefox o Edge
3. Carga tu archivo de inventario y procesa

No requiere instalación, servidor ni conexión a internet (después de la primera carga).

---

## Módulo 1 — Validación de Inventarios

Sube dos archivos simultáneamente:

**ListadoWarehouse.xlsx** — columnas requeridas:
```
WR | Tracking | Cliente | Tipo Paquete | Peso | Estado
```
> Solo se procesan registros con `Estado = RECIBIDO`

**INVENTARIO.csv** — columnas requeridas:
```
numero_warehouse | numero_tracking | nombre_consignado | tipo_paquete | peso_usa
```

El sistema compara ambos y reporta:
- **Faltantes** — WR que están en un sistema pero no en el otro
- **Inconsistencias** — registros presentes en ambos pero con datos distintos (tracking, cliente, tipo, peso)

Exporta los resultados a CSV con un clic.

---

## Módulo 2 — Facturación y Mensajería

**Archivo de entrada** (CSV o Excel):
```
nombre_cliente | numero_tracking | peso_usa | descripcion |
fecha_registro_usa | nombre_consignado | tipo_paquete | nombre_tienda | valor
```

### Lógica de facturación

**Envío:** `$10.00 × peso total (kg)` por cliente

**Desaduanaje — paquetes con valor ≤ $200:**

| Rango valor acumulado | Concepto | Precio |
|----------------------|----------|--------|
| $0 – $50 | DESADUANAJE 0-50$ | $4.00 |
| $51 – $100 | DESADUANAJE 50-100$ | $7.00 |
| $101 – $200 | DESADUANAJE 100-200$ | $11.00 |

**Desaduanaje — paquetes con valor > $200 (Viajero):**

| Concepto | Cálculo |
|----------|---------|
| SERVICIO VIAJERO | 10% del valor declarado |
| SEGURO VIAJERO | 1% del valor declarado |

### Salidas generadas

- **Facturas CSV** — 47 columnas en formato Zoho Invoice, listo para importar
- **Mensajes CSV** — Un mensaje WhatsApp personalizado por cliente con tracking, peso, días hábiles y fecha de entrega
- **Resumen** — Tabla por cliente + gráficos Top 5 (doughnut de ingresos + barras de peso/paquetes)

---

## Tecnologías

- HTML + CSS + JavaScript vanilla (sin frameworks)
- [Chart.js 4.4](https://www.chartjs.org/) — gráficos
- [PapaParse 5.4](https://www.papaparse.com/) — lectura de CSV
- [SheetJS 0.18](https://sheetjs.com/) — lectura de Excel
- Fuentes: Montserrat + JetBrains Mono (Google Fonts)

Todo se carga desde CDN. El archivo HTML es completamente autocontenido.

---

## Privacidad

Los archivos que subes **nunca salen de tu navegador**. Todo el procesamiento ocurre localmente. No hay backend, no hay base de datos, no se envía ningún dato a servidores externos.

---

## Estructura del repositorio

```
/
└── index.html    ← El sistema completo
└── README.md     ← Este archivo
```

---

*Shipper Perú · Uso interno*
