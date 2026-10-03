# ESP32 Marauder — ESP32-S3 Super Mini + 0.96" SSD1306

Compact build bundle based on the
[upstream ESP32 Marauder repository](https://github.com/justcallmekoko/ESP32Marauder),
master at `41f11df` (2026-10-03). It keeps the firmware source and the files required
to build this target; historical firmware binaries and unrelated board,
installer, and photo assets are omitted. The GitHub Actions workflow fetches
the required upstream libraries during each build.

## Default wiring

Power the OLED from **3V3**, share ground with the ESP32, and connect:

| Component | ESP32-S3 Super Mini pin |
|---|---:|
| OLED SDA | GPIO 8 |
| OLED SCL | GPIO 9 |
| Up button | GPIO 4 |
| Down button | GPIO 5 |
| Left button | GPIO 6 |
| Right button | GPIO 7 |
| Select / center button | GPIO 1 |
| Back button | GPIO 2 |

The OLED is configured as a 128×64 SSD1306 at I²C address `0x3C`. Each push
button connects its GPIO to **GND** when pressed; the firmware enables the
internal pull-ups. Use the pin labels and pinout for your exact Super Mini
revision before wiring. These defaults avoid GPIO0, GPIO3, GPIO45/46, and the
native USB pins GPIO19/20.

To change the defaults, edit the `MARAUDER_POOM` button block and OLED defines
in `esp32_marauder/configs.h`. The OLED driver and its SSD1306 initialization
are in `esp32_marauder/PoomDisplay.cpp`.

## Push from Termux

Create an **empty** GitHub repository first (do not pre-create a README or
license). In Termux, unzip this bundle, then run:

```sh
pkg update
pkg install git unzip gh
unzip ESP32Marauder-S3-SuperMini-OLED.zip
cd s3-marauder-build
gh auth login
git init -b main
git add .
git commit -m "Add ESP32-S3 Super Mini OLED firmware target"
git remote add origin https://github.com/OWNER/REPOSITORY.git
git push -u origin main
```

Replace `OWNER/REPOSITORY` with the empty repository you created. Authenticate
with GitHub in Termux; never put a token or password in the source files.

## Get the firmware

The included `.github/workflows/build-super-mini.yml` builds this board on
push to `main` or `master`, on pull requests, and when manually run. In GitHub,
open **Actions**, select **Build ESP32-S3 Super Mini OLED**, open the latest
successful run, and download its firmware artifact. It contains a merged
`esp32-s3-super-mini-oled-merged.bin` image for flashing at address `0x0`, plus
the individual application, bootloader, and partition binaries.

The workflow targets an ESP32-S3 with a 4 MB flash layout and Arduino-ESP32
3.3.4. A board with more flash can still use this 4 MB layout.

## Source and use

This is an unofficial hardware adaptation, not an upstream Marauder release.
The upstream source retains its own license in `LICENSE`. Use wireless
assessment features only on equipment and networks you own or have explicit
permission to test.