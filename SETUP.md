# Home Assistant Dashboard Setup Guide

## Overview

A mobile-first, responsive dashboard with **8 views** tailored to your integrations:

| View | Icon | What's on it |
|------|------|-------------|
| **Home** | `mdi:home` | Weather, commute (Waze), FAA delays, locks/alarm status, mail, house modes |
| **Security** | `mdi:shield-home` | Alarm.com panel, 2x Aqara U100 locks, smoke/CO, 5 Wyze cameras |
| **Climate** | `mdi:thermostat` | Upstairs + Basement ADC-T 3000 thermostats, Dreo fan, temp graphs |
| **Lights** | `mdi:lightbulb-group` | 4 Wyze lamps, light strip, 3 Sengled bulbs, outdoor string lights |
| **Media** | `mdi:play-circle` | YouTube Music, Plex (ZaurusPlex), Google Cast devices, radio |
| **Family** | `mdi:account-group` | Shopping list, KidsChores, Google Calendar, laundry, mail & packages |
| **Network** | `mdi:lan` | UniFi gateway stats, system health (CPU/RAM/disk), printer |
| **Settings** | `mdi:cog` | House modes, automations, system info, quick nav links |

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

- **modern_dashboard** — Dark theme with indigo accents (recommended)
- **modern_dashboard_light** — Light theme with indigo accents

To apply a theme:
1. Go to your **User Profile** (bottom-left)
2. Under **Theme**, select `modern_dashboard` or `modern_dashboard_light`

## Entity ID Customization

The dashboard uses best-guess entity IDs based on your integrations. If a card shows "Entity not found", update the entity ID:

1. Go to **Settings > Devices & Services > Entities**
2. Search for the device (e.g., "lamp" or "lock")
3. Copy the actual `entity_id`
4. Update it in `dashboards/main.yaml` (search for `# CUSTOMIZE:` comments)

### Entity Reference

| Device | Expected Entity ID | Integration |
|--------|-------------------|-------------|
| Front Door Lock | `lock.aqara_smart_lock_u100` | Matter |
| Back Door Lock | `lock.aqara_smart_lock_u100_2` | Matter |
| Alarm | `alarm_control_panel.alarm_com` | Alarm.com |
| Upstairs Thermostat | `climate.upstairs_thermostat` | Z-Wave JS |
| Basement Thermostat | `climate.basement_thermostat` | Z-Wave JS |
| Smoke/CO | `binary_sensor.zcombo_g_smoke_and_co_detector` | Z-Wave JS |
| Bedroom Lamp 1 | `light.lamp_1` | Wyze |
| Bedroom Lamp 2 | `light.lamp_2` | Wyze |
| Bedroom 2 Lamp | `light.lamp_3` | Wyze |
| Bedroom 3 Lamp | `light.lamp_4` | Wyze |
| Light Strip | `light.light_strip_1` | Wyze |
| Sengled Bulb 1 | `light.sengled_e11_g13_light` | ZHA |
| Sengled Bulb 2 | `light.sengled_e11_g13_light_2` | ZHA |
| Sengled Bulb 3 | `light.sengled_e11_g13_light_3` | ZHA |
| String Lights | `switch.string_lights` | Wyze |
| Outdoor Plug | `switch.outdoor_plug` | Wyze |
| Front Yard Cam | `camera.front_yard_cam` | Wyze |
| Driveway Cam | `camera.driveway_cam` | Wyze |
| Garage Cam | `camera.garage_cam` | Wyze |
| Sideyard Cam | `camera.sideyard_cam` | Wyze |
| Baby Cam | `camera.baby_cam` | Wyze |
| Weather | `weather.forecast_home` | Met.no |
| YouTube Music | `media_player.ytube_music_player` | ytube_music_player |
| Plex | `media_player.plex_zauruspplex` | Plex |
| Commute | `sensor.waze_travel_time` | Waze Travel Time |
| FAA Delay | `binary_sensor.atl` | FAA Delays |
| Dreo Fan | `fan.dreo_fan` | Dreo |

## Adding UniFi Integration

### UniFi Network (router, switches, APs)

1. Go to **Settings > Devices & Services > Add Integration**
2. Search for **UniFi Network**
3. Enter your UniFi controller details:
   - **Host**: IP of your UniFi Dream Machine/CloudKey (e.g., `192.168.1.1`)
   - **Username**: Your UniFi admin username
   - **Password**: Your UniFi admin password
   - **Port**: `443` (default for UDM) or `8443` (CloudKey)
4. Select which site to monitor (usually "Default")

This creates entities like:
- `sensor.unifi_gateway_wan_download` — WAN download speed
- `sensor.unifi_gateway_wan_upload` — WAN upload speed
- `sensor.unifi_gateway_wan_ip` — WAN IP address
- `sensor.unifi_default_clients` — Connected client count
- `device_tracker.unifi_*` — Individual client device trackers
- `switch.unifi_*` — PoE port switches
- `update.unifi_*` — Firmware update entities

### UniFi Protect (cameras/NVR)

If you have a UniFi NVR, CloudKey G2+, or Dream Machine Pro with Protect:

1. Go to **Settings > Devices & Services > Add Integration**
2. Search for **UniFi Protect**
3. Enter your Protect console details:
   - **Host**: IP of your NVR/UDM-Pro
   - **Username/Password**: Your Protect credentials
4. Cameras appear as `camera.unifi_protect_<name>`

After adding, uncomment the UniFi camera section in `dashboards/main.yaml` (View 2: Security) and update entity IDs.

## Free Remote Access (No Nabu Casa)

Here are the best free methods to access Home Assistant remotely:

### Option 1: Cloudflare Tunnel (Recommended)

