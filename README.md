# ZMK Config - DasBob with Prospector Dongle

ZMK Configuration für das DasBob Split Keyboard mit Prospector Dongle als BLE Central.

## Hardware

- **Keyboard Halves**: DasBob mit nice!nano v2
- **Dongle/Central**: [Prospector](https://github.com/carrefinho/prospector) mit Seeed XIAO nRF52840
- **Display**: Prospector mit 1.69" IPS LCD Screen
- **Connection**: Wireless BLE

## Firmware Components

```
build.yaml
├── dasbob_left   (nice_nano_v2)  - Linke Keyboard-Hälfte
├── dasbob_right  (nice_nano_v2)  - Rechte Keyboard-Hälfte
└── prospector_adapter (seeeduino_xiao_ble) - Display Dongle & Central
```

## Installation & Build

### 1. Prerequisites
- [ZMK CLI](https://docs.zmk.dev/docs/users/getting-started/installation) oder [ZMK Builder](https://zmkfirmware.dev/)
- Git

### 2. Build all firmwares
```bash
# Mit ZMK CLI
west build -d build/dasbob_left -b nice_nano_v2 -s app -- -DSHIELD=dasbob_left
west build -d build/dasbob_right -b nice_nano_v2 -s app -- -DSHIELD=dasbob_right
west build -d build/prospector -b seeeduino_xiao_ble -s app -- -DSHIELD=prospector_adapter
```

Oder einfach den ZMK Builder nutzen - dieser Repository wird automatisch erkannt.

### 3. Flash
- **Dasbob Left**: `build/dasbob_left/zephyr/zmk.uf2` → nice!nano left
- **Dasbob Right**: `build/dasbob_right/zephyr/zmk.uf2` → nice!nano right
- **Prospector**: `build/prospector/zephyr/zmk.uf2` → Seeed XIAO nRF52840

## Configuration

### Keymap: `config/dasbob.keymap`
- **Base Layer**: QWERTY mit Homerow Mods
- **Num Layer**: Zahlen & Funktionstasten
- **Sym Layer**: Symbole & Navigation
- **Misc Layer**: Spezielle Makros

**Display Names** sind für den Prospector Layer Roller optimiert.

### Prospector Settings: `config/prospector_adapter.conf`
- Auto Brightness mit Ambient Light Sensor
- Layer Roller Anzeige
- Batterie-Status von Peripherals

## Pairing

1. **Prospector flashen** und einschalten
2. **Dasbob Left flashen** - sollte automatisch mit Prospector pairen
3. **Dasbob Right flashen** - sollte automatisch mit Prospector pairen

> **Wichtig**: Pairing-Reihenfolge ist wichtig für Battery-Widget Layout!

## Prospector-Features

- 🎨 Layer Roller mit Display Names
- 🔋 Batterie-Status für beide Halves
- 🔗 Connection Status
- 💡 Auto-Brightness mit Lichtsensor
- ✋ Caps Word Indicator

## Weitere Infos

- [Prospector Repository](https://github.com/carrefinho/prospector)
- [Prospector ZMK Module Docs](https://github.com/carrefinho/prospector-zmk-module)
- [ZMK Documentation](https://docs.zmk.dev/)
- [DasBob Repository](https://github.com/tapioki/cephalopoda)
