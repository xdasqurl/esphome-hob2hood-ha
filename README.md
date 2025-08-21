# ESPHome Hob2Hood Controller

A smart kitchen hood controller that integrates AEG/Electrolux/Electrolux Hob2Hood IR communication with manual override controls using ESPHome and Home Assistant.

## Features

- **Automatic Hood Control**: Responds to AEG/Electrolux cooktop IR signals (hob2hood functionality)
- **Manual Override**: Physical switches for independent hood control
- **Dual Mode Operation**: Seamlessly switches between automatic and manual modes
- **Home Assistant Integration**: Full native support with auto-discovery
- **Safety Protection**: Multi-speed protection prevents motor damage during switch transitions
- **Professional Hardware**: Built on Athom ESP32 4-channel relay board

## Compatibility

### **Cooktops**
- AEG/Electrolux Hob2Hood enabled cooktops
- Other brands using 38kHz IR with NEC protocol (IR codes will need adjustment)

### **Kitchen Hoods** 
- 3-speed fan motors (Off/Speed1/Speed2/Speed3)
- Separate light circuit
- 230V AC switching to motor windings

### **Home Assistant**
- ESPHome integration
- Any Home Assistant installation with ESPHome add-on

## Hardware Requirements

### **Core Components**
| Component | Description | Approximate Cost |
|-----------|-------------|------------------|
| [Athom ESP32 4CH Relay Board](https://www.athom.tech/blank-1/4ch-inching-self-lock-relay-for-esphome) | ESP32 with 4x 10A relays, AC 90-250V input | $23 (on sale) |
| TSOP4838 IR Receiver | 38kHz infrared receiver module | $3-5 |
| Low voltage wire | For manual control connections | $5-10 |
| Terminal blocks (optional) | For easier connections | $5 |


### **Tools Needed**
- Screwdriver set
- Wire strippers  
- Multimeter (for testing)
- Soldering iron (for IR receiver connections)

## Wiring Diagram

- TBD

### **IR Receiver Connection (Right GPIO Header)**
```
TSOP4838 Pin → Athom Board
├─ VCC → 3V3
├─ GND → GND  
└─ OUT → GPIO2 (IO2)
```

### **Manual Controls (Left GPIO Header)**
```
Hood Control → Athom Board
├─ Light Button → GPIO25 + GND
├─ Fan Speed 1 → GPIO26 + GND  
├─ Fan Speed 2 → GPIO32 + GND
└─ Fan Speed 3 → GPIO33 + GND
```

### **Hood Motor Connections (230V)**
```
Athom Relay → Hood Circuit
├─ Relay 1 COM ← 230V Live
├─ Relay 1 NO → Fan Speed 1 Winding
├─ Relay 2 COM ← 230V Live  
├─ Relay 2 NO → Fan Speed 2 Winding
├─ Relay 3 COM ← 230V Live
├─ Relay 3 NO → Fan Speed 3 Winding
├─ Relay 4 COM ← 230V Live
└─ Relay 4 NO → Hood Light Circuit
```

## Installation

### **1. Hardware Setup**

1. **Disconnect hood from mains power**
2. Mount Athom board in suitable location near hood
3. Connect IR receiver to right GPIO header
4. Wire manual controls (**disconnect from 230V, connect to GPIO pins**)
5. Connect relay outputs (**NO,normally open**) to hood motor windings and light

### **2. Software Configuration**

1. Clone this repository:
   ```bash
   git clone https://github.com/yourusername/esphome-hob2hood
   cd esphome-hob2hood
   ```

2. Create `secrets.yaml`:
   ```yaml
   wifi_ssid: "YourWiFiNetwork"
   wifi_password: "YourWiFiPassword"
   ```

3. Generate API encryption key in ESPHome dashboard

4. Flash the configuration:
   ```bash
   esphome run athom-hob2hood.yaml
   ```

### **3. Initial Testing**

1. Verify IR reception: Check ESPHome logs for AEG/Electrolux IR codes
2. Test manual switches: Confirm mode switching works
3. Test relay operation: Ensure proper fan speed switching
4. Add to Home Assistant: Device should auto-discover

## Operation

### **Automatic Mode (Default)**
- Hood responds to AEG/Electrolux cooktop IR signals
- Automatically adjusts fan speed based on cooking activity
- Light control via cooktop commands

### **Manual Mode**
- Activated when any physical switch is used
- Manual fan speed and light control
- Returns to automatic mode after 10 seconds of inactivity

### **Home Assistant Entities**
- Fan: `fan.hood_ventilation` (3 speed levels)
- Light: `switch.hood_light`
- Mode: `sensor.hood_control_mode`
- Status: Various diagnostic sensors

## Configuration

### **AEG/Electrolux IR Command Codes**
The following AEG/Electrolux hob2hood IR codes are pre-configured:

```cpp
const long IRCMD_VENT_1 = 0xE3C01BE2;     // Hob2hood On (level 1)
const long IRCMD_VENT_2 = 0xD051C301;     // Hob2hood level 2
const long IRCMD_VENT_3 = 0xC22FFFD7;     // Hob2hood level 3
const long IRCMD_VENT_4 = 0xB9121B29;     // Hob2hood level 4 (mapped to 3)
const long IRCMD_VENT_OFF = 0x55303A3;    // Hob2hood off
const long IRCMD_LIGHT_ON = 0xE208293C;   // Light on
const long IRCMD_LIGHT_OFF = 0x24ACF947;  // Light off
```

### **Customization**
- GPIO pin assignments: Modify pin numbers in YAML configuration
- Timeouts: Adjust manual mode timeout (default: 10 seconds)
- IR codes: Update for different cooktop brands
- Relay logic: Modify for different relay board configurations

## Safety Features

### **Multi-Speed Protection**
- Prevents multiple fan speeds from being active simultaneously
- Sequential relay switching with delays
- Input validation during slider transitions

### **Manual Override Safety**
- Automatic return to hob2hood mode
- Clear mode indication
- Manual timeout protection

### **Electrical Safety**
- Optoisolated relay control
- Proper 230V isolation
- Low voltage control circuits

## Troubleshooting

### **IR Not Working**
- Check TSOP4838 wiring and orientation
- Verify 38kHz frequency compatibility
- Enable `dump: all` in configuration to see raw IR codes
- Ensure clear line of sight between cooktop and receiver

### **Manual Switches Not Responding**
- Verify GPIO pin connections
- Check internal pullup configuration
- Test with multimeter for continuity
- Ensure switches connect GPIO to GND

### **Relays Not Switching**
- Check 230V power to relay board
- Verify relay output connections
- Test individual relay operation
- Check relay contact ratings vs load

### **Home Assistant Integration Issues**
- Verify ESPHome API connection
- Check Home Assistant ESPHome integration
- Confirm network connectivity
- Review ESPHome device logs

## Safety Warnings

- ** DANGER! HIGH VOLTAGE!**: This project involves 230V AC wiring. During installation, turn off mains power and follow local electrical codes.
- ** Proper Installation**: Use appropriate wire gauges and electrical protection.
- **️ Professional Help**: Consider hiring an electrician for 230V connections.
- **️ Compliance**: Ensure installation meets local electrical safety regulations.

## References

- [ESPHome Documentation](https://esphome.io/)
- [Athom ESP32 Relay Board](https://www.athom.tech/)
- [AEG/Electrolux Hob2Hood Technology](https://www.AEG/Electrolux.com/)
- [Home Assistant ESPHome Integration](https://www.home-assistant.io/integrations/esphome/)

---

**️Disclaimer**: This project involves high voltage electrical work. The authors are not responsible for any damage, injury, or safety issues resulting from the use of this information. Always consult a qualified electrician and follow local electrical codes.