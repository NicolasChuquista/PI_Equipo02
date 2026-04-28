# Relación entre Parámetros

## pH
El pH óptimo para el desarrollo de *E. coli* se sitúa en la neutralidad. Las desviaciones hacia la acidez provocan una disminución lineal en la tasa de crecimiento debido a la desnaturalización proteica y la inhibición del transporte de nutrientes (1).

Basándose en modelos experimentales previos (2), se puede extrapolar una ecuación que sirve para analizar el crecimiento a cualquier nivel de pH con una temperatura de referencia:

<p align="center">
  <img src="https://raw.githubusercontent.com/NicolasChuquista/PI_Equipo02/refs/heads/main/img/param_1.png" width="350">
  <br>

**Donde:**
* **$y_{pH}$:** Tasa de crecimiento basal (1/Generation time (min)), calculada a una temperatura de referencia ($T_{ref}$). Es el tiempo que tarda una población de organismos en duplicarse.
* **$[H^+]$:** Concentración de iones hidrógeno en micromolar (u/M), derivada de la medición del sensor: $[H^+] = 10^{-pH}$.
* **$m$:** Es la pendiente de la línea, igual a $-7.74 \times 10^{-5}$.


<p align="center">
  <img src="https://raw.githubusercontent.com/NicolasChuquista/PI_Equipo02/refs/heads/main/img/Parametros1.JPG" width="500">
  <br>
  <b>Gráfico 1: Tasa de Crecimiento E.Coli a diferentes pH (2)</b>
</p>

---

## Temperatura
En entornos ambientales reales, la temperatura actúa como el principal modulador cinético. Para el AquaBalde, se ha implementado un Factor de Ajuste Térmico ($F_T$) basado en el Modelo de Raíz Cuadrada de Ratkowsky.

<p align="center">
  <img src="https://raw.githubusercontent.com/NicolasChuquista/PI_Equipo02/refs/heads/main/img/Parametros2.JPG" width="200">
  <br>
  <b>Figura 1: Modelo de Raíz Cuadrada de Ratkowsky (3)</b>
</p>



Este modelo se ajusta con mayor precisión a sistemas biológicos complejos al reconocer una temperatura mínima conceptual, por debajo de la cual el crecimiento es nulo. La integración se realiza mediante un proceso de normalización de variables, resultando en la siguiente expresión: 

<p align="center">
  <img src="https://raw.githubusercontent.com/NicolasChuquista/PI_Equipo02/refs/heads/main/img/param_2.png" width="250">
  <br>

**Donde:**
* **$F_T$:** Factor de Ajuste Térmico.
* **$T_{sensor}$:** Temperatura medida en tiempo real por el dispositivo. 
* **$T_0$:** 7°C, temperatura mínima de actividad metabólica para coliformes (4). 
* **$T_{ref}$:** 20°C, temperatura de control del estudio base de pH (2).

Esta normalización asegura que cuando la temperatura medida sea igual a la de control ($T_{sensor} = T_{ref}$), el factor sea igual a 1, respetando la validez del modelo de pH original, pero ajustándose dinámicamente para climas fríos o cálidos. 

Si la temperatura medida es menor o igual a la temperatura mínima, el sistema asigna automáticamente una tasa de crecimiento final de 0, reflejando la inactivación por frío. 

<p align="center">
  <img src="https://raw.githubusercontent.com/NicolasChuquista/PI_Equipo02/refs/heads/main/img/param_3.png" width="250">
  <br>

---

## Tasa de Crecimiento Final:

La tasa de crecimiento final es el producto de la susceptibilidad química y la capacidad cinética:

Sustituyendo los valores constantes ($T_0=7°C, T_{ref}=20°C$):

<p align="center">
  <img src="https://raw.githubusercontent.com/NicolasChuquista/PI_Equipo02/refs/heads/main/img/param_4.png" width="600">
  <br>

* **Alta Proliferación:** $y_{final} \geq 0.008 \text{ min}^{-1}$
* **Moderada Proliferación:** $0.004 \text{ min}^{-1} \leq y_{final} < 0.008 \text{ min}^{-1}$
* **Bajo Proliferación:** $y_{final} < 0.004 \text{ min}^{-1}$

---

## Turbidez:

A diferencia de los parámetros cinéticos, la turbidez (NTU) funciona como un indicador directo de la presencia de sólidos suspendidos. Su importancia radica en que niveles elevados de turbidez suelen correlacionarse con la presencia de coliformes (5) que son resistentes a factores ambientales como el frío o la acidez.

<p align="center">
  <img src="https://raw.githubusercontent.com/NicolasChuquista/PI_Equipo02/refs/heads/main/img/Parametros3.JPG" width="270">
  <br>
  <b>Tabla 1: Riesgo de presencia de Coliformes a diferentes niveles de Turbidez (6)</b>
</p>



Para la categorización del riesgo, se adoptan los umbrales de la bibliografía revisada, donde cualquier valor por encima de 5 NTU se considera un indicador crítico de contaminación, independientemente de la tasa de proliferación bacteriana.

---

## Criterios Conjuntos de Clasificación:

<p align="center">
  <img src="https://raw.githubusercontent.com/NicolasChuquista/PI_Equipo02/refs/heads/main/img/Parametros4.JPG" width="700">
  <br>
  <b>Figura 2: Criterios (Creación Propia) </b>
</p>


---

## Referencias:
1. Cacho S. *Supervivencia y parámetros cinéticos de Escherichia coli O157:H7 en zumos de frutas comerciales*. Universidad de Valladolid, 2018.
2. Presser KA, Ratkowsky DA, Ross T. *Modelling the growth rate of Escherichia coli as a function of pH and lactic acid concentration*. Appl Environ Microbiol. 1997.
3. Ratkowsky DA, Olley J, McMeekin TA, Ball A. *Relationship between temperature and growth rate of bacterial cultures*. Journal of Bacteriology. 1982.
4. Área Soporte al Análisis de Riesgo (ACHIPIA). *Escherichia coli productora de toxina Shiga (STEC)*. Ministerio de Agricultura de Chile, 2017.
5. Martínez M., Mendoza J., et. al. *Evaluation of turbidity as a parameter indicator of treatment in a drinking water treatment plant*. Revista UIS Ingenierías, 2020.
6. Martínez Fernández A, Rebollo Roldán J, Rojas G. *Indicadores de calidad microbiológica del agua de consumo: la turbidez como posible indicador indirecto de contaminación microbiológica*. Hig Sanid Ambient. 2004.
