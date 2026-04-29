Informe de Laboratorio: Reto 1 y 2
1. Introducción
En esta sesión se exploraron las capacidades del microcontrolador ESP32, una herramienta versátil en el desarrollo de proyectos de Ingeniería Ambiental y monitoreo en tiempo real. El trabajo se dividió en dos etapas: la integración de hardware para la captura de señales físicas y la configuración de comunicación inalámbrica.

2. Reto N° 1: Adquisición de Datos y Control Local
Objetivo: Leer una señal analógica de un sensor y controlar un actuador de forma física.

2.1 Configuración de Hardware
Sensor: Se utilizó un potenciómetro conectado al pin D34. Este pin actúa como ADC (Conversor Analógico a Digital), transformando el voltaje variable en valores numéricos entre 0 y 4095.

Actuador: Un LED conectado al pin D2, protegido por una resistencia de 330Ω.

2.2 Lógica de Programación
El código cargado en Arduino IDE permitió:

Monitorear el valor del sensor en tiempo real a través del Monitor Serie.

Implementar una lógica condicional: si el valor superaba el umbral de 2000, el LED se encendía; de lo contrario, permanecía apagado.

3. Reto N° 2: Escaneo y Conexión a Red (WiFi)
Objetivo: Habilitar la conectividad inalámbrica del ESP32 para su futura integración en redes IoT.

3.1 Implementación de Software
Se utilizó la librería WiFi.h. El proceso consistió en:

Escaneo de Redes: El ESP32 identificó los puntos de acceso cercanos, mostrando su nombre (SSID) y potencia de señal (RSSI).

Interfaz Interactiva: Mediante el Monitor Serie, se seleccionó la red correspondiente al hotspot de un dispositivo móvil.

Autenticación y Asignación de IP: Tras ingresar la contraseña, el dispositivo estableció una conexión exitosa, obteniendo una dirección IP local.

4. Análisis y Conclusiones
Integración de Sistemas: Se demostró que el ESP32 puede procesar información del entorno (Reto 1) y, simultáneamente, tener la capacidad de transmitir esa información a través de una red (Reto 2).

Importancia del Monitor Serie: Esta herramienta fue fundamental en ambos retos para depurar errores y visualizar el comportamiento interno del programa.

Aplicación Práctica: Estos ejercicios sientan las bases para desarrollar estaciones de monitoreo ambiental autónomas que puedan enviar datos de sensores (como calidad de aire o agua) a la nube de manera inalámbrica.
