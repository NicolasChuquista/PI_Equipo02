# Taller 08: Informe de Laboratorio
**Fecha:** 28 de abril de 2026  
**Equipo:** 02

**Integrantes:**
- Tomás del Castillo Mogollón
- Flavio Francisco Rabanal Bravo
- Nicolás Genaro Chuquista Rivadeneira

---

El presente laboratorio tuvo como propósito fundamental la integración de capacidades de hardware y software en el microcontrolador ESP32 para el desarrollo de un nodo de monitoreo inteligente. La experiencia se estructuró en dos fases críticas: La digitalización de señales analógicas mediante la captura de datos provenientes de sensores físicos, y la conectividad a través de la habilitación de protocolos de comunicación inalámbrica.

---

## Reto 1

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

## Reto 2

La segunda etapa consistió en activar el WiFi del ESP32 para que pudiera conectarse a internet.

Al inicio, la principal dificultad fue localizar e instalar correctamente la librería `<WiFi.h>` dentro del entorno de Arduino, ya que es un componente indispensable para que el código pueda dar instrucciones al chip de radio del microcontrolador. Una vez configurado el modo Estación (STA), el dispositivo comenzó a realizar un escaneo de todas las señales inalámbricas cercanas en la banda de 2.4 GHz.

Durante las pruebas de escaneo, el equipo notó que el ESP32 no detectaba con la misma facilidad todas las redes disponibles. Por ejemplo, aunque los tres integrantes activaron el "Hotspot" de sus celulares, el dispositivo solo lograba identificar la señal de uno de ellos. Esto nos permitió observar cómo factores como la configuración de la banda del celular o la potencia de transmisión influyen directamente en la visibilidad del dispositivo.

El proceso de conexión se gestionó de forma interactiva a través del Monitor Serie, permitiendo que el usuario seleccionara la red deseada y proporcionara las credenciales de acceso de manera dinámica.

Seleccionamos la red detectada en la lista y escribimos la contraseña manualmente. Tras ello, observamos cómo el código entraba en una "espera activa" mientras intentaba el enlace. Una vez lograda la conexión, el router nos asignó una dirección IP mediante el protocolo DHCP. Esta "identidad digital" es el paso final que confirma que nuestro ESP32 ya es parte de una red y está listo para enviar datos de sensores a cualquier servidor remoto.

---

## Reto 3

<p align="center">
  <img src="https://raw.githubusercontent.com/NicolasChuquista/PI_Equipo02/refs/heads/main/img/1 CAP.png" width="400">
  <br>
  <b>Figura 3 </b>
</p>
Como primer paso esta descargar el archivo .JSON brindado en clase para poder importarlo 
<p align="center">
  <img src="https://raw.githubusercontent.com/NicolasChuquista/PI_Equipo02/refs/heads/main/img/2 CAP.png" width="400">
  <br>
  <b>Figura 3 </b>
</p>
Luego con el archivo descargado lo importamos en "Node-RED" para visualizar el dashboard del documento.

<p align="center">
  <img src="https://raw.githubusercontent.com/NicolasChuquista/PI_Equipo02/refs/heads/main/img/3 CAP.png" width="400">
  <br>
  <b>Figura 3 </b>
</p>
Logramso ver que el documento se importo de manera correcta sin errores. 

<p align="center">
  <img src="https://raw.githubusercontent.com/NicolasChuquista/PI_Equipo02/refs/heads/main/img/4 CAP.png" width="400">
  <br>
  <b>Figura 3 </b>
</p>
Llenamos el apartado de TEMA con el nombre el cual se va identificar y tener presente este nombre ya que se va utililzar mas adelante.

<p align="center">
  <img src="https://raw.githubusercontent.com/NicolasChuquista/PI_Equipo02/refs/heads/main/img/5 CAP.png" width="400">
  <br>
  <b>Figura 3 </b>
</p>
Damos click en el boton de "instanciar" para poder proseguir con los siguientes pasos.

<p align="center">
  <img src="https://raw.githubusercontent.com/NicolasChuquista/PI_Equipo02/refs/heads/main/img/6 CAP.png" width="400">
  <br>
  <b>Figura 3 </b>
</p>
Instalamos el programa MQTTX para la comunicacion de datos y completamos con los datos y numeor de host correspondiente.

<p align="center">
  <img src="https://raw.githubusercontent.com/NicolasChuquista/PI_Equipo02/refs/heads/main/img/7 CAP.png" width="400">
  <br>
  <b>Figura 3 </b>
</p>
Para finalizar usamos el parametro de "temp" de ejemplo para corroborar que el envio de datos sea el correcto. 

<p align="center">
  <img src="https://raw.githubusercontent.com/NicolasChuquista/PI_Equipo02/refs/heads/main/img/8 CAP.png" width="400">
  <br>
  <b>Figura 3 </b>
</p>
Finalmente se pude observar el mimso valor colocado en el programa que que da salida al "gauge".

---

## Conclusiones

El desarrollo de este laboratorio permitió validar la eficacia del ESP32 como una unidad central de procesamiento capaz de cerrar la brecha entre el entorno físico y el ecosistema digital.

Los resultados demuestran una alta fiabilidad en la digitalización de señales analógicas, donde la resolución del ADC proporcionó una respuesta fluida y coherente entre el desplazamiento mecánico del potenciómetro y los valores reflejados en el terminal. Asimismo, a pesar de los desafíos técnicos iniciales relacionados con la configuración del software y la compatibilidad de hardware entre los equipos, la estabilidad de la conexión WiFi y la rapidez en la asociación de red confirmaron la robustez de la plataforma para operar en entornos de monitoreo dinámicos.

Podemos concluir que la integración exitosa de la captura de datos y la conectividad inalámbrica establece las bases técnicas para proyectos de ingeniería de mayor envergadura. Este flujo de trabajo no solo facilita la recolección de datos en campo de manera autónoma, sino que habilita el análisis de datos masivos en la nube, optimizando la toma de decisiones tecnológicas basadas en evidencias precisas y transmitidas en tiempo real.
