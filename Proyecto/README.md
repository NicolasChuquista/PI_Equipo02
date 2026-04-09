# AquaBalde 🪣💧
> Sistema Inteligente de Monitoreo de Calidad de Agua para Comunidades Rurales y Periurbanas.

## ⚠️ El Problema
En diversas zonas alejadas y periurbanas, muchas familias se ven obligadas a recolectar agua de la naturaleza (ríos, lagos, manantiales) y guardarla en recipientes comunes sin poder verificar su estado. No saber si este recurso es seguro expone a las personas a graves peligros sanitarios y enfermedades gástricas o infecciosas. 

Esta situación evidencia una falta de cumplimiento de las recomendaciones de la OMS y de las normas nacionales vigentes, como el DS N° 031-2010-SA (Reglamento de la Calidad del Agua para Consumo Humano) en el territorio peruano.

## 💡 La Solución
AquaBalde es un recipiente inteligente creado para evaluar el agua almacenada. Funciona sobre un balde de doble pared hecho de polietileno de alta densidad (HDPE) apto para alimentos. El equipo integra sensores ópticos y electroquímicos de muy bajo consumo.

No es necesario ser un experto para usarlo: una vez que se llena el balde, el dispositivo analiza el líquido y en menos de un minuto indica de forma clara si es APTA o NO APTA para el consumo.

### ⚙️ Características Principales
* **Interfaz Intuitiva:** La información se muestra en una pantalla OLED de 1.3" y se apoya en un semáforo de luces LED RGB.
* **Diseño Seguro:** Cuenta con cierre hermético y sus componentes electrónicos están protegidos contra el agua (certificación IP67) entre las paredes del recipiente para evitar contaminación cruzada.
* **Autonomía y Economía:** Opera con una batería recargable de 3000mAh (por puerto micro-USB o panel solar) y no requiere pagos mensuales; solo se cambia el cartucho del biosensor una vez al año.

## 📊 Parámetros Evaluados
El dispositivo utiliza una lógica basada en los límites de salud pública. El resultado solo será "APTA" si todos los valores están dentro de lo permitido:

* **Acidez (pH):** Evaluado con electrodo ISFET/vidrio (Rango ideal: 6.5 - 8.5).
* **Turbidez:** Analizado mediante sensor infrarrojo a 880nm (Límite: < 5 NTU).
* **Conductividad Eléctrica:** Medida con puntas de acero inoxidable 316L (Límite: < 400 µS/cm).
* **Bacterias (Coliformes):** Detección cualitativa usando un biosensor enzimático (Límite: 0 UFC/100mL).

## 🌍 Impacto y ODS (Objetivos de Desarrollo Sostenible)
El proyecto se alinea con las metas globales de la Agenda 2030:
* **ODS 6 (Agua Limpia y Saneamiento):** Ayuda a democratizar la verificación del agua en poblaciones que no cuentan con redes de tuberías seguras.
* **ODS 3 (Salud y Bienestar):** Funciona como un escudo preventivo para disminuir la mortalidad por enfermedades que se transmiten por agua contaminada.

## 🛠️ Stack Tecnológico (Hardware & Firmware)
* **Procesador Central:** Microcontrolador ESP32-S3 WROOM (Xtensa LX7 Dual Core 240MHz).
* **Desarrollo de Software:** Programado en C/C++ usando FreeRTOS y ESP-IDF.
* **Comunicaciones:** Módulos WiFi 2.4GHz y Bluetooth LE 5.0 para enviar datos si se requiere.
* **Conversión de Datos:** ADC de 12-bit incorporado para leer los valores de los sensores.
