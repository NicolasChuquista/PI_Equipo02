# Análisis de Regresión de Concentraciones de Ozono Troposférico en Nueva York (2023–2025)

**Autor:** [Tu nombre]  
**Curso:** [Nombre del curso]  
**Fecha:** Abril 2026  

---

## 1. Introducción

El ozono troposférico (O₃) es un contaminante secundario que se forma en la atmósfera baja mediante reacciones fotoquímicas entre óxidos de nitrógeno (NOₓ) y compuestos orgánicos volátiles (COV) en presencia de radiación solar [1]. A diferencia del ozono estratosférico, que cumple una función protectora frente a la radiación ultravioleta, el ozono a nivel del suelo representa un riesgo significativo para la salud pública, asociado con enfermedades respiratorias, reducción de la función pulmonar y agravamiento del asma [2].

El estado de Nueva York, con su combinación de zonas urbanas densamente pobladas, corredores industriales y áreas rurales y montañosas, constituye un caso de estudio relevante para el monitoreo de este contaminante. La Agencia de Protección Ambiental de los Estados Unidos (EPA) establece mediante el estándar NAAQS (*National Ambient Air Quality Standards*) un límite de 0.070 ppm como concentración máxima promedio de 8 horas para el ozono [3].

El presente informe analiza la evolución de las concentraciones diarias máximas de ozono en 28 estaciones de monitoreo del estado de Nueva York durante el período 2023–2025, mediante tres modelos de regresión de complejidad creciente. Se proyectan además concentraciones para el año 2026.

---

## 2. Metodología

### 2.1 Fuente de Datos

Los datos fueron obtenidos del portal **EPA AirData** (*Outdoor Air Quality Data – Download Daily Data*), disponible en `https://www.epa.gov/outdoor-air-quality-data` [4]. Se descargaron tres archivos de datos diarios correspondientes a los años 2023, 2024 y 2025 para el contaminante ozono (código AQS 44201) en el estado de Nueva York.

| Año | Registros | Fuente principal |
|-----|-----------|-----------------|
| 2023 | 9,452 | AQS |
| 2024 | 9,530 | AQS |
| 2025 | 9,620 | AQS / AirNow |
| **Total** | **28,602** | |

Los datos AQS (*Air Quality System*) corresponden a mediciones verificadas y certificadas. Un subconjunto de 413 registros de 2025 proviene de AirNow, sistema de datos en tiempo casi-real sin validación definitiva [5]. Tras la integración se verificó la ausencia total de duplicados y valores nulos en la variable objetivo.

### 2.2 Esquema de Entrenamiento y Validación

Se adoptó una partición temporal (*temporal train/test split*) coherente con la naturaleza secuencial de datos ambientales diarios [6]:

```
Entrenamiento:  2023 – 2024  →  19,082 registros  (el modelo aprende)
Prueba (test):  2025         →   9,520 registros  (el modelo es evaluado)
Proyección:     2026         →   365 días         (el modelo predice)
```

Este enfoque evita la fuga de información temporal (*data leakage*): el modelo nunca ve datos futuros durante el entrenamiento [7]. Se eligió 2025 como test por ser el año más reciente con datos completos, mientras que 2026 se reservó para la proyección final.

### 2.3 Variables y Transformaciones

| Variable | Tipo | Transformación | Justificación |
|---|---|---|---|
| `Year` | Numérica continua | Sin transformación | Captura tendencia interanual |
| `DayOfYear` | Numérica (1–365) | **Trigonométrica** | Captura ciclo estacional continuo |
| `Site ID` | Categórica (28 sitios) | **One-Hot Encoding** | Captura diferencias geográficas sin orden falso |

**Transformación trigonométrica del día del año:**

Para capturar la estacionalidad de forma continua y matemáticamente correcta, el día del año se transformó en dos componentes armónicas [8]:

```
sin_day = sin(2π × DayOfYear / 365)
cos_day = cos(2π × DayOfYear / 365)
```

Esta representación es superior a los dummies de mes porque: (1) trata el día como variable cíclica (el día 365 es "adyacente" al día 1), (2) genera una curva suave sin saltos bruscos entre meses, y (3) reduce el número de parámetros a estimar de 11 dummies a solo 2 coeficientes.

**One-Hot Encoding del sitio de monitoreo:**

La variable `Site ID` fue codificada mediante OHE, generando una columna binaria independiente por cada una de las 28 estaciones. Esto evita que el modelo interprete un orden numérico inexistente entre estaciones [9].

**Variable objetivo:** `Daily Max 8-hour Ozone Concentration` (ppm)

### 2.4 Modelos Ajustados

Se ajustaron tres modelos de regresión por mínimos cuadrados ordinarios (OLS) con complejidad creciente:

| Modelo | Features | Variables |
|---|---|---|
| M1 — Lineal simple | 1 | Tiempo ordinal |
| M2 — Múltiple + OHE | 72 | Year + dummies mes/día semana + OHE(Site, County, Source) |
| **M3 — Trigonométrico + OHE** | **31** | **Year + sin(día) + cos(día) + OHE(Site)** ★ |

