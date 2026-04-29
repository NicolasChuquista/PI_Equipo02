Taller 08: Informe
Fecha: 28 de abril de 2026


1. Resumen y Objetivos
El presente documento describe el proceso de configuración y validación de un nodo de procesamiento basado en el microcontrolador ESP32. El objetivo principal fue integrar la captura de variables analógicas del entorno con capacidades de comunicación inalámbrica mediante el stack de protocolos WiFi. Para ello, se diseñó un sistema capaz de cuantificar cambios de voltaje a través de un potenciómetro y, simultáneamente, gestionar la autenticación en redes locales para la futura transmisión de datos en aplicaciones de ingeniería ambiental.

2. Metodología y Procedimiento Experimental: Reto 1
La fase inicial del laboratorio se centró en la implementación de una interfaz de entrada y salida para la adquisición de datos en tiempo real. Se realizó el montaje físico vinculando un potenciómetro al pin GPIO 34 del ESP32, el cual fue seleccionado por su resolución nativa de 12 bits en el convertidor analógico-digital (ADC). Esta configuración permitió transformar el rango de voltaje de 0V a 3.3V en una escala numérica de 0 a 4095 unidades. En paralelo, se acondicionó un actuador visual mediante un LED conectado al pin GPIO 2 con una resistencia limitadora de 220Ω para garantizar la integridad del componente.

En cuanto al desarrollo del software en Arduino IDE, se programó una lógica de control de lazo abierto que utilizaba el bus serie a una velocidad de 115200 baudios. El programa ejecutaba una lectura constante del sensor y, mediante una estructura condicional, activaba el estado lógico del LED al superar un umbral crítico de 2000 unidades. Este procedimiento validó la capacidad del microcontrolador para procesar señales externas y ejecutar acciones de control local de manera inmediata.

3. Desarrollo de Conectividad Inalámbrica: Reto 2
La segunda fase consistió en la habilitación del módulo de radiofrecuencia para integrar el dispositivo en una red de área local. Utilizando la librería estándar WiFi.h, se configuró el ESP32 en modo Estación (STA), procediendo inicialmente a un escaneo dinámico de los canales de la banda de 2.4 GHz. Este barrido permitió identificar los SSIDs disponibles y evaluar la calidad del enlace mediante el indicador RSSI (Received Signal Strength Indicator), fundamental para determinar la viabilidad de la transmisión de datos en entornos con interferencia.

El proceso de conexión se gestionó de forma interactiva a través del Monitor Serie, donde el usuario seleccionaba la red y proporcionaba las credenciales de acceso. El código implementó un algoritmo de espera activa que monitoreaba el registro de estado del chip hasta confirmar la asociación exitosa con el router (Hotspot). Finalmente, el sistema recuperó y desplegó la dirección IP asignada mediante el protocolo DHCP, lo que representa la obtención de una identidad digital dentro de la red, paso indispensable para cualquier proyecto de monitoreo remoto o IoT.

4. Análisis de Resultados y Conclusiones
Los resultados obtenidos demuestran una alta fiabilidad en la digitalización de la señal analógica, observándose una respuesta fluida y coherente entre el movimiento mecánico del potenciómetro y los valores reflejados en el terminal. Asimismo, la estabilidad de la conexión WiFi, con tiempos de asociación menores a cinco segundos, confirma que el ESP32 es una plataforma robusta para el despliegue de estaciones de monitoreo autónomas.

Como conclusión, la integración de estos dos retos permite establecer las bases técnicas para sistemas de ingeniería más complejos. La capacidad de capturar datos físicos y contar con una interfaz de comunicación inalámbrica funcional abre la posibilidad de enviar métricas ambientales a servidores en la nube (Cloud Computing), optimizando la recolección de información en campo y permitiendo el análisis de datos masivos para la toma de decisiones tecnológicas.
