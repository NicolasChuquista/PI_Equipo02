# Informe de Modelamiento Estacional de Material Particulado ($PM_{10}$)

## 1. Introducción
El material particulado con un diámetro aerodinámico inferior a 10 micrómetros ($PM_{10}$) representa uno de los contaminantes atmosféricos más críticos en áreas urbanas e industriales. Estas partículas provienen de diversas fuentes, incluyendo la resuspensión de polvo, actividades de construcción, emisiones vehiculares y procesos industriales [1].

La predicción y el modelamiento de la concentración de $PM_{10}$ es fundamental para la gestión de la calidad del aire y la alerta temprana. Sin embargo, su comportamiento está sujeto a una alta volatilidad por la influencia de variables meteorológicas (como el viento y la precipitación) y patrones estacionales. El desarrollo de modelos que logren capturar la estacionalidad  es un primer paso importante y esencial para entender las dinámicas a largo plazo de este contaminante.

## 2. Metodología
Para este análisis, se implementó un modelo de Regresión Lineal (OLS) enfocado en capturar la estacionalidad anual y la variabilidad espacial del contaminante. El flujo de trabajo computacional se estructuró de la siguiente manera:

* **Procesamiento de Datos:** Se consolidaron registros diarios de concentración de $PM_{10}$ correspondientes al año 2024 (conjunto de entrenamiento) y 2025 (conjunto de prueba).
* **Ingeniería de Características:** * *Transformación Trigonométrica:* Para modelar la naturaleza cíclica anual, se transformó el "día del año" en componentes continuos utilizando funciones de seno y coseno ($sin\_tiempo$ y $cos\_tiempo$). Esto permite que el modelo interprete el 31 de diciembre y el 1 de enero como fechas temporalmente cercanas.
  * *Codificación Espacial:* Se aplicó *One-Hot Encoding* a la variable `Site ID` para que el modelo asigne un peso específico a las características base de cada estación de monitoreo.
* **Entrenamiento y Evaluación:** Se entrenó el modelo multivariado utilizando únicamente los datos de 2024. El desempeño se evaluó sobre los datos invisibles de 2025 mediante el Coeficiente de Determinación ($R^2$) y el Error Cuadrático Medio ($RMSE$).

## 3. Resultados y Discusión
La Figura 1 muestra la comparación entre la concentración media diaria real de $PM_{10}$ (línea verde) y la predicción del modelo estacional (línea roja discontinua).


* **Desempeño en Entrenamiento (2024):** El modelo obtuvo un $R^2$ de $0.2017$. Como se observa en la gráfica, la curva de predicción logra delinear una tendencia suave que captura la macro-estacionalidad anual (por ejemplo, el ligero descenso a mediados de año y el incremento hacia los extremos del año). No obstante, el bajo valor de $R^2$ refleja que la mayor parte de la varianza del $PM_{10}$ proviene de fluctuaciones diarias agudas (picos que superan los 25 o 30 $\mu g/m^3$) que un modelo puramente trigonométrico no puede prever.
* **Desempeño en Prueba (2025):** Las métricas revelan un $RMSE$ de $3.8391$ y un $R^2$ negativo de $-0.1616$. Un coeficiente de determinación negativo indica que, frente a datos nuevos, el modelo predictivo tiene un rendimiento inferior al que se obtendría simplemente prediciendo el promedio histórico constante. 
* **Análisis del Comportamiento de la Curva:** Las caídas abruptas y escalonadas que se observan en la línea de predicción (especialmente notables en julio y octubre) no corresponden a un efecto temporal puro, sino a la influencia de la variable categórica `Site ID`. Al graficar el promedio diario de las predicciones, la entrada o salida de datos de estaciones específicas con coeficientes base muy bajos o altos distorsiona la media diaria predicha de forma escalonada.

En conclusión, aunque la transformación trigonométrica es útil para identificar la línea base anual, no es suficiente como predictor único para el $PM_{10}$. Las concentraciones están dominadas por fenómenos a corto plazo.

## 4. Referencias

[1] Organización Mundial de la Salud (OMS), *Directrices mundiales de la OMS sobre la calidad del aire: partículas ($PM_{2.5}$ y $PM_{10}$), ozono, dióxido de nitrógeno, dióxido de azufre y monóxido de carbono*. Ginebra, Suiza: OMS, 2021. 

[2] https://www.epa.gov/pm-pollution/particulate-matter-pm-basics#PM 