El Modelo 3 es el modelo final recomendado por combinar parsimonia (menos variables), correcta representación de la ciclicidad anual, y el mejor desempeño en el conjunto de prueba.

Las métricas de evaluación utilizadas fueron el coeficiente de determinación R² y la raíz del error cuadrático medio (RMSE).

---

## 3. Resultados

### 3.1 Estadísticas Descriptivas

| Métrica | 2023 | 2024 | 2025 |
|---|---|---|---|
| Registros | 9,452 | 9,530 | 9,620 |
| Media (ppm) | 0.0377 | 0.0375 | 0.0382 |
| Mediana (ppm) | 0.0370 | 0.0370 | 0.0370 |
| Máximo (ppm) | 0.0860 | 0.0860 | 0.0870 |
| Mínimo (ppm) | 0.0000 | 0.0010 | 0.0050 |
| Desv. estándar | 0.0115 | 0.0108 | 0.0105 |

**Distribución AQI por año:**

| Categoría | 2023 | 2024 | 2025 |
|---|---|---|---|
| 🟢 Good (0–50) | 8,786 | 8,865 | 8,921 |
| 🟡 Moderate (51–100) | 591 | 622 | 640 |
| 🟠 Unhealthy for Sensitive (101–150) | 73 | 42 | 56 |
| 🔴 Unhealthy (>150) | 2 | 1 | 3 |

### 3.2 Serie Temporal y Modelo Estacional

![Serie temporal completa con modelo estacional](../../../img/fig1_serie_temporal.png)

*Figura 1. Concentración diaria promedio de ozono (28 estaciones) y curva del modelo trigonométrico estacional. La línea vertical roja separa el período de entrenamiento (2023–2024) del período de prueba (2025).*

La Figura 1 muestra que el modelo captura correctamente el patrón estacional — picos en verano y mínimos en invierno — y que su desempeño en el período de prueba (2025) es consistente con el entrenamiento.

### 3.3 Estacionalidad Mensual

![Estacionalidad mensual real vs predicha](../../../img/fig2_estacionalidad.png)

*Figura 2. Concentración promedio mensual de ozono: valores reales (2023–2025) vs predicciones del modelo trigonométrico.*

| Mes | Real (ppm) | Predicho (ppm) |
|---|---|---|
| Enero | 0.0297 | 0.0290 |
| Febrero | 0.0357 | 0.0342 |
| Marzo | 0.0407 | 0.0398 |
| Abril | 0.0448 | 0.0443 |
| Mayo | 0.0413 | 0.0432 |
| **Junio** | **0.0446** | **0.0453** |
| **Julio** | **0.0459** | **0.0453** |
| Agosto | 0.0395 | 0.0384 |
| Septiembre | 0.0370 | 0.0357 |
| Octubre | 0.0328 | 0.0324 |
| Noviembre | 0.0296 | 0.0292 |
| Diciembre | 0.0283 | 0.0281 |

### 3.4 Comparativa de Modelos

![Comparativa R² de los tres modelos](../../../img/fig5_comparativa_modelos.png)

*Figura 3. Comparativa de R² en entrenamiento y prueba para los tres modelos ajustados.*

| Métrica | M1: Lineal simple | M2: Múltiple + OHE | M3: Trigonométrico + OHE ★ |
|---|---|---|---|
| Features | 1 | 72 | **31** |
| R² train | 0.0078 | 0.3542 | 0.3413 |
| **R² test** | −0.0764 | 0.1991 | **0.2090** |
| RMSE train (ppm) | 0.01110 | 0.00897 | 0.00906 |
| **RMSE test (ppm)** | 0.01090 | 0.00943 | **0.00938** |

El Modelo 3 logra el **mejor R² en el conjunto de prueba (0.2090)** y el **menor RMSE de prueba (0.00938 ppm)** usando solo 31 features, frente a las 72 del Modelo 2. Esto confirma que la representación trigonométrica es más eficiente y parsimoniosa que los dummies de mes.

### 3.5 Real vs Predicho

![Dispersión real vs predicho en train y test](../../../img/fig3_real_vs_predicho.png)

*Figura 4. Dispersión de valores reales vs predichos en los conjuntos de entrenamiento (izquierda) y prueba (derecha). La línea punteada representa la predicción perfecta.*

### 3.6 Proyección 2026

![Proyección de ozono para 2026](../../../img/fig4_prediccion_2026.png)

*Figura 5. Proyección de concentraciones de ozono para el año completo 2026. La banda azul representa la incertidumbre del modelo (±RMSE = ±0.00938 ppm). La línea roja punteada indica el límite NAAQS de 0.070 ppm.*

La proyección anticipa un pico en junio–julio de aproximadamente 0.045 ppm, muy por debajo del límite NAAQS de 0.070 ppm, con una media anual proyectada de 0.0367 ppm.

---

