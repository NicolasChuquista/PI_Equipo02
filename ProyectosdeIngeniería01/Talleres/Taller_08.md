# Taller 08: Informe de Laboratorio
**Fecha:** 28 de abril de 2026  
**Equipo:** 02

---

## 1. Introducción
El presente laboratorio tuvo como propósito fundamental la integración de capacidades de hardware y software en el microcontrolador ESP32 para el desarrollo de un nodo de monitoreo inteligente. La experiencia se estructuró en dos fases críticas: La digitalización de señales analógicas mediante la captura de datos provenientes de sensores físicos, y la conectividad a través de la habilitación de protocolos de comunicación inalámbrica.

## 2. Objetivo
Consolidar una plataforma capaz de interactuar con el entorno y comunicarse dentro de una arquitectura de **Internet de las Cosas (IoT)**.

---

## 3. Reto 1

La fase inicial del laboratorio se centró en la implementación de una interfaz de entrada y salida para la adquisición de datos en tiempo real.

El proceso comenzó con la preparación del entorno de desarrollo, lo cual representó el primer desafío técnico del equipo. Se instaló el software Arduino IDE y fue necesario configurar el gestor de tarjetas mediante una URL adicional para descargar e instalar el core de ESP32 de Espressif Systems, permitiendo que el programa reconociera la arquitectura del microcontrolador.

Durante esta etapa, se identificaron dificultades de conectividad, ya que el hardware no era reconocido inicialmente por todas las laptops del equipo. Tras un proceso de descarte, se determinó que el problema radicaba en la ausencia de los drivers en algunos sistemas operativos; finalmente, se logró establecer la comunicación serial exitosa utilizando la computadora de uno de los integrantes. A pesar de que los tiempos de compilación y carga del código fueron prolongados, el flujo de trabajo se normalizó una vez establecida la vinculación entre el software y el hardware.

En cuanto al montaje físico, se vinculó un potenciómetro al pin GPIO 34, seleccionado por su resolución nativa de 12 bits en el convertidor analógico-digital (ADC). Esta configuración permitió transformar el rango de voltaje en una escala numérica de 0 a 4095 unidades. En paralelo, se acondicionó un actuador visual mediante un LED conectado al pin GPIO 2, protegido por una resistencia de 330Ω para garantizar la integridad del componente ante posibles picos de corriente.

<p align="center">
  <img src="https://raw.githubusercontent.com/NicolasChuquista/PI_Equipo02/refs/heads/main/img/Taller08_1.JPG" width="400">
  <br>
  <b>Figura 1: Circuito - Reto 1</b>
</p>

El software se configuró con un bus serie a 115200 baudios. El algoritmo ejecutaba una lectura constante del sensor y, mediante una estructura condicional, permitía variar la intensidad y el estado del LED. Al superar un umbral crítico de 2000 unidades, el sistema activaba el estado lógico del actuador. Este procedimiento validó la capacidad de respuesta del microcontrolador ante estímulos externos, permitiendo una interacción directa donde el giro del potenciómetro controlaba dinámicamente la respuesta lumínica del sistema.

<p align="center">
  <img src="https://raw.githubusercontent.com/NicolasChuquista/PI_Equipo02/refs/heads/main/img/Taller08_2.JPG" width="400">
  <br>
  <b>Figura 2: Panel - Reto 1</b>
</p>

---

## 4. Reto 2

La segunda fase consistió en la utilización de la librería `<WiFi.h>` para configurar el ESP32 en modo Estación (STA), realizando un escaneo dinámico de los canales de la banda de **2.4 GHz**. Este barrido permitió identificar SSIDs y evaluar la calidad del enlace mediante el indicador RSSI.


El proceso de conexión se gestionó de forma interactiva a través del Monitor Serie, permitiendo que el usuario seleccionara la red deseada y proporcionara las credenciales de acceso de manera dinámica.

El código implementó un algoritmo de espera activa que monitoreaba el registro de estado del chip hasta confirmar la asociación exitosa con el punto de acceso inalámbrico.

Finalmente, el sistema recuperó y desplegó la dirección IP asignada mediante el protocolo DHCP, lo que representa la obtención de una identidad digital dentro de la red, paso indispensable para el despliegue de cualquier sistema de monitoreo remoto o telemetría.

---

## 5. Análisis de Resultados y Conclusiones

Los resultados demuestran una alta fiabilidad en la digitalización de la señal analógica, con una respuesta fluida entre el movimiento del potenciómetro y los valores seriales. La estabilidad de la conexión WiFi, con tiempos de asociación rápidos, confirma la robustez del ESP32 como plataforma para estaciones de monitoreo.

### Conclusiones
* La integración exitosa establece las bases para proyectos de ingeniería de mayor envergadura.
* La capacidad de capturar datos físicos y contar con conectividad inalámbrica permite proyectar este sistema hacia el envío de métricas a la **nube**.
* Este flujo de trabajo facilita la recolección de información en campo y el análisis de datos masivos para la toma de decisiones en tiempo real.
