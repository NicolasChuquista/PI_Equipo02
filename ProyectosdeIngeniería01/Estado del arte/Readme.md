# 💧 AquaBalde

## Sistema Inteligente de Monitoreo de Calidad de Agua

Proyecto desarrollado en el curso **Proyectos I — 2024**

AquaBalde es un sistema integrado de monitoreo de calidad de agua diseñado para comunidades rurales y periurbanas. Permite evaluar si el agua almacenada en recipientes domésticos es **APTA o NO APTA para consumo humano** mediante sensores multiparamétricos de bajo costo.

---

# 📌 Problema

En muchas comunidades rurales de América Latina:

* No existe acceso continuo a agua potable segura
* El agua se almacena en baldes sin control sanitario
* Los análisis de laboratorio son costosos y poco accesibles
* Muchos contaminantes son invisibles al usuario

Según la OMS:

> Aproximadamente 2.000 millones de personas consumen agua contaminada.

Esto genera:

* enfermedades gastrointestinales
* intoxicaciones crónicas
* mortalidad infantil
* pérdida económica familiar

---

# 🚀 Solución: AquaBalde

AquaBalde integra sensores dentro de un balde doméstico de doble pared capaz de medir:

* pH
* Turbidez
* Conductividad eléctrica

Entrega resultados en menos de:

```
60 segundos
```

Mostrando:

```
🟢 APTA
🟡 PRECAUCIÓN
🔴 NO APTA
```

---

# 📊 Parámetros monitoreados

| Parámetro     | Rango          | Límite OMS  |
| ------------- | -------------- | ----------- |
| pH            | 0 – 14         | 6.5 – 8.5   |
| Turbidez      | 0 – 1000 NTU   | < 5 NTU     |
| Conductividad | 0 – 2000 µS/cm | < 400 µS/cm |
| Coliformes    | Cualitativo    | 0 UFC/100mL |


# 🧠 Arquitectura del sistema

## Sensores

* Electrodo pH ISFET
* Sensor turbidez óptico 880nm
* Electrodo conductividad acero 316L

## Procesamiento

```
ESP32-S3
```

## Interfaz usuario

* Pantalla OLED 1.3”
* NeoPixel Ring
* Buzzer

## Energía

* Batería LiPo 3000mAh
* Panel solar opcional


# 🔬 Funcionamiento técnico

Proceso:

```
Inicio
↓
Auto-test sensores
↓
Lectura multiparamétrica
↓
Procesamiento
↓
Clasificación OMS
↓
Visualización resultado

# 📈 Benchmark competitivo

| Solución  | pH | Turbidez | EC | Coliformes | Precio       |
| --------- | -- | -------- | -- | ---------- | ------------ |
| AquaBalde | ✔  | ✔        | ✔  | ✔          | 70 USD       |
| Libelium  | ✔  | ✔        | ✔  | ✔          | >3000 USD    |
| Hach      | ✔  | ✔        | ✔  | ✘          | 800–2500 USD |
| mWater    | ✘  | ✘        | ✘  | Manual     | Gratis       |

Ventaja principal:

```
Integración multiparamétrica en recipiente doméstico
```

---

# 🌍 Impacto ODS

AquaBalde contribuye a:

### ODS 6

Agua limpia y saneamiento

### ODS 3

Salud y bienestar

### ODS 9

Innovación tecnológica

### ODS 10

Reducción de desigualdades

### ODS 17

Alianzas para desarrollo

---

# 👥 Usuario objetivo

Jefes de hogar en:

```
Comunidades rurales
Comunidades periurbanas
Zonas sin red de agua potable segura
```

Beneficios:

* reducción enfermedades
* información inmediata
* decisión informada
* tecnología accesible

---

# 🔐 Potencial de patente

Elementos protegibles:

* integración multiparamétrica en recipiente
* algoritmo clasificación semafórica
* arquitectura doble pared

---

# 📚 Referencias

* WHO (2023)
* UNICEF & WHO JMP (2023)
* IEEE Sensors (2023)
* Libelium Smart Water Platform
* AquaWatch (Water Research 2022)
* NTC-813 Calidad de agua potable

---

# 📌 Resumen ejecutivo

AquaBalde es un sistema de monitoreo doméstico de calidad de agua que integra sensores multiparamétricos dentro de un recipiente de almacenamiento convencional, permitiendo determinar rápidamente si el agua es segura para consumo humano en contextos rurales con infraestructura limitada.

Precio objetivo:

```
< USD 35
```

Tiempo medición:

```
< 60 segundos
```

Impacto potencial:

```
> 500 millones de hogares en países en desarrollo
```
