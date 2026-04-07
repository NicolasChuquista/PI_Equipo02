# LISTA DE EXIGENCIAS

**Proyecto:** AquaBalde — Sistema de Monitoreo de Calidad de Agua en Balde Doméstico  
**Cliente:** Universidad Peruana Cayetano Heredia — Curso Proyectos I  
**Fecha:** 01/04/2026  
**Elaborado:** Raúl J., Tomás C., Nicolás C., Flavio R.  

## 1. MECÁNICA

| Fecha (cambios) | Deseo o exigencia | Descripción | Responsable |
|---|---|---|---|
| 01/04/26 | E | Estructura: El sistema electrónico debe estar integrado en la doble pared del recipiente, protegido del contacto directo con el agua almacenada. | Flavio R. |
| 01/04/26 | E | Material: El recipiente debe estar fabricado con materiales grado alimentario, libres de sustancias tóxicas que puedan contaminar el agua. | Raúl J. |
| 01/04/26 | E | Cámara de sensado: El sistema debe contar con una cámara o compuerta que permita la entrada controlada de la muestra de agua y su retorno al exterior tras el análisis. | Flavio R. |
| 01/04/26 | E | Protección: Los componentes electrónicos expuestos a humedad deben contar con protección contra ingreso de agua adecuada para uso doméstico intensivo. | Nicolás C. |
| 01/04/26 | D | Capacidad: Se desea que el recipiente tenga una capacidad útil de 10 litros en su versión estándar, adecuada para uso familiar cotidiano. | Raúl J. |
| 01/04/26 | D | Peso: Se desea que el peso total del sistema no supere 1 kg para facilitar su transporte y uso cotidiano en zonas rurales. | Flavio R. |

## 2. ELECTRÓNICA

| Fecha (cambios) | Deseo o exigencia | Descripción | Responsable |
|---|---|---|---|
| 01/04/26 | E | Alimentación: El sistema debe operar con una fuente de energía portátil de bajo voltaje (batería recargable) sin necesidad de conexión a red eléctrica. | Nicolás C. |
| 01/04/26 | E | Encendido/apagado: El sistema debe incluir un mecanismo físico de encendido y apagado manual que interrumpa completamente el suministro de energía. | Nicolás C. |
| 01/04/26 | E | Reinicio: El sistema debe incluir un botón de RESET/PLAY que permita reiniciar el ciclo de medición sin necesidad de apagar el equipo. | Nicolás C. |
| 01/04/26 | E | Medición de pH: El sistema debe medir el nivel de pH del agua mediante un sensor electroquimico en el rango de 0 a 14, con resolución mínima de ±0.1 unidades. | Nicolás C. |
| 01/04/26 | E | Medición de turbidez: El sistema debe medir la turbidez del agua mediante un sensor óptico, expresando el resultado en unidades NTU. | Nicolás C. |
| 01/04/26 | E | Medición de conductividad: El sistema debe medir la conductividad eléctrica del agua mediante un sensor de dos o más electrodos, expresando el resultado en µS/cm como indicio de presencia de metales o sales disueltas. | Nicolás C. |
| 01/04/26 | E | Detección de coliformes: El sistema debe detectar la presencia de coliformes termotolerantes mediante un sensor electroquimico de cartucho reemplazable, entregando un resultado cualitativo (positivo/negativo). | Raúl J. |
| 01/04/26 | E | Señal visual: El sistema debe emitir una señal visual diferenciada (LED o pantalla) para cada nivel de riesgo, identificable sin necesidad de leer texto. | Nicolás C. |
| 01/04/26 | E | Señal auditiva: El sistema debe emitir una alarma sonora cuando el resultado corresponda a un nivel de alto riesgo para el consumo humano. | Nicolás C. |
| 01/04/26 | D | Bajo consumo: Se desea que el sistema opere en modo reposo de bajo consumo energético para maximizar la autonomía de la batería. | Nicolás C. |
| 01/04/26 | D | Reposición: Se desea que los sensores sean reemplazables de forma individual, sin necesidad de herramientas especiales, para facilitar el mantenimiento en zonas rurales. | Raúl J. |

## 3. SOFTWARE

| Fecha (cambios) | Deseo o exigencia | Descripción | Responsable |
|---|---|---|---|
| 01/04/26 | E | Inicialización: El software debe verificar el estado de todos los sensores y realizar un autodiagnóstico antes de cada ciclo de medición. | Tomás C. |
| 01/04/26 | E | Lectura y conversión: El software debe leer las señales eléctricas de los sensores y convertirlas a valores físicos calibrados (pH, NTU, µS/cm, resultado coliformes). | Tomás C. |
| 01/04/26 | E | Umbrales normativos: El software debe comparar los valores medidos contra los umbrales definidos en las Guías de Calidad del Agua de la OMS (4.ª edición) para determinar aptitud de consumo. | Raúl J. |
| 01/04/26 | E | Clasificación: El software debe clasificar el resultado en tres niveles — Alto Riesgo, Mediano Riesgo y Bajo Riesgo — y comunicarlo al usuario mediante la interfaz del dispositivo. | Tomás C. |
| 01/04/26 | E | Borrado de datos: El software debe eliminar automáticamente los datos de la medición anterior al iniciar un nuevo ciclo, garantizando lecturas independientes. | Tomás C. |
| 01/04/26 | E | Señal de fin de proceso: El software debe emitir una señal (visual y/o auditiva) que indique al usuario que el proceso de medición ha concluido correctamente. | Tomás C. |
| 01/04/26 | D | Tiempo de respuesta: Se desea que el tiempo total desde la activación hasta la presentación del resultado no supere los 60 segundos, excluyendo la incubación del biosensor de coliformes. | Tomás C. |
| 01/04/26 | D | Historial: Se desea que el software almacene un registro de mediciones anteriores con fecha y hora en la memoria interna del dispositivo, para seguimiento de la calidad del agua a lo largo del tiempo. | Raúl J. |
| 01/04/26 | D | Conectividad: Se desea que el software permita transmitir los datos de medición a un dispositivo móvil mediante conectividad inalámbrica, para monitoreo comunitario y reporte a autoridades sanitarias. | Raúl J. |
