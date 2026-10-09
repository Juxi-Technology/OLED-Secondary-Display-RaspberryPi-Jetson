[English](README.md) | 简体中文

# 树莓派 / Jetson OLED 副屏

把一块 0.91 英寸 SSD1306 OLED（128×32，I2C）接到树莓派、香橙派或 NVIDIA Jetson 上做副屏。Python 脚本读取 `/proc/stat`、`free` 和 `df`，在屏幕上显示 **CPU 占用率、系统时间、内存占用、磁盘占用和本机 IP**。

教程提供 11 种语言版本。图片**只存放在 `0.91-inch-OLED-Tutorial/en/images/` 一份**，其他语言统一引用这一份。

## 语言

| 语言 | 目录 |
|------|------|
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

## 教程

| 中文 | English |
|------|---------|
| [树莓派 / Jetson OLED 副屏教程](0.91-inch-OLED-Tutorial/zh-hans/0.91-inch-oled-tutorial.md) | [Raspberry Pi / Jetson OLED Secondary Screen Tutorial](0.91-inch-OLED-Tutorial/en/0.91-inch-oled-tutorial.md) |

覆盖两个平台：

- **Jetson 系列** —— GPIO 引脚、I2C 测试、查看 IP 地址和端口、运行程序
- **树莓派 / 香橙派** —— 开启 I2C、安装 I2C 库、字体配置、设备权限、运行程序

源码只有一份，放在仓库根目录：[`oled_i2c.py`](oled_i2c.py)。

## 快速开始

### 1. 接线

把屏幕接到开发板的 I2C 引脚：**VCC → 3.3 V，GND → GND，SCL → SCL，SDA → SDA**。

> 注意不要接错或短路，接错可能损坏主板硬件。

![OLED 接线](0.91-inch-OLED-Tutorial/en/images/截图_20250813145424.png)

本模块的 I2C 地址是 **0x3c**。

### 2. 安装依赖

```
sudo apt update
sudo apt install -y python3-pip python3-smbus i2c-tools python3-pil fonts-dejavu-core
sudo pip3 install Adafruit_SSD1306
```

![安装 Adafruit_SSD1306](0.91-inch-OLED-Tutorial/en/images/Screenshot%20from%202025-08-14%2009-58-10.png)

### 3. 找到 I2C 总线和设备

```
i2cdetect -l          # 列出所有总线
i2cdetect -y -r 7     # 列出指定总线上的设备
```

![i2cdetect 结果](0.91-inch-OLED-Tutorial/en/images/Screenshot%20from%202025-08-14%2010-03-23.png)

### 4. 运行

```
python3 oled_i2c.py       # 默认总线 7
python3 oled_i2c.py 1     # 也可以指定总线，例如总线 1
```

屏幕约每 100 ms 刷新一次；内存、磁盘和 IP 的读取周期更长一些。拔掉 OLED 脚本不会退出，重新插上会自动恢复显示。

![Jetson 上运行效果](0.91-inch-OLED-Tutorial/en/images/image%201.png)

![OLED 装在板子上的样子](0.91-inch-OLED-Tutorial/en/images/OLED屏幕-Jetson主板使用图-侧视图-1.png)

## 说明

- 如果你的 OLED 不在总线 7 上，请改总线号。树莓派上通常是总线 **1**。
- 脚本里的 `getLocalIP()` 读取的是 Jetson 的网卡名 `enP8p1s0` 和 `wlP1p1s0`。在树莓派上请换成 `ifconfig` 实际显示的网卡名（例如 `eth0` / `wlan0`），否则屏幕会显示 `x.x.x.x`。
- 脚本需要 `sudo` 才能执行 `i2cdetect`，会正常提示你输入密码 —— 文件里不保存任何密码。
- 如果提示权限错误，检查 I2C 设备权限：
  ```
  sudo chmod a+rw /dev/i2c-*
  ```
