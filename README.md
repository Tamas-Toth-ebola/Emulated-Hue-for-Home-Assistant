
---
<p align="center">
  <span style="font-size: 24px; font-weight: bold; vertical-align: middle;">AI Assisted Development</span>
</p>
<table align="center" width="600">
  <tr>
    <td width="130" align="center">
      <a href="https://gemini.google.com/">
        <img src="https://upload.wikimedia.org/wikipedia/commons/8/8a/Google_Gemini_logo.svg" width="120" alt="Gemini">
      </a>
    </td>
    <td>
      <sub>This project has evolved through active collaboration with <a href="https://gemini.google.com/">Google - Gemini</a>. The development process blends human expertise and precision with AI assistance in coding, testing, and deployment for infinite efficiency! 😜</sub>
    </td>
  </tr>
</table>

> **WARNING: Work in Progress!**
> This project is currently undergoing heavy expansion, refactoring, and restructuring. The code and functionality are not yet considered final or publicly stable. Please use with caution and only for testing purposes until the next release.
---

# Emulated Hue for Home Assistant

Convert your Home Assistant instance to a fully functional virtual [Philips - Hue](https://www.philips-hue.com/) bridge!
Control all devices connected to your Home Assistant with Hue compatible apps/devices like the official Hue app, Hue essentials, Philips - Ambilight+Hue, and Logitech - Harmony remotes.


## Features
- **V2 Emulation:** Fully emulates a modern "V2" Hue Bridge with a persistent ID.
- **Low Latency:** Communicates with Home Assistant via high-speed WebSockets.
- **Auto-Discovery:** Home Assistant Areas are automatically mapped to Hue rooms.
- **Entertainment API:** Experimental support for Hue Entertainment mode _(syncing with TVs/Games)_.
- **Compatible Device Types:**
  - **Switches:** Exposed as Smart Plugs.
  - **Lights:** Supports all Home Assistant light features.
  - **Covers/Shutters:** Works as inverted lights _(brightness maps to position)_.

## Setup
### Home Assistant OS / Supervised (Add-on)
- Add this custom repository to the Add-on Store: `https://github.com/hass-emulated-hue/hassio-repo`
- Install the **Emulated Hue** addon.
- Once started, it will appear as a Philips - Hue Bridge on your network.

### Home Assistant Core / Docker (Standalone)
If you're a Docker ninja 🥷, you can run this as a standalone container. Since we're still in the "garage phase" 🔧, you'll need to build the image locally for now.

1. Clone this repo to your 'Home Assistant server' _(rPI or whatever you're running)_.
2. Build the image _(care the 'arch' attribute!)_:
   ```bash
   docker build -t emulated-hue:local -f docker/Dockerfile --build-arg HASS_ARCH=aarch64 .
   ```
3. Fire it up with Docker Compose _(see `compose.example.yaml` for a head start)_.

#### Parameters
| Variable | Description |
| :--- | :--- |
| `HASS_URL` | The URL of your Home Assistant instance _(e.g., `http://10.20.30.1:8123`)_. |
| `HASSIO_TOKEN` | A Long-Lived Access Token generated in HA. |
| `EMULATED_HUE_ADVERTISE_IP` | **Mandatory for Bridge Mode!** Set this to your Host's LAN IP. |
| `EMULATED_HUE_FORCE_LINK_MODE` | _(Optional)_ Set to `true` to keep pairing mode always open. Use with care! |

## Connect (to the virtual Hue Bridge)
1. Search for Hue Bridges in your app _(e.g., Logitech Harmony or official Hue app)_.
2. When found, a notification will appear **within Home Assistant**.
3. Click the link in the notification to enable "Pairing Mode" _(simulates pressing the physical button)_ of the virtual Hue Bridge.
4. Once the pairing mode is active, your client will automatically handshake with the bridge.
5. Complete the setup in your app and start controlling your house like a boss! 😎

## How the Magic Happens (Naming Logic) 🎩✨
Tired of seeing "Light 1", "Light 2" and having no clue which is which? We got you!
The bridge uses **Smart Naming** to keep things tidy:
- **Format:** `{Area} - {Entity Name}` _(e.g., *Kitchen - Cupboard Light*)_.
- **Auto-Cleanup:** If your entity name already contains the area _(like "Kitchen Light" in "Kitchen")_, we intelligently strip the duplicate to avoid "Kitchen - Kitchen Light" silliness.
- **Title Case:** Everything is automatically converted to Title Case for that premium look. 💅

## Important Notes
- **Legacy Components:** Disable any existing `emulated_hue` components in your `configuration.yaml` before using this standalone version.
- **Ports:** This bridge requires HTTP port 80 and HTTPS port 443. These are hardcoded requirements of the Hue protocol.
- **Docker Network:**
  - The simplest way is to use `host` network mode.
  - For the privacy-conscious, **custom bridge networks work too!** 🛡️ Just make sure you have an SSDP/multicast relay _(like `multicast-relay` or `avahi`)_ running between your host and the container, and don't forget to set `EMULATED_HUE_ADVERTISE_IP`.

## Notes on Philips Hue Entertainment API support
The [Hue Entertainment API](https://developers.meethue.com/develop/hue-entertainment/philips-hue-entertainment-api/) allows high-speed light streaming. Our implementation translates these packets into Home Assistant commands. While functional, it is highly experimental and subject to platform throttling. Expect some lag unless you have a beast of a machine! 🚀

## Ideas for Future Development
- **Dynamic Refresh:** Trigger device list updates without restarting the container _(via HA Webhook or API)_.
- **Integration Labeling:** Smart suffixes to distinguish between devices from different platforms _(e.g., Zigbee vs. Wi-Fi)_.
- **Native Forwarding:** Native Entertainment packet forwarding for Zigbee-native Hue lights.

## Credits & Contributors 🏆
Massive thanks to everyone who helped bring this project to life!

- [**Google - Gemini**](https://gemini.google.com/) ♊: The AI mastermind behind refactoring, Pydantic V2 compatibility, and network isolation fixes.
- [**ebola**](https://github.com/Tamas-Toth-ebola): Just a fan who tries to keep the solution up-to-date as his RPi's humming and the code clean.
- [**hass-emulated-hue/core**](https://github.com/hass-emulated-hue/core): The original upstream project that provided the solid foundation.

---
Please use the GitHub issue tracker for bug reports or feature requests!
