# MSP430G2553 – Flujo Mixto (Energia + mspdebug)

Este proyecto documenta el flujo de trabajo mixto:

- ✏️ Edición y compilación con Energia  
- 🔥 Programación manual con `mspdebug`  
- 🎯 Control total del archivo `.elf`

Placa objetivo: MSP430G2553 (LaunchPad MSP-EXP430G2)

---

# 📦 Requisitos

Instalar dependencias:

```bash
sudo apt update
sudo apt install mspdebug
```

Energia debe estar previamente instalado.

---

# ⚙️ Configuración en Energia

Seleccionar:

Herramientas → Placa →  
```
MSP-EXP430G2 w/ MSP430G2553
```

Verificar puerto correcto en:

Herramientas → Puerto

---

# 🧪 Ejemplo de prueba (Blink)

```cpp
#define LED RED_LED

void setup() {
  pinMode(LED, OUTPUT);
}

void loop() {
  digitalWrite(LED, HIGH);
  delay(1000);
  digitalWrite(LED, LOW);
  delay(1000);
}
```

---

# ✔️ Compilar (NO subir)

Presionar únicamente:

✔ Verificar

Energia generará un directorio temporal:

```
/tmp/arduino_build_xxxxxx/
```

Dentro se encontrarán archivos como:

```
Blink.ino.elf
Blink.ino.hex
Blink.ino.cpp
```

---

# 📂 Ir al directorio de build

Entrar manualmente:

```bash
cd /tmp/arduino_build_xxxxxx
ls
```

Atajo para ir al último build generado:

```bash
cd $(ls -td /tmp/arduino_build_* | head -1)
```

---

# 🚀 Programar manualmente

Desde el directorio de build:

```bash
mspdebug rf2500 "erase prog Blink.ino.elf"
```

La placa será programada usando el `.elf` generado por Energia.

---

# 🎯 Ventajas del flujo mixto

- Se evita el uploader interno de Energia.
- Control total sobre el `.elf`.
- Permite inspeccionar binarios antes de grabar.
- Posibilidad de usar herramientas adicionales:
  
```bash
msp430-size Blink.ino.elf
msp430-objdump -d Blink.ino.elf
```

- Flujo más transparente y reproducible.

---

# 🔎 Flujo de trabajo recomendado

1. Editar código en Energia
2. Presionar ✔ Verificar
3. Ir a `/tmp/arduino_build_xxxxxx`
4. Programar con `mspdebug`

---

# 📌 Notas importantes

- El directorio `/tmp` es temporal y puede borrarse al reiniciar el sistema.
- No cerrar Energia antes de copiar o usar el `.elf` si se desea conservarlo.
- Verificar permisos USB (udev rules) si `mspdebug` requiere `sudo`.

---

# 📂 Estructura del repositorio

```
.
├── README.md
└── (archivos .ino desarrollados en Energia)
```

---

# 📜 Licencia

Uso educativo / laboratorio.

