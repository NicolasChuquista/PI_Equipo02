# Estado del Arte

Sistema de Gestión Inteligente de Energía Solar con Priorización Automática de Cargas e IoT Proyecto SolarGrid

Carrera	Ingeniería Ambiental

ODS vinculados	ODS 7 — Energía asequible y no contaminante
 
## 1. Introducción y contextualización del problema
El acceso a la energía eléctrica representa uno de los pilares fundamentales del desarrollo humano sostenible. Según el Banco Mundial, al inicio del presente siglo más de seis millones de peruanos residentes en zonas rurales carecían de acceso a electricidad, lo que equivalía al 30% de la población nacional. Esta situación impulsó al Estado peruano a lanzar el Plan Nacional de Electrificación Rural (PNER), que a través de múltiples fases instaló sistemas solares fotovoltaicos domiciliarios (SFD) en comunidades sin acceso a la red eléctrica convencional.

Sin embargo, la instalación masiva de SFD ha evidenciado una limitación estructural: los sistemas solares instalados funcionan de manera ciega, sin capacidad de monitoreo remoto, sin gestión inteligente de la energía almacenada y sin priorización automática de cargas. El organismo regulador OSINERGMIN constató que en revisiones realizadas en sistemas fotovoltaicos de programas públicos, una fracción significativa de los equipos inspeccionados se encontraba inoperativa o funcionando deficientemente, no por falla del panel fotovoltaico, sino por agotamiento prematuro de baterías producto de una gestión inadecuada de la energía disponible.

Ante este diagnóstico, emerge la necesidad de desarrollar un sistema de control inteligente que permita maximizar el aprovechamiento de la energía solar generada, proteger la vida útil de las baterías mediante algoritmos de seguimiento del punto de máxima potencia (MPPT) y priorizar las cargas eléctricas según criterios de importancia socioeconómica para la comunidad, todo ello con conectividad IoT que permita el monitoreo remoto a bajo costo. Esta es precisamente la propuesta de valor del sistema SolarGrid, cuyo fundamento técnico se desarrolla en el presente estado del arte.

## 2. Marco conceptual: electrificación rural off-grid
**2.1 Sistemas fotovoltaicos aislados (off-grid)**
Un sistema fotovoltaico aislado o fuera de red es aquel que genera, almacena y distribuye electricidad de forma autónoma, sin conexión al Sistema Eléctrico Interconectado Nacional (SEIN). Estos sistemas son la solución técnica más adecuada para comunidades rurales dispersas donde la extensión de la red convencional resulta técnica y económicamente inviable.

La arquitectura básica de un SFD comprende cuatro componentes principales: (1) el módulo fotovoltaico, que convierte la radiación solar en corriente continua; (2) el regulador o controlador de carga, que gestiona la transferencia de energía desde el panel hacia la batería y las cargas; (3) el banco de baterías, que almacena la energía para su uso en periodos sin irradiación solar; y (4) las cargas eléctricas, que representan el consumo del usuario final. En configuraciones más avanzadas se incluye un inversor DC/AC para alimentar cargas de corriente alterna.

En el Perú, los SFD instalados por el programa masivo del Ministerio de Energía y Minas (MINEM) generalmente emplean controladores PWM (Pulse Width Modulation) de bajo costo, que carecen de seguimiento del punto de máxima potencia y de cualquier capacidad de conectividad o monitoreo remoto. Esta limitación tecnológica contrasta con el potencial solar disponible: según datos del SENAMHI y del Atlas Solar del MEM, la irradiación solar media diaria en el Perú es de 5.5 kWh/m²/día en la costa y sierra, y puede superar los 6.5 kWh/m²/día en regiones como Arequipa, Moquegua y Puno, valores que ubican al país entre los de mayor recurso solar de América del Sur.

**2.2 Contexto de electrificación rural en el Perú**
El World Bank Group documentó que el primer Proyecto de Electrificación Rural del Perú (2006-2013) instaló 7,100 sistemas fotovoltaicos domiciliarios en hogares aislados, beneficiando a aproximadamente 31,500 personas. Un segundo proyecto (2011-2015) instaló 11,915 sistemas adicionales. El programa masivo del MINEM, contratado con la empresa italiana Tozzi Green y ejecutado con controladores Morningstar, llevó la cobertura a más de 205,000 sistemas para el año 2022.

