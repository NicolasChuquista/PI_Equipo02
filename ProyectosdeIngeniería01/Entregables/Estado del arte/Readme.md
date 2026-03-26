# ☀️ SolarGrid

> **Sistema de gestión inteligente de energía solar con priorización automática de cargas e IoT**
> Proyecto de tesis de pregrado — Ingeniería Ambiental | ODS 7

[![ODS 7](https://img.shields.io/badge/ODS-7%20Energía%20Asequible-0F6E56?style=flat-square)](https://sdgs.un.org/goals/goal7)
[![Platform](https://img.shields.io/badge/Platform-ESP32-blue?style=flat-square)](https://www.espressif.com/)
[![Costo](https://img.shields.io/badge/Costo%20prototipo-%24110--150%20USD-green?style=flat-square)]()
[![License](https://img.shields.io/badge/License-MIT-lightgrey?style=flat-square)]()

---

## 📋 Tabla de contenidos

- [Descripción del proyecto](#-descripción-del-proyecto)
- [Estado del arte](#-estado-del-arte)
  - [1. Contextualización del problema](#1-contextualización-del-problema)
  - [2. Marco conceptual](#2-marco-conceptual-electrificación-rural-off-grid)
  - [3. Algoritmos MPPT](#3-algoritmos-mppt-y-control-de-carga-solar)
  - [4. Sistemas IoT para FV](#4-sistemas-iot-para-monitoreo-de-energía-fotovoltaica)
  - [5. Gestión inteligente de cargas](#5-gestión-inteligente-de-cargas-y-priorización)
  - [6. Benchmarking](#6-sistemas-existentes-benchmarking-técnico)
  - [7. Brechas e innovación](#7-análisis-de-brechas-e-identificación-de-oportunidad)
  - [8. Alineación ODS 7](#8-alineación-con-el-ods-7-y-contexto-peruano)
  - [9. Conclusiones](#9-conclusiones-del-estado-del-arte)
  - [10. Referencias](#10-referencias-bibliográficas)
- [Arquitectura del sistema](#-arquitectura-del-sistema)
- [Hardware requerido](#-hardware-requerido)
- [Modelo de negocio](#-modelo-de-negocio)

---

## 📌 Descripción del proyecto

**SolarGrid** es un dispositivo plug-and-play que convierte cualquier sistema solar básico en uno inteligente. Lee en tiempo real el voltaje del panel fotovoltaico y el estado de carga de la batería, ejecuta un algoritmo de priorización de cargas en tres niveles, y transmite los datos vía WiFi a una plataforma cloud. Ante fallas o eventos críticos, genera alertas SMS mediante un módulo GSM sin depender de conexión a Internet.

**Propuesta de valor en una línea:**
> Convierte cualquier sistema solar existente en inteligente por menos de $150 USD, con gestión automática de cargas, monitoreo remoto y alertas SMS que funcionan sin WiFi.

---

## 📚 Estado del Arte

### 1. Contextualización del problema

El acceso a la energía eléctrica representa uno de los pilares fundamentales del desarrollo humano sostenible. Según el Banco Mundial, al inicio del presente siglo más de seis millones de peruanos residentes en zonas rurales carecían de acceso a electricidad, equivalente al 30% de la población nacional. Esta situación impulsó al Estado peruano a lanzar el **Plan Nacional de Electrificación Rural (PNER)**, que a través de múltiples fases instaló sistemas solares fotovoltaicos domiciliarios (SFD) en comunidades sin acceso a la red eléctrica convencional.

Sin embargo, la instalación masiva de SFD evidenció una limitación estructural: los sistemas funcionan de manera **ciega**, sin capacidad de monitoreo remoto, sin gestión inteligente de la energía almacenada y sin priorización automática de cargas. OSINERGMIN constató que en revisiones de sistemas fotovoltaicos de programas públicos, una fracción significativa de los equipos inspeccionados se encontraba inoperativa, no por falla del panel, sino por **agotamiento prematuro de baterías** producto de una gestión inadecuada.

Ante este diagnóstico, emerge la necesidad de un sistema de control inteligente que:
- Maximice el aprovechamiento de la energía solar generada
- Proteja la vida útil de las baterías mediante algoritmos MPPT
- Priorice las cargas eléctricas según criterios de importancia socioeconómica
- Ofrezca conectividad IoT para monitoreo remoto a bajo costo

---

### 2. Marco conceptual: electrificación rural off-grid

#### 2.1 Sistemas fotovoltaicos aislados

Un sistema fotovoltaico aislado (off-grid) genera, almacena y distribuye electricidad de forma autónoma, sin conexión al SEIN. Su arquitectura básica comprende cuatro componentes:

| Componente | Función |
|---|---|
| Módulo fotovoltaico | Convierte radiación solar en corriente continua |
| Controlador de carga | Gestiona la transferencia panel → batería → cargas |
| Banco de baterías | Almacena energía para periodos sin irradiación |
| Cargas eléctricas | Consumo del usuario final |

En el Perú, los SFD instalados por el programa masivo del MINEM generalmente emplean **controladores PWM de bajo costo**, que carecen de seguimiento del punto de máxima potencia y de conectividad o monitoreo remoto. Esta limitación contrasta con el potencial solar disponible: la irradiación solar media diaria en el Perú es de **5.5 kWh/m²/día** en la costa y sierra, y puede superar los **6.5 kWh/m²/día** en regiones como Arequipa, Moquegua y Puno.

#### 2.2 Contexto de electrificación rural en el Perú

El World Bank Group documentó que el primer Proyecto de Electrificación Rural del Perú (2006–2013) instaló **7,100 sistemas fotovoltaicos domiciliarios** beneficiando a ~31,500 personas. Un segundo proyecto (2011–2015) instaló **11,915 sistemas adicionales**. El programa masivo del MINEM llevó la cobertura a más de **205,000 sistemas** para el año 2022.

Una investigación sobre sostenibilidad de la electrificación rural en el Perú publicada en *MDPI Sustainability* identificó deficiencias sistémicas: fallas prematuras en baterías por sobredescarga, ausencia de estándares técnicos actualizados y **falta de capacidad de monitoreo remoto** por parte de los operadores.

El estudio de microrred híbrida instalada en Laguna Grande, Ica *(Frontiers in Energy Research, 2020)* reportó una irradiación media anual de 6 kWh/m²/día y un LCOE de 0.90 PEN/kWh, demostrando la viabilidad técnica de sistemas solares autónomos con gestión activa de energía en contextos rurales peruanos.

---

### 3. Algoritmos MPPT y control de carga solar

#### 3.1 Fundamentos del seguimiento del punto de máxima potencia

El comportamiento eléctrico de un panel fotovoltaico está determinado por su curva característica corriente-voltaje (I-V). Existe un único punto de operación —el **punto de máxima potencia (MPP)**— en el cual la potencia entregada es máxima. Los controladores convencionales PWM no rastrean este punto, resultando en pérdidas de eficiencia de entre el **10% y 30%** respecto a la potencia disponible.

El **algoritmo de Perturbación y Observación (P&O)** es el método MPPT más ampliamente implementado en sistemas de pequeña escala. Consiste en perturbar periódicamente el voltaje de operación del panel y ajustar la perturbación según si la potencia aumenta o disminuye, convergiendo hacia el MPP de forma iterativa.

```
Si ΔP > 0 y ΔV > 0  →  aumentar V (mover hacia el MPP derecho)
Si ΔP > 0 y ΔV < 0  →  disminuir V (mover hacia el MPP izquierdo)
Si ΔP < 0 y ΔV > 0  →  disminuir V
Si ΔP < 0 y ΔV < 0  →  aumentar V
```

#### 3.2 Implementación en microcontroladores de bajo costo

Un estudio publicado en *MDPI Electronics* (2020) presentó un controlador MPPT-SCC con plataforma IoT usando microcontrolador PIC16F877A con algoritmo P&O y convertidor buck-boost, con **eficiencias de seguimiento superiores al 95%** en condiciones variables de irradiancia, validado simultáneamente en MATLAB/Simulink y montaje físico.

El proyecto de código abierto **Open Solar Project (OSPController)** *(Hackaday, 2019)* implementó un controlador MPPT completo basado en ESP32 conectado a un convertidor DC-DC buck, logrando **eficiencia de conversión del 95%** con integración a ThingSpeak. Este trabajo es uno de los antecedentes técnicos más directos para SolarGrid, aunque carece de gestión de cargas por prioridad y de alertas SMS.

Lahabar et al. (2023), en *IJEEE*, desarrollaron un controlador fotovoltaico inteligente combinando Arduino Nano con módulo ESP32 para transmisión de datos a la nube vía dweet.io, con validación en Proteus y confirmación de las fases de carga bulk, absorción y flotación.

---

### 4. Sistemas IoT para monitoreo de energía fotovoltaica

#### 4.1 Arquitecturas IoT aplicadas a sistemas solares

Los trabajos académicos en este campo convergieron hacia una **arquitectura de tres capas**:

```
┌─────────────────────────────────────────────┐
│  CAPA 3 — APLICACIÓN                        │
│  Plataformas cloud, dashboards, alertas     │
│  (ThingSpeak, Blynk, Grafana, MQTT)         │
├─────────────────────────────────────────────┤
│  CAPA 2 — COMUNICACIÓN                      │
│  WiFi (ESP32), GSM/4G (SIM800L), LoRa       │
├─────────────────────────────────────────────┤
│  CAPA 1 — PERCEPCIÓN                        │
│  Sensores de voltaje (INA219) y corriente   │
└─────────────────────────────────────────────┘
```

**Trabajos clave:**
- **Zhang et al. (2019)** — ESP32 + sensores I/V para rastreo de desempeño FV en tiempo real
- **Wang et al. (2020)** — Sistema ESP32 + Arduino IDE + LCD para visualización local
- **Liu et al. (2022)** — Integración cloud con análisis en tiempo real y mantenimiento predictivo

#### 4.2 Plataformas de comunicación

**ThingSpeak** (MathWorks) es la plataforma más usada en proyectos académicos de monitoreo solar: visualización de hasta 8 variables, integración nativa con MATLAB. Para contextos sin WiFi, el módulo **SIM800L** permite transmisión vía 2G/3G y alertas SMS — fundamental para comunidades rurales peruanas donde la cobertura celular supera ampliamente la cobertura WiFi.

Un estudio publicado en *ScienceDirect* (2025) desarrolló un dispositivo IoT con capacidad hasta 90 kW combinando Arduino Mega + ESP32 + módulo GSM, con almacenamiento local en SD y alertas SMS para condiciones críticas. Sus resultados mostraron transmisión confiable al servidor ThingSpeak (MATLAB) cada 2 minutos. Esta combinación **Arduino-ESP32-GSM es técnicamente análoga a la arquitectura propuesta para SolarGrid**.

---

### 5. Gestión inteligente de cargas y priorización

#### 5.1 Demand Side Management (DSM) en microredes

La gestión del lado de la demanda (DSM) coordina el consumo de cargas en función de la disponibilidad de generación y almacenamiento. En microredes aisladas, el DSM es crítico porque no existe una red de respaldo: si la energía no se gestiona, el sistema colapsa y las cargas más importantes dejan de funcionar.

Un trabajo publicado en el *Journal of The Institution of Engineers (India)* (2025) presentó un algoritmo de DSM para sistemas FV off-grid con **descarga de carga por prioridades (priority-wise load shedding)**, con criterios diferenciados para el día y la noche. Conclusión: la gestión basada en prioridades permite extender la autonomía del sistema en periodos de baja generación **sin sacrificar las cargas más críticas**.

Mortaji et al. (2017), en *IEEE Transactions on Industrial Applications*, demostraron que la desconexión inteligente de cargas mediante IoT puede reducir el pico de consumo de manera significativa, comunicando el estado del sistema en tiempo real para activar/desactivar cargas según disponibilidad.

#### 5.2 Resultados cuantitativos en la literatura

| Estudio | Resultado clave |
|---|---|
| Souissi et al., *ScienceDirect* 2025 | Modo ECO aumentó contribución batería al pico de 14.5% → 31.8%; ahorros del 17% |
| Scientific Reports, 2025 | IoT + algoritmos ML mejoraron eficiencia hasta 72.3% y redujeron costos 61% |
| Bakht et al., *IEEE Access* 2024 | Diseño óptimo de SHR con DSM garantiza suministro ininterrumpido en load shedding |

> Estos resultados validan a escala académica la hipótesis central de SolarGrid: **la gestión inteligente con priorización de cargas mejora significativamente la eficiencia de sistemas solares off-grid**.

---

### 6. Sistemas existentes: benchmarking técnico

| Sistema | Precio (USD) | MPPT | WiFi / App | Priorización cargas | Alertas SMS | Open source | Target |
|---|---|---|---|---|---|---|---|
| Victron SmartSolar MPPT 100/20 | $120 | ✅ Ultra-Fast | ⚡ Solo BT | ❌ 1 salida | ❌ | ❌ | RV europeo |
| Victron + VRM Portal | $350+ | ✅ | ✅ vía GX ($200+) | ❌ | ❌ | ❌ | Industrial |
| EPEver Tracer AN 30A + WiFi | $73 | ✅ ≥99.5% | ✅ Solar Guardian | ❌ 1 salida | ❌ | ❌ | Off-grid general |
| Renogy ONE M1 | $260 + $30–60/año | ❌ Solo monitoreo | ✅ WiFi + app | ⚡ 3 cargas ON/OFF | ❌ | ❌ | RV norteamericano |
| Open Solar Project (OSPController) | ~$80 DIY | ✅ P&O ESP32 | ✅ ThingSpeak | ❌ | ❌ | ✅ | Makers |
| Proyectos académicos ESP32 | $30–80 | ⚡ Básico | ✅ Blynk/ThingSpeak | ❌ | ❌ | ⚡ | Investigación |
| **SolarGrid (propuesta)** | **$110–150** | **✅ P&O adaptativo** | **✅ WiFi + SMS** | **✅ 3 niveles auto** | **✅ SIM800L** | **✅** | **Comunidades rurales Perú** |

> ⚡ = parcial &nbsp; ✅ = sí &nbsp; ❌ = no

**Fuentes:** Victron Energy (2024); EPEver (2024); Renogy (2022); Open Solar Project GitHub (2019); Lahabar et al. (2023); Nassar et al. (2024).

---

### 7. Análisis de brechas e identificación de oportunidad

Se identificaron **tres brechas técnicas** que justifican el desarrollo de SolarGrid:

#### 🔴 Brecha 1 — Priorización automática de cargas en dispositivos sub-$200

Ningún producto comercial en el rango de precio inferior a $200 USD implementa lógica de priorización automática por niveles de importancia. Los controladores Victron y EPEver ofrecen una única salida de carga con umbral de voltaje (BatteryLife), que solo desconecta o reconecta **todas las cargas simultáneamente**. Los trabajos académicos que implementan priorización *(Mortaji 2017; Bakht 2024; Souissi 2025)* operan a escalas de red de mayor complejidad y no han sido trasladados a hardware embebido de bajo costo para sistemas domiciliarios rurales.

#### 🔴 Brecha 2 — Conectividad SMS independiente de WiFi

Todos los sistemas comerciales revisados requieren conectividad WiFi activa para notificaciones y monitoreo remoto. Esta limitación es crítica en comunidades rurales peruanas donde la infraestructura WiFi es prácticamente inexistente, pero **la cobertura celular 2G/3G alcanza muchas zonas remotas** (Movistar, Claro, Entel). La integración de un módulo SIM800L representa una decisión de diseño diferenciadora.

#### 🔴 Brecha 3 — Solución adaptada al contexto latinoamericano

Los productos comerciales disponibles fueron diseñados para el mercado europeo (Victron, EPEver) o norteamericano (Renogy), con interfaces en inglés y precios que representan barreras para comunidades rurales andinas. Los proyectos académicos revisados provienen principalmente de Asia y el Medio Oriente, **sin contemplar las condiciones de irradiancia peruana**, los patrones de consumo de familias rurales andinas ni la problemática específica de la degradación de baterías de plomo-ácido en condiciones de altitud y temperatura extremas.

---

### 8. Alineación con el ODS 7 y contexto peruano

El **ODS 7** de la Agenda 2030 establece la meta de garantizar el acceso a energía asequible, fiable, sostenible y moderna para todos. Sus metas específicas son directamente pertinentes para SolarGrid:

| Meta ODS 7 | Cómo SolarGrid contribuye |
|---|---|
| **7.1** — Acceso universal a la electricidad | Mejora operatividad de sistemas ya instalados en comunidades rurales |
| **7.2** — Aumentar participación de renovables | Optimiza el aprovechamiento del recurso solar disponible (MPPT) |
| **7.3** — Duplicar tasa de mejora de eficiencia | Reduce pérdidas por sobredescarga de baterías y gestiona cargas inteligentemente |

El informe *MDPI Sustainability* (2024) sobre implementación de energía solar FV en el Perú registró que al cierre de 2023 la capacidad instalada era de **13,693 MW totales**, de los cuales solo el **9.9% correspondía a energías no convencionales** (solar y eólica). El mismo estudio documenta la puesta en operación de plantas solares de gran escala en el sur del país: **Clemesi 116 MW** (Moquegua, 2023) y **Matarani 80 MW** (Arequipa, 2024), evidenciando el creciente interés institucional en la energía solar como eje de la transición energética peruana.

Un sistema como SolarGrid, que extiende la vida útil de la batería mediante gestión inteligente y permite el diagnóstico remoto de fallos, **contribuye directamente a mejorar la tasa de operatividad** de los sistemas instalados, maximizando el retorno de la inversión pública en electrificación rural.

---

### 9. Conclusiones del estado del arte

1. ✅ La combinación **MPPT P&O + ESP32 + convertidor DC-DC buck** es una arquitectura técnicamente validada por múltiples publicaciones académicas, con eficiencias de seguimiento superiores al 95% en condiciones variables *(OSPController; Lahabar et al. 2023; MDPI Electronics 2020)*.

2. ✅ La integración de módulos IoT **(ESP32 + ThingSpeak/Blynk/MQTT + GSM)** para monitoreo remoto de sistemas fotovoltaicos es tecnológicamente madura, con múltiples prototipos documentados en IEEE, MDPI y ScienceDirect entre 2019 y 2025.

3. 🔴 La **gestión de cargas por priorización automática** en sistemas off-grid de bajo costo constituye la **brecha tecnológica principal** identificada en la literatura. Los trabajos que implementan DSM con priorización operan en contextos de mayor escala o simulación, sin trasladar la lógica a hardware embebido de menos de $200 USD.

4. 🔴 El mercado comercial **(Victron, EPEver, Renogy)** no ofrece soluciones que combinen MPPT + priorización automática de múltiples niveles de carga + conectividad WiFi + alertas SMS en un único dispositivo por menos de $150 USD.

5. 🔴 El contexto peruano de electrificación rural off-grid (>205,000 sistemas instalados, alta irradiancia solar, limitada conectividad WiFi rural, red celular 2G/3G disponible) configura un **mercado específico no atendido** por las soluciones comerciales actuales.

6. ✅ La propuesta SolarGrid se sustenta en una base técnica sólida documentada en la literatura, identifica una brecha de innovación verificable, y se alinea directamente con las metas **7.1, 7.2 y 7.3 del ODS 7** de la Agenda 2030.

---

### 10. Referencias bibliográficas

- Bakht, M. P. et al. (2024). Optimal design and performance analysis of hybrid renewable energy system. *IEEE Access*, 12, 5792–5813. https://doi.org/10.1109/ACCESS.2024.3349594

- Frontiers in Energy Research (2020). Hybrid Photovoltaic-Wind Microgrid With Battery Storage for Rural Electrification: A Case Study in Perú. https://doi.org/10.3389/fenrg.2020.528571

- Kapoor, S. et al. (2020). IoT-Enabled High Efficiency Smart Solar Charge Controller with MPPT. *MDPI Electronics*, 9(8), 1267. https://doi.org/10.3390/electronics9081267

- Krishna Rao, C., Sahoo, S. & Yanine, F. (2024). An IoT-based intelligent smart energy monitoring system for solar PV. *Energy Harvesting and Systems*, 11(1). https://doi.org/10.1515/ehs-2023-0015

- Lahabar, S. et al. (2023). IoT-Powered Smart Photovoltaic Charge Controller. *IJEEE*, 10(1), 219–225.

- MDPI Sustainability (2018). Is Peru Prepared for Large-Scale Sustainable Rural Electrification? https://doi.org/10.3390/su10051683

- MDPI Sustainability (2024). Implementation of Renewable Energy from Solar PV Facilities in Peru. https://doi.org/10.3390/su16114388

- Mortaji, H. et al. (2017). Load shedding and smart-direct load control using IoT in smart grid demand response. *IEEE Trans. Industry Applications*, 53(6), 5155–5163. https://doi.org/10.1109/TIA.2017.2740832

- Nassar, Y. F. et al. (2024). A smart energy monitoring system using ESP32. *ScienceDirect – Energy Reports*.

- Open Solar Project — OSPController (2019). ESP32 Smart Solar Charger. https://github.com/opensolarproject/OSPController

- Renogy (2022). Renogy ONE: First All-In-One Energy Monitoring and Smart Living Center. https://www.prnewswire.com/news-releases/301588100.html

- Saini, K. K. et al. (2024). Techno-economic and reliability assessment of an off-grid solar-powered energy system. *Applied Energy*, 371. https://doi.org/10.1016/j.apenergy.2024.123579

- ScienceDirect (2025). Development of a low-cost monitoring device for solar electric (PV) system using IoT.

- Souissi, A. et al. (2025). A comprehensive review of smart energy management systems for PV using IoT. *ScienceDirect*.

- Scientific Reports (2025). Empowering smart homes by IoT-driven hybrid renewable energy integration. https://doi.org/10.1038/s41598-025-25328-2

- Victron Energy (2024). SmartSolar MPPT documentation. https://www.victronenergy.com/solar-charge-controllers

- World Bank (2014). Peru Brings Electricity to Rural Communities. https://www.worldbank.org/en/results/2014/09/24/peru-brings-electricity-to-rural-communities

- World Bank (2019). Promoting Rural Electrification in Peru. https://www.worldbank.org/en/results/2019/05/13/promoting-rural-electrification-in-peru

---

## ⚙️ Arquitectura del sistema

```
┌──────────────┐     ┌─────────────────────────────────────────┐     ┌──────────────┐
│  Panel solar │────▶│           SolarGrid Device              │────▶│  Batería 12V │
│   20–50W     │     │  ┌──────────┐  ┌────────┐  ┌────────┐  │     │  Plomo-ácido │
│              │     │  │  ESP32   │  │INA219  │  │INA219  │  │     │  o Li-ion    │
└──────────────┘     │  │          │  │Panel   │  │Batería │  │     └──────────────┘
                     │  │ MPPT P&O │  │V + I   │  │V + I   │  │
                     │  │ Algorithm│  └────────┘  └────────┘  │     ┌──────────────┐
                     │  │          │  ┌───────────────────┐   │────▶│  Carga Niv.1 │
                     │  │ Priority │  │  Buck DC-DC Conv  │   │     │  (crítica)   │
                     │  │ Logic    │  │  Ajuste ciclo PWM │   │     └──────────────┘
                     │  │          │  └───────────────────┘   │
                     │  │ WiFi IoT │  ┌────────────────────┐  │     ┌──────────────┐
                     │  │ SIM800L  │  │  Relay x3 canales  │  │────▶│  Carga Niv.2 │
                     │  │ OLED     │  └────────────────────┘  │     │  (importante)│
                     │  └──────────┘                          │     └──────────────┘
                     └─────────────────────────────────────────┘
                                        │  WiFi / GSM                ┌──────────────┐
                                        ▼                       ────▶│  Carga Niv.3 │
                              ┌──────────────────┐                   │  (diferible) │
                              │  ThingSpeak /    │                   └──────────────┘
                              │  Blynk dashboard │
                              │  + SMS alerts    │
                              └──────────────────┘
```

**Lógica de priorización:**

| Nivel | Tipo de carga | Condición de activación |
|---|---|---|
| 🟢 Nivel 1 — Crítico | Iluminación LED, radio | Siempre activo si hay energía disponible |
| 🟡 Nivel 2 — Importante | Ventilador, carga de celular | SoC batería > 50% |
| 🔵 Nivel 3 — Diferible | TV, electrodomésticos | SoC batería > 75% + excedente solar |

---

## 🔩 Hardware requerido

| Componente | Modelo | Costo aprox. (USD) |
|---|---|---|
| Microcontrolador | ESP32 DevKit V1 | $5–8 |
| Sensor corriente/voltaje × 2 | INA219 I2C | $4–8 |
| Módulo relé 3 canales | 5V, contactos 10A | $3–5 |
| Convertidor DC-DC buck | XL4015, ajustable 5A | $3–5 |
| Panel solar | 20–50W policristalino, 18V | $15–30 |
| Batería | 12V 7–12Ah plomo-ácido sellada | $15–25 |
| Módulo GSM | SIM800L (alertas SMS) | $5–8 |
| Display OLED | 0.96" SSD1306 I2C | $2–4 |
| Módulo microSD | SPI genérico | $2–3 |
| Caja, cables, protoboard | — | $10–15 |
| **Total estimado** | | **$64–111** |

---

## 💼 Modelo de negocio

| Segmento | Canal | Propuesta de valor | Precio |
|---|---|---|---|
| Usuarios residenciales (B2C) | Venta directa online | Dispositivo plug-and-play | $110–150 + $3–5/mes app |
| Instaladores de paneles (B2B) | Distribuidores | Accesorio para reventa con margen 30–40% | $55–70 mayorista |
| ONGs / proyectos institucionales (B2G) | Licitaciones MINEM / BID | Componente de monitoreo en proyectos FV rurales | Variable por proyecto |

---

<p align="center">
  Hecho con ☀️ para comunidades rurales del Perú &nbsp;|&nbsp; ODS 7 — Energía Asequible y No Contaminante
</p>
