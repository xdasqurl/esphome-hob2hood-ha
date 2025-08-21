# Changelog

All notable changes to the ESPHome Hob2Hood Controller project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [v0.1.0] - 2025-08-21

### Added
- Initial release of ESPHome Hob2Hood Controller
- AEG/Electrolux Hob2Hood IR command support (38kHz, NEC protocol)
- Manual override controls with automatic mode switching
- Athom ESP32 4CH Relay Board integration
- Home Assistant native integration with auto-discovery
- Dual-mode operation (automatic hob2hood + manual override)
- Safety features for multi-speed protection during slider transitions
- Sequential relay switching with protective delays
- Manual mode timeout (10 seconds) with automatic return to hob2hood mode
- Complete wiring documentation and installation guide
- Support for 3-speed fan motors and separate light control
- Status monitoring and diagnostic logging
- Web server interface for local device management

### Supported Hardware
- Athom ESP32 4CH Relay Board (ESP32-based)
- TSOP4838 IR receiver (38kHz)
- 230V AC fan motors (3-speed)
- Manual slider/button controls
- Kitchen hood light circuits

### Supported IR Commands
- `0xE3C01BE2` - Ventilation Level 1
- `0xD051C301` - Ventilation Level 2  
- `0xC22FFFD7` - Ventilation Level 3
- `0xB9121B29` - Ventilation Level 4 (mapped to Level 3)
- `0x055303A3` - Ventilation Off
- `0xE208293C` - Light On
- `0x24ACF947` - Light Off

### Home Assistant Entities
- Fan entity with 3 speed levels
- Light switch entity
- Control mode status sensor
- Device status and diagnostic sensors
- WiFi signal strength monitoring
- Uptime tracking