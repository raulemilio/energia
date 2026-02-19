# Blink Bare-Metal – MSP430G2553

Ejemplo mínimo de parpadeo de LED en la MSP430G2553 usando:

- `msp430-gcc`
- `mspdebug`
- Sin Energia
- Sin Arduino core
- 100% bare-metal

---

## 📦 Requisitos

Linux (Ubuntu/Debian recomendado)

Instalar toolchain:

```bash
sudo apt update
sudo apt install gcc-msp430 msp430-libc mspdebug
```

Verificar instalación:

```bash
msp430-gcc --version
mspdebug --version
```

---

## 📁 Crear carpeta del proyecto

Crear ` mkdir /Blink`:

## 📁 Archivo fuente

Crear `blink.c`:

```c
#include <msp430.h>

int main(void)
{
    WDTCTL = WDTPW | WDTHOLD;   // Stop watchdog

    P1DIR |= BIT0;              // P1.0 como salida (LED rojo)
    
    while(1)
    {
        P1OUT ^= BIT0;          // Toggle LED
        __delay_cycles(500000); // ~0.5s delay @ 1MHz
    }
}
```

---

## ⚙️ Compilación

```bash
msp430-gcc -mmcu=msp430g2553 -Os -o blink.elf blink.c
```

Opcional: generar `.hex`

```bash
msp430-objcopy -O ihex blink.elf blink.hex
```

Ver tamaño del binario:

```bash
msp430-size blink.elf
```

---

## 🚀 Programación

Conectar LaunchPad y ejecutar:

```bash
mspdebug rf2500
```

```bash
(mspdebug) prog blink.elf 
```

```bash
(mspdebug) run

```
El LED conectado a **P1.0** debería comenzar a parpadear.

---

## 🧠 Qué está pasando

- Se desactiva el watchdog.
- Se configura P1.0 como salida.
- Se alterna el bit del puerto.
- El delay depende del DCO por defecto (~1 MHz).
- `-mmcu=msp430g2553` selecciona el linker script correcto.

---

## 📂 Estructura mínima del repo

```
.
├── blink.c
└── README.md
```

---

## 🎯 Objetivo

Este proyecto demuestra:

- Flujo embebido sin IDE gráfico
- Control total del hardware
- Compilación y flasheo desde CLI
- Base para proyectos más complejos (timers, interrupciones, bajo consumo)

---

## 📜 Licencia

Uso educativo / laboratorio.

