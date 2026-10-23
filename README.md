# Reloj Digital de 6 Dígitos con ESP32

Proyecto de reloj digital basado en **ESP32**, diseñado con fines educativos y de desarrollo de hardware. Utiliza multiplexación de cátodo común controlada por transistores NPN y lógica de desplazamiento eficiente mediante un registro **74HCT595**, garantizando una interfaz robusta y de excelente brillo.

---

## 🚀 Características Principales

* **Controlador Central:** ESP32 (operando con lógica de control a 3.3V).
* **Interfaz de Visualización:** 6 displays de 7 segmentos de **cátodo común** (HH:MM:SS) multiplexados mediante transistores NPN (**BC547**).
* **Registro de Desplazamiento:** Integrado **74HCT595** (variante TTL-compatible), ideal para recibir señales de control a 3.3V del ESP32 mientras alimenta los displays y segmentos a **5V** para asegurar máxima luminosidad.
* **Módulo de Entradas (Pulsadores):** 
  * Botones físicos con red de filtrado **RC** antirrebote.
  * Inversores **Schmitt Trigger (`74HC14`)** alimentados a 3.3V para proteger los pines del microcontrolador y entregar flancos limpios.
* **Separadores:** LEDs separadores (`:`) controlados de forma independiente por transistor NPN para efectos de parpadeo (modo de configuración).

---

## 📌 Tabla de Conexiones Definitiva (ESP32)

| Bloque / Etapa | Señal / Función | Pin ESP32 | Descripción de la Conexión |
| :--- | :--- | :--- | :--- |
| **Entradas (Pulsadores)** | `MODE` (Configuración) | `GPIO 32` | Salida del inversor Schmitt Trigger (74HC14) del botón de Modo. |
| **Entradas (Pulsadores)** | `INC` (Incrementar) | `GPIO 33` | Salida del inversor Schmitt Trigger del botón de aumento. |
| **Entradas (Pulsadores)** | `DEC` (Decrementar) | `GPIO 25` | Salida del inversor Schmitt Trigger del botón de disminución. |
| **Entradas (Pulsadores)** | `SET` (Confirmar/Guardar) | `GPIO 26` | Salida del inversor Schmitt Trigger del botón de selección. |
| **Display (SPI / Shift)** | `DATA` (MOSI / DS) | `GPIO 23` | Conectado al pin DS (Pin 14) del registro 74HCT595. |
| **Display (SPI / Shift)** | `CLOCK` (SCK / SRCLK)| `GPIO 18` | Conectado al pin SRCLK (Pin 11) del registro 74HCT595. |
| **Display (SPI / Shift)** | `LATCH` (Storage / RCLK)| `GPIO 5` | Conectado al pin RCLK (Pin 12) del registro 74HCT595. |
| **Multiplexado** | `SEL1` (Horas - Decena) | `GPIO 13` | Base del transistor BC547 (Dígito 1) vía resistencia $4.7\text{ k}\Omega$. |
| **Multiplexado** | `SEL2` (Horas - Unidad) | `GPIO 12` | Base del transistor BC547 (Dígito 2) vía resistencia $4.7\text{ k}\Omega$. |
| **Multiplexado** | `SEL3` (Minutos - Decena)| `GPIO 14` | Base del transistor BC547 (Dígito 3) vía resistencia $4.7\text{ k}\Omega$. |
| **Multiplexado** | `SEL4` (Minutos - Unidad)| `GPIO 27` | Base del transistor BC547 (Dígito 4) vía resistencia $4.7\text{ k}\Omega$. |
| **Multiplexado** | `SEL5` (Segundos - Decena)| `GPIO 4` | Base del transistor BC547 (Dígito 5) vía resistencia $4.7\text{ k}\Omega$. |
| **Multiplexado** | `SEL6` (Segundos - Unidad)| `GPIO 15` | Base del transistor BC547 (Dígito 6) vía resistencia $4.7\text{ k}\Omega$. |
| **Interfaz Visual** | `COLON` (LEDs `:` ) | `GPIO 2` | Control de los puntos separadores con resistencia limitadora. |

---

## ⚡ Consideraciones de Diseño y Niveles Lógicos

1. **Alimentación Mixta Segura:** 
   * La etapa de potencia (displays y registro 74HCT595) opera a **5V** para garantizar brillo óptimo.
   * La etapa de control de pulsadores y el inversor Schmitt Trigger (`74HC14`) operan rígidamente a **3.3V** (tomados del regulador interno del ESP32), protegiendo los pines de entrada del microcontrolador.
2. **Compatibilidad HCT:** El uso del `74HCT595` permite recibir comandos a 3.3V desde el ESP32 sin requerir conversores de nivel externos, simplificando drásticamente el circuito impreso.

---

## 🛠️ Autor
**Prof. Damián Rimoldi**