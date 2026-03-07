# Eve Display

Dark-themed ESPHome UI for the **GUITION ESP32-S3-4848S040** (480×480, MIPI RGB).

## Hardware

| Part | Detail |
|---|---|
| Board | ESP32-S3, 16 MB flash, 8 MB PSRAM |
| Display | 480×480 MIPI RGB, GUITION-4848S040 |
| Touch | GT911 on I2C (SDA GPIO19, SCL GPIO45) |
| Backlight | LEDC on GPIO38 |

## Repo layout

```
eve_display.yaml               ← main config (run this)
packages/
  hardware_core.yaml           ← ESP32, display, touch, backlight
  sensors.yaml                 ← WiFi, API, OTA, logger
common/
  colors.yaml                  ← shared colour palette
  fonts.yaml                   ← Nunito, icons_v2, MDI font declarations
assets/fonts/
  Nunito-SemiBold.ttf
  icons_v2.ttf
  materialdesignicons-webfont.ttf
images/
  eve_placeholder.png          ← 300×300 RGBA placeholder sprite
eve_display_theme_starter.yaml ← LVGL dark theme (clock, nav bar, stat row)
```

## Quick start

1. Clone the repo next to your `secrets.yaml`:
   ```yaml
   wifi_ssid: "YourSSID"
   wifi_password: "YourPassword"
   ```
2. Flash:
   ```
   esphome run eve_display.yaml --device COM5
   ```

## Theme

Based on [alaltitov/Guition-ESP32-S3-4848S040](https://github.com/alaltitov/Guition-ESP32-S3-4848S040) (MIT).  
Colour palette: `#343645` background · `#9BA2BC` text · `#3FA7F3` accent.  
Fonts: Nunito SemiBold + icons_v2 + Material Design Icons.

## SNTP / clock

Your main config (or `sensors.yaml`) must declare a time source with `id: sntp_time`:

```yaml
time:
  - platform: sntp
    id: sntp_time
    timezone: "Europe/Amsterdam"
```
