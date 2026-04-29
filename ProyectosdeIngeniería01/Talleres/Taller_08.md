# Taller 08: Informe de Laboratorio
**Fecha:** 28 de abril de 2026  
**Equipo:** 02

---

## 1. Resumen
El presente laboratorio tuvo como propósito fundamental la integración de capacidades de hardware y software en el microcontrolador **ESP32** para el desarrollo de un nodo de monitoreo inteligente. La experiencia se estructuró en dos fases críticas:
1. **Digitalización de señales analógicas:** Captura de datos provenientes de sensores físicos.
2. **Conectividad:** Habilitación de protocolos de comunicación inalámbrica.

## 2. Objetivo
Consolidar una plataforma capaz de interactuar con el entorno y comunicarse dentro de una arquitectura de **Internet de las Cosas (IoT)**.

---

## 3. Metodología y Procedimiento Experimental: Reto 1

La fase inicial se centró en la implementación de una interfaz de entrada y salida para la adquisición de datos en tiempo real. Se realizó el montaje físico vinculando un potenciómetro al pin **GPIO 34**, seleccionado por su resolución nativa de **12 bits** en el convertidor analógico-digital (ADC). 

Esta configuración permitió transformar el voltaje (0V a 3.3V) en una escala numérica de **0 a 4095 unidades**. En paralelo, se acondicionó un actuador visual mediante un LED conectado al pin **GPIO 2** con una resistencia limitadora de **330Ω**.

<p align="center">
  <img src="https://raw.githubusercontent.com/NicolasChuquista/PI_Equipo02/refs/heads/main/img/Taller08_1.JPG" width="400">
  <br>
  <b>Figura 1: Circuito - Reto 1</b>
</p>

En cuanto al software, se configuró el bus serie a **115200 baudios**. El algoritmo ejecutaba una lectura constante y, mediante una estructura condicional, activaba el LED al superar un umbral crítico de **2000 unidades**, validando la capacidad de respuesta del microcontrolador ante estímulos externos.

<p align="center">
  <img src="https://raw.githubusercontent.com/NicolasChuquista/PI_Equipo02/refs/heads/main/img/Taller08_2.JPG" width="400">
  <br>
  <b>Figura 2: Panel - Reto 1</b>
</p>

---

## 4. Desarrollo de Conectividad Inalámbrica: Reto 2

La segunda fase consistió en la habilitación del módulo de radiofrecuencia. Utilizando la librería `<WiFi.h>`, se configuró el ESP32 en modo **Estación (STA)**, realizando un escaneo dinámico de los canales de la banda de **2.4 GHz**. Este barrido permitió identificar SSIDs y evaluar la calidad del enlace mediante el indicador **RSSI** (*Received Signal Strength Indicator*).

El proceso de conexión se gestionó de forma interactiva a través del **Monitor Serie**:
* El usuario seleccionaba la red detectada.
* Se proporcionaban las credenciales de acceso.
* El código implementaba una espera activa hasta confirmar la asociación con el punto de acceso (Hotspot).
* Finalmente, se desplegó la **dirección IP** asignada mediante el protocolo **DHCP**.

---

## 5. Análisis de Resultados y Conclusiones

### Análisis
Los resultados demuestran una alta fiabilidad en la digitalización de la señal analógica, con una respuesta fluida entre el movimiento del potenciómetro y los valores seriales. La estabilidad de la conexión WiFi, con tiempos de asociación rápidos, confirma la robustez del ESP32 como plataforma para estaciones de monitoreo.

### Conclusiones
* La integración exitosa establece las bases para proyectos de ingeniería de mayor envergadura.
* La capacidad de capturar datos físicos y contar con conectividad inalámbrica permite proyectar este sistema hacia el envío de métricas a la **nube**.
* Este flujo de trabajo facilita la recolección de información en campo y el análisis de datos masivos para la toma de decisiones en tiempo real.
