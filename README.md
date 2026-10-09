English | [简体中文](README_CN.md)

# OLED Secondary Display for Raspberry Pi / Jetson

A 0.91-inch SSD1306 OLED (128×32, I2C) wired to a Raspberry Pi, Orange Pi or NVIDIA Jetson as a secondary display. A Python script reads `/proc/stat`, `free` and `df` and shows **CPU usage, system time, RAM usage, disk usage and the local IP address** on the panel.

The tutorial is available in 11 languages. Images are stored **only** under `0.91-inch-OLED-Tutorial/en/images/`; every other language links to that one copy.

## Language

| Language | Folder |
|----------|--------|
| English | [`en/`](0.91-inch-OLED-Tutorial/en/) |
| Deutsch | [`de/`](0.91-inch-OLED-Tutorial/de/) |
| Español | [`es/`](0.91-inch-OLED-Tutorial/es/) |
| Français | [`fr/`](0.91-inch-OLED-Tutorial/fr/) |
| Italiano | [`it/`](0.91-inch-OLED-Tutorial/it/) |
| 日本語 | [`ja/`](0.91-inch-OLED-Tutorial/ja/) |
| 한국어 | [`ko/`](0.91-inch-OLED-Tutorial/ko/) |
| Português (BR) | [`pt-br/`](0.91-inch-OLED-Tutorial/pt-br/) |
| Português (PT) | [`pt-pt/`](0.91-inch-OLED-Tutorial/pt-pt/) |
| 简体中文 | [`zh-hans/`](0.91-inch-OLED-Tutorial/zh-hans/) |
| 繁體中文 | [`zh-hant/`](0.91-inch-OLED-Tutorial/zh-hant/) |

## Tutorial

| English | 中文 |
|---------|------|
| [Raspberry Pi / Jetson OLED Secondary Screen Tutorial](0.91-inch-OLED-Tutorial/en/0.91-inch-oled-tutorial.md) | [树莓派 / Jetson OLED 副屏教程](0.91-inch-OLED-Tutorial/zh-hans/0.91-inch-oled-tutorial.md) |

Covers both platforms:

- **Jetson series** — GPIO pinout, I2C test, viewing the IP address and port, running the program
- **Raspberry Pi / Orange Pi** — enabling I2C, installing the I2C libraries, font configuration, device permissions, running the program

The source code lives in a single place: [`oled_i2c.py`](oled_i2c.py).

## Quick Start

### 1. Wire the OLED

Connect the panel to the board's I2C pins: **VCC → 3.3 V, GND → GND, SCL → SCL, SDA → SDA**.

> Do not connect the pins incorrectly or short them — a mistake may damage the mainboard.

![OLED wiring](0.91-inch-OLED-Tutorial/en/images/截图_20250813145424.png)

The I2C address of this OLED is **0x3c**.

### 2. Install the dependencies

```
sudo apt update
sudo apt install -y python3-pip python3-smbus i2c-tools python3-pil fonts-dejavu-core
sudo pip3 install Adafruit_SSD1306
```

![installing Adafruit_SSD1306](0.91-inch-OLED-Tutorial/en/images/Screenshot%20from%202025-08-14%2009-58-10.png)

### 3. Find the I2C bus and device

```
i2cdetect -l          # list all buses
i2cdetect -y -r 7     # list the devices on a given bus
```

![i2cdetect result](0.91-inch-OLED-Tutorial/en/images/Screenshot%20from%202025-08-14%2010-03-23.png)

### 4. Run it

```
python3 oled_i2c.py       # default bus 7
python3 oled_i2c.py 1     # or specify the bus, e.g. bus 1
```

The panel refreshes about every 100 ms; RAM, disk and IP are read on a slower cycle. If the OLED is unplugged the script keeps running and reconnects when it is plugged back in.

![OLED running on a Jetson](0.91-inch-OLED-Tutorial/en/images/image%201.png)

![OLED mounted on the board](0.91-inch-OLED-Tutorial/en/images/OLED屏幕-Jetson主板使用图-侧视图-1.png)

## Notes

- Change the bus number if your OLED is not on bus 7. On a Raspberry Pi it is usually bus **1**.
- `getLocalIP()` in the script looks up the Jetson interface names `enP8p1s0` and `wlP1p1s0`. On a Raspberry Pi, replace them with the actual interface names reported by `ifconfig` (for example `eth0` / `wlan0`), otherwise the panel shows `x.x.x.x`.
- The script needs `sudo` for `i2cdetect`. It asks for your password normally — no password is stored in the file.
- If you get a permission error, check the I2C device permissions:
  ```
  sudo chmod a+rw /dev/i2c-*
  ```
