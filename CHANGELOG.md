# Changelog 📅

All the juicy bits and bobs we've added to make this virtual bridge the best in the business! 🚀

## [2025-12] - The Stability & Isolation Update 🛠️

This month was all about making the bridge rock-solid and Docker-friendly. We teamed up with **Gemini 3** to crush bugs and add features that matter.

### Added
- **Docker Network Isolation Support:** Introduced the legendary `EMULATED_HUE_ADVERTISE_IP`. You can now run in isolated Bridge mode and still have your Hue clients find the bridge on your Host's LAN IP. Pure magic! 🎩
- **Improved Authentication Logic:** We now prioritize existing registered clients (looking at you, Harmony Hub!). This kills those annoying notification loops and race conditions. 🛑
- **URL Flexibility:** Added support for trailing slashes in API endpoints (`/api/`). Because some clients are just... consistent in their inconsistency. 🙄
- **Persistent Link Mode:** Added `EMULATED_HUE_FORCE_LINK_MODE` for those times you just want things to pair without the notification dance. 💃

### Fixed
- **Pydantic V2 Compatibility:** Dragged the codebase into the modern era. No more validation errors when Home Assistant sends decimal/float values for brightness. It just works! 📏
- **Cover/Shutter Support:** Finally! Non-light entities like covers and shutters are now correctly exposed and handled without making the server go "boom". 💥
- **MAC Address Persistence:** The Bridge MAC address is now saved after the first run. Your Bridge ID stays the same even if you blow away your container. Bye-bye re-pairing! 👋

---
*Stay tuned for more sci-fi updates!* 🛸
