# Homekit led strip

## What it does

Based on a NEO-Pixel WS2812, RGB LED strip. ON/OFF, Brightness, HUE and Saturation.

## Wiring

Connect `LED STRIP` pin to the following pin:

| Name | Description | Defaults |
|------|-------------|----------|
| `CONFIG_ESP_LED_GPIO` | GPIO number for `LED` pin | "2" Default |



## Notes

- Choose your GPIO number under `StudioPieters` in `menuconfig`. The default is `2` (On an ESP32 WROOM 32D).
- Choose your strip length under `StudioPieters` in `menuconfig`. The default is `3`.
- Set your `WiFi SSID` and `WiFi Password` under `StudioPieters` in `menuconfig`.
- Optional: You can change `HomeKit Setup Code` and `HomeKit Setup ID` under `StudioPieters` in `menuconfig`. (Note:  you need to make a new QR-CODE To make it work)

<b>REPRODUCTION STEPS</b>

Open a terminal window on your mac.

```
docker pull espressif/idf:latest
```
- At this point idf (ESP-IDF v5.3-dev-2815-gbe06a6f5ff)
```
git clone --recursive https://github.com/jonkofee/esp32-homekit-ledstrip.git
```
```
docker run -it -v ./:/project -w /project espressif/idf:latest
```
```
idf.py set-target esp32
```
```
idf.py menuconfig
```
- Select `Serial flasher config` and then `Flash size (2MB)` set to `4MB`
- Select `Partition table` and then `Partition Table(Single factory app, no OTA)` Set to `Custom partition table CSV`
- Select `StudioPieters` and then `(mysid) WIFI SSID` and fill in your Wi-Fi Network name, then Select `(mypassword) WiFI Password` and fill in your Wi-Fi Network password.
- Then press `ESC` until you are asked `Save Configuration?` and select `(Y)es`
```
idf.py build
```
Open a new terminal window on your mac.
```
esptool.py -p /dev/tty.wchusbserial110 erase_flash
```
```
esptool.py -p /dev/tty.wchusbserial110 -b 460800 --before default_reset --after hard_reset --chip esp32  write_flash --flash_mode dio --flash_size detect --flash_freq 40m 0x1000 build/bootloader/bootloader.bin 0x8000 build/partition_table/partition-table.bin 0x10000 build/main.bin
```
- Replace `/dev/tty.usbserial-01FD1166` with your USB port.
```
screen /dev/tty.wchusbserial110 115200
```
- Replace `/dev/tty.usbserial-01FD1166` with your USB port.