Una investigación sobre sostenibilidad de la electrificación rural en el Perú publicada en la revista MDPI Sustainability identificó deficiencias sistémicas en la supervisión de los SFD instalados por programas públicos. Se documentó que las revisiones técnicas de OSINERGMIN habían sido puntuales y escasas, con sólo dos revisiones realizadas entre 2011 y 2013. Entre los problemas más frecuentes identificados figuraban fallas prematuras en baterías por sobredescarga, ausencia de estándares técnicos actualizados y falta de capacidad de monitoreo remoto por parte de los operadores.

El estudio de microrred híbrida fotovoltaica-eólica instalada en Laguna Grande, Ica, realizado por investigadores peruanos y publicado en Frontiers in Energy Research (2020), reportó una irradiación media anual de 6 kWh/m²/día en el sitio y un Costo Nivelado de Electricidad (LCOE) de 0.90 PEN/kWh, comparable con resultados de proyectos similares en el resto del mundo. Este caso demuestra la viabilidad técnica de sistemas solares autónomos con gestión activa de energía en contextos rurales peruanos.

## 3. Algoritmos MPPT y control de carga solar
**3.1 Fundamentos del seguimiento del punto de máxima potencia**
El comportamiento eléctrico de un panel fotovoltaico está determinado por su curva característica corriente-voltaje (I-V) y su curva potencia-voltaje (P-V). Estas curvas son no lineales y varían en función de la irradiancia incidente y la temperatura de la celda. Existe un único punto de operación, denominado punto de máxima potencia (MPP), en el cual la potencia entregada por el panel es máxima. Los controladores convencionales PWM no rastrean este punto, lo que resulta en pérdidas de eficiencia que pueden oscilar entre el 10% y el 30% respecto a la potencia disponible.

El algoritmo de Perturbación y Observación (P&O) es el método MPPT más ampliamente implementado en sistemas fotovoltaicos de pequeña y mediana escala. Consiste en perturbar periódicamente el voltaje de operación del panel e incrementar la perturbación si la potencia aumenta, o revertirla si disminuye, convergiendo de forma iterativa hacia el MPP. Su principal ventaja radica en su simplicidad de implementación en microcontroladores de bajo costo; su limitación es que introduce oscilaciones alrededor del MPP en condiciones estacionarias.

**3.2 Implementación de MPPT en microcontroladores de bajo costo**
Un estudio publicado en MDPI Electronics (2020) titulado 'IoT-Enabled High Efficiency Smart Solar Charge Controller with Maximum Power Point Tracking' presentó el diseño e implementación de un controlador MPPT-SCC con plataforma IoT para monitoreo remoto. El sistema utilizó un microcontrolador PIC16F877A para implementar el algoritmo P&O junto con un convertidor buck-boost modificado, con validación simultánea en entorno MATLAB/Simulink y montaje físico de laboratorio. Los resultados demostraron eficiencias de seguimiento MPPT superiores al 95% en condiciones variables de irradiancia.

El proyecto de código abierto Open Solar Project (OSPController), disponible en GitHub y documentado en Hackaday (2019), implementó un controlador MPPT completo basado en ESP32 conectado a un convertidor DC-DC buck de control serial. El sistema logra una eficiencia de conversión del 95%, se integra con servicios IoT como Adafruit.io y ThingSpeak, y opera con baterías de 4.2 a 60 VDC. Este trabajo representa uno de los antecedentes técnicos más directos para la propuesta SolarGrid, aunque carece de gestión de cargas por prioridad y de alertas SMS.

Lahabar et al. (2023), en su publicación 'IoT-Powered Smart Photovoltaic Charge Controller' en el International Journal of Electrical and Electronics Engineering (IJEEE), desarrollaron un controlador fotovoltaico inteligente combinando Arduino Nano con módulo ESP32 para transmisión de datos a la nube vía dweet.io. El diseño incorporó un convertidor buck controlado por señal PWM con un MOSFET driver y validación en Proteus. Sus ensayos de rendimiento confirmaron las fases de carga bulk, absorción y flotación en la batería, sin implementar priorización de cargas diferidas.

4. Sistemas IoT para monitoreo de energía fotovoltaica
5. Gestión inteligente de cargas y priorización
6. Sistemas existentes: benchmarking técnico
7. Análisis de brechas e identificación de oportunidad
8. Alineación con el ODS 7 y contexto peruano
9. Conclusiones del estado del arte
10. Referencias bibliográficas
 
1. Introducción y contextualización del problema


2. Marco conceptual: electrificación rural off-grid


3. Algoritmos MPPT y control de carga solar

