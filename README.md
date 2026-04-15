# Mobile Privacy Hardening Guide
Practical settings to reduce mobile tracking, advertising identifiers, and unnecessary data exposure on iOS and Android.

________________________________________________________________________________________________________________________

## App Tracking Control (iOS)

- Disable “Allow Apps to Request to Track”

Path:
Settings → Privacy & Security → Tracking → OFF

### Effect

- Reduces advertising ID correlation
- Blocks most cross-app tracking requests
- Limits third-party profiling

________________________________________________________________________________________________________________________

## Location History (iOS)

- Disable Significant Locations

Path:
Settings → Privacy & Security → Location Services → System Services → Significant Locations → OFF

### Effect

- Stops local device location history storage
- Reduces long-term movement profiling

________________________________________________________________________________________________________________________

## Permission Audit (Android)

Review and restrict permissions:

- Location
- Camera
- Microphone
- Files & Media

Path:
Settings → Privacy → Permission Manager

### Rule

Only grant permissions when strictly required for functionality.

________________________________________________________________________________________________________________________

## Microphone Control (iOS / Android)

- Monitor microphone access indicators
- Disable microphone access for non-essential apps
- Use system-level privacy indicators when available

### Effect

- Reduces passive audio access risk
- Improves visibility over background usage
________________________________________________________________________________________________________________________

## WiFi Privacy Hardening

Enable randomized MAC address:

- iOS: Private Wi-Fi Address → ON
- Android: MAC Randomization → ON

### Effect

- Prevents persistent device tracking across WiFi networks
- Reduces local network profiling

________________________________________________________________________________________________________________________

## Bluetooth Privacy

- Disable Bluetooth when not in use
- Restrict unnecessary app access to Bluetooth services

Path (Android):
Settings → Privacy → Bluetooth permissions

### Effect

- Reduces proximity-based tracking vectors
- Limits background device discovery exposure

________________________________________________________________________________________________________________________

## Screenshot & Media Metadata

- Screenshots generally do not include GPS data
- Re-encoding can reintroduce metadata depending on apps

### Recommended handling

- Share sensitive media via privacy-focused apps (e.g. Signal)

### Effect

- Reduces metadata retention during sharing workflows

________________________________________________________________________________________________________________________

## Final

These settings reduce:

- Cross-app tracking
- Device-level advertising correlation
- Passive location history storage
- Network-based device identification

They do not provide full anonymity but significantly reduce mobile data exposure.
