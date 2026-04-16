
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
  
### 2.2 Ingeniería de Variables
El dataset original de la EPA contenía más de 20 columnas (coordenadas, códigos de método, códigos CBSA, etc.). Para evitar el sobreajuste y el "ruido" estadístico, se realizó una limpieza profunda seleccionando únicamente 3 variables lógicas que afectan la concentración:

1.  Temporalidad Lineal (Tendencia): El paso del tiempo en días.
2.  Ciclicidad (Estaciones): El momento del año en el que nos encontramos.
3.  Ubicación Geográfica: El sitio específico de monitoreo.

### 2.3 Tratamiento de la Fecha y Estacionalidad
Para que el modelo lineal pudiera "leer" la fecha sin confundirse, el tiempo se separó en dos dimensiones:

- Días Transcurridos: Se calculó el número de días desde el inicio del estudio (01/01/2018). Esto permite al modelo detectar si el ozono está subiendo o bajando de forma general a través de los años.

- Transformación Trigonométrica (Seno/Coseno): Para capturar las estaciones, el día del año (1-365) se transformó en coordenadas circulares, permitiendo una curva de predicción fluida.

```python
# Tratamiento de Variables
fecha_inicio = df['Date'].min() 
df['Dias_Transcurridos'] = (df['Date'] - fecha_inicio).dt.days
df['Año'] = df['Date'].dt.year
df['Dia_Anio'] = df['Date'].dt.dayofyear
df['Seno_Anual'] = np.sin(2 * np.pi * df['Dia_Anio'] / 365.25)
df['Coseno_Anual'] = np.cos(2 * np.pi * df['Dia_Anio'] / 365.25)
df['Dia_Semana'] = df['Date'].dt.dayofweek.astype(str)
```
- One-Hot Encoding (OHE): Se transformó la variable categórica Local Site Name en variables binarias independientes, permitiendo que el modelo asigne un coeficiente específico a cada ubicación geográfica en Florida.

```python
X = pd.get_dummies(X, columns=['Dia_Semana', 'Local Site Name'], drop_first=True, dtype=int)
X = sm.add_constant(X)
```

### 2.4 Predicción a Largo Plazo (Simulación de Futuro)
Finalmente, se realizó la predicción definitiva del año 2023. El modelo se apoya exclusivamente en la matemática del calendario (Seno/Coseno) y en la tendencia histórica de los 5 años previos. El objetivo de este subpunto es demostrar que el modelo puede proyectar la "montaña estacional" completa del próximo año para fines de planeación a largo plazo.

## 3. Resultados

### 3.1 Estadísticos del Modelo
El modelo fue ajustado mediante Mínimos Cuadrados Ordinarios (OLS) con los siguientes resultados:

| Métrica | Valor obtenido |
| :--- | :--- |
| **R-cuadrado (Entrenamiento)** | 0.1792 |
| **Error Cuadrático Medio (MSE)** | 0.000097 |
| **Variables Predictoras (X)** | 77 |

### 3.2 Visualización de la Predicción Ciega (2023)
La validación comparó la predicción "ciega" del modelo (línea morada) contra la realidad (línea verde).
![Modelo Estacional de 2023](https://github.com/NicolasChuquista/PI_Equipo02/blob/main/img/Modelo.png?raw=true)
