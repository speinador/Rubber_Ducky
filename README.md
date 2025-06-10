
# 💻 Rubber Ducky Casero con Microcontroladores (Digispark, Raspberry Pi Pico y ESP32)

Este proyecto te permite crear dispositivos estilo "Rubber Ducky" utilizando placas como **Digispark ATtiny85**, **Raspberry Pi Pico** o **ESP32**. Se comportan como teclados (HID) capaces de ejecutar comandos automáticamente al conectarse a una PC o por Bluetooth.

> ⚠️ Este proyecto es solo para fines educativos y de pruebas éticas. El uso no autorizado puede ser ilegal.

---

## 🧰 Materiales por placa

### 🔹 Digispark ATtiny85

- Digispark (con USB integrado)
- PC con Arduino IDE

### 🔹 Raspberry Pi Pico

- Raspberry Pi Pico
- Cable microUSB
- Firmware CircuitPython

### 🔹 ESP32

- Placa ESP32 (con soporte BLE)
- Cable microUSB
- Biblioteca BLE Keyboard

---

## 🔧 Instalación por plataforma

### 🛠️ Digispark

1. Abrí Arduino IDE
2. Agregá esta URL en `Archivo → Preferencias → URLs adicionales`:
   ```
   http://digistump.com/package_digistump_index.json
   ```
3. Instalá "Digistump AVR Boards" desde el Gestor de Placas
4. Seleccioná `Digispark (Default - 16.5mhz)` en Herramientas > Placa

### 🛠️ Raspberry Pi Pico

1. Descargá CircuitPython desde [circuitpython.org](https://circuitpython.org/board/raspberry_pi_pico/)
2. Copiá el archivo `.uf2` al Pico (modo BOOTSEL)
3. En el almacenamiento "CIRCUITPY", copiá el código `.py`
4. Incluí la librería `adafruit_hid`

### 🛠️ ESP32

1. Instalá soporte para ESP32 en Arduino IDE:
   - Agregá URL: `https://raw.githubusercontent.com/espressif/arduino-esp32/gh-pages/package_esp32_index.json`
2. Instalá la placa ESP32 desde el Gestor de Placas
3. Usá la librería [ESP32 BLE Keyboard](https://github.com/T-vK/ESP32-BLE-Keyboard)

---

## 🧪 Ejemplos de Payloads

### ✅ Digispark

```cpp
#include "DigiKeyboard.h"
void setup() {
  DigiKeyboard.sendKeyStroke(0);
  DigiKeyboard.delay(500);
  DigiKeyboard.sendKeyStroke(KEY_R, MOD_GUI_LEFT);
  DigiKeyboard.delay(500);
  DigiKeyboard.print("cmd");
  DigiKeyboard.sendKeyStroke(KEY_ENTER);
  DigiKeyboard.print("echo HACKED > C:\\hacked.txt");
  DigiKeyboard.sendKeyStroke(KEY_ENTER);
}
void loop() {}
```

### ✅ Raspberry Pi Pico (CircuitPython)

```python
import time, usb_hid
from adafruit_hid.keyboard import Keyboard
from adafruit_hid.keyboard_layout_us import KeyboardLayoutUS
from adafruit_hid.keycode import Keycode

kbd = Keyboard(usb_hid.devices)
layout = KeyboardLayoutUS(kbd)
kbd.press(Keycode.WINDOWS, Keycode.R)
kbd.release_all()
time.sleep(0.5)
layout.write("cmd")
kbd.press(Keycode.ENTER)
kbd.release_all()
layout.write("echo PICO HACKED > C:\\pico.txt")
kbd.press(Keycode.ENTER)
```

### ✅ ESP32 (Bluetooth HID)

```cpp
#include <BleKeyboard.h>
BleKeyboard bleKeyboard;
void setup() {
  bleKeyboard.begin();
}
void loop() {
  if (bleKeyboard.isConnected()) {
    bleKeyboard.press(KEY_LEFT_GUI);
    bleKeyboard.press('r');
    bleKeyboard.releaseAll();
    delay(500);
    bleKeyboard.print("cmd\n");
    delay(500);
    bleKeyboard.print("echo ESP32 HACKED > C:\\esp32.txt\n");
    while (1);
  }
  delay(100);
}
```

---

## 🔌 Diagramas y Conexión por Pines

### 📌 Digispark ATtiny85

- Conecta directo por USB
- Pines:
  - P0 → D0
  - P1 → D1
  - P2 → D2 (PWM)
  - P5 → RESET

### 📌 Raspberry Pi Pico

- Conexión microUSB (no necesita cableado GPIO para HID)
- Si deseás añadir LED:
  - LED: GPIO25 (positivo), GND (negativo)

### 📌 ESP32

- Conexión microUSB
- Opcional LED:
  - LED: GPIO2 (positivo), GND (negativo)

---

## 🔀 Duckyscript → Arduino

Podés convertir payloads Rubber Ducky al formato Arduino usando:

- [Dckuino.js](https://github.com/Nurrl/Dckuino.js)
- [Duckduino](https://github.com/Plazmaz/duckduino)

---

## 📷 Ejemplos de uso

- Automatizar tareas en PC
- Pentesting de acceso físico
- Simulación de usuarios

---

## 📆 Estructura del repositorio

```bash
rubber-ducky-casero/
├── digispark/
│   └── ejemplo_payload.ino
├── raspberry-pico/
│   └── payload.py
├── esp32/
│   └── ble_payload.ino
├── docs/
│   ├── diagramas.png
│   └── logo.png
├── .gitignore
└── README.md
```

---

## 📄 .gitignore sugerido

```gitignore
__pycache__/
.DS_Store
*.uf2
*.pyc
build/
.idea/
*.bin
*.hex
*.elf
```

---

## 📌 Consejos de Seguridad

- No usar sin permiso
- En ambientes controlados
- No almacenar código malicioso permanente

---

## 🤝 Créditos

Proyecto educativo basado en Hak5, Duckyscript, DigiKeyboard, CircuitPython HID y BLE Keyboard de T-vK.
