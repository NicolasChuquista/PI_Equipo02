# Taller 04 - IA
## Rabanal Bravo Flavio Francisco

---

## 1. Introducción
La contaminación atmosférica por material particulado fino (PM2.5) representa uno de los principales riesgos para la salud pública, debido a su capacidad de penetrar profundamente en el sistema respiratorio. El análisis temporal de este contaminante permite identificar patrones de variabilidad y tendencias que son fundamentales para la gestión ambiental y la toma de decisiones.

En este estudio se analizan datos de concentraciones diarias de PM2.5 correspondientes al estado de Alabama para los años 2022 y 2023. Los datos fueron obtenidos a partir de la plataforma oficial de la United States Environmental Protection Agency (EPA), específicamente desde su portal de descarga de calidad de aire.

El objetivo es modelar el comportamiento temporal del PM2.5 mediante un enfoque estacional y generar una predicción para el año 2024. Dado que este tipo de series suele presentar patrones periódicos, se emplea un modelo trigonométrico que permite capturar la estacionalidad anual.

---

## 2. Metodología
### 2.1 Fuente de Datos
Los datos fueron descargados desde la plataforma oficial de la:

- **United States Environmental Protection Agency (EPA)**  
- Plataforma: *Outdoor Air Quality Data*  
- Herramienta utilizada: *Download Daily Data*  

Desde esta fuente se descargaron los registros diarios de PM2.5 correspondientes a los años 2022 y 2023 para el estado de Alabama. Posteriormente, ambos conjuntos de datos fueron integrados en un único dataset para su análisis.

---

### 2.2 Procesamiento de Datos
El tratamiento de los datos incluyó las siguientes etapas:

- Conversión de la variable fecha a formato `datetime`  
- Eliminación de valores faltantes  
- Agrupación por fecha para obtener el promedio diario de PM2.5 (considerando múltiples estaciones)  

---

### 2.3 Modelo Utilizado
Se implementó un modelo de regresión lineal multivariable con componente estacional trigonométrica, definido como:

![Modelo Taller 04](https://raw.githubusercontent.com/NicolasChuquista/PI_Equipo02/main/img/Rabanal%20_Flavio_Modelo_Taller04.png)

Donde:
- t: representa la tendencia temporal (en días)
- m: corresponde al mes del año
- Las funciones seno y coseno permiten modelar la estacionalidad anual

### 2.4 Entrenamiento y predicción
Periodo de entrenamiento: 2022–2023
Periodo de predicción: 2024 (generado de forma sintética)

El modelo fue ajustado mediante el método de mínimos cuadrados ordinarios (OLS).

## 3. Resultados
El modelo permitió ajustar una curva estacional suave que representa el comportamiento general del PM2.5, así como generar una predicción continua para el año 2024.

![Resultado](https://raw.githubusercontent.com/NicolasChuquista/PI_Equipo02/main/img/Flavio_Rabanal_Multivariable.png)


## 4. Discusión
El modelo estacional trigonométrico logra representar de manera adecuada el comportamiento general del PM2.5, particularmente su componente estacional. La presencia de ciclos anuales bien definidos valida el uso de funciones seno y coseno para este tipo de análisis.

No obstante, el modelo presenta limitaciones importantes. En primer lugar, no incorpora variables explicativas externas como condiciones meteorológicas (temperatura, velocidad del viento, humedad) o factores antropogénicos, los cuales influyen directamente en la concentración de PM2.5. Esto limita la capacidad del modelo para capturar eventos extremos o fluctuaciones abruptas.

Asimismo, al tratarse de un modelo lineal, su capacidad para representar dinámicas complejas es reducida. En consecuencia, aunque el modelo es adecuado para describir tendencias generales y realizar predicciones básicas, su precisión podría mejorarse mediante la inclusión de variables adicionales o el uso de modelos más avanzados.

## 5. Referencias
- United States Environmental Protection Agency. (2023). Outdoor Air Quality Data.