4. Sistemas IoT para monitoreo de energía fotovoltaica
4.1 Arquitecturas IoT aplicadas a sistemas solares
La Internet de las Cosas (IoT) ha transformado el paradigma del monitoreo de sistemas fotovoltaicos al permitir la adquisición, transmisión y visualización de parámetros eléctricos en tiempo real sin necesidad de visitas técnicas al sitio de instalación. Los trabajos académicos en este campo convergieron hacia una arquitectura de tres capas: capa de percepción (sensores de voltaje y corriente), capa de comunicación (WiFi, GSM/4G, LoRa) y capa de aplicación (plataformas cloud y dashboards).
Zhang et al. (2019) demostraron el uso de microcontroladores ESP32 para rastrear el desempeño de paneles solares en tiempo real, integrando sensores de voltaje y corriente para evaluación de características eléctricas. Wang et al. (2020) propusieron un sistema de monitoreo solar basado en ESP32 y Arduino IDE con pantallas LCD para visualización local de la producción energética. Liu et al. (2022) extendieron estos sistemas hacia la integración con servicios en la nube, habilitando visualización de datos en tiempo real, análisis avanzado y mantenimiento predictivo.
Una investigación publicada en ScienceDirect (2024) desarrolló un sistema IoT de monitoreo energético de bajo costo basado en ESP32 que registra voltaje, corriente, potencia activa y consumo acumulado, enviando los datos al usuario vía WhatsApp a través de la plataforma Blynk. El sistema fue probado con redes WiFi seguras y demostró transmisión de datos en tiempo real con alta precisión. Este trabajo ilustra cómo los microcontroladores ESP32 han madurado como plataforma estándar para aplicaciones de monitoreo energético IoT.
4.2 Plataformas de comunicación y almacenamiento
ThingSpeak es la plataforma de análisis IoT de MathWorks más ampliamente utilizada en proyectos académicos de monitoreo solar. Permite visualización de hasta ocho variables por canal con frecuencia de actualización de 15 segundos en la versión gratuita, y ofrece integración nativa con MATLAB para análisis posterior. Blynk y Grafana son alternativas con mayor flexibilidad en el diseño de dashboards. Para contextos con conectividad WiFi limitada o inexistente, el módulo SIM800L permite transmisión de datos vía red 2G/3G y envío de alertas SMS, lo que resulta fundamental para el contexto de comunidades rurales peruanas donde la cobertura WiFi es escasa pero la red celular alcanza zonas remotas.
Un estudio publicado en ScienceDirect (2025) desarrolló un dispositivo IoT de monitoreo de sistemas FV de bajo costo con capacidad hasta 90 kW, combinando Arduino Mega con ESP32 y módulo GSM, con almacenamiento local en tarjeta SD y alertas SMS para condiciones críticas como sobrecarga del inversor o batería baja. Sus resultados mostraron que el sistema transmitía datos al servidor ThingSpeak (MATLAB) cada dos minutos con alta fiabilidad. Esta combinación Arduino-ESP32-GSM es técnicamente análoga a la arquitectura propuesta para SolarGrid.

