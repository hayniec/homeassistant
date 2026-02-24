# Home Assistant Dashboard Setup Guide

## Overview

This configuration provides a modern, polished Home Assistant dashboard with **8 views**:

| View | Description |
|------|-------------|
| **Home** | Overview with weather, quick actions, house mode, and status |
| **Weather** | Detailed weather with forecasts, graphs, and atmospheric data |
| **Lights** | Room-by-room light controls with brightness/color sliders |
| **Climate** | Thermostat control, fan control, and temperature graphs |
| **Media** | Mini media players for TVs and speakers |
| **Security** | Alarm panel, door/window sensors, cameras, locks, motion |
| **Energy** | Power consumption charts, system monitor graphs |
| **Settings** | Automation toggles, system info, and quick navigation links |

## Required HACS Plugins

Install [HACS](https://hacs.xyz/) first, then install these frontend plugins:

| Plugin | Repository |
|--------|-----------|
| **Mushroom Cards** | `piitaya/lovelace-mushroom` |
| **Mini Graph Card** | `kalkih/mini-graph-card` |
| **Layout Card** | `thomasloven/lovelace-layout-card` |
| **Card Mod** | `thomasloven/lovelace-card-mod` |
| **Mini Media Player** | `kalkih/mini-media-player` |
| **Weather Card** | `bramkragten/weather-card` |
| **ApexCharts Card** | `RomRider/apexcharts-card` |

### Installation Steps

1. Go to **HACS > Frontend**
2. Click **+ Explore & Download Repositories**
3. Search for and install each plugin listed above
4. Restart Home Assistant after installing all plugins

## Theme Setup

Two themes are included:

- **modern_dashboard** — Dark theme with indigo accents
- **modern_dashboard_light** — Light theme with indigo accents

To apply a theme:
1. Go to your **User Profile** (bottom-left)
2. Under **Theme**, select `modern_dashboard` or `modern_dashboard_light`

## Entity Configuration

The dashboard references common entity IDs. You'll need to update these to match your actual entities:

### Lights
- `light.living_room`, `light.bedroom`, `light.kitchen`
- `light.office`, `light.bathroom`, `light.porch`

### Climate
- `climate.thermostat`, `fan.living_room`

### Media Players
- `media_player.living_room_tv`, `media_player.bedroom_speaker`
- `media_player.kitchen_speaker`, `media_player.office_speaker`

### Security
- `alarm_control_panel.home_alarm`
- `binary_sensor.front_door`, `binary_sensor.back_door`
- `cover.garage_door`, `lock.front_door`
- `binary_sensor.motion_front_yard`, `binary_sensor.motion_back_yard`
- `binary_sensor.motion_driveway`, `binary_sensor.motion_garage`
- `camera.front_door`, `camera.backyard`

### Weather
- `weather.home` — Configure via **Settings > Integrations** (e.g., OpenWeatherMap, Met.no)

## File Structure

```
homeassistant/
├── configuration.yaml      # Main config — integrations, input helpers, includes
├── sensors.yaml            # Template sensors, system monitor, time/date
├── automations.yaml        # Sunrise/sunset, weather alerts, mode automations
├── scripts.yaml            # Reusable scripts (movie mode, welcome home, etc.)
├── scenes.yaml             # Predefined scenes (good night, morning, etc.)
├── groups.yaml             # Entity groups
├── customize.yaml          # Entity customizations (names, icons)
├── dashboards/
│   └── main.yaml           # The main Lovelace dashboard (8 views)
├── themes/
│   └── modern_dashboard.yaml  # Dark & light themes
└── www/                    # Static files (custom icons, images)
```

## Customization Tips

- **Change entity IDs**: Search and replace the placeholder entity IDs with your actual device entities
- **Add/remove cards**: Each view's cards are in a simple YAML list — add or remove blocks as needed
- **Change colors**: Edit the gradient backgrounds in `card_mod` styles or the theme file
- **Add more views**: Copy a view block and customize it for your needs (e.g., garden, garage, office)
