# CLAUDE.md

## Project Overview

ESP32-S3 radio dial prop for the Upside-Down escape room. Reads a potentiometer to detect station tuning and plays corresponding audio via DY-HV8F module.

**Target Board:** ESP32-S3-DevKitC-1 (N8R2: 8MB Flash, 2MB PSRAM)

## Build Commands

```bash
pio run                    # Build
pio run --target upload    # Upload to ESP32-S3
pio run --target clean     # Clean build
pio device monitor         # Serial monitor (115200 baud)
```

## Architecture

### Code Flow
```
main.cpp          → Entry points, ESP-NOW callbacks
├── setup.cpp     → Module initialization
├── loop.cpp      → Main loop dispatcher
├── RadioDial.cpp → Potentiometer reading, position detection
└── AudioPlayer.cpp → DY-HV8F control, track selection
```

### Key Modules
- **RadioDial**: Reads ADC, applies hysteresis, maps to 5 positions (0 = static zone)
- **AudioPlayer**: Controls DY-HV8F via UART, handles track switching with settle delay
- **ESP-NOW**: Client mode - listens for enable/disable from hub

### Configuration
All settings in `src/config.h`:
- `DEVICE_IDENTIFIER` - mDNS/OTA name
- `POT_PIN`, `DYPLAYER_*` - Hardware pins
- `NUM_POSITIONS`, `HYSTERESIS` - Dial tuning
- `DYPLAYER_VOLUME`, `DIAL_SETTLE_MS` - Audio settings

## ESP32-S3 Notes

- GPIO34 is ADC1_CH6 on ESP32-S3 (verify pin mapping)
- Different GPIO numbering than original ESP32
- PSRAM enabled for large audio buffers if needed