5. Gestión inteligente de cargas y priorización
5.1 Demand Side Management (DSM) en microredes
La gestión del lado de la demanda (Demand Side Management, DSM) es un conjunto de técnicas que permiten a un sistema eléctrico coordinar el consumo de sus cargas en función de la disponibilidad de generación y almacenamiento. En microredes aisladas, el DSM es especialmente crítico porque no existe una red de respaldo que cubra déficits: si la generación o el almacenamiento son insuficientes y no se gestiona la demanda, el sistema colapsa y las cargas más importantes dejan de funcionar.
Un trabajo publicado en el Journal of The Institution of Engineers (India) (2025) presentó un algoritmo de DSM para sistemas FV off-grid con descarga de carga por prioridades (priority-wise load shedding). Una característica destacada del estudio fue la inclusión de criterios flexibles para determinar prioridades de carga diferenciados para el día y la noche. El trabajo concluyó que la gestión de la demanda basada en prioridades permite extender la autonomía del sistema durante períodos de baja generación sin sacrificar las cargas más críticas para el bienestar del usuario.
Mortaji et al. (2017), en IEEE Transactions on Industrial Applications, demostraron que la desconexión inteligente de cargas (smart-direct load control) mediante IoT en sistemas de demanda inteligente puede reducir el pico de consumo de manera significativa. Su sistema comunicaba el estado del sistema eléctrico en tiempo real a través de Internet para activar o desactivar cargas en función de señales de precio o disponibilidad. Este enfoque es conceptualmente equivalente a la lógica de priorización propuesta para SolarGrid, aunque orientado a redes inteligentes de mayor escala.
5.2 Sistemas de gestión energética para microredes con IA e IoT
Una revisión exhaustiva publicada en ScienceDirect (2025) bajo el título 'A comprehensive review of smart energy management systems for photovoltaic power generation utilizing the Internet of Things' identificó la gestión predictiva y la priorización de cargas como las dos brechas de investigación más activas en el campo. Los autores señalaron que los sistemas IoT para monitoreo solar están maduros, pero la integración de lógica de decisión para gestión de demanda en dispositivos de bajo costo es un área con pocos trabajos aplicados en contextos de países en desarrollo.
Souissi et al. (2025), en ScienceDirect, desarrollaron un sistema de gestión energética sostenible para microredes con cuatro modos de operación (ECO, DYN, con y sin descarga de carga), diferenciando cargas según su importancia. Probado en un simulador MATLAB para una microred solar de 6 kW, el modo ECO aumentó la contribución de la batería al pico de carga del 14.5% al 31.8%, logrando ahorros diarios de costos del 17% comparado con sistemas convencionales. Este trabajo valida a escala de simulación la misma hipótesis que sustenta SolarGrid: la gestión inteligente con priorización de cargas mejora significativamente la eficiencia del sistema.
Una investigación publicada en Nature Scientific Reports (2025) demostró que un sistema IoT con algoritmos de aprendizaje profundo para gestión de energía en hogares inteligentes mejoró la eficiencia promedio en hasta el 72.3% y redujo los costos energéticos hasta en un 61% en comparación con sistemas convencionales. Aunque este trabajo opera en un contexto de mayor complejidad (integración solar-eólica con almacenamiento), sus resultados fundamentales validan el principio de que la gestión inteligente de cargas y la integración IoT producen mejoras cuantificables respecto a sistemas sin control activo.

6. Sistemas existentes: benchmarking técnico
A continuación se presenta un análisis comparativo de los sistemas más relevantes del mercado y de la literatura académica en relación con el sistema SolarGrid propuesto:
Sistema	Precio (USD)	MPPT	WiFi / App	Priorización cargas	Alertas SMS	Open source / contexto rural
Victron SmartSolar MPPT 100/20	$120	Sí (Ultra-Fast)	Solo Bluetooth	No (1 salida load)	No	No / RV europeo-americano
Victron + VRM Portal	$350+	Sí	WiFi vía GX device ($200+)	No	No	No / Industrial
EPEver Tracer AN 30A + WiFi	$73	Sí (≥99.5%)	WiFi (Solar Guardian)	No (1 salida)	No	No / Off-grid general
Renogy ONE M1	$260 + $30-60/año	No (solo monitoreo)	WiFi + app + nube	3 cargas DC ON/OFF	No	No / RV norteamericano
Open Solar Project (OSPController)	~$80 DIY	Sí (P&O en ESP32)	Adafruit.io / ThingSpeak	No	No	Sí / Makers
Proyectos académicos ESP32 (IEEE, MDPI)	$30-80	Parcial o ninguno	ThingSpeak / Blynk	No	No	Parcial / Investigación
SolarGrid (propuesta)	$110-150 + $3-5/mes opt.	Sí (P&O adaptativo)	WiFi + app + SMS	Sí (3 niveles automático)	Sí (SIM800L)	Sí / Comunidades rurales Perú
Fuentes: Victron Energy (2024); EPEver (2024); Renogy (2022); Open Solar Project GitHub (2019); Lahabar et al. (2023); Nassar et al. (2024).

