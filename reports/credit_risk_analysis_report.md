# Reporte de Análisis de Riesgo Crediticio

## Alcance

Este reporte describe patrones históricos en solicitudes de Home Credit. No se
entrenó ningún modelo, no se genera scoring individual y los segmentos no
constituyen reglas de aprobación o rechazo.

## KPIs Generales

| Indicador | Resultado |
|---|---:|
| Total de solicitudes | 307.511 |
| Solicitudes con dificultad de pago | 24.825 |
| Tasa de dificultad de pago | 8,07% |
| Ingreso promedio | 168.797,92 |
| Crédito promedio | 599.026,00 |
| Ratio crédito/ingreso promedio | 3,96x |
| Edad promedio | 43,9 años |

## Hallazgos

- El segmento de ingreso con mayor tasa observada es **Medio bajo** con **8,53%**.
- El rango de ratio crédito/ingreso con mayor tasa es **2x-4x** con **8,87%**; el comportamiento no es monotónico.
- El grupo de edad con mayor tasa es **Hasta 25** con **12,11%**.
- Considerando ocupaciones con al menos 1.000 solicitudes, **Low-skill Laborers** muestra la tasa más alta: **17,15%**.
- El perfil de exposición financiera creado no separa fuertemente la tasa de dificultad; debe mostrarse como descriptor de cartera, no como clasificación de riesgo.

## Uso para Power BI

El dashboard debe destacar tasa general, composición de cartera y comparaciones
por segmento. Las vistas sociodemográficas sirven para análisis agregado y
discusión de políticas responsables, nunca para automatizar decisiones.

## Limitaciones

- Se analiza únicamente `application_train.csv`, sin historiales adicionales.
- Las asociaciones observadas no prueban causalidad.
- El dataset proviene de una competencia pública y no representa una política
  crediticia productiva de una entidad específica.