The most popular free option. Cloudflare proxies traffic to your HA instance through an encrypted tunnel — no port forwarding needed.

**Requirements**: A domain name (can be cheap, ~$10/year on Cloudflare Registrar)

**Setup**:
1. Install the **Cloudflared** add-on:
   - Go to **Settings > Add-ons > Add-on Store**
   - Add repository: `https://github.com/brenner-tobias/addon-cloudflared`
   - Install **Cloudflared**
2. Create a free Cloudflare account at https://dash.cloudflare.com
3. Add your domain to Cloudflare (follow their nameserver instructions)
4. In the Cloudflared add-on config, set:
   ```yaml
   external_hostname: ha.yourdomain.com
   tunnel_name: homeassistant
   ```
5. Start the add-on and follow the authentication link
6. Add to `configuration.yaml`:
   ```yaml
   http:
     use_x_forwarded_for: true
     trusted_proxies:
       - 172.30.33.0/24
   ```
7. Set `external_url` in configuration.yaml:
   ```yaml
   homeassistant:
     external_url: "https://ha.yourdomain.com"
   ```
8. Restart Home Assistant

**Pros**: Free, fast, secure, no port forwarding, auto SSL, DDoS protection
**Cons**: Requires a domain name

### Option 2: Tailscale VPN (Easiest)

Tailscale creates a private mesh VPN between your devices. Your HA instance gets a private IP accessible from anywhere.

**Setup**:
1. Install the **Tailscale** add-on:
   - Go to **Settings > Add-ons > Add-on Store**
   - Search for **Tailscale**
   - Install it
2. Create a free Tailscale account at https://tailscale.com
3. Start the add-on and authenticate via the link
4. Install Tailscale on your phone/tablet/laptop
5. Access HA at `http://homeassistant:8123` or the Tailscale IP

**Pros**: Easiest setup, free for up to 100 devices, no domain needed, encrypted
**Cons**: Requires Tailscale app on every device, slightly slower than Cloudflare

### Option 3: WireGuard VPN (Most Private)

Run your own VPN server on the same machine or router.

**Setup**:
1. Install the **WireGuard** add-on from the Add-on Store
2. Configure port forwarding on your router (UDP port 51820)
3. Generate client configs for each device
4. Install WireGuard app on phone/tablet

**Pros**: No third-party services, very fast, fully encrypted
**Cons**: Requires port forwarding, more complex setup, need dynamic DNS if your IP changes

### Option 4: DuckDNS + Let's Encrypt

Free dynamic DNS with free SSL certificates.

**Setup**:
1. Create a free subdomain at https://www.duckdns.org
2. Install the **DuckDNS** add-on
3. Configure port forwarding (port 8123 or 443) on your router
4. The add-on handles SSL via Let's Encrypt

**Pros**: Completely free, no third-party app needed on devices
**Cons**: Requires port forwarding (security risk), more complex router config

### Recommendation

**For most people**: Start with **Tailscale** (5-minute setup, works immediately). If you want a proper domain URL and don't want to install an app on every device, set up **Cloudflare Tunnel**.

## Tablet/Phone Tips

### Kiosk Mode
For a wall-mounted tablet, install **Kiosk Mode** via HACS to hide the sidebar and header:
1. Install `maykar/kiosk-mode` from HACS Frontend
2. Add to each view you want in kiosk mode:
   ```yaml
   kiosk_mode:
     kiosk: true
   ```

### Companion App
Install the official **Home Assistant Companion** app:
- [iOS App Store](https://apps.apple.com/app/home-assistant/id1099568401)
- [Google Play Store](https://play.google.com/store/apps/details?id=io.homeassistant.companion.android)

The companion app provides:
- Push notifications for automations
- Phone sensors (battery, location, WiFi) as HA entities
- Widgets for quick actions
- Persistent connection for remote access (works with Tailscale/Cloudflare)

### Browser Optimization
For tablet browsers, add HA as a **Progressive Web App (PWA)**:
1. Open HA in Chrome/Safari
2. Tap "Add to Home Screen"
3. This gives you a full-screen app-like experience

## File Structure

```
homeassistant/
├── configuration.yaml       # Core config, integrations, input helpers
├── dashboards/
│   └── main.yaml            # 8-view mobile-first Lovelace dashboard
├── sensors.yaml             # System monitor + template sensors
├── automations.yaml         # 11 automations (lights, locks, safety, weather)
├── scripts.yaml             # 5 scripts (movie mode, lock all, welcome home)
├── scenes.yaml              # 4 scenes (movie night, morning, good night, all bright)
├── groups.yaml              # Device groups (lights, locks, cameras, outdoor)
├── customize.yaml           # Friendly names and icons for entities
├── themes/
│   └── modern_dashboard.yaml # Dark & light themes
└── www/                     # Static files (custom icons, images)
```

## Automations Included

| Automation | What it does |
|-----------|-------------|
| Sunrise Lights Off | Turns off all lights 30 min after sunrise |
| Sunset Ambient Lights | Turns on bedroom + Sengled lights at sunset (Home mode) |
| Good Night | Lights off, media off, locks locked, mode → Night |
| Vacation Random Lights | Randomly toggles lights hourly at night |
| Severe Weather Alert | Notifies on lightning/hail/exceptional weather |
| High Temperature Alert | Notifies when temp exceeds 95°F |
| Guest Mode Welcome | All lights on, mode → Guest |
| Lock Doors at Night | Auto-locks both Aqara locks at 10 PM |
| Low Lock Battery | Alert when lock battery drops below 20% |
| Door Unlocked Alert | Alert if a door is unlocked for 10+ minutes |
| Smoke/CO Alarm | Emergency notification + all lights on |