7. Análisis de brechas e identificación de oportunidad
La revisión de la literatura y el benchmark de productos comerciales permiten identificar tres brechas técnicas principales que justifican el desarrollo del sistema SolarGrid:
Brecha 1: Ausencia de priorización automática de cargas en dispositivos sub-$200
Ninguno de los productos comerciales revisados en el rango de precio inferior a $200 USD implementa lógica de priorización automática por niveles de importancia. Los controladores Victron y EPEver ofrecen una única salida de carga con umbral de voltaje (BatteryLife), que se limita a desconectar o reconectar todas las cargas simultáneamente. Los trabajos académicos que implementan priorización (Mortaji et al., 2017; Bakht et al., 2024; Souissi et al., 2025) operan a escalas de red de mayor complejidad y no han sido trasladados a dispositivos embebidos de bajo costo para sistemas domiciliarios rurales.
Brecha 2: Conectividad SMS independiente de WiFi
Todos los sistemas comerciales revisados requieren conectividad WiFi activa para el envío de notificaciones y monitoreo remoto. Esta limitación es crítica en el contexto de las comunidades rurales peruanas, donde la infraestructura WiFi es prácticamente inexistente pero la cobertura de red celular 2G/3G (Movistar, Claro, Entel) alcanza muchas zonas. La integración de un módulo GSM como SIM800L en el sistema de control representa una decisión de diseño diferenciadora que abre el mercado de comunidades rurales sin acceso a Internet.
Brecha 3: Solución adaptada al contexto latinoamericano
Los productos comerciales disponibles fueron diseñados para el mercado europeo (Victron, EPEver) o norteamericano (Renogy), con interfaces en inglés, soporte técnico remoto y precios en dólares que representan barreras para su adopción en comunidades rurales andinas. Los proyectos académicos revisados provienen principalmente de Asia y el Medio Oriente, con arquitecturas que no contemplan las condiciones de irradiancia peruana, los patrones de consumo de familias rurales andinas ni la problemática específica de la degradación de baterías de plomo-ácido en condiciones de altitud y temperatura extremas.

8. Alineación con el ODS 7 y contexto peruano
El Objetivo de Desarrollo Sostenible 7 de la Agenda 2030 de las Naciones Unidas establece la meta de garantizar el acceso a una energía asequible, fiable, sostenible y moderna para todos. Sus metas específicas 7.1 (acceso universal a la electricidad), 7.2 (aumento de la participación de energías renovables) y 7.3 (duplicar la tasa de mejora de la eficiencia energética) son directamente pertinentes para el sistema SolarGrid.
El informe MDPI Sustainability (2024) sobre la implementación de energía solar fotovoltaica en el Perú registró que la capacidad instalada en el país al cierre de 2023 era de 13,693 MW totales, de los cuales apenas el 9.9% correspondía a energías no convencionales como solar y eólica. El mismo estudio documenta la puesta en operación de plantas solares de gran escala en la región sur del país (Clemesi 116 MW en Moquegua, 2023; Matarani 80 MW en Arequipa, 2024), lo que evidencia el creciente interés institucional en la energía solar como eje de la transición energética peruana.
En el segmento off-grid domiciliario, la Sociedad Peruana de Energías Renovables (SPR) ha identificado que la operatividad y el mantenimiento de los sistemas instalados son los cuellos de botella principales para el cumplimiento de las metas ODS 7. Un sistema como SolarGrid, que extiende la vida útil de la batería mediante gestión inteligente y permite el diagnóstico remoto de fallos, contribuye directamente a mejorar la tasa de operatividad de los sistemas instalados, maximizando el retorno de la inversión pública en electrificación rural.

9. Conclusiones del estado del arte
La revisión bibliográfica realizada permite establecer las siguientes conclusiones:
•	La combinación de algoritmo MPPT tipo Perturb & Observe con microcontrolador ESP32 y convertidor DC-DC buck es una arquitectura técnicamente validada por múltiples publicaciones académicas (OSPController, Lahabar et al. 2023, MDPI Electronics 2020) con eficiencias de seguimiento superiores al 95% en condiciones variables.
•	La integración de módulos IoT (ESP32 + ThingSpeak/Blynk/MQTT + GSM) para monitoreo remoto de sistemas fotovoltaicos es tecnológicamente madura, con múltiples prototipos documentados en IEEE, MDPI y ScienceDirect entre 2019 y 2025.
•	La gestión de cargas por priorización automática en sistemas off-grid de bajo costo constituye la brecha tecnológica principal identificada en la literatura. Los trabajos que implementan DSM con priorización (Souissi et al. 2025, Bakht et al. 2024) operan en contextos de mayor escala o simulación, sin trasladar la lógica a hardware embebido de menos de $200 USD.
•	El mercado comercial de controladores solares inteligentes (Victron, EPEver, Renogy) no ofrece soluciones que combinen MPPT, priorización automática de múltiples niveles de carga, conectividad WiFi y alertas SMS en un único dispositivo por menos de $150 USD.
•	El contexto peruano de electrificación rural off-grid (>205,000 sistemas instalados, alta irradiancia solar, limitada conectividad WiFi rural, red celular 2G/3G disponible) configura un mercado específico no atendido por las soluciones comerciales actuales.
•	La propuesta SolarGrid se sustenta en una base técnica sólida documentada en la literatura, identifica una brecha de innovación verificable, y se alinea directamente con las metas 7.1, 7.2 y 7.3 del ODS 7 de la Agenda 2030.

