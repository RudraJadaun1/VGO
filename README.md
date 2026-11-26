# VGO v1 — Compact Wireless Embedded Development Platform

**Creator:** Rudra  
**Hardware Revision:** Rev 1.0  
**Form Factor:** 51.0 mm × 21.0 mm (4-Layer FR-4)

VGO v1 is an original, compact wireless embedded development platform built around the **STM32WB55RGV6** dual-core Bluetooth 5.4 / IEEE 802.15.4 microcontroller. Engineered for precision sensing, battery-powered portability, and rapid prototyping, VGO v1 integrates a 6-axis IMU, high-precision barometric pressure sensor, 1S LiPo charging with dynamic power-path management, a dedicated I2C battery fuel gauge, and plug-and-play Qwiic/STEMMA QT expansion.

---

## Key Features

- **Dual-Core Wireless MCU:** STMicroelectronics `STM32WB55RGV6` (Arm Cortex-M4 @ 64 MHz + Cortex-M0+ @ 32 MHz for BLE 5.4 / 802.15.4, 1 MB Flash, 256 KB SRAM, USB 2.0 FS) in VFQFPN-68.
- **RF Front-End:** ST `MLPF-WB55-01E3` integrated harmonic filter/matching network paired with a Johanson `2450AT18A100E` 2.4 GHz ceramic chip antenna and full all-layer antenna keepout.
- **6-Axis Motion Sensing:** TDK InvenSense `ICM-42688-P` 16-bit accelerometer & gyroscope on dedicated `SPI1` (`PB3/PB4/PB5`, `CS = PD1`, `INT1/2 = PB6/PB7`).
- **Barometric Pressure & Temperature:** Bosch `BMP580` on `I2C1` (`0x46`, `PB8/PB9`, `INT = PC0`, `CSB` pulled up to `+3.3V` via `10 kΩ`).
- **1S LiPo Power & Fuel Gauge:**
  - TI `BQ24072RGT` charger (`500 mA` charge current, `1.5 A` input limit, dynamic power-path `VSYS`, `~PGOOD` on `PC13`).
  - ADI/Maxim `MAX17048G+T10` fuel gauge on `I2C1` (`0x36`) for real-time state-of-charge (%) and cell voltage telemetry.
  - Keyed 2-pin `JST-SH` 1.0 mm battery connector (`J5`).
  - Physical `SW_PWR` slide switch (`PCM12SMTR`) controlling `TPS631000DRLR` `EN` for ultra-low storage drain.
- **High-Efficiency 3.3 V Regulation:** TI `TPS631000DRLR` buck-boost converter (`1.6 V–5.5 V` input → `3.31 V` output, `1.5 A` continuous / `3 A` peak).
- **USB-C & Protection:** 16-pin USB-C receptacle with `5.1 kΩ` `CC1/CC2` pulldowns, `TPD2EUSB30` D+/D- ESD protection, `SMF5.0A` VBUS TVS surge clamp, and `PA9` `VBUS_SENSE` divider.
- **Expansion & Debugging:**
  - Two 1×20 `2.54 mm` headers (`17.78 mm` row spacing) breaking out 30 user GPIOs, `+3.3V`, `VSYS`, `VBUS`, and `GND`.
  - `Qwiic / STEMMA QT` 4-pin `JST-SH` I2C port (`J4`).
  - Dedicated 5-pin `1.27 mm` `SWD` debug header (`J3`: `3V3`, `SWDIO`, `GND`, `SWCLK`, `NRST`) plus bottom test pads.
  - `BOOT0` (`SW1`), `RESET` (`SW2`), `USER_BTN` (`SW3` on `PC12`), `PWR` Green LED (`D2`), `CHG` Amber LED (`D3`), and `SK6812MINI-E` addressable RGB LED (`D4` on `PA8`).

---

## Hardware Architecture

| Subsystem | Component | Interface / Address |
|---|---|---|
| **MCU** | `STM32WB55RGV6` | `64 MHz` M4 + `32 MHz` M0+, `32 MHz` HSE, `32.768 kHz` LSE |
| **IMU** | `ICM-42688-P` | `SPI1` (`PB3/4/5`), `CS = PD1`, `INT1/2 = PB6/7` |
| **Barometer** | `BMP580` | `I2C1` (`PB8/9`), Address `0x46`, `INT = PC0` |
| **Fuel Gauge** | `MAX17048G+T10` | `I2C1` (`PB8/9`), Address `0x36` |
| **Charger** | `BQ24072RGTR` | `VBUS` → `VSYS` / `VBAT`, `~PGOOD = PC13` |
| **Regulator** | `TPS631000DRLR` | `VSYS` → `+3.3V` (`EN` via `SW_PWR`) |
| **RGB LED** | `SK6812MINI-E` | `PA8` |
| **User Button** | `SW3` | `PC12` (Active Low, 10 kΩ pull-up) |

---

## Repository Structure

```text
VGO-v1/
├── .github/                 # CI firmware build workflow & issue templates
├── documentation/           # Architecture, electrical calculations, pinout, bring-up & guides
├── firmware/                # Bare-metal C driver & diagnostic foundation (arm-none-eabi-gcc)
├── hardware/                # KiCad 6 project, 13-sheet schematic, 4-layer PCB & self-contained libs
├── production/              # Gerbers, Excellon drill files, BOM.csv, positions.csv & fab notes
├── renders/                 # Front, back, and 3D assembly renders
├── validation/              # 20-point engineering validation, ERC, DRC & netlist reports
├── CHANGELOG.md
├── CONTRIBUTING.md
├── JOURNAL.md
├── LICENSE
└── README.md
