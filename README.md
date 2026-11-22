# telecom-analysis

Análisis ConnectaTel
Objetivo del proyecto

Evaluar el comportamiento de los clientes de ConnectaTel, una empresa de telecomunicaciones en Latinoamérica, con información registrada hasta 2024. El análisis construye un perfil estadístico de los usuarios, detecta comportamientos atípicos (outliers) y segmenta a los clientes por edad y nivel de uso, con el fin de identificar patrones de consumo, diseñar estrategias de retención y sugerir mejoras en los planes ofrecidos.

Datasets utilizados
Archivo	Descripción	Tamaño
plans.csv	Planes actuales: precio mensual, mensajes, minutos y GB incluidos, costo por extra (planes Básico y Premium)	2 filas
users_latam.csv	Clientes: user_id, nombre, edad, ciudad, fecha de registro (reg_date), plan y fecha de baja (churn_date)	4,000 filas
usage.csv	Uso real de los servicios: llamadas (call, con duration) y mensajes (text, con length)	40,000 filas
Etapas del análisis
Carga y exploración: lectura de los tres datasets, revisión de estructura con .head(), .shape e .info().
Calidad de datos: conteo de nulos, detección de valores centinela y revisión de fechas fuera de rango.
Limpieza:
age: 55 registros (1.4%) con el centinela -999, reemplazados por la mediana de edades válidas.
reg_date: 40 registros (1.0%) con año 2026 (fuera del periodo), convertidos a nulo (NaT).
city: 469 nulos (11.7%), conservados como NaN.
usage.duration: 22,076 nulos (55.2%) que corresponden a mensajes de texto, que no tienen duración. Se verificó con groupby('type') que no son errores de captura.
Métricas por usuario: agregación de usage por user_id (cant_mensajes, cant_llamadas, cant_minutos_llamada) y unión con users.
Resumen estadístico y distribuciones: estadísticos descriptivos, distribución porcentual de plan e histogramas por plan.
Outliers: boxplots y método IQR. Se encontraron 46 outliers en mensajes, 30 en llamadas y 96 en minutos de llamada; se conservaron por ser comportamientos plausibles de usuarios intensivos.
Segmentación:
grupo_uso: Bajo uso, Uso medio y Alto uso, según llamadas y mensajes.
grupo_edad: Joven (<30), Adulto (30 a 59) y Adulto Mayor (60 o más).
Insight ejecutivo: hallazgos y recomendaciones para el negocio (revisar la propuesta de valor del plan Premium, crear planes para usuarios intensivos y de bajo uso, enfocar la retención en Uso medio y monitorear consumo extremo).
Cómo ejecutar el notebook
Opción A: Google Colab
Abre Google Colab y elige Archivo → Subir cuaderno para cargar el .ipynb.
Sube los tres CSV al entorno de Colab (panel de archivos de la izquierda).
Ajusta las rutas de lectura si es necesario. El notebook usa /datasets/plans.csv, /datasets/users_latam.csv y /datasets/usage.csv; en Colab, cámbialas por la ruta donde subiste los archivos (por ejemplo plans.csv).
Ejecuta todas las celdas con Entorno de ejecución → Ejecutar todo.

Guía de reproducción
Requisitos: Python 3.9 o superior, con pandas, numpy, seaborn y matplotlib.
Orden de ejecución: las celdas dependen unas de otras (por ejemplo, user_profile se crea en el Paso 4 y se usa en los pasos 5 y 6), así que deben ejecutarse de arriba hacia abajo.
Rutas: verifica que las rutas de los CSV coincidan con tu entorno.
Resultados esperados: 3,999 usuarios en user_profile, con distribución de uso de aproximadamente 73.6% Uso medio, 19.5% Bajo uso y 7.0% Alto uso, y distribución de edad de 50.4% Adulto, 30.6% Adulto Mayor y 19.0% Joven.
