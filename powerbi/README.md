# Power BI - To Do Del Dashboard De Riesgo Crediticio

## Objetivo

Esta carpeta queda preparada para que construya posteriormente un dashboard
ejecutivo con el archivo local `data/clean/credit_risk_clean.csv`. El dashboard
debe presentar patrones descriptivos de dificultad de pago y exposición
financiera; no debe mostrarse como un modelo predictivo ni un sistema de
aprobación automática.

## Insumo

1. Ejecutar los notebooks `01` a `04` en orden.
2. Confirmar que se generó `data/clean/credit_risk_clean.csv`.
3. En Power BI Desktop, seleccionar **Obtener datos > Texto/CSV** y cargar ese archivo.
4. Nombrar la tabla como `credit_risk_clean`.

El CSV se mantiene local y está ignorado en Git porque conserva datos a nivel
de solicitud. Al publicar el repositorio, usar capturas y resultados agregados.

## Medidas DAX Recomendadas

```DAX
Total Solicitudes =
COUNTROWS(credit_risk_clean)

Clientes Con Dificultad =
CALCULATE(
    COUNTROWS(credit_risk_clean),
    credit_risk_clean[TARGET] = 1
)

Tasa Dificultad =
DIVIDE([Clientes Con Dificultad], [Total Solicitudes])

Ingreso Promedio =
AVERAGE(credit_risk_clean[AMT_INCOME_TOTAL])

Credito Promedio =
AVERAGE(credit_risk_clean[AMT_CREDIT])

Ratio Credito Ingreso Promedio =
AVERAGE(credit_risk_clean[CREDIT_INCOME_RATIO])

Edad Promedio =
AVERAGE(credit_risk_clean[AGE])
```

Formatear `Tasa Dificultad` como porcentaje con dos decimales y los montos
como moneda o número con separador de miles, según el estilo elegido.

## Páginas Sugeridas

### Página 1 - Vista Ejecutiva

| Elemento | Configuración |
|---|---|
| Tarjetas | Total solicitudes, tasa dificultad, crédito promedio, ingreso promedio |
| Dona o barra | `TARGET_LABEL` por total de solicitudes |
| Barras | Tasa de dificultad por `INCOME_SEGMENT` |
| Filtros | Tipo de contrato, educación, ocupación y vivienda |

Mensaje: la cartera tiene una tasa general de dificultad de pago, y el
dashboard permite ubicar diferencias agregadas entre segmentos.

### Página 2 - Perfil Financiero

| Visual | Campos |
|---|---|
| Columnas | `CREDIT_INCOME_SEGMENT` y medida `Tasa Dificultad` |
| Barras | `INCOME_SEGMENT` y medida `Tasa Dificultad` |
| Dispersión | `AMT_INCOME_TOTAL` vs. `AMT_CREDIT`, leyenda `TARGET_LABEL` |
| Tabla | `ANALYTICAL_PROFILE`, solicitudes y tasa |

Mensaje: el ratio crédito/ingreso describe exposición, pero en el análisis no
presentó un aumento monotónico de la dificultad de pago.

### Página 3 - Perfil Sociodemográfico

| Visual | Campos |
|---|---|
| Barras | `AGE_GROUP` y tasa dificultad |
| Barras horizontales | `OCCUPATION_TYPE` y tasa; filtrar grupos con suficiente volumen |
| Barras | `NAME_EDUCATION_TYPE` y tasa |
| Comparación | `FLAG_OWN_REALTY` y tasa |

Mensaje: estas variables muestran asociaciones históricas agregadas; no deben
utilizarse aisladamente para aprobar o negar solicitudes.

### Página 4 - Conclusiones Y Seguimiento

| Elemento | Contenido |
|---|---|
| Tabla priorizada | Segmento, total solicitudes, clientes con dificultad, tasa |
| Tarjeta | Segmento de mayor tasa visible bajo filtros |
| Cuadro de texto | Limitaciones del análisis y próximos pasos |
| Navegación | Botones a las otras tres páginas |

## Validaciones Antes De Publicar

- Verificar que los totales coincidan con el reporte de Python: 307.511
  solicitudes y 8,07% de dificultad de pago.
- Validar que cada gráfico muestre volumen además de tasa cuando se comparen
  segmentos pequeños.
- Ocultar cualquier identificador si se añaden otras tablas en una extensión.
- Exportar capturas a `reports/dashboard_screenshots/` para que GitHub pueda
  mostrar el dashboard sin abrir Power BI.

## Archivo A Guardar

Guardar el entregable en esta carpeta:

```text
powerbi/credit_risk_dashboard.pbix
```

Para un control de versiones más detallado, también se puede guardar como
proyecto Power BI (`.pbip`) y versionar su estructura. Esta opción puede
requerir habilitar la característica de proyecto en Power BI Desktop. El
`.gitignore` ya excluye los archivos locales de caché de `.pbi`; aun así, debo
revisar siempre que no se publique información que no deba estar expuesta.
