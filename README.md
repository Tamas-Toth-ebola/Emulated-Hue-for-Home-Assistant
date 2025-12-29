# ♊ Gemini 3 Assisted Project

> **WARNING: Work in Progress!**
> This project is currently undergoing heavy expansion, refactoring, and restructuring. The code and functionality are not yet considered final or publicly stable. Please use with caution and only for testing purposes until the official release.

This fork has been evolved with the active collaboration of [Gemini 3](https://gemini.google.com/). The development process combines senior system administrator expertise with AI precision to deliver professional home automation solutions.

---

<a href="https://github.com/hass-emulated-hue/core/actions"><img alt="GitHub Actions Build" src="https://github.com/hass-emulated-hue/core/actions/workflows/docker-build.yaml/badge.svg"></a>
<a href="https://hub.docker.com/r/hassemulatedhue/core"><img alt="Docker Pulls" src="https://img.shields.io/docker/pulls/hassemulatedhue/core.svg"></a>

# Hue Emulation for Home Assistant (ebola fork)

Convert your Home Assistant instance to a fully functional Philips HUE bridge!
Control all lights connected to your Home Assistant box with HUE compatible apps/devices like the official Hue app, Hue essentials, Philips Ambilight+Hue, and Logitech Harmony remotes.

## Key Changes and Enhancements (Fork Features)

- **Docker Network Isolation Support:** Introduced advertised IP override (`EMULATED_HUE_ADVERTISE_IP`). This allows containers running in Bridge mode (isolated networks) to correctly advertise the Host's LAN IP for SSDP discovery and `description.xml`.
- **Pydantic V2 Compatibility:** Updated the codebase for modern Python environments, fixing validation errors related to decimal/float values (e.g., brightness levels).
- **Cover/Shutter Support:** Added and fixed support for non-light entities. Covers and shutters are now correctly exposed and handled without crashing the server.
- **MAC Address Persistence:** The Bridge MAC address is persisted in the configuration after the first run. This ensures the Bridge ID remains stable across container recreations, preventing pairing loss with Harmony Hubs.
- **Improved Authentication Logic:** Existing registered clients (like Harmony Hub) are prioritized during authentication requests to prevent redundant notification loops and race conditions.
- **URL Flexibility:** Added support for trailing slashes in API endpoints (`/api/`) to accommodate various Hue client implementations.

## Main Features
- **Auto-Discovery:** Home Assistant Areas are automatically mapped to Hue rooms.
- **Full Light Functionality:** Supports all Home Assistant light features.
- **Smart Naming:** Devices are automatically prefixed with their Area name (e.g., "Living Room Bedroom Lamp") to avoid confusion.
- **V2 Emulation:** Fully emulates a modern "V2" Hue bridge with a persistent ID.
- **Low Latency:** Communicates with Home Assistant via high-speed WebSockets.
- **Entertainment API:** Experimental support for Hue Entertainment mode (syncing with TVs/Games).

## How to use this thing?
- Add this custom repository to your Home Assistant Supervisor: `https://github.com/hass-emulated-hue/hassio-repo`
- Install the **Emulated HUE** addon.
- Once started, it will appear as a Philips Hue Bridge on your network.

## How to connect?
1. Search for Hue Bridges in your app (e.g., Logitech Harmony or official Hue app).
2. When found, a notification will appear **within Home Assistant**.
3. Click the link in the notification to enable "Pairing Mode" (simulates pressing the physical button).
4. Complete the setup in your app.

## Important Notes
- **Ports:** This bridge requires HTTP port 80 and HTTPS port 443. These are hardcoded requirements of the Hue protocol.
- **Docker Bridge Mode:** When running in a custom Docker network, set the environment variable `EMULATED_HUE_ADVERTISE_IP` to your Raspberry Pi's LAN IP.
- **Legacy Components:** Disable any existing `emulated_hue` components in your `configuration.yaml` before using this standalone version.

## Notes on Philips HUE Entertainment API support
The [Hue Entertainment API](https://developers.meethue.com/develop/hue-entertainment/philips-hue-entertainment-api/) allows high-speed light streaming. Our implementation translates these packets into Home Assistant commands. While functional, it is highly experimental and subject to platform throttling.

## Backlog / Future Ideas
- Support for more device types (Switches, Fans).
- Native Entertainment packet forwarding for Zigbee-native Hue lights.

Please use the GitHub issue tracker for bug reports or feature requests!
