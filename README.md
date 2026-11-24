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

### Layout visualization

`boards/shields/dasbob/info.json` describes the 36 physical key positions in
keymap order for tools such as [customkeymap.com](https://customkeymap.com).
Load `config/dasbob.keymap` and then drop the `info.json` onto the page. The
geometry is based on the [Dasbob physical layout](https://github.com/PandaScience/zmk-module-dasbob/blob/main/boards/shields/dasbob/dasbob-layouts.dtsi);
the thumb positions follow the switch order in the [original PCB](https://github.com/GroooveBob/DASBOB/blob/main/dasbob.kicad_pcb).
This file is only for visualization and does not change the firmware.

### Keymap: `config/dasbob.keymap`
- **Base Layer**: QWERTY mit Homerow Mods
- **Num Layer**: Zahlen & Funktionstasten
- **Sym Layer**: Symbole & Navigation
- **Misc Layer**: Spezielle Makros

**Display Names** sind für den Prospector Layer Roller optimiert.

### Bluetooth und USB

Auf dem Fun-Layer (linken Daumen auf Delete halten):

```text
                                    USB/BLE                        Profil löschen
                                    Profil 1   Profil 2   Profil 3   Profil 4   Studio
```

Die vier Profiltasten liegen auf der rechten Hälfte in der unteren
Buchstabenreihe. USB/BLE umschalten liegt in der mittleren Reihe ganz links
auf der rechten Hälfte; Profil löschen ganz rechts. Zum Löschen die Taste
1,5 Sekunden halten. Das löscht nur die Host-Kopplung des aktiven Profils,
nicht die Kopplung der Tastaturhälften.

Für ein neues Gerät ein freies Profil wählen und den Prospector am Gerät per
Bluetooth koppeln. Ist das Profil bereits belegt, zuerst die Kopplung am
alten Host entfernen und das Profil löschen. Ist der Prospector per USB mit
Strom versorgt, gegebenenfalls mit USB/BLE auf Bluetooth umschalten.

Falls die Keymap zuvor mit ZMK Studio geändert wurde, bleiben diese gespeicherten
Änderungen auch nach dem Flashen aktiv. „Restore Stock Settings“ in Studio
übernimmt die neue Firmware-Keymap und verwirft dabei die Studio-Änderungen.

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
