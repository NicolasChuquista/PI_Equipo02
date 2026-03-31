# AquaBalde

Sistema inteligente de monitoreo básico de calidad de agua con activación automática por peso, usando ESP32 y sensores de pH, TDS y turbidez.

## Descripción general

AquaBalde es un prototipo de monitoreo de agua que se activa únicamente cuando detecta ingreso de agua mediante una celda de carga. Una vez activado, el sistema mide parámetros fisicoquímicos básicos y muestra el estado del agua mediante LEDs.

El diseño mecánico considera un **doble fondo**, destinado a alojar y proteger los componentes electrónicos frente a humedad, salpicaduras y contacto directo con el agua.

## Componentes usados

- ESP32
- Celda de carga de 20 kg
- Módulo HX711 para celda de carga
- Módulo sensor de pH líquido con sonda BNC
- Módulo sensor TDS
- Sensor de turbidez de agua
- LEDs
- Cables
- Motores (reservados para futuras mejoras)

## Lógica de funcionamiento

1. El usuario vierte agua en el balde.
2. La celda de carga detecta el aumento de peso.
3. El ESP32 activa el sistema.
4. Se leen los sensores de pH, TDS y turbidez.
5. Se procesan los datos.
6. Se clasifica el agua en un nivel de riesgo.
7. Los LEDs indican el resultado final.

## Lista de exigencias

### 1. Exigencias funcionales

| Nº | Tipo | Función | Descripción técnica | Criterio de verificación |
|----|------|--------|---------------------|--------------------------|
| 1 | E | Activación por peso | El sistema se activa automáticamente mediante celda de carga al detectar ingreso de agua | Activación ≥ 1 kg |
| 2 | E | Medición de pH | Medición mediante sensor pH con sonda BNC | Rango 0–14 |
| 3 | E | Medición de TDS | Medición de sólidos disueltos mediante sensor TDS | Lectura estable |
| 4 | E | Medición de turbidez | Medición mediante sensor óptico de turbidez | Lectura estable |
| 5 | E | Clasificación | El sistema interpreta los parámetros y determina el estado del agua | Resultado visible por LED |

### 2. Exigencias electrónicas

| Nº | Tipo | Elemento | Descripción técnica | Verificación |
|----|------|----------|---------------------|-------------|
| 6 | E | Microcontrolador | Uso de ESP32 para procesamiento de datos | Funcionamiento estable |
| 7 | E | Sensor de peso | Celda de carga de 20 kg con HX711 | Detección correcta |
| 8 | E | Conexiones | Cableado adecuado para sensores y salidas | Sin fallas de conexión |
| 9 | E | Salida visual | LEDs para indicar nivel de riesgo | Encendido correcto |
| 10 | D | Actuadores | Motores reservados para futuras mejoras | No obligatorio en versión actual |

### 3. Exigencias mecánicas

| Nº | Tipo | Elemento | Descripción técnica | Verificación |
|----|------|----------|---------------------|-------------|
| 11 | E | Material del recipiente | Balde apto para uso con agua | Inspección física |
| 12 | E | Capacidad | Capacidad operativa del sistema | 10–20 L |
| 13 | E | Resistencia estructural | Debe soportar el peso total del agua | ≥ 20 kg |
| 14 | E | Doble fondo | Compartimento inferior para alojar electrónica | Presencia física |
| 15 | E | Aislamiento | Componentes electrónicos aislados del agua | 0 filtraciones |
| 16 | E | Soporte de sensores | Sensores fijados con estabilidad | Sin movimiento durante uso |

### 4. Exigencias de integración

| Nº | Tipo | Función | Descripción | Verificación |
|----|------|--------|-------------|-------------|
| 17 | E | Integración de sensores | Sensores operan con ESP32 dentro del mismo sistema | Lecturas estables |
| 18 | E | Tiempo de activación | El sistema inicia medición tras detectar peso | ≤ 5 s |
| 19 | E | Procesamiento | El ESP32 procesa y clasifica el estado del agua | Resultado coherente |

### 5. Exigencias de interfaz

| Nº | Tipo | Función | Descripción | Verificación |
|----|------|--------|-------------|-------------|
| 20 | E | Indicador visual | LEDs muestran el nivel de riesgo | Verde / Amarillo / Rojo |
| 21 | E | Interpretación simple | El usuario comprende el resultado sin capacitación técnica | Comprensión directa |
| 22 | D | Pantalla | Implementación futura de OLED | No obligatoria en esta versión |

## Criterios de clasificación del agua

La clasificación del agua en este prototipo es referencial y se basa en umbrales simples:

- **Riesgo bajo:** pH entre 6.5 y 8.5, TDS bajo, turbidez baja
- **Riesgo medio:** un parámetro en zona de alerta
- **Riesgo alto:** uno o más parámetros críticos fuera de rango

## Estructura del repositorio

```text
AquaBalde/
├── README.md
├── src/
│   └── AquaBalde.ino
└── docs/
    └── pinout.md
