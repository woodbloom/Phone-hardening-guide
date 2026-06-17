# Phone Hardening Guide
> Practical settings to reduce mobile tracking, advertising identifiers, and data exposure on iOS and Android.

**Threat model:** Reduce passive tracking, advertising profiling, and network-based device identification. Not designed for physical seizure or nation-state adversaries.

---

## Table of Contents

- [Recommended OS](#recommended-os)
- [iOS Hardening](#ios-hardening)
- [Android Hardening](#android-hardening)
- [Network Privacy](#network-privacy)
- [App Hygiene](#app-hygiene)
- [Browser](#browser)
- [Communication](#communication)
- [Physical Security](#physical-security)
- [Common Mistakes](#common-mistakes)

---

## Recommended OS

| OS | Platform | Notes |
|----|----------|-------|
| **GrapheneOS** | Android (Pixel only) | Best privacy/security on Android. Sandboxed Google Play available. |
| **CalyxOS** | Android (Pixel) | MicroG-based, easier transition from stock Android |
| **Stock iOS** (hardened) | iPhone | Good baseline if hardened correctly. iCloud is a risk. |
| **Stock Android** (hardened) | Any | Last resort. Remove as many Google services as possible. |

**Recommendation:** GrapheneOS on a Pixel device is the strongest option available today.

---

## iOS Hardening

### Privacy & Tracking

```
Settings → Privacy & Security → Tracking
  ✗ Allow Apps to Request to Track → OFF
```
Blocks IDFA (Advertising ID) requests from all apps.

```
Settings → Privacy & Security → Apple Advertising
  ✗ Personalised Ads → OFF
```

### Location Services

```
Settings → Privacy & Security → Location Services
  → Each app: set to "While Using" or "Never" — never "Always"
  → System Services → Significant Locations → OFF
  → System Services → iPhone Analytics → OFF
  → System Services → Routing & Traffic → OFF
```

### Analytics & Diagnostics

```
Settings → Privacy & Security → Analytics & Improvements
  ✗ Share iPhone Analytics → OFF
  ✗ Share iCloud Analytics → OFF
  ✗ Improve Siri & Dictation → OFF
```

### iCloud (high risk)

```
Settings → [Your Name] → iCloud
  → Review every toggle — disable anything not strictly needed
  → iCloud Backup: OFF (backs up all your data to Apple servers)
  → Photos: OFF (unless you need cross-device sync)
```

iCloud backup stores your messages, photos, and app data on Apple's servers. Law enforcement requests can access this data.

### Siri

```
Settings → Siri & Search
  ✗ Listen for "Hey Siri" → OFF
  ✗ Siri Suggestions in Search → OFF
  ✗ Siri Suggestions in Look Up → OFF
```

### Lockscreen & Passcode

```
Settings → Face ID & Passcode
  → Use a 6-digit PIN minimum (alphanumeric is stronger)
  → Allow Access When Locked: disable everything not needed
  → Erase Data after 10 failed attempts → ON (optional, destructive)
```

### AirDrop

```
Settings → General → AirDrop → Receiving Off (when not in use)
```

---

## Android Hardening

### Advertising ID

```
Settings → Privacy → Ads
  → Delete advertising ID
```
On Android 12+: Delete the Advertising ID entirely — don't just opt out of personalization.

### Permission Audit

```
Settings → Privacy → Permission Manager
  → Review each permission category
  → Revoke: Location, Camera, Microphone, Files for apps that don't need them
```

Rules:
- Location: "While using" only — never always-on for apps that don't require it
- Microphone: revoke for all apps that don't need voice input
- Camera: revoke for everything except camera app and video call apps

### Network Permissions (Android 14+)

```
Settings → Privacy → Permission Manager → Network
  → Restrict background network access per app
```

### Sensors Off (Developer Options)

```
Settings → Developer Options → Quick Settings Developer Tiles
  → Add "Sensors Off" tile
```
Disables all sensors (camera, mic, accelerometer, etc.) system-wide. Use when needed.

### Work Profile (GrapheneOS / Samsung)

Use a work profile to isolate apps you don't fully trust:
- Install social media, banking, or corporate apps in the work profile
- Turn the work profile off when not in use — apps in it cannot run in the background

---

## Network Privacy

### WiFi

```
iOS:   Settings → Wi-Fi → [Network] → Private Wi-Fi Address → ON (rotate per network)
Android: Settings → Wi-Fi → [Network] → Privacy → Use randomized MAC
```

Prevents device fingerprinting across WiFi networks.

### Bluetooth

- Disable Bluetooth when not in use
- Never leave Bluetooth discoverable in public
- Review which apps have Bluetooth access and revoke if unnecessary

### VPN

- Use always-on VPN with kill switch
- Recommended: Mullvad, ProtonVPN, IVPN
- WireGuard protocol preferred

```
iOS:   Settings → General → VPN & Device Management → VPN → Connect On Demand
Android: Settings → Network & Internet → VPN → [VPN name] → Always-on VPN + Block connections without VPN
```

### DNS

- Use encrypted DNS (DoH / DoT)
- iOS: Settings → General → VPN & Device Management → DNS → Add DoH profile
- Android: Settings → Network & Internet → Private DNS → Enter DoH hostname

Providers: `dns.mullvad.net`, `1dot1dot1dot1.cloudflare-dns.com`, `dns.nextdns.io/your-id`

---

## App Hygiene

| Action | Why |
|--------|-----|
| Install only apps you actively use | Each app is a potential data source |
| Use browser instead of app where possible | Apps have broader OS access than web pages |
| Review permissions after every install | Apps request more than they need by default |
| Disable background app refresh | Reduces passive data collection |
| Use open-source alternatives where possible | Auditable code, no hidden tracking |

### Open-source replacements

| Instead of | Use |
|------------|-----|
| Google Maps | OsmAnd, Organic Maps |
| Chrome | Firefox (hardened), Brave |
| Gmail | Proton Mail, Tutanota |
| Google Photos | Ente Photos (E2EE), local storage |
| WhatsApp | Signal, SimpleX |
| Google Authenticator | Aegis (Android), Raivo (iOS) |

---

## Browser

### Android
- **Firefox** with uBlock Origin
- **Brave** (built-in ad/tracker blocking)
- **Mull** (GrapheneOS — hardened Firefox fork)

### iOS
- **Firefox Focus** (strong tracking protection, no history)
- **Safari** with aggressive content blockers (1Blocker, AdGuard)

### Settings
- Block third-party cookies
- Enable HTTPS-only mode
- Clear cookies on close

---

## Communication

| Tool | Use case |
|------|----------|
| **Signal** | Trusted contacts, voice calls, disappearing messages |
| **Session** | No phone number required, decentralized |
| **SimpleX** | Maximum metadata protection |
| **Matrix** | Self-hosted option |

### Rules
- Use disappearing messages by default
- Do not grant contacts access to your full phonebook
- Use separate accounts per context if possible

---

## Physical Security

- Set strong passcode (alphanumeric preferred)
- Enable biometric unlock only if you trust the implementation
- Enable remote wipe (Find My on iOS, Find My Device on Android)
- Disable USB data connection when locked:
  - iOS: Settings → Face ID & Passcode → USB Accessories → OFF
  - Android: Developer Options → Default USB configuration → No data transfer
- Never leave phone unlocked and unattended in public

---

## Common Mistakes

| Mistake | Fix |
|---------|-----|
| "Always" location access for most apps | Change to "While Using" or "Never" |
| iCloud backup enabled | Disable or encrypt sensitive data before backup |
| Bluetooth always on | Turn off when not in use |
| Default browser with tracking | Switch to hardened alternative |
| WhatsApp for sensitive conversations | Use Signal or Session |
| Skipping system updates | Update immediately — patches fix active exploits |
| Same Google account on work and personal profiles | Use separate accounts or work profile |

---

## Quick Reference

```
OS         →  GrapheneOS (Pixel) or hardened iOS
Advertising→  Delete IDFA / Advertising ID
Location   →  While Using only — no Always
VPN        →  Always-on + kill switch (Mullvad / ProtonVPN)
DNS        →  DoH or DoT
WiFi       →  Randomized MAC per network
Bluetooth  →  Off when not in use
Browser    →  Firefox + uBlock Origin / Brave
Comms      →  Signal / Session / SimpleX
Updates    →  Automatic, install immediately
```

---

> These settings reduce cross-app tracking, device-level profiling, and network-based identification.  
> They do not provide full anonymity but significantly reduce mobile data exposure.