## 4. Discusión

### 4.1 El Modelo Trigonométrico como Mejor Opción

El Modelo 3 supera al Modelo 2 en el conjunto de prueba a pesar de usar menos de la mitad de variables (31 vs 72 features). Esto ilustra el principio de parsimonia: añadir variables redundantes o mal codificadas no mejora la generalización del modelo y puede empeorarla [9]. La representación trigonométrica del día del año es matemáticamente más apropiada para datos cíclicos que los dummies de mes, ya que captura la continuidad entre el fin de diciembre y el inicio de enero.

### 4.2 Estacionalidad como Factor Dominante

El factor más determinante en la variabilidad del ozono es la época del año. Los coeficientes trigonométricos capturan un ciclo con pico en junio–julio y mínimo en diciembre–enero, patrón consistente con la formación fotoquímica que depende directamente de la radiación solar disponible [1][2].

### 4.3 Patrones Geográficos

Las estaciones con mayor ozono predicho son WHITEFACE SUMMIT (zona montañosa, Essex) y RIVERHEAD / Flax Pond (Long Island costero, Suffolk). Este resultado es coherente con el transporte de masas de aire cargadas de precursores desde zonas urbanas hacia áreas rurales, donde el NO local es menor y el ozono se acumula en lugar de ser titulado [1]. Las estaciones urbanas (IS 52 en el Bronx, CCNY en Manhattan) presentan menores concentraciones por efecto de titulación del ozono por el NO vehicular [2].

### 4.4 Limitaciones

- **R² moderado (0.21 en test):** la variabilidad no explicada corresponde principalmente a factores meteorológicos (temperatura, radiación solar, viento) que no están disponibles en el dataset de la EPA [10].
- **Datos AirNow en 2025:** 413 registros no certificados introducen incertidumbre marginal en el conjunto de prueba.
- **Proyección 2026:** el modelo asume estabilidad de patrones; eventos extraordinarios como olas de calor o humo de incendios forestales pueden elevar el ozono de forma no anticipada [11].

### 4.5 Implicaciones para la Salud Pública

En los tres años analizados, 6 registros superaron AQI 150 ("Unhealthy"), todos en junio–julio. Ninguna estación superó el límite NAAQS de 0.070 ppm como media estatal, aunque Babylon y Queens College 2 alcanzaron 0.087 ppm en el verano de 2025. La proyección 2026 no anticipa superaciones del estándar NAAQS.

---

## 5. Referencias

[1] D. J. Jacob, *Introduction to Atmospheric Chemistry*. Princeton, NJ, USA: Princeton University Press, 1999.

[2] U.S. Environmental Protection Agency, "Ozone Pollution and Your Patients' Health," EPA, Washington, D.C., USA, Tech. Rep. EPA-456/R-04-002, 2004. [Online]. Available: https://www.epa.gov/ozone-pollution-and-your-patients-health

[3] U.S. Environmental Protection Agency, "NAAQS Table," EPA, Washington, D.C., USA. [Online]. Available: https://www.epa.gov/criteria-air-pollutants/naaqs-table. [Accessed: Apr. 2026].

[4] U.S. Environmental Protection Agency, "Outdoor Air Quality Data – Download Daily Data," AirData, Washington, D.C., USA. [Online]. Available: https://www.epa.gov/outdoor-air-quality-data/download-daily-data. [Accessed: Apr. 2026].

[5] U.S. Environmental Protection Agency, "About AirNow," AirNow, Washington, D.C., USA. [Online]. Available: https://www.airnow.gov/about-airnow/. [Accessed: Apr. 2026].

[6] R. J. Hyndman and G. Athanasopoulos, *Forecasting: Principles and Practice*, 3rd ed. Melbourne, Australia: OTexts, 2021. [Online]. Available: https://otexts.com/fpp3/

[7] T. G. Dietterich, "Approximate statistical tests for comparing supervised classification learning algorithms," *Neural Computation*, vol. 10, no. 7, pp. 1895–1923, Oct. 1998.

[8] P. J. Brockwell and R. A. Davis, *Introduction to Time Series and Forecasting*, 3rd ed. New York, NY, USA: Springer, 2016.

[9] A. Géron, *Hands-On Machine Learning with Scikit-Learn, Keras, and TensorFlow*, 3rd ed. Sebastopol, CA, USA: O'Reilly Media, 2022.

[10] A. Russo, F. Raischel, and P. G. Lind, "Air quality prediction using optimal neural networks with stochastic variables," *Atmospheric Environment*, vol. 79, pp. 822–830, Nov. 2013.

[11] J. T. Abatzoglou and A. P. Williams, "Impact of anthropogenic climate change on wildfire across western US forests," *Proceedings of the National Academy of Sciences*, vol. 113, no. 42, pp. 11770–11775, Oct. 2016.

---

*Datos procesados con Python 3 (pandas, numpy, scikit-learn, matplotlib). Dataset integrado, código fuente y gráficos disponibles en este repositorio.*
