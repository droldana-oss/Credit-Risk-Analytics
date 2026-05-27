# Power BI - Dashboard De Riesgo Crediticio

## Objetivo

Esta carpeta contiene el dashboard ejecutivo construido con el archivo local
`data/clean/credit_risk_clean.csv`. Presenta patrones descriptivos de
dificultad de pago y exposición financiera; no representa un modelo predictivo
ni un sistema de aprobación automática.

## Vista Previa

![Vista general del dashboard de riesgo crediticio](../images/credit_risk_dashboard_overview.png)

El archivo editable se encuentra en
[`archivo de power bi.pbix`](archivo%20de%20power%20bi.pbix).

## Insumo Y Reproducción

1. Ejecutar los notebooks `01` a `04` en orden.
2. Confirmar que se generó `data/clean/credit_risk_clean.csv`.
3. En Power BI Desktop, seleccionar **Obtener datos > Texto/CSV** y cargar ese archivo.
4. Nombrar la tabla como `credit_risk_clean`.

El CSV se mantiene local y está ignorado en Git porque conserva datos a nivel
de solicitud. Al publicar el repositorio, usar capturas y resultados agregados.

## Medidas DAX

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

## Vista Ejecutiva Implementada

| Elemento | Lectura presentada |
|---|---|
| Tarjetas KPI | Total de clientes, ingreso promedio, crédito promedio y tasa de incumplimiento |
| Barras por género | Diferencia observada de incumplimiento entre categorías |
| Barras por tipo de préstamo | Comparación entre préstamos tradicionales y créditos revolving |
| Dispersión | Relación entre ingreso y monto del crédito, distinguida por `TARGET` |
| Barras por educación | Comparación agregada de dificultad de pago por nivel educativo |
| Barras por tipo de ingreso | Perfiles laborales con mayores tasas observadas |
| Barras por hijos | Variación de la tasa según cantidad de dependientes |

La vista está diseñada para comunicar asociaciones históricas por segmento.
Las comparaciones sociodemográficas no deben convertirse en criterios aislados
para decisiones crediticias.

## Validaciones De Presentación

- Verificar que los totales coincidan con el reporte de Python: 307.511
  solicitudes y 8,07% de dificultad de pago.
- Validar que cada gráfico muestre volumen además de tasa cuando se comparen
  segmentos pequeños.
- Ocultar cualquier identificador si se añaden otras tablas en una extensión.
- Mantener la captura `images/credit_risk_dashboard_overview.png` actualizada
  si el archivo Power BI recibe cambios.

## Archivo Entregado

El entregable versionado en esta carpeta es:

```text
powerbi/archivo de power bi.pbix
```

Para un control de versiones más detallado, también se puede guardar como
proyecto Power BI (`.pbip`) y versionar su estructura. Esta opción puede
requerir habilitar la característica de proyecto en Power BI Desktop. El
`.gitignore` ya excluye los archivos locales de caché de `.pbi`; aun así, se
debe revisar siempre que no se publique información que no deba estar expuesta.
