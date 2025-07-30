
UNIVERSIDAD DE LA SALLE  
Maestría en Analítica e Inteligencia de Negocios  
Proyecto de Grado  
Título: Patrones de comportamientos contrarios a la convivencia en Bogotá (2024), mediante MCA y K-Modes, para la formulación de políticas públicas  
Autor: Claudia Judith Rodríguez Ladino  
Versión del archivo: Julio 2025  
Archivo: README_estructura_analitica.txt  
Descripción: Documento explicativo del flujo analítico, estructura y etapas del procesamiento aplicado a la base de datos "ComparendosNS_limpio.csv"  

--------------------------------------------------------------------------------
README – ESTRUCTURA ANALÍTICA DEL PROYECTO  
--------------------------------------------------------------------------------

Este archivo describe el flujo de trabajo ejecutado para el análisis de los comparendos asociados a comportamientos contrarios a la convivencia en Bogotá D.C., en el año 2024, a partir de técnicas de análisis factorial múltiple (MCA) y agrupamiento categórico (K-Modes).

La estructura se desarrolla en el entorno Python (Jupyter Notebook) y contempla desde la limpieza inicial hasta la interpretación de resultados y validación de perfiles.

--------------------------------------------------------------------------------
ETL – Extracción, Transformación y Carga de Datos
--------------------------------------------------------------------------------

- **Fuente de datos**: Registro Nacional de Medidas Correctivas (RNMC), año 2024.
- **Base inicial**: 80.190 registros
- **Registros finales tras depuración**: 80.164 (depuración mínima del 0,03 %)

**Procesos de transformación aplicados:**
- Conversión de variables a texto plano, eliminación de espacios y normalización con `.str.lower().str.strip()`.
- Conversión de columnas clave a tipo string con `.astype(str)`.
- Reemplazo de valores atípicos o incorrectos mediante `.replace()`.
- Verificación de registros con longitudes inusuales en cadenas.
- Eliminación de registros nulos en variables críticas para el análisis.
- Generación de nuevas variables para análisis temporal y contextual:
  - `HORA`, `MES`, `DIA_SEMANA`, `HORA_AGRUPADA`, `FECHA_SOLAMENTE`, 'GRUPO'ETAREO'

Este proceso permitió mejorar la calidad de los datos y asegurar su consistencia para los análisis posteriores.

--------------------------------------------------------------------------------
ETAPAS DEL ANÁLISIS
--------------------------------------------------------------------------------

**1. Análisis exploratorio univariado (descriptivo):**  
Se examinaron distribuciones individuales de variables como:
- Tipo de comportamiento (`COMPORTAMIENTO_CORTO`)
- Localidad, tipo de lugar
- Hora, día de la semana, mes
- Actividad comercial (`ACTIVIDAD_CLASIFICADA`)  
Esta etapa identificó sesgos, concentraciones y categorías dominantes.

**2. Reducción de dimensionalidad con MCA:**  
Se aplicó Análisis de Correspondencias Múltiples (MCA) sobre variables categóricas seleccionadas.  
Se conservaron las dimensiones que explicaron el 96,1 % de la inercia total acumulada.  
Estas dimensiones sintetizan relaciones estructurales entre categorías como `LOCALIDAD`, `TIPO_LUGAR`, `DIA_SEMANA` y `GRUPO_ETARIO`.

**3. Evaluación de independencia entre variables (Chi²):**  
Se aplicaron pruebas de independencia para confirmar asociaciones entre pares de variables categóricas relevantes, como `LOCALIDAD` y `COMPORTAMIENTO_CORTO`, validando inferencialmente los resultados de MCA.

**4. Agrupamiento con K-Modes:**  
Utilizando las dimensiones generadas por MCA, se aplicó el algoritmo K-Modes para identificar perfiles homogéneos de comportamiento.  
- Número óptimo de clústeres: K=3  
- Criterio: Método del codo sobre costo de disimilitud (no euclidiana)  
- Validación: Coeficiente de silueta con distancia de Hamming

**5. Interpretación de perfiles por clúster:**  
Se caracterizaron los tres clústeres mediante análisis bivariado (tablas cruzadas) según variables como:  
`COMPORTAMIENTO_CORTO`, `LOCALIDAD`, `TIPO_LUGAR`, `HORA_AGRUPADA`, `GRUPO_ETARIO`.  
Esto permitió construir perfiles diferenciados de infractores y escenarios de ocurrencia.

**6. Análisis de sensibilidad (validación):**  
Se evaluó el impacto de la infracción dominante (“Ingreso indebido a estaciones”), que representa el 85 % de los casos.  
Se replicó el análisis excluyéndola, verificando la estabilidad de los perfiles y robustez de la segmentación obtenida.

--------------------------------------------------------------------------------
ARCHIVOS ENTREGADOS
--------------------------------------------------------------------------------
1. `Anexo1_MCA_KMODES.ipynb` – Cuaderno con análisis completo  
3. `README_estructura_analitica.txt` – Este documento  

--------------------------------------------------------------------------------
BIBLIOTECAS UTILIZADAS EN EL PROCESO ANALÍTICO
--------------------------------------------------------------------------------
- `pandas` – Manejo y transformación de datos  
- `numpy` – Cálculos numéricos  
- `matplotlib` y `seaborn` – Visualización de datos  
- `sklearn` – Cálculo de métricas (silhouette_score, pruebas de validación)  
- `kmodes` – Algoritmo K-Modes para clustering categórico  
- `prince` – Análisis de Correspondencias Múltiples (MCA)  

--------------------------------------------------------------------------------
OBSERVACIÓN FINAL
--------------------------------------------------------------------------------
Este README busca cumplir con los principios de **transparencia**, **reproducibilidad** y **trazabilidad** del análisis.  
La estructura detallada y los archivos adjuntos permiten comprender paso a paso la construcción del modelo y replicar sus resultados con base en evidencia y buenas prácticas.
