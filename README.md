# Eve Display

Dark-themed ESPHome UI for the **GUITION ESP32-S3-4848S040** (480×480, MIPI RGB).  
Four-page LVGL interface with Home Assistant integration, Eve expression system, and day/night automation.

## Hardware

| Part | Detail |
|---|---|
| Board | ESP32-S3, 16 MB flash, 8 MB PSRAM (octal, 80 MHz) |
| Display | 480×480 MIPI RGB, GUITION-4848S040 |
| Touch | GT911 on I2C (SDA GPIO19, SCL GPIO45) |
| Backlight | LEDC on GPIO38 · 100 Hz · min_power 0.01 |

## Repo layout

```
eve_display.yaml               ← main config (run this)
packages/
  hardware_core.yaml           ← ESP32, display, touch, backlight
  sensors.yaml                 ← WiFi, API, OTA, SNTP, HA sensors
  ui_home.yaml                 ← Page 1: Eve / clock, colours, fonts
  ui_energy.yaml               ← Page 2: Grid + solar arcs
  ui_weather.yaml              ← Page 3: Outdoor/indoor climate
  ui_scenes.yaml               ← Page 4: Scene & device quick controls
  expressions.yaml             ← Eve expression globals + scripts
  automations.yaml             ← Day/night, backlight, ticker, flash
assets/fonts/
  Nunito-SemiBold.ttf
  icons_v2.ttf
  materialdesignicons-webfont.ttf
images/
  eve_placeholder.png          ← 300×300 RGBA placeholder sprite
eve_display_theme_starter.yaml ← (legacy) original single-page starter
```

## Pages

| # | Name | Content |
|---|------|---------|
| 1 | Home / Eve | Clock, date, Eve sprite, status bar, ticker |
| 2 | Energy | Grid consumption arc, solar production arc, net usage |
| 3 | Climate | Outdoor temp hero, indoor temp + humidity, comfort index |
| 4 | Scenes | 6 scene buttons (Evening, Night, Away, Morning, Movie, All Off) + 4 device toggles |

Navigate using the bottom nav bar on any page.

## HA Sensors wired

| Entity ID | Used for |
|-----------|---------|
| `sensor.electricity_meter_energieverbruik` | Grid arc + net usage |
| `sensor.solaredge_current_power` | Solar arc + net usage |
| `sensor.home_temperature` | Indoor temperature |
| `sensor.home_humidity` | Indoor humidity |
| `sensor.outdoor_temperature` | Outdoor hero card |
| `sensor.weather_condition` | Weather icon + condition text |
| `sensor.eve_display_wifi_signal` | WiFi icon strength (optional) |
| `sun.sun` | Auto day/night mode |
| `sensor.eve_emotion` | Eve expression system |

Adjust entity IDs in `packages/sensors.yaml` to match your setup.

## Eve Expression System

Globals: `current_expression` (0–35), `is_night_mode`, `night_brightness_cap`  
Scripts: `set_expression`, `handle_eve_emotion`, `apply_day_night_mode`

Current placeholder images (`eye_0..eye_5`, `mouth_0..mouth_5`) all point to `images/eve_placeholder.png`.  
Replace with 120×60 ARGB PNGs (eyes) and 100×50 ARGB PNGs (mouth) when artwork is ready.

## Backlight automation

| Trigger | Brightness |
|---------|-----------|
| Touch | 100% immediately |
| 60 s idle (day) | 20% |
| 60 s idle (night) | 15% |
| Night mode cap | 40% max |
| flash_and_wake script | 100% → 300 ms → 80% |

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


