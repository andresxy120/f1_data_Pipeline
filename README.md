# F1 Data Analytics
Gestión de Datos Masivos · G002 · Institución Universitaria Pascual Bravo

## Objetivo
¿Cómo varía el rendimiento de los pilotos de Fórmula 1 según la velocidad, las revoluciones del motor y el uso del acelerador durante una sesión?

Fuente: https://api.openf1.org/v1/car_data — sesión 9159, 15 de septiembre de 2023, pilotos 55 y 1.

## Arquitectura
OpenF1 → Python → RAW JSONL → Kafka → HDFS → MapReduce / PySpark → Parquet (batch). Kafka → Structured Streaming (speed). Ambas capas → consolidación Lambda → SQLite → dashboard HTML.

## Ejecución
1. Abrir F1_Data_Analytics.ipynb en Google Colab.
2. Ejecutar las fases 1 a 8 en orden, autorizar Google Drive y mantener activa la sesión. El notebook instala/configura los servicios necesarios.
3. Abrir dashboard/index.html en el navegador para consultar la instantánea incluida, o abrir la nueva versión generada en Drive.

## Resultados registrados
| Fase | Evidencia |
|---|---|
| 1–2 | 36.940 registros, 18.470 por piloto |
| 3 | 36.940 mensajes enviados y recibidos, 3 particiones |
| 4 | MapReduce y Spark coinciden: 36.940 |
| 5 | 29.180 registros limpios; reducción de almacenamiento 96,42 % |
| 6 | Primera y segunda carga: 29.180; cero duplicados finales |
| 7 | 135 ventanas antes y después del reinicio; cero mensajes releídos |
| 8 | 139 ventanas consolidadas, cero duplicados, cinco gráficos |

## Alcance y limitaciones
Streaming mediante reproducción histórica, no telemetría en vivo. Ventanas de un minuto, watermark de dos minutos y checkpoint. La consolidación prioriza batch para claves coincidentes. El dashboard es una instantánea, no se actualiza automáticamente. Colab utiliza servicios locales y no representa un clúster distribuido de producción. La comparación de pilotos es descriptiva, no prueba superioridad deportiva.

## Archivos pendientes de incorporar desde Drive
- servicio/f1_analytics.db y los tres CSV de indicadores.
- evidencias/ (JSON de las fases), manifiestos/ y resultados MapReduce.
- RAW y Parquet si la guía exige adjuntarlos; no publicar credenciales ni checkpoints innecesarios.

Los resultados están tomados de las salidas guardadas en el notebook y el HTML entregado. No se ha repetido la ejecución para preparar este paquete.
