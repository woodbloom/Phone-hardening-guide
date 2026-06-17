# Phone Hardening Guide

> by [woodbloom](https://github.com/woodbloom)

![](https://img.shields.io/badge/GrapheneOS-Recommended-00b894?style=flat-square&labelColor=0d1117)
![](https://img.shields.io/badge/iOS-Hardening%20Guide-00b894?style=flat-square&labelColor=0d1117)
![](https://img.shields.io/badge/Android-Privacy%20Settings-00b894?style=flat-square&labelColor=0d1117)
![](https://img.shields.io/badge/IMSI%20Catcher-Protection-00b894?style=flat-square&labelColor=0d1117)

[![Typing SVG](https://readme-typing-svg.demolab.com?font=JetBrains+Mono&weight=600&size=15&duration=2500&pause=900&color=00B894&center=true&vCenter=true&width=750&lines=GrapheneOS+%2B+Pixel+%3D+Best+Combo;Delete+Advertising+ID+%E2%80%94+Not+Just+Opt+Out;Always-On+VPN+%2B+Kill+Switch;MAC+Randomization+per+Network;Disable+2G+%E2%80%94+Blocks+IMSI+Catcher+Downgrades;Signal+%2B+Session+%2B+SimpleX;Hardware+Sensors+Off+%E2%80%94+Mic%2FCam%2FGPS)](https://github.com/woodbloom/Phone-hardening-guide)

> **Threat model:** Reduce passive tracking, advertising profiling, app-level surveillance, carrier tracking, and network-based device identification. This guide is not designed for physical seizure forensics or nation-state adversaries.

---

## Table of Contents

- [Recommended Operating Systems](#-recommended-operating-systems)
- [GrapheneOS — Complete Setup](#-grapheneos--complete-setup)
- [CalyxOS Setup](#-calyxos-setup)
- [DivestOS](#-divestos)
- [iOS Hardening — Every Setting](#-ios-hardening--every-setting)
- [Android Stock Hardening](#-android-stock-hardening)
- [Network Privacy](#-network-privacy)
- [SIM Card & Carrier Hardening](#-sim-card--carrier-hardening)
- [IMSI Catcher Protection](#-imsi-catcher-protection)
- [App Hygiene & Permission Audit](#-app-hygiene--permission-audit)
- [Browser — Mobile](#-browser--mobile)
- [Password Management & 2FA](#-password-management--2fa)
- [Secure Communication](#-secure-communication)
- [Physical Security](#-physical-security)
- [Sensor & Hardware Privacy](#-sensor--hardware-privacy)
- [Backup Strategy](#-backup-strategy)
- [Open Source App Replacements](#-open-source-app-replacements)
- [Threat-Specific Scenarios](#-threat-specific-scenarios)
- [Common Mistakes](#-common-mistakes)
- [Quick Reference](#-quick-reference)

---

## Recommended Operating Systems

| OS | Device | Privacy | Security | Anonymity | Notes |
|----|--------|---------|---------|-----------|-------|
| **GrapheneOS** | Pixel 6–9 | ✅✅✅ | ✅✅✅ | ✅✅ | Gold standard. Hardened kernel, sandboxed Play, per-app network/sensor perms |
| **CalyxOS** | Pixel, Fairphone | ✅✅ | ✅✅ | ✅✅ | MicroG instead of Google Play — easier transition |
| **DivestOS** | 190+ devices | ✅✅ | ✅✅ | ✅ | Broadest device support, patched kernels |
| **LineageOS** | Many | ✅ | ✅ | ✅ | Widely supported, less hardened |
| **Stock iOS 17+** (hardened) | iPhone | ✅✅ | ✅✅ | ✅ | Strong baseline — iCloud is the main risk |
| **Stock Android** (hardened) | Any | ✅ | ✅ | ✅ | Weakest option — follow Android section carefully |

### Why GrapheneOS is the Best Option

- **Hardened memory allocator** — far fewer exploitable memory bugs than stock Android
- **Per-app network permission** — block internet entirely for any app (exclusive to GrapheneOS)
- **Per-app sensor permission** — block camera, mic, GPS per app (exclusive to GrapheneOS)
- **Sandboxed Google Play** — run Google apps without giving them system-level access
- **Storage Scopes** — apps see only files you explicitly share, not your entire storage
- **Exploit mitigations** — ASLR improvements, CFI, shadow call stack, hardware-backed memory tagging
- **Auto-reboot** — phone reboots after configured idle time, clears memory, requires PIN to decrypt
- **USB protection** — configurable USB data port blocking
- **Verified Boot** — with custom key support, ensures OS integrity

---

## GrapheneOS — Complete Setup

### Installation

```
1. Buy a Google Pixel 6, 7, 8, or 9
   (Newer = longer support. Pixel 9 has the best hardware security chip)

2. Go to: https://grapheneos.org/install/web
   (Use Chrome or a Chromium browser — WebUSB required)

3. On your Pixel:
   Settings → About Phone → Build Number (tap 7 times) → Developer Options
   Developer Options → OEM Unlocking → Enable

4. Connect USB-C and follow the web installer
   (~10 minutes total)

5. After install: lock OEM unlock again
   Developer Options → OEM Unlocking → Disable
```

### First Boot — Security Setup

```
Settings → Security → Screen Lock
  → Alphanumeric password (strongest) or 6+ digit PIN

Settings → Security → Auto-reboot
  → 72 hours (phone reboots if idle, clears decrypted state)

Settings → Security → USB accessories
  → Block new USB accessories when locked → ON
  (Prevents juice jacking and forensic USB tools)

Settings → Security → Secure App Spawning → ON
  (Each app gets isolated process spawning)

Settings → Network & Internet → Private DNS
  → dns.mullvad.net
  (Encrypted DNS system-wide)

Settings → Network & Internet → Wi-Fi
  → Use randomized MAC per network → Randomized (per network)

Settings → Privacy → Permission manager
  → Audit every single permission category
```

### Per-App Network Permission (GrapheneOS Exclusive)

```
Settings → Apps → [App name] → Permissions → Network
  → Allow network access: OFF

Block network for:
  ✗ Calculator
  ✗ Gallery/Photos viewer
  ✗ File manager
  ✗ Notes app
  ✗ Local music/video players
  ✗ Offline games
  ✗ Clock / Timer

These apps have zero reason to send data anywhere.
```

### Per-App Sensor Permission (GrapheneOS Exclusive)

```
Settings → Apps → [App name] → Permissions → Sensors
  → Allow sensor access: OFF

Sensors include: accelerometer, gyroscope, barometer, magnetometer, step counter
Block for: shopping apps, social media, games, weather apps, anything that doesn't need motion data
```

### Storage Scopes

```
Settings → Apps → [App name] → Permissions → Photos/Media
  → Selected photos (not "All photos")
  → Pick only what the app needs

Settings → Apps → [App name] → Permissions → Files
  → Requires user to manually pick files
  → App cannot browse your entire storage
```

### Sandboxed Google Play (If Needed)

```
Install from GrapheneOS App Store (Accrescent) or F-Droid:
  → Install "Sandboxed Google Play" profile

This runs Google Play, Play Services, and Google Maps inside
a completely isolated sandbox — identical to any other app.
Google cannot read your contacts, files, location, or sensors
without explicit permission just like any other app.

Use separate user profile for Google Play if you want stronger isolation:
Settings → System → Multiple users → Add user → Install sandboxed Play there only
```

### App Stores on GrapheneOS

| Store | Notes |
|-------|-------|
| **Accrescent** | GrapheneOS-native, signed and audited apps |
| **F-Droid** | Open-source apps only — FOSS only |
| **Obtainium** | Install directly from GitHub/GitLab releases |
| **Sandboxed Play** | For apps requiring Google — fully isolated |

---

## CalyxOS Setup

```
# Download from: https://calyxos.org/install/

Key differences from GrapheneOS:
- MicroG replaces Google Play Services (FOSS implementation)
- F-Droid + Aurora Store pre-installed
- Easier for users coming from stock Android
- Less security hardening than GrapheneOS

Post-install:
Settings → Network & Internet → Private DNS → dns.mullvad.net
Settings → Privacy → MicroG → Review what's enabled
Settings → Security → Screen Lock → Alphanumeric password

Disable unused MicroG features:
Settings → Apps → MicroG Services → Permissions
  → Only grant what you actively use
```

---

## DivestOS

```
# Download from: https://divestos.org/pages/downloads

Supports 190+ devices — use if you can't get a Pixel for GrapheneOS.

Key features:
- Patched kernels (CVE patches backported)
- Removed Google tracking
- Hardened WebView
- F-Droid pre-installed
- Randomized MAC by default

Post-install hardening same as Android Stock section below.
```

---

## iOS Hardening — Every Setting

### Tracking & Advertising

```
Settings → Privacy & Security → Tracking
  ✗ Allow Apps to Request to Track → OFF
  (Blocks IDFA requests entirely — all apps denied before they can ask)

Settings → Privacy & Security → Apple Advertising
  ✗ Personalised Ads → OFF

Settings → Privacy & Security → Analytics & Improvements
  ✗ Share iPhone Analytics → OFF
  ✗ Share iCloud Analytics → OFF
  ✗ Improve Siri & Dictation → OFF
  ✗ Improve Health & Activity → OFF
  ✗ Share with App Developers → OFF
```

### Location Services — Full Audit

```
Settings → Privacy & Security → Location Services
  → Go through EVERY app:

  Navigation (Maps, Waze)     → While Using
  Camera                      → While Using (for geotag — disable if unwanted)
  Weather                     → While Using (or Never + enter manually)
  Fitness/Health apps         → While Using
  EVERYTHING ELSE             → Never

System Services (scroll to bottom of Location Services):
  ✗ Significant Locations → OFF (stores where you've been)
  ✗ iPhone Analytics → OFF
  ✗ Routing & Traffic → OFF
  ✗ Improve Maps → OFF
  ✗ Location-Based Alerts → OFF
  ✗ Location-Based Suggestions → OFF
  ✗ HomeKit → OFF (unless actively using)
  ✗ Motion Calibration & Distance → OFF
  ✗ System Customisation → OFF
```

### iCloud — Full Risk Assessment

```
iCloud backup = everything on your phone stored on Apple's servers.
Law enforcement can subpoena this. Apple complies.

Settings → [Your Name] → iCloud
  → Go through EVERY toggle:

  iCloud Backup          → OFF (or use Advanced Data Protection)
  Messages               → OFF (or ON only with ADP enabled)
  Photos                 → OFF (use Ente Photos E2EE or local)
  iCloud Drive           → OFF (use local or Cryptomator)
  Safari                 → OFF
  Keychain               → OFF (use KeePassXC / Bitwarden instead)
  Mail                   → OFF (use ProtonMail)
  Contacts               → OFF
  Calendar               → OFF (or Proton Calendar)
  Reminders              → OFF
  Notes                  → OFF
  Health                 → OFF (very sensitive data)
  Siri                   → OFF
  Game Center            → OFF
```

### Advanced Data Protection (If You Must Use iCloud)

```
Settings → [Your Name] → iCloud → Advanced Data Protection → Turn On

What this does:
- End-to-end encrypts almost all iCloud data
- Apple cannot read your backups, photos, notes, etc.
- Law enforcement requests get encrypted data Apple cannot decrypt
- Recovery contact or recovery key required (store offline)

What is NOT covered by ADP:
- iCloud Mail (technical constraints)
- Contacts (for directory reasons)
- Calendars (federation)

After enabling ADP:
Settings → [Your Name] → iCloud → Advanced Data Protection
  → Add Recovery Contact AND save Recovery Key offline
```

### Passcode & Face ID

```
Settings → Face ID & Passcode
  → Change to: Custom Alphanumeric Code (strongest)
  → Minimum: 6-digit PIN if you must use numbers

  Allow Access When Locked:
  ✗ Control Center → OFF
  ✗ Notification Center → OFF
  ✗ Siri → OFF
  ✗ Reply with Message → OFF
  ✗ Home Control → OFF
  ✗ Wallet → OFF (unless actively using)
  ✗ Return Missed Calls → OFF
  ✗ USB Accessories → OFF (prevents forensic tools via Lightning/USB-C)

  Erase Data → ON (optional — erases after 10 failed attempts)
```

### Biometrics — Know the Risks

```
Face ID / Touch ID is convenient but:
- Police can force biometric unlock in many jurisdictions
- Face ID unlocks while you're sleeping (use Require Attention)
- Fingerprint can be forced or copied

When to disable biometrics:
  → Border crossing / customs check
  → Protest or demonstration
  → If detained or arrested

Quick disable:
  iPhone X+: Hold Volume Down + Side button → Emergency SOS screen → Face ID disabled
  This forces PIN entry until you unlock normally
  
Settings → Face ID & Passcode:
  Require Attention → ON (must look directly at phone)
  Attention Aware Features → ON
```

### Siri

```
Settings → Siri & Search
  ✗ Listen for "Hey Siri" → OFF
  ✗ Press Side Button for Siri → OFF (or leave ON but restrict below)
  ✗ Allow Siri When Locked → OFF
  ✗ Show in App Library Search → OFF

Per-app Siri suggestions:
  Settings → [App] → Siri & Search → Show App in Search → OFF
  Do this for every app — prevents Siri from indexing app content
```

### Safari

```
Settings → Safari
  ✅ Prevent Cross-Site Tracking → ON
  ✅ Hide IP Address → Trackers and Websites
  ✅ Block All Cookies → ON (may break some sites — toggle off per-site if needed)
  ✅ Fraudulent Website Warning → ON
  ✅ Privacy Preserving Ad Measurement → OFF (still sends data)
  ✅ Check for Apple Pay → OFF
  ✗ Allow Websites to Check for Apple Pay → OFF
  ✗ Share iPad/iPhone Browsing → OFF
  Advanced → Website Data → Remove All Website Data
```

### App Privacy Report

```
Settings → Privacy & Security → App Privacy Report → Turn On

After one week it shows:
- Which sensors each app accessed (mic, camera, location)
- Which domains each app contacted
- When exactly it happened (even in background)

Review weekly — apps checking mic/location in background unexpectedly = spyware
```

### AirDrop

```
Settings → General → AirDrop → Receiving Off
(Only enable when actively transferring files)

AirDrop has had multiple serious vulnerabilities including:
- Leaking your phone number and email hash to strangers (2021)
- Remote code execution on older iOS
Always keep off in public.
```

---

## Android Stock Hardening

### Delete Advertising ID

```
Settings → Privacy → Ads → Delete advertising ID

Android 12+: Delete the ID entirely (not just opt out of personalization).
Deleting removes the identifier completely — apps receive a string of zeros.
```

### Permission Audit — Every Category

```
Settings → Privacy → Permission Manager

Go through EVERY permission:

LOCATION
  → Set all non-navigation apps to "While using" or "Never"
  → Remove "All the time" from every app except maps (and even then, consider "While using")

MICROPHONE
  → Revoke from: social media, shopping, games, tools, utility apps
  → Keep: phone dialer, voice apps, video call apps ONLY

CAMERA
  → Revoke from: social media (block until you open it), shopping, games
  → Keep: camera app, video calling apps

CONTACTS
  → Revoke from everything except your messaging app and dialer

CALENDAR
  → Revoke from everything except your calendar app

SMS
  → Only your default messaging app needs this

PHONE (call logs)
  → Only your dialer

NEARBY DEVICES (Bluetooth)
  → Revoke from everything except what actively uses Bluetooth

BODY SENSORS
  → Only health/fitness apps

BACKGROUND LOCATION ("All the time")
  → This should be empty. No app needs this.
  → If anything is listed here, revoke it.
```

### Google Account Privacy

```
Go to: myaccount.google.com

Data & Privacy:
  Web & App Activity → Pause
  Location History → Pause (or delete)
  YouTube History → Pause
  Ad Settings → Delete advertising ID + Turn off personalization

myactivity.google.com:
  Delete all activity
  Auto-delete: set to 3 months minimum

Google Account → Personal Info:
  Remove phone number if not needed for account recovery
  Use a non-identifying profile photo
```

### Background Activity

```
Settings → Apps → [App] → Battery
  → Background activity → OFF for apps that don't need real-time updates
  → Restrict background data → ON (data-hungry apps)

Settings → Apps → [App] → Data Usage
  → Background data → Restrict

Revoke background activity from: social media, news apps, shopping apps
They don't need to run when you're not using them.
```

### Work Profile Isolation (Stock Android)

```
Install Shelter (F-Droid) to create a work profile:
https://f-droid.org/packages/net.typeblog.shelter/

Isolate in work profile:
- Social media apps
- Banking apps
- Corporate apps
- Any app you don't fully trust

Turn work profile OFF when not in use:
- Apps in the profile cannot run in background while paused
- Prevents background data collection
```

### Developer Options Security

```
Settings → About Phone → Build Number (tap 7 times)
Settings → Developer Options:
  USB Debugging → OFF (enable only when needed, never in public)
  Verify Apps over USB → ON
  Default USB configuration → No data transfer (charging only)
  Disable: Bluetooth HCI snoop log
  Disable: Wi-Fi scan throttling override
  Keep screen on → OFF
```

---

## Network Privacy

### WiFi — MAC Randomization

```
iOS:
Settings → Wi-Fi → [Network] → Private Wi-Fi Address
  → Rotating (changes periodically, best)

Android:
Settings → Wi-Fi → [Network] → Privacy
  → Use randomized MAC

GrapheneOS:
Settings → Network & Internet → Wi-Fi → [Network]
  → MAC address type → Randomized (per-network by default)

WHY: Your MAC address is broadcast to every network you connect to.
Without randomization, shops, airports, and tracking companies can
identify your specific device across locations.
```

### VPN — Always-On with Kill Switch

```
iOS:
Settings → General → VPN & Device Management → VPN
  → Add WireGuard configuration (download from Mullvad/ProtonVPN)
  → After connecting: set Connect On Demand → ON
  → Status must show connected before any browsing

Android / GrapheneOS:
Settings → Network & Internet → VPN → [VPN name]
  → Always-on VPN → ON
  → Block connections without VPN → ON  ← THIS IS THE KILL SWITCH

GrapheneOS specific:
  Settings → Network & Internet → Internet → [Network]
  → VPN per-network settings available
```

**Recommended providers:**

| Provider | Protocol | Price | Notes |
|----------|---------|-------|-------|
| **Mullvad** | WireGuard | €5/month | No account email needed, cash/Monero accepted |
| **ProtonVPN** | WireGuard | Free–€10 | Free tier available, good for beginners |
| **IVPN** | WireGuard | $6/month | Privacy-focused, Monero accepted |

### DNS — Encrypted at System Level

```
GrapheneOS / Android:
Settings → Network & Internet → Private DNS
  → Private DNS provider hostname:
    dns.mullvad.net        ← Recommended (Sweden, no logs)
    1dot1dot1dot1.cloudflare-dns.com
    dns.nextdns.io/YOUR_ID (custom blocklists)

iOS:
Settings → General → VPN & Device Management
  → DNS → Add configuration
  Download profiles from:
    https://github.com/paulmillr/encrypted-dns
    (DoH profiles for Mullvad, Cloudflare, NextDNS, etc.)
```

### Bluetooth

```
When to disable:
  ✗ Public places (coffee shops, airports, malls)
  ✗ When not actively using a Bluetooth device
  ✗ Sleeping (unless you use BT headphones/smartwatch for sleep tracking)

WHY: Bluetooth constantly broadcasts your device identifier.
Even with MAC randomization, Bluetooth tracking beacons can track your
movements through stores and public spaces.

Review Bluetooth permissions:
Android: Settings → Privacy → Permission Manager → Nearby Devices
iOS: Settings → Privacy & Security → Bluetooth
  → Revoke from every app that doesn't actively use Bluetooth devices
```

### WiFi Networks

```
Disable auto-join on saved public networks:
iOS: Settings → Wi-Fi → [Network] → Auto-Join → OFF
Android: Settings → Wi-Fi → [Network] → uncheck Auto-connect

Disable Wi-Fi scanning (used for location even when Wi-Fi is off):
Android: Settings → Location → Wi-Fi scanning → OFF
Android: Settings → Location → Bluetooth scanning → OFF

iOS: Settings → Privacy & Security → Location Services → System Services
  → Wi-Fi Networking → OFF
```

---

## SIM Card & Carrier Hardening

### Enable SIM PIN

```
iOS: Settings → Cellular → SIM PIN → Enable
     Set a PIN different from your device PIN (4-8 digits)

Android: Settings → Security → SIM card lock → Lock SIM card
         Set PIN

WHY: Without a SIM PIN, someone can remove your SIM, put it in another phone,
and immediately receive your SMS/calls — including 2FA codes.
SIM swap attacks often start with the physical SIM or social engineering your carrier.
```

### Carrier Opt-Out

```
US Carriers:
AT&T:     att.com → Account → Profile → Privacy Choices → Enhanced Relevant Advertising → OFF
Verizon:  verizonwireless.com → Account → Account Overview → Manage Privacy Settings
T-Mobile: t-mobile.com → My T-Mobile → Profile → Add Services → Advertising Opt-Out

EU Carriers:
GDPR gives you the right to opt out — contact carrier support and request full data processing opt-out.

German carriers:
Telekom: telekom.de → Mein Telekom → Datenschutz
Vodafone: vodafone.de → Mein Vodafone → Datenschutzcenter
O2: o2online.de → Mein O2 → Datenschutz
```

### Anonymous SIM Options

```
Purchase prepaid SIM with cash — legal in:
  Germany, Netherlands, Austria, UK (some), Czech Republic, etc.
  NOT legal in: Spain, Italy, Norway, some others (require ID)

VOIP numbers for account registration:
  JMP.chat     — XMPP-based, private
  MySudo       — iOS/Android, multiple numbers
  TextNow      — US numbers, free
  Hushed        — Temporary numbers

Never use your real number for:
  → Account 2FA (use TOTP app instead)
  → Anonymous service registration
  → Any account unlinked to your identity
```

### SS7 Attacks & Phone Number Risks

```
SS7 (Signaling System 7) is the protocol that routes calls/SMS globally.
It has known vulnerabilities that allow:
  - Location tracking via cell towers without your knowledge
  - SMS interception (this is why SMS 2FA is weak)
  - Call forwarding abuse

Protection:
  → Never use SMS for 2FA — use TOTP (Aegis) or hardware key
  → Use encrypted calling (Signal) for sensitive calls
  → Consider a separate SIM for identity-sensitive accounts
```

---

## IMSI Catcher Protection

IMSI catchers (Stingrays, DRTBOX) are fake cell towers used by law enforcement and some attackers to:
- Track your real-time location
- Intercept unencrypted calls and SMS
- Collect IMSI (your SIM's unique ID)
- Force phone to downgrade to 2G (unencrypted)

### Detection Signs

```
Possible IMSI catcher nearby:
  → Sudden drop from 4G/5G to 2G/EDGE in a normally good-signal area
  → Unusual battery drain in a specific location
  → Phone gets unusually warm when idle
  → Unexpected "SIM card changed" or "network changed" notifications
  → Signal bar shows different carrier than expected
```

### Protection — Disable 2G

```
Android (Android 12+):
Settings → Network & Internet → SIMs → [SIM] → Allow 2G → OFF

This forces your phone to stay on LTE/5G only.
IMSI catchers work by forcing a downgrade to 2G — disabling 2G blocks this attack.

GrapheneOS:
Settings → Network & Internet → SIMs → Allow 2G → OFF (same)

iOS 17+ (iPhone 14+):
Settings → Cellular → LTE/5G Options → 5G Auto/On
(iOS doesn't have explicit 2G disable but 5G-only mode helps)
```

### Detection Apps

```
SnoopSnitch (Android, requires root):
  - Analyzes baseband radio for IMSI catcher signatures
  - Tests for SS7 attacks
  - Shows cell tower information
  F-Droid: https://f-droid.org/packages/de.srlabs.snoopsnitch/

AIMSICD (Android, open source):
  - IMSI catcher detector
  - Cell tower database comparison
  - Alerts for suspicious towers
  GitHub: https://github.com/CellularPrivacy/Android-IMSI-Catcher-Detector
```

### Best Protection Against IMSI Catchers

```
1. Disable 2G → blocks tower downgrade attack
2. Use WiFi Calling → calls route over internet, bypassing cellular network
3. Use Signal → E2EE content even if intercepted at cellular level
4. Keep airplane mode ON when not needed → no cellular connection at all
5. Faraday bag → blocks all signals (camera bags sold for this purpose)
```

---

## App Hygiene & Permission Audit

### Minimal Installation Principle

```
Rule: Every app is an attack surface.

Before installing:
  → Do I actually use this regularly?
  → Is there a web version I could use instead?
  → Does it have open-source alternatives?
  → What permissions does it request?
  → Who is the developer? What's their revenue model?

If an app is free and doesn't sell a product → YOU are the product.
```

### Dangerous Permission Combinations

```
Any single app with ALL of these simultaneously = high risk:
  Location + Contacts + Microphone + Camera + Storage
  → This is essentially full surveillance capability
  → Most legitimate apps only need 1-2 of these

Flag for review:
  Social media app requesting: Contacts + Microphone + Location (always)
  → Classic data harvesting setup
  → Use web version or revoke all except camera/mic when actively posting
```

### App Privacy Report Usage

```
iOS: Settings → Privacy & Security → App Privacy Report

After 1 week shows:
  [App Name] accessed: Location, Microphone
  [App Name] contacted: api.branch.io, ads.facebook.com, firebase.google.com

Domains to watch for (third-party trackers embedded in apps):
  *.facebook.com/net  — Meta tracking SDK
  *.google-analytics.com — Google Analytics
  *.doubleclick.net    — Google ad tracking
  *.branch.io          — Deep link + tracking
  *.adjust.com         — Mobile advertising tracking
  *.appsflyer.com      — Attribution tracking
  *.amplitude.com      — Product analytics
  *.segment.com        — Data collection

If a calculator app is contacting facebook.com — uninstall it.
```

### Android Privacy Dashboard

```
Settings → Privacy → Privacy Dashboard

Shows 24-hour timeline:
  Which app → accessed which permission → at what time

Check for:
  → Location access when you weren't using the app
  → Microphone access outside of calls/voice apps
  → Camera access outside of photo apps
```

---

## Browser — Mobile

### Android Browser Ranking

| Browser | Privacy | Notes |
|---------|---------|-------|
| **Mull** (F-Droid) | ✅✅✅ | Hardened Firefox fork for Android, uBlock Origin support |
| **Tor Browser** (Android) | ✅✅✅ | Maximum anonymity, slow |
| **Firefox** + uBlock Origin | ✅✅ | Good, supports full extensions |
| **Brave** | ✅✅ | Built-in shields, no extension support as strong |
| **DuckDuckGo Browser** | ✅ | Basic tracking protection |
| **Chrome** | ❌ | Avoid — heavy telemetry |
| **Samsung Internet** | ❌ | Samsung telemetry |

### iOS Browser Ranking

| Browser | Privacy | Notes |
|---------|---------|-------|
| **Onion Browser** | ✅✅✅ | Tor on iOS |
| **Firefox Focus** | ✅✅ | Deletes history after every session |
| **Brave** | ✅✅ | Good shields |
| **Safari** (hardened) | ✅✅ | Built-in ITP, best iOS native option |
| **Firefox iOS** | ✅ | No extension support on iOS (Apple restriction) |
| **Chrome** | ❌ | Avoid |

### Mull / Firefox on Android — about:config

```
Type about:config in address bar, search and toggle:

privacy.resistFingerprinting = true
geo.enabled = false
media.peerconnection.enabled = false      ← Blocks WebRTC IP leak
privacy.firstparty.isolate = true
network.cookie.cookieBehavior = 1
dom.battery.enabled = false
browser.safebrowsing.malware.enabled = false   ← Stops URL sending to Google
network.http.referer.XOriginPolicy = 2
toolkit.telemetry.enabled = false
datareporting.healthreport.uploadEnabled = false
```

### uBlock Origin — Mobile (Firefox/Mull Android)

```
Firefox on Android is the ONLY mobile browser that supports real extensions.
Install uBlock Origin from Firefox Add-ons.

Recommended filter lists to enable:
  ✅ uBlock filters (All)
  ✅ EasyPrivacy
  ✅ AdGuard Mobile Ads
  ✅ AdGuard Tracking Protection
  ✅ Peter Lowe's Ad and tracking server list
  ✅ Fanboy Annoyances
```

---

## Password Management & 2FA

### Mobile Password Managers

| App | Platform | Type | Notes |
|-----|---------|------|-------|
| **Bitwarden** | iOS & Android | Cloud E2EE | Free, open-source, audited |
| **KeePassDX** | Android | Local file | Opens KeePassXC databases |
| **Strongbox** | iOS | Local + iCloud | KeePass-compatible, Secure Enclave |
| **Vaultwarden** | Both | Self-hosted | Self-hosted Bitwarden server |

### 2FA Apps

| App | Platform | Notes |
|-----|---------|-------|
| **Aegis** (F-Droid) | Android | Best — open-source, encrypted backup, biometric unlock |
| **Raivo OTP** | iOS | Open-source, E2EE iCloud backup |
| **Ente Auth** | Both | E2EE cloud sync, open-source, cross-platform |
| **YubiKey NFC** | Both | Hardware key — strongest option |

### Critical 2FA Rules

```
NEVER use SMS for 2FA because:
  → SIM swap attacks → attacker takes over your number
  → SS7 attacks → SMS intercepted in transit
  → Carrier social engineering → staff tricked into porting number

Priority order:
  1. Hardware key (YubiKey NFC) → phishing-resistant, physical
  2. TOTP app (Aegis/Raivo) → offline, E2EE backup
  3. Email OTP → as secure as your email (weak)
  4. SMS → avoid whenever possible

Backup your TOTP codes:
  Aegis: Settings → Backups → Export encrypted backup
  Store this encrypted file on a USB drive offline — NOT in cloud storage
```

---

## Secure Communication

| App | Anonymity | Metadata | Phone Nr | E2EE | Best For |
|-----|---------|---------|---------|------|---------|
| **Signal** | ⭐⭐⭐ | Minimal | Required | ✅ | Trusted contacts, calls |
| **Session** | ⭐⭐⭐⭐ | None | None | ✅ | No account, pseudonymous |
| **SimpleX** | ⭐⭐⭐⭐⭐ | None | None | ✅ | Maximum metadata protection |
| **Briar** | ⭐⭐⭐⭐ | None | None | ✅ | Offline, P2P, Tor-capable |
| **Cwtch** | ⭐⭐⭐⭐⭐ | None | None | ✅ | Tor-based, metadata-resistant |
| **Matrix/Element** | ⭐⭐⭐ | Variable | Optional | ✅ | Groups, self-hostable |
| **WhatsApp** | ⭐ | Extensive | Required | ✅* | Avoid — Meta-owned |
| **Telegram** | ⭐ | Extensive | Required | ❌ default | Avoid — not E2EE by default |

*WhatsApp E2EE for messages but metadata (who talks to whom, when, how long) is fully collected.

### Signal Hardening

```
Settings → Privacy:
  ✗ Read receipts → OFF
  ✗ Typing indicators → OFF
  ✗ Link previews → OFF
  ✅ Screen lock → ON
  ✅ Screen security (hide in app switcher) → ON
  ✅ Incognito keyboard → ON (keyboard app doesn't see what you type)
  ✅ Phone number privacy → Who can find me by number → Nobody
  ✅ Who can see my phone number → Nobody

Settings → Notifications:
  Show → No name or message (lockscreen shows nothing)

Settings → Chats:
  Default message timer:
    Trusted contacts → 1 week
    Sensitive contacts → 1 day or 8 hours

Settings → Payments:
  → Only enable if you actively use Signal Payments
```

### Which App for Which Situation

```
Talking to family / close friends who know you:
  → Signal

Contacting someone without sharing your phone number:
  → Session or SimpleX (create a new identity)

Group chats, communities:
  → Matrix/Element (on a privacy-respecting server)
  → or Session groups

Maximum anonymity (Tor-routed):
  → Briar (enable Tor transport)
  → SimpleX with Tor proxy configured
  → Cwtch

Completely offline (no internet):
  → Briar (Bluetooth + WiFi mesh)
```

---

## Physical Security

### Passcode Strength

```
Best: Custom Alphanumeric Password (e.g., "CorrectHorse7Battery!")
Good: 8-digit numeric PIN
Minimum: 6-digit numeric PIN (NOT 4-digit)

AVOID:
  ✗ 1234, 0000, 1111 (most common PINs in the world)
  ✗ Birthdays (19xx, 200x)
  ✗ Year (2023, 2024)
  ✗ Sequential (1234, 4321)
  ✗ Same as bank/email/computer PIN
```

### Auto-Lock

```
iOS: Settings → Display & Brightness → Auto-Lock → 1 minute
Android: Settings → Display → Screen timeout → 30 seconds or 1 minute
```

### Biometrics — When to Disable

```
DISABLE biometrics and switch to PIN-only when:
  → Crossing a border or going through customs
  → At a protest, demonstration, or political event
  → In a country with coercive law enforcement
  → You are about to be detained or arrested

How to quickly disable:
  iPhone: Hold Volume Down + Side button → SOS screen → Face ID disabled
  Android / GrapheneOS: Power button → Lockdown mode (disables all biometrics)
  GrapheneOS: Can configure PIN-only startup
  
After using lockdown mode: requires your PIN/password to re-enable biometrics.
```

### USB Data Protection

```
iOS: Settings → Face ID & Passcode → USB Accessories → OFF
  → This blocks data connection when locked
  → Chargers still work — only data is blocked
  → Prevents: forensic extraction tools, juice jacking

Android: Developer Options → Default USB configuration → No data transfer
GrapheneOS: Settings → Security → USB accessories when locked → Block new accessories
```

### Remote Wipe Setup

```
iOS: Settings → [Your Name] → Find My → Find My iPhone → ON
  → When needed: findmy.apple.com → Erase iPhone

Android: Settings → Google → Find My Device → ON
  → When needed: android.com/find → Erase device

GrapheneOS: Same as Android but without Google account
  → Or use a Nextcloud + DAVx5 setup for non-Google remote wipe alternative

Set these up NOW — not when you need them.
```

### Juice Jacking (Public USB Chargers)

```
Never plug into public USB ports (airports, hotels, cafes, charging kiosks).

WHY: USB carries both power AND data. Malicious chargers can:
  → Install malware
  → Extract files
  → Inject HID commands (simulate keyboard)

Solutions:
  ✅ Carry your own wall charger + cable
  ✅ Use a USB data blocker ("PortaPow" or similar) — blocks data pins
  ✅ Charge via wireless charging pad (power only, no data port)
```

---

## Sensor & Hardware Privacy

### What Sensors Your Phone Has

| Sensor | Data it provides | Risk |
|--------|-----------------|------|
| GPS | Precise location | High |
| WiFi | Location triangulation | High |
| Bluetooth | Location + device tracking | Medium-High |
| Accelerometer | Movement, steps, falls | Medium |
| Gyroscope | Orientation, typing patterns | Medium |
| Barometer | Floor level (altitude) | Low-Medium |
| Magnetometer | Compass direction | Low |
| Proximity sensor | If you're on a call | Low |
| Ambient light | Screen brightness | Low |
| Microphone | Everything spoken near phone | High |
| Camera (front+rear) | Everything visible | High |
| NFC | Payment, tag reading | Medium |
| Fingerprint reader | Biometric authentication | Medium |

### Sensors Off — Android (Developer Tile)

```
Settings → Developer Options → Quick Settings developer tiles
  → Enable "Sensors Off" tile

Pull down notification shade → Sensors Off:
  → Disables: camera, microphone, GPS, accelerometer, gyroscope,
    barometer, magnetometer — ALL sensors at once
  → Microphone returns silence, camera returns black frames
  → No app can bypass this — it's at OS level

Use when: sensitive conversation nearby, paranoid moment, meeting, sleep
```

### GrapheneOS Per-App Sensor Control

```
Settings → Apps → [App Name] → Permissions

Available permissions unique to GrapheneOS:
  ✅ Network (block internet entirely per app)
  ✅ Sensors (block accelerometer/gyroscope per app)
  ✅ Camera → OFF
  ✅ Microphone → OFF
  ✅ Location → OFF

No app can access what you haven't explicitly granted.
Even with malicious code — the OS blocks the call.
```

### Microphone Indicator

```
Android 12+ / iOS 14+:
  Green dot (top-right) = camera or microphone in use

Watch for:
  → Indicator while phone is "idle" = something is recording
  → App accessing mic during a completely different task
  
If you see the mic indicator unexpectedly:
  → Pull down notification shade → it shows which app is using it
  → Force-stop that app
  → Revoke microphone permission
  → Consider uninstalling
```

### NFC

```
Android: Settings → Connected Devices → NFC → OFF (when not using)
iOS: NFC cannot be fully disabled but only activates on proximity
     Remove all NFC cards from Apple Wallet if not using

WHY: NFC operates in short range (~4cm) but custom antennas can read
at larger distances. Mostly theoretical risk but worth disabling.
```

---

## Backup Strategy

| Method | Security | Risk | Notes |
|--------|---------|------|-------|
| **iCloud (no ADP)** | ⚠️ | Law enforcement access | Apple can be subpoenaed |
| **iCloud + ADP** | ✅✅ | E2EE | Apple cannot read it |
| **iTunes/Finder encrypted** | ✅✅ | Local only — lose if computer dies | Set strong passphrase |
| **Google Backup** | ⚠️ | Google has access | Avoid for sensitive data |
| **Seedvault (GrapheneOS)** | ✅✅✅ | USB or Nextcloud | Open-source, E2EE, no Google |
| **Manual export** | ✅✅ | Manual effort | Most control, export what you care about |
| **No backup** | — | Lose everything if phone breaks | Acceptable if using cloud accounts |

### GrapheneOS — Seedvault

```
Settings → System → Backup → Seedvault
  → Backup to: USB drive or Nextcloud instance
  → Encrypt backup → ON (uses your device password)
  → Schedule: Daily

Restore on new device:
  → Install GrapheneOS
  → Settings → System → Restore from Seedvault backup
```

### iOS — Local Encrypted Backup

```
Connect iPhone to Mac (Finder) or Windows (iTunes)
  → Select "Encrypt local backup"
  → Set a STRONG passphrase (write it down, store offline)

This encrypts:
  → Health data
  → Saved passwords
  → Call history
  → iMessages (if not in iCloud)
  → App-specific data
  
A backup without encryption doesn't include passwords or Health data.
```

---

## Open Source App Replacements

| Instead of | Use | Platform | Why |
|------------|-----|---------|-----|
| Google Maps | **Organic Maps** / **OsmAnd** | Both | Offline, no tracking |
| Chrome | **Mull** / **Brave** / **Firefox** | Both | No telemetry |
| Gmail | **Proton Mail** / **Tutanota** | Both | E2EE email |
| Google Photos | **Ente Photos** | Both | E2EE, open-source |
| Google Drive | **Proton Drive** / **Nextcloud** | Both | E2EE or self-hosted |
| Google Calendar | **Proton Calendar** / **Etar** | Both | No Google tracking |
| Google Authenticator | **Aegis** / **Raivo** / **Ente Auth** | Both | Encrypted backup |
| WhatsApp | **Signal** / **Session** / **SimpleX** | Both | E2EE + no metadata |
| Telegram | **Signal** / **Matrix** | Both | Telegram is not E2EE by default |
| TikTok | Delete it | — | Chinese government access to data |
| YouTube | **NewPipe** (Android) / **Invidious** | Both | No tracking, no account needed |
| Google Keyboard | **OpenBoard** / **Heliboard** | Android | Doesn't send keystrokes to Google |
| Samsung Keyboard | **OpenBoard** | Android | Same |
| Google Translate | **LibreTranslate** / **DeepL** | Both | More private |
| Shazam | **SongRec** | Android | Open-source, local |
| Google Photos Albums | **Ente Photos** | Both | Zero-knowledge E2EE |
| Google Contacts | **DAVx5** + **Nextcloud** | Android | Self-hosted sync |
| Notes | **Standard Notes** / **Joplin** | Both | E2EE notes |
| Spotify | **Spotube** / download music | Both | No tracking |
| Weather | **Breezy Weather** | Android | Open-source, no account |

---

## Threat-Specific Scenarios

### Scenario: Crossing a Border

```
Before crossing:
  ✅ Disable biometrics (PIN only)
  ✅ Log out of sensitive apps (Signal messages wiped)
  ✅ Enable Full Disk Encryption (is already on if GrapheneOS/iOS)
  ✅ Remove any apps you don't want visible
  ✅ Turn phone OFF (at rest, decryption key is not in memory)

At the border:
  → If asked to unlock: you have the right to refuse in many countries
    (check laws for your specific destination/citizenship)
  → If device is seized: encryption protects data if PIN is not given

After crossing:
  ✅ Change passwords for anything you logged into on the trip
  ✅ Re-enable biometrics
  ✅ Reinstall any apps you removed
```

### Scenario: Attending a Protest

```
Before going:
  ✅ Enable Lockdown Mode (iOS) or Lockdown tile (Android)
  ✅ Disable biometrics — PIN only
  ✅ Remove face photos from gallery temporarily
  ✅ Turn off location services entirely
  ✅ Enable airplane mode if possible (Briar works over Bluetooth)
  ✅ Remove SIM if you need to (Airplane mode + WiFi is safer)
  ✅ Use Signal with disappearing messages (1 hour)
  ✅ Write emergency contact number on your ARM in pen

Communication:
  → Use Briar (works over Bluetooth mesh without internet)
  → Signal with disappearing messages
  → Don't use regular SMS/calls
```

### Scenario: Avoiding Stalkerware

```
Signs of stalkerware:
  → Unusual battery drain
  → Phone getting warm when idle
  → Unexplained data usage
  → Notification LED flickering when screen is off
  → New apps you didn't install

Detection:
  Android: Download "Certo Mobile Security" or check Privacy Dashboard
  iOS: Settings → Privacy & Security → App Privacy Report (review all domains)
  
Nuclear option — factory reset:
  iOS: Settings → General → Transfer or Reset iPhone → Erase All Content
  Android: Settings → General Management → Reset → Factory Data Reset
  
After reset: restore only what you need manually (not a full backup restore
which could restore the stalkerware too)
```

---

## Common Mistakes

| Mistake | Fix |
|---------|-----|
| "Always" location for most apps | "While Using" or "Never" for non-navigation apps |
| iCloud backup enabled without ADP | Disable or enable Advanced Data Protection |
| Bluetooth always on | Disable when not actively using a device |
| Chrome/Samsung browser | Switch to Firefox/Mull/Brave |
| WhatsApp/Telegram for sensitive talk | Signal, Session, or SimpleX |
| Not updating immediately | Update now — security patches address real exploits |
| SMS for 2FA | Aegis (TOTP) or YubiKey |
| Same Google account everywhere | Separate accounts or work profile |
| No SIM PIN | Enable it — prevents SIM swap at a physical level |
| Charging at public USB ports | Use your own charger or a USB data blocker |
| Weak PIN (0000, 1234, birthday) | Alphanumeric passphrase |
| Not auditing permissions monthly | Check Privacy Dashboard / App Privacy Report |
| Auto-join on public Wi-Fi | Disable auto-join for all saved public networks |
| Selfies at sensitive locations | Strip GPS EXIF or disable camera geotag |
| Ignoring 2G networks | Disable 2G to block IMSI catcher downgrades |
| Telegram as primary messenger | Not E2EE by default — use Secret Chats or avoid |

---

## Quick Reference

```
OS            →  GrapheneOS (Pixel) best · CalyxOS (easier) · DivestOS (more devices)
Advertising   →  Delete IDFA entirely · not just opt out
Location      →  "While Using" only · Never for most apps · Audit monthly
VPN           →  Always-on + kill switch · Mullvad / ProtonVPN
DNS           →  dns.mullvad.net (private DNS setting)
Wi-Fi         →  Randomized MAC per network · Auto-join OFF on public networks
Bluetooth     →  OFF when not using a BT device
2G            →  DISABLE — blocks IMSI catcher downgrade attacks
SIM PIN       →  Enable — prevents physical SIM theft 2FA bypass
Browser       →  Mull + uBlock (Android) · Firefox Focus / Brave (iOS)
2FA           →  Aegis / Raivo TOTP · YubiKey NFC · never SMS
Passwords     →  Bitwarden / KeePassDX · unique everywhere
Comms         →  Signal (known contacts) · Session / SimpleX (anon)
Backup        →  Seedvault → USB (GrapheneOS) · Encrypted local (iOS)
Updates       →  Install immediately
USB           →  Block when locked · never use public USB ports
Biometrics    →  Disable at borders/protests · use lockdown mode
Physical      →  Remote wipe configured · auto-lock 1 min
```

---

<div align="center">

> **Privacy on mobile is hard because defaults are deliberately terrible.**
> These settings raise the cost of tracking you to the point where passive, mass surveillance fails.

[![Device Guide](https://img.shields.io/badge/Also%20Read-Device%20Hardening%20Guide-00b894?style=for-the-badge&labelColor=0d1117)](https://github.com/woodbloom/Device-hardening-guide)
[![Server Guide](https://img.shields.io/badge/Also%20Read-Server%20Hardening%20Guide-00b894?style=for-the-badge&labelColor=0d1117)](https://github.com/woodbloom/Server-hardening-guide)

</div>
