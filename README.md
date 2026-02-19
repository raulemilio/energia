# MSP430G2553 con Energia – Instalación Completa en Linux

Guía completa para instalar y configurar Energia en Linux para trabajar con la MSP430G2553.

Incluye:

- Instalación de dependencias
- Configuración de permisos USB (udev rules)
- Selección de placa
- Actualización de biblioteca MSP430
- Compilación y grabación del firmware

---

# 📦 1. Requisitos

Sistema: Linux (Ubuntu/Debian recomendado)

Instalar dependencias:

```bash
sudo apt update
sudo apt install default-jre mspdebug
```

Verificar:

```bash
java -version
mspdebug --version
```

---

# 🔐 2. Configurar permisos USB (udev rules) ⚠ IMPORTANTE

Si no se configura esto, será necesario usar `sudo` para programar la placa.

Crear archivo de reglas:

```bash
sudo nano /etc/udev/rules.d/99-ti-launchpad.rules
```

Agregar:

```bash
# TI MSP430 LaunchPad
SUBSYSTEM=="usb", ATTR{idVendor}=="0451", ATTR{idProduct}=="f432", MODE="0666"
```

Guardar y luego ejecutar:

```bash
sudo udevadm control --reload-rules
sudo udevadm trigger
```

Desconectar y volver a conectar la LaunchPad.

Verificar que ahora se pueda ejecutar:

```bash
mspdebug rf2500
```

sin `sudo`.

---

# ⬇️ 3. Descargar e instalar Energia

Descargar desde:

https://energia.nu/download/

Ejemplo:

```bash
tar -xvf energia-1.8.10E23-linux64.tgz
mkdir -p ~/opt
mv energia-1.8.10E23 ~/opt/
```

Ejecutar:

```bash
cd ~/opt/energia-1.8.10E23
./energia
```

---

# ⚙️ 4. Configurar la placa

En Energia:

Herramientas → Placa →  

```
MSP-EXP430G2 w/ MSP430G2553
```

Herramientas → Puerto → seleccionar `/dev/ttyACM0` (o el correspondiente)

---

# 🔄 5. Actualización necesaria de la biblioteca MSP430

Para que la compilación funcione correctamente fue necesario actualizar la biblioteca MSP430 incluida en Energia.

Problema observado:
- Fallas en compilación
- Generación incorrecta del `.elf`
- Incompatibilidades con toolchains actuales

Solución aplicada:
- Reemplazar el core MSP430 en:

```
energia/hardware/msp430/
```

por una versión actualizada.
- Verificar que el `.elf` se genere correctamente en:

```
/tmp/arduino_build_xxxxx/
```

Después de la actualización, la compilación y programación funcionaron correctamente.

---

# 🧪 6. Proyecto de prueba (Blink)

Ejemplo mínimo:

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

# ✔️ 7. Compilar

Presionar **Verificar**.

Energia generará el build en:

```
/tmp/arduino_build_xxxxxx/
```

Dentro se encontrará:

```
Blink.ino.elf
Blink.ino.hex
```

---

# 🚀 8. Programar (método recomendado)

En lugar de usar el uploader interno de Energia, se recomienda usar `mspdebug`:

```bash
cd /tmp/arduino_build_xxxxxx
mspdebug rf2500 "erase prog Blink.ino.elf"
```

Ventajas:
- Mayor control
- Evita problemas del uploader Java
- Permite debug manual

---

# 🔎 Flujo recomendado de trabajo

1. Editar código en Energia
2. Presionar ✔ Verificar
3. Ir al directorio `/tmp/arduino_build_xxxxxx`
4. Programar con `mspdebug`

Atajo para ir al último build:

```bash
cd $(ls -td /tmp/arduino_build_* | head -1)
```

---

# 🎯 Resultado esperado

- Compilación exitosa
- Generación correcta del `.elf`
- Programación sin `sudo`
- LED en P1.0 parpadeando

---

# 📌 Notas importantes

- La MSP430G2553 usa P1.0 como LED rojo en la LaunchPad.
- Si no detecta la placa:
  - Verificar reglas udev
  - Confirmar idVendor / idProduct con `lsusb`
  - Revisar permisos del dispositivo

---

# 📜 Licencia

Uso educativo / laboratorio.

