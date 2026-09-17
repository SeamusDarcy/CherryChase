# Cherry Chase

An embedded game written in C for the STM32F031K6 microcontroller, built on the
Nucleo-F031K6 development board and drawn on a 128 × 160 SPI colour display.

The player moves an animated character around the screen with four physical
directional buttons and tries to reach the target. It was made as a way to
practise low-level embedded programming talking to hardware through direct
register access, driving a display over SPI, and writing simple graphics and
game logic from scratch without a game engine or graphics framework.

## Features

- Four-direction movement using physical GPIO buttons
- Animated character sprites that change orientation with the direction of travel
- Movement clamped to the visible play area
- Collision detection against the target, with a win state
- Custom bitmap graphics for the menu and character frames
- A hand-written SPI display driver and graphics routines
- SysTick-based timing and delays
- A small Python utility for converting BMP images into C colour data

## Controls

The game reads four buttons wired to GPIO pins:

| Action | Pin |
| --- | --- |
| Move right | PB4 |
| Move left | PB5 |
| Move up | PA8 |
| Move down | PA11 |

Reaching the target displays `WIN` on the screen.

## Project layout

| Path | Description |
| --- | --- |
| `src/main.c` | Hardware setup, GPIO input, the game loop, movement, animation and collision detection. |
| `src/display.c` | SPI and display initialisation plus the low-level graphics primitives. |
| `src/display.h` | Declarations for the display and graphics functions. |
| `src/font5x7.h` | A 5 × 7 ASCII bitmap font used for on-screen text. |
| `bmptoh.py` | Converts BMP images into 16-bit colour values for use as C arrays. |
| `platformio.ini` | PlatformIO build configuration. |
| `*.bmp` | Source images for the menu graphics and character animation frames. |

## Graphics primitives

The display driver exposes a small set of drawing operations that the game is
built from:

```
putPixel()      putImage()
drawLine()      drawRectangle()
drawCircle()    fillCircle()
printText()     printTextX2()     printNumber()
```

## Converting images

`bmptoh.py` turns a BMP file into the 16-bit colour values the microcontroller
renders. It requires [Pillow](https://python-pillow.org/):

```bash
pip install pillow
python bmptoh.py image.bmp
```

The printed comma-separated values can be pasted straight into a `const uint16_t`
array in the C source.

## Building

This project uses [PlatformIO](https://platformio.org/).

### Requirements

- PlatformIO (CLI, or the VS Code extension)
- A Nucleo-F031K6 board
- A compatible 128 × 160 SPI colour display

The environment is defined in `platformio.ini`:

```ini
[env:nucleo_f031k6]
platform = ststm32
board = nucleo_f031k6
framework = cmsis
```

Build the firmware:

```bash
pio run
```

Flash it to a connected board:

```bash
pio run --target upload
```

## Notes

A college project, and an exercise in embedded C register-level GPIO and SPI,
SysTick timing, and rendering graphics directly on a microcontroller display.
