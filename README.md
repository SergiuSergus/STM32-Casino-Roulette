# WS2812B LED Strip Controller using STM32F103C8T6

This project implements control over a 24-LED WS2812B (NeoPixel) strip using the STM32F103C8T6 microcontroller. The data signal required by the LEDs is generated through Timer2 in PWM mode, with DMA used to transfer the data efficiently.

## Features

- Control of 24 WS2812B LEDs (individually addressable)
- GRB color format
- DMA transfer for low CPU usage
- Effects: static color, blinking, and roulette-style animation
- Developed without STM32CubeIDE
- Pure C code using STM32 HAL drivers

## Hardware

- **Microcontroller**: STM32F103C8T6 (Blue Pill)
- **LED Strip**: WS2812B, 24 LEDs
- **Data Pin**: PA1 (TIM2_CH2)
- **Power Supply**: 5V for the LED strip

## How it works

- Timer2 is configured for PWM output with a 1.25 µs period to match WS2812B timing.
- DMA1_Channel7 is used to transfer a prepared array of pulse widths to the timer's compare register.
- Each LED requires 24 bits (GRB), and a full buffer is generated and sent to the strip.
- A short delay after the DMA transfer ensures the latch time (>50 µs) is respected.

## Build & Flash

1. Use your preferred ARM GCC toolchain and Makefile or flash tool.
2. Flash the `.hex` or `.bin` to the STM32F103C8T6 using ST-Link or UART bootloader.

## Memory Usage (approximate)

- **PWM buffer**: 24 LEDs × 24 bits = 576 entries (uint16_t) = 1152 bytes
- **Stack/Heap**: minimal
- Total RAM usage: ~1.2KB (of 20KB available on STM32F103C8T6)

## Notes

- Timing is critical for WS2812B — incorrect PWM values or DMA timing can lead to LEDs not responding.
- Ensure that the LEDs receive a stable 5V and that data signal levels are compatible (you may need a level shifter).

## License

MIT License – free to use and modify.
