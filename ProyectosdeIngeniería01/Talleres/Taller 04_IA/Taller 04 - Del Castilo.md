
# Análisis de Regresión: Ozono Troposférico en Florida (2018–2023)

## 1. Introducción
Este informe analiza la dinámica del ozono troposférico (O₃) en el estado de Florida utilizando datos históricos de la EPA. El objetivo es desarrollar un modelo de regresión lineal capaz de capturar la estacionalidad del contaminante y evaluar su capacidad predictiva, entrenando el modelo con datos de 2018-2022 y validándolo con el año 2023.

---

## 2. Metodología

## 2.1 Preparación de Datos y Validación Temporal
Se integraron seis conjuntos de datos diarios para consolidar una base de 120,548 registros. 
Para garantizar la validez del modelo, se aplicó una partición temporal estricta:

- Entrenamiento: 2018 – 2022 (El modelo aprende los ciclos históricos).

- Prueba (Test): 2023 (Evaluación a ciegas del modelo).
### 2.1 Ingeniería de Variables
Para mejorar la precisión de la **Regresión Lineal**, se aplicaron transformaciones matemáticas avanzadas:

* **Trigonometría (Seno/Coseno):** Para capturar la estacionalidad anual.
* **One-Hot Encoding:** Para diferenciar el impacto de cada estación de monitoreo (`Local Site Name`).

### 2.2 Código Principal (Python)