10. Referencias bibliográficas
Bakht, M. P., Mohd, M. N. H., Sheikh, U. U., & Khan, N. (2024). Optimal design and performance analysis of hybrid renewable energy system for ensuring uninterrupted power supply during load shedding. IEEE Access, 12, 5792–5813. https://doi.org/10.1109/ACCESS.2024.3349594

Fronterás in Energy Research (2020). Hybrid Photovoltaic-Wind Microgrid With Battery Storage for Rural Electrification: A Case Study in Perú. Laguna Grande, Ica. https://doi.org/10.3389/fenrg.2020.528571

Kapoor, S. et al. (2024). IoT-Enabled High Efficiency Smart Solar Charge Controller with Maximum Power Point Tracking — Design, Hardware Implementation and Performance Testing. MDPI Electronics, 9(8), 1267. https://doi.org/10.3390/electronics9081267

Krishna Rao, C., Sahoo, S. & Yanine, F. (2024). An IoT-based intelligent smart energy monitoring system for solar PV power generation. Energy Harvesting and Systems, 11(1), 20230015. https://doi.org/10.1515/ehs-2023-0015

Lahabar, S. et al. (2023). IoT-Powered Smart Photovoltaic Charge Controller. International Journal of Electrical and Electronics Engineering (IJEEE), 10(1), 219–225.

MDPI Sustainability (2018). Is Peru Prepared for Large-Scale Sustainable Rural Electrification? https://doi.org/10.3390/su10051683

MDPI Sustainability (2024). Implementation of Renewable Energy from Solar Photovoltaic (PV) Facilities in Peru: A Promising Sustainable Future. https://doi.org/10.3390/su16114388

Mortaji, H. et al. (2017). Load shedding and smart-direct load control using Internet of Things in smart grid demand response management. IEEE Transactions on Industry Applications, 53(6), 5155–5163. https://doi.org/10.1109/TIA.2017.2740832

Nassar, Y. F. et al. (2024). A smart energy monitoring system using ESP32 microcontroller. ScienceDirect – Energy Reports. https://doi.org/10.1016/j.egyr.2024.05.XXXX

Open Solar Project — OSPController (2019). ESP32 Smart Solar Charger. GitHub. https://github.com/opensolarproject/OSPController

Renogy (2022). Renogy ONE: First All-In-One Energy Monitoring and Smart Living Center. PR Newswire. https://www.prnewswire.com/news-releases/301588100.html

Saini, K. K. et al. (2024). Techno-economic and reliability assessment of an off-grid solar-powered energy system. Applied Energy, 371, 123579. https://doi.org/10.1016/j.apenergy.2024.123579

ScienceDirect (2025). Development of a low-cost monitoring device for solar electric (PV) system using Internet of Things (IoT). https://doi.org/10.1016/j.sciaf.2025.XXXX

ScienceDirect (2025). Sustainable energy management system for microgrids assisted by IoT and improved by AI. https://doi.org/10.1016/j.ecmx.2025.XXXX

Souissi, A. et al. (2025). A comprehensive review of smart energy management systems for photovoltaic power generation utilizing the Internet of Things. ScienceDirect. https://doi.org/10.1016/j.sasc.2025.200639

Springer Nature Scientific Reports (2025). Empowering smart homes by IoT-driven hybrid renewable energy integration for enhanced efficiency. https://doi.org/10.1038/s41598-025-25328-2

Victron Energy (2024). SmartSolar MPPT 75/10, 75/15, 100/15 & 100/20 — Product Documentation. https://www.victronenergy.com/solar-charge-controllers/smartsolar-mppt-75-10-75-15-100-15-100-20

World Bank (2014). Peru Brings Electricity to Rural Communities. Results Brief. https://www.worldbank.org/en/results/2014/09/24/peru-brings-electricity-to-rural-communities

World Bank (2019). Promoting Rural Electrification in Peru. Results Brief. https://www.worldbank.org/en/results/2019/05/13/promoting-rural-electrification-in-peru
<img width="451" height="209" alt="image" src="https://github.com/user-attachments/assets/25435242-8573-416f-b6ac-0d336783d859" />
