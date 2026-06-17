<div align="center">

<img width="100%" src="https://capsule-render.vercel.app/api?type=waving&color=gradient&customColorList=2,3,6&height=140&section=header&text=Phone+Hardening+Guide&fontSize=40&fontColor=ffffff&animation=fadeIn&fontAlignY=45&desc=Reduce+mobile+tracking%2C+advertising+IDs+%26+data+exposure&descAlignY=65&descSize=16&descColor=00b894"/>

[![Typing SVG](https://readme-typing-svg.demolab.com?font=JetBrains+Mono&weight=600&size=16&duration=2500&pause=1000&color=00B894&center=true&vCenter=true&width=700&lines=GrapheneOS+%2B+Pixel+%3D+Best+Combo;Delete+Advertising+ID+%E2%80%94+Not+Just+Opt+Out;Always-On+VPN+%2B+Kill+Switch;MAC+Randomization+per+Network;Signal+%2B+Session+%2B+SimpleX)](https://github.com/cordinsanity/Phone-hardening-guide)

</div>

> **Threat model:** Reduce passive tracking, advertising profiling, app-level surveillance, and network-based device identification.
> This guide does **not** address physical seizure forensics or nation-state adversaries.

---

## Table of Contents

- [Recommended Operating Systems](#-recommended-operating-systems)
- [GrapheneOS Setup Guide](#-grapheneos-setup-guide)
- [iOS Hardening](#-ios-hardening)
- [Android Hardening](#-android-hardening-stock)
- [Network Privacy](#-network-privacy)
- [SIM Card & Carrier Privacy](#-sim-card--carrier-privacy)
- [App Hygiene](#-app-hygiene)
- [Browser](#-browser)
- [Password Management & 2FA](#-password-management--2fa)
- [Secure Communication](#-secure-communication)
- [Physical Security](#-physical-security)
- [IMSI Catcher Protection](#-imsi-catcher-protection)
- [Sensor & Hardware Privacy](#-sensor--hardware-privacy)
- [Backup Strategy](#-backup-strategy)
- [Open Source Replacements](#-open-source-replacements)
- [Common Mistakes](#-common-mistakes)
- [Quick Reference](#-quick-reference)

---

## 📱 Recommended Operating Systems

| OS | Device | Privacy | Security | Notes |
|----|--------|---------|---------|-------|
| **GrapheneOS** | Pixel 6–9 | ✅✅✅ | ✅✅✅ | Best option available. Hardened Android, sandboxed Google Play, per-app permissions |
| **CalyxOS** | Pixel | ✅✅ | ✅✅ | MicroG instead of full Google — easier transition |
| **DivestOS** | Many devices | ✅✅ | ✅✅ | Broader device support than Graphene, less hardening |
| **Stock iOS 17+** (hardened) | iPhone | ✅✅ | ✅✅ | Good baseline — iCloud is the main risk |
| **Stock Android** (hardened) | Any | ✅ | ✅ | Last resort — see Android section for minimal hardening |

### Why GrapheneOS?

- Hardened memory allocator (far fewer exploitable bugs)
- Per-app sensor permissions (disable camera/mic/network per app)
- Sandboxed Google Play — runs Google apps without granting them system access
- Network permission toggle per app (block internet entirely)
- Storage Scopes — apps only see files you explicitly share
- Exploit mitigations: ASLR improvements, CFI, shadow call stack
- Verified Boot with custom key support

---

## ⚙️ GrapheneOS Setup Guide

### Installation

1. Get a **Google Pixel 6, 7, 8, or 9** (newest = longest support)
2. Visit [grapheneos.org/install/web](https://grapheneos.org/install/web) — use the web installer
3. Enable OEM Unlock in developer options, connect via USB-C
4. Follow the on-screen steps — takes ~10 minutes

### After Install — First Steps

```
Settings → Security → Screen Lock → Set a strong PIN or passphrase (6+ digits minimum)
Settings → Security → Auto reboot → 72 hours (device reboots if not unlocked, clears memory)
Settings → Security → USB accessories → Block new USB accessories (prevents juice jacking)
Settings → Network → WiFi → Use randomized MAC per network → On
Settings → Network → Internet → Private DNS → dns.mullvad.net
Settings → Apps → Sandboxed Google Play → Install only if needed
```

### App Store Options on GrapheneOS

| Store | Notes |
|-------|-------|
| **Accrescent** | GrapheneOS-native app store, signed apps |
| **F-Droid** | Open-source apps only |
| **Obtainium** | Install directly from GitHub releases |
| **Sandboxed Google Play** | For apps that require Google — runs fully sandboxed |

### Per-App Network Permission (GrapheneOS exclusive)

```
Settings → Apps → [App name] → Permissions → Network
→ Block internet for apps that don't need it
```

Block internet for: calculator, gallery, file manager, notes, local tools — they have no reason to phone home.

### Storage Scopes

```
Settings → Apps → [App name] → Permissions → Photos/Media
→ "Selected photos" instead of "All photos"
→ Prevents apps from scanning your entire gallery
```

---

## 🍎 iOS Hardening

### Tracking & Advertising

```
Settings → Privacy & Security → Tracking
  ✗ Allow Apps to Request to Track → OFF (blocks IDFA for all apps)

Settings → Privacy & Security → Apple Advertising
  ✗ Personalised Ads → OFF

Settings → Privacy & Security → Analytics & Improvements
  ✗ Share iPhone Analytics → OFF
  ✗ Share iCloud Analytics → OFF
  ✗ Improve Siri & Dictation → OFF
  ✗ Improve Health & Activity → OFF
```

### Location Services — Full Audit

```
Settings → Privacy & Security → Location Services
  → Go through every single app:
    - Maps, navigation apps → "While Using" only
    - Camera → "While Using" (needed for geotagging — disable if not wanted)
    - Everything else → "Never" unless you have a specific reason

System Services (scroll to bottom):
  ✗ Significant Locations → OFF
  ✗ iPhone Analytics → OFF
  ✗ Routing & Traffic → OFF
  ✗ Improve Maps → OFF
  ✗ Location-Based Alerts → OFF
  ✗ Location-Based Suggestions → OFF
```

### iCloud — Risk Assessment

iCloud backup is law enforcement gold — it contains your messages, photos, app data, and device backup.

```
Settings → [Your Name] → iCloud
  → Go through every toggle individually
  → iCloud Backup → OFF (or enable Advanced Data Protection if you need backup)
  → Messages in iCloud → OFF (or on with E2EE enabled via ADP)
  → Photos → OFF (local storage or Ente Photos instead)
  → Safari → OFF
  → Keychain → OFF (use a dedicated password manager)
```

**Advanced Data Protection** (if you must use iCloud):
```
Settings → [Your Name] → iCloud → Advanced Data Protection → Turn On
```
This enables E2EE for most iCloud data. Apple cannot access it. Recovery contact required.

### Passcode & Face ID

```
Settings → Face ID & Passcode
  → Change to: Custom Alphanumeric Code (strongest) or 6-digit PIN minimum
  → Allow Access When Locked:
    ✗ Control Center → OFF
    ✗ Notification Center → OFF
    ✗ Siri → OFF
    ✗ Reply with Message → OFF
    ✗ Home Control → OFF
    ✗ Wallet → OFF (unless you use it and trust your threat model)
  → USB Accessories → OFF (prevents juice jacking via USB data connection)
  → Erase Data after 10 failed attempts → ON (optional — destructive but stops brute force)
```

### Siri — Disable Unless Needed

```
Settings → Siri & Search
  ✗ Listen for "Hey Siri" → OFF
  ✗ Press Side Button for Siri → OFF (or leave ON but set privacy below)
  ✗ Allow Siri When Locked → OFF
  ✗ Siri Suggestions in Search → OFF
  ✗ Siri Suggestions in Look Up → OFF
  ✗ Siri Suggestions in App → OFF (per-app)
```

### AirDrop

```
Settings → General → AirDrop → Receiving Off

Only enable when actively using it — AirDrop has had multiple serious vulnerabilities.
```

### Safari Hardening

```
Settings → Safari
  → Prevent Cross-Site Tracking → ON
  → Hide IP Address → ON (Proxies IP from trackers)
  → Block All Cookies → Consider (breaks some sites)
  → Fraudulent Website Warning → ON
  → Privacy Preserving Ad Measurement → OFF (still sends data to Apple)
  → Check for Apple Pay → OFF
  → Allow websites to check for Apple Pay → OFF
```

---

## 🤖 Android Hardening (Stock)

If you're on stock Android without GrapheneOS, apply these settings.

### Delete Advertising ID

```
Settings → Privacy → Ads
  → Delete advertising ID
```

On Android 12+: Delete the ID entirely — not just opt out of personalization. Deleting removes the ID completely.

### Permission Manager Full Audit

```
Settings → Privacy → Permission Manager
```

Go through **every permission category**:

| Permission | Rule |
|-----------|------|
| **Location** | "While using" only — never "Always" unless navigation app |
| **Microphone** | Revoke for everything except voice apps |
| **Camera** | Revoke for everything except camera/video call apps |
| **Contacts** | Revoke for everything except your dialer/contacts app |
| **Calendar** | Revoke unless the app has a clear legitimate need |
| **SMS** | Only your messaging app needs this |
| **Phone** | Only your dialer needs this |
| **Files/Storage** | Revoke from apps that don't need file access |
| **Body Sensors** | Revoke from everything except health apps |
| **Nearby Devices** | Revoke unless using Bluetooth features actively |

### Google Account Privacy

```
Google Account → Data & Privacy:
  → Turn off: Web & App Activity
  → Turn off: Location History
  → Turn off: YouTube History
  → Ad Settings → Delete advertising ID
  → Personalized ads → OFF

myactivity.google.com:
  → Delete all activity
  → Auto-delete: 3 months
```

### Developer Options — Security Settings

```
Settings → About Phone → Tap Build Number 7 times → Developer Options
  → USB Debugging → OFF (enable only when needed)
  → Verify apps over USB → ON
  → Default USB configuration → No data transfer (charging only)
```

### Disable Background App Activity

```
Settings → Apps → [App] → Battery
  → Background activity → OFF for apps that don't need it
  → Restrict background data → ON for untrusted apps
```

### Work Profile for Untrusted Apps

Create a work profile to isolate apps you don't trust:

```
Settings → Accounts → Work Profile (on Samsung/GrapheneOS)
Or use Shelter app from F-Droid for stock Android
```

Install social media, banking apps, or corporate tools in the work profile. Turn it off when not in use — apps cannot run in the background while the profile is paused.

---

## 🌐 Network Privacy

### WiFi MAC Randomization

MAC addresses identify your device to every network you connect to. Randomize them.

```
iOS:     Settings → Wi-Fi → [Network name] → Private Wi-Fi Address → Rotating
Android: Settings → Wi-Fi → [Network name] → Privacy → Use randomized MAC
GrapheneOS: Same as Android — enabled by default per network
```

### VPN — Always-On with Kill Switch

```
iOS:
Settings → General → VPN & Device Management → VPN
→ Add VPN Configuration (WireGuard or IKEv2)
→ Connect On Demand → ON
→ Status must show Connected before browsing

Android:
Settings → Network & Internet → VPN → [VPN name]
→ Always-on VPN → ON
→ Block connections without VPN → ON (this is the kill switch)
```

Recommended: **Mullvad** (WireGuard, no account email needed) · **ProtonVPN** · **IVPN**

### DNS — Encrypted

```
iOS:
Settings → General → VPN & Device Management → DNS
→ Install DoH profile from: https://nextdns.io/ios or https://github.com/paulmillr/encrypted-dns

Android / GrapheneOS:
Settings → Network & Internet → Private DNS
→ Private DNS provider hostname: dns.mullvad.net
(or: 1dot1dot1dot1.cloudflare-dns.com)
```

### Bluetooth

- **Disable Bluetooth when not in use** — it broadcasts your MAC address and enables tracking across physical spaces
- Bluetooth vulnerabilities are discovered regularly (BIAS, BlueBorne, BLUFFS)
- Review which apps have Bluetooth access: Settings → Privacy → Nearby Devices → revoke all unnecessary

### WiFi in Public

- Disable **"Auto-join"** for public networks
- Disable **Wi-Fi scanning** (used by location services even when WiFi is off)

```
Android: Settings → Location → Wi-Fi scanning → OFF
iOS: Disable via Settings → Privacy & Security → Location Services → System Services → Wi-Fi Networking → OFF
```

---

## 📡 SIM Card & Carrier Privacy

Your carrier knows every cell tower your phone connects to, who you call, and for how long.

### SIM PIN

```
iOS:     Settings → Cellular → SIM PIN → Enable
Android: Settings → Security → SIM card lock → Lock SIM card
```

Use a SIM PIN to prevent someone from swapping your SIM to another device and hijacking your number (SIM swap attack).

### Carrier Tracking Opt-Out

Most carriers sell data about your location and usage to third parties.

```
US carriers:
- AT&T: Enhanced Relevant Advertising → OFF at att.com/privacy
- Verizon: Custom Experience programs → OFF at verizonwireless.com/privacy
- T-Mobile: Advertising → OFF at t-mobile.com/profile/addservices

EU carriers are bound by GDPR — opt-out still recommended
```

### eSIM Considerations

eSIM is more resistant to physical SIM swapping but remains traceable by carrier. When changing carriers, use the eSIM transfer process rather than giving out your phone number.

### Anonymous SIM Options

- Buy a prepaid SIM with cash — in countries where this is legal (Germany, Netherlands, etc.)
- Use a VOIP number (JMP.chat, MySudo) for accounts that require a phone number
- Never use your real number for 2FA on critical accounts if avoidable

---

## 📲 App Hygiene

### Principles

- **Fewer apps = smaller attack surface** — uninstall everything you don't actively use
- **Use the browser** instead of the app where possible — apps have far broader OS access than websites
- **Audit permissions** after every install — apps request more than they need
- **Open source > closed source** — auditable code, no hidden telemetry

### Permission Rules by Category

| App Type | Location | Microphone | Camera | Contacts | Background |
|----------|---------|-----------|--------|---------|-----------|
| Navigation | While Using | ❌ | ❌ | ❌ | Off |
| Social Media | Never | ❌ | While Using | ❌ | Off |
| Messaging | Never | While Using | While Using | Yes (limited) | On |
| Shopping | Never | ❌ | While Using (for QR) | ❌ | Off |
| Banking | Never | ❌ | While Using (for scan) | ❌ | Off |
| Games | Never | ❌ | ❌ | ❌ | Off |

### Checking What Apps Do in Background

```
iOS:
Settings → Privacy & Security → App Privacy Report
(Enable it — after a week it shows exactly what each app accessed and when)

Android:
Settings → Privacy → Privacy Dashboard
→ Shows 24h timeline of which apps accessed which sensors
```

---

## 🌍 Browser

### Android

| Browser | Anti-Tracking | Notes |
|---------|-------------|-------|
| **Mull** (F-Droid) | ✅✅✅ | Hardened Firefox fork for Android — best option |
| **Firefox** + uBlock Origin | ✅✅ | Good if you configure it |
| **Brave** | ✅✅ | Built-in shields, Chromium base |
| **Tor Browser** (Android) | ✅✅✅ | Maximum anonymity — slow |
| Chrome | ❌ | Heavy telemetry — avoid |

### iOS

| Browser | Anti-Tracking | Notes |
|---------|-------------|-------|
| **Firefox Focus** | ✅✅ | Strict tracking protection, no history by default |
| **Brave** | ✅✅ | Shields block most trackers |
| **Safari** (hardened) | ✅ | Built-in ITP, enable all privacy settings |
| **Onion Browser** | ✅✅✅ | Tor on iOS |

### Firefox on Android — about:config

```
Type about:config in address bar:
privacy.resistFingerprinting = true
geo.enabled = false
media.peerconnection.enabled = false
privacy.firstparty.isolate = true
network.cookie.cookieBehavior = 1
dom.battery.enabled = false
browser.safebrowsing.malware.enabled = false
network.http.referer.XOriginPolicy = 2
```

### Install uBlock Origin

Firefox on Android is the only mobile browser that supports full browser extensions including uBlock Origin. This alone is a major reason to choose it.

---

## 🔑 Password Management & 2FA

### Password Managers for Mobile

| App | Platform | Type | Notes |
|-----|---------|------|-------|
| **Bitwarden** | iOS & Android | Cloud | Free, open-source, audited |
| **KeePassDX** | Android | Local | Offline, opens KeePassXC databases |
| **Strongbox** | iOS | Local | KeePass-compatible, Secure Enclave |
| **Vaultwarden** | Both | Self-hosted | Full control of your vault |

### 2FA Apps

| App | Platform | Notes |
|-----|---------|-------|
| **Aegis** | Android | Open-source, encrypted backup, best option |
| **Raivo OTP** | iOS | Open-source, iCloud encrypted backup |
| **Ente Auth** | Both | E2EE cloud sync, open-source |
| **YubiKey** | Both (NFC) | Hardware key — strongest option |

### Rules

- **Never use SMS for 2FA** — SIM swap attacks are common and easy to execute
- Keep your TOTP app database backed up encrypted (Aegis export → store offline)
- Use hardware keys (YubiKey NFC) for your most critical accounts

---

## 💬 Secure Communication

| App | Anonymity | Metadata | Phone Number | Notes |
|-----|---------|---------|-------------|-------|
| **Signal** | ⭐⭐⭐ | Minimal | Required | Best for trusted contacts |
| **Session** | ⭐⭐⭐⭐ | None | Not required | Decentralized, no metadata |
| **SimpleX** | ⭐⭐⭐⭐⭐ | None | Not required | No user IDs at all |
| **Briar** | ⭐⭐⭐⭐ | None | Not required | Works over Bluetooth/Tor |
| **Matrix/Element** | ⭐⭐⭐ | Variable | Not required | Self-hostable |
| **WhatsApp** | ⭐ | Extensive | Required | Owned by Meta — avoid |
| **Telegram** | ⭐ | Extensive | Required | Not E2EE by default — avoid |

### Signal Configuration

```
Settings → Privacy:
  ✗ Read receipts → OFF
  ✗ Typing indicators → OFF
  ✗ Link previews → OFF
  → Screen lock → ON
  → Screen security → ON
  → Incognito keyboard → ON

Settings → Notifications:
  → Show → No name or message (lockscreen)

Settings → Chats:
  → Default message timer → 1 week (sensitive contacts: 1 day)
```

### Which App for Which Situation

| Situation | Recommendation |
|-----------|---------------|
| Talking to family/close friends | Signal |
| Anonymous contact (no phone number) | Session or SimpleX |
| Maximum security + anonymity | SimpleX over Tor |
| Group chats, communities | Matrix/Element (self-hosted server) |
| Offline communication | Briar |

---

## 🔒 Physical Security

### Passcode / PIN

- **Alphanumeric passphrase** — strongest option
- Minimum **6 digits** if using numeric PIN
- Avoid: birthdays, repeated digits (1111), sequential numbers (1234)
- Enable: auto-lock after 1–2 minutes
- Enable: erase data after 10 failed attempts (destructive — make sure you have a backup)

### Biometrics — When to Use and When Not To

Biometrics (Face ID / fingerprint) are **convenient** but have specific risks:

| Scenario | Recommendation |
|---------|---------------|
| Daily use, trusted environment | Biometrics OK |
| Crossing borders | Disable biometrics, use PIN only |
| Protests, high-risk events | Disable biometrics, use PIN only |
| Arrested or detained | Do not consent to biometric unlock |
| Sleeping | Require attention / attention-aware for Face ID |

```
iOS: Settings → Face ID & Passcode → Require Attention → ON
iOS: Emergency SOS (press volume + side button) → disables Face ID temporarily
Android: Power button → Lockdown mode → disables biometrics until PIN entered
```

### USB Data Protection

```
iOS: Settings → Face ID & Passcode → USB Accessories → OFF
Android Developer Options: Default USB configuration → No data transfer

This blocks forensic extraction tools that connect via USB.
```

### Remote Wipe

```
iOS: Find My → findmy.apple.com → Erase iPhone (requires Find My enabled)
Android: android.com/find → Erase device (requires Find My Device enabled)
GrapheneOS: Same as Android
```

Enable Find My / Find My Device now — before you need it.

---

## 📡 IMSI Catcher Protection

IMSI catchers (Stingrays) are devices used by law enforcement and some attackers that impersonate cell towers to track your location and intercept communications.

### Detection

Signs you may be near a Stingray:
- Sudden drop to 2G/EDGE signal
- Unusual battery drain
- Phone gets warm without obvious cause
- Calls that drop immediately

### Protection

```
Android (developer options):
Settings → Mobile Network → 2G (Allow/Disable)
→ Disable 2G — forces phone to stay on LTE/5G
→ Stingrays often force phones to downgrade to 2G

iOS 17+:
Settings → Cellular → LTE / 5G Only Mode
→ Available on iPhone 14+ in some regions
```

**Apps for detection:**

| App | Platform | Notes |
|-----|---------|-------|
| **SnoopSnitch** | Android (root) | Analyzes baseband for IMSI catcher signatures |
| **AIMSICD** | Android | Open-source IMSI catcher detector |

### Best Protection

The only reliable protection is:
1. Use WiFi calling (calls go over internet, not cell network)
2. Use Signal — E2EE protects content even if intercepted
3. Disable 2G to prevent tower downgrade attacks

---

## 🎛️ Sensor & Hardware Privacy

Modern phones have many sensors that can be used to track you.

### Sensor Off (Android)

```
Settings → Developer Options → Quick Settings developer tiles
→ Enable "Sensors Off" tile

Pull down quick settings → Sensors Off tile:
→ Disables ALL sensors: camera, microphone, accelerometer, gyroscope, GPS, magnetometer
→ Use when you want a phone that cannot sense or report anything
```

### GrapheneOS Per-App Sensor Permission

```
Settings → Apps → [App] → Permissions
→ Sensors: Block for apps that don't need hardware sensors
→ Camera: Block
→ Microphone: Block
→ Location: Block
→ Network: Block (unique to GrapheneOS)
```

### Microphone Indicator

On Android 12+ and iOS 14+: a **green dot** (camera) or **orange dot** (microphone) appears in the top right when these are actively used. Watch for unexpected indicators.

---

## 💾 Backup Strategy

| Method | Security | Risk |
|--------|---------|------|
| **iCloud Backup** | ⚠️ Weak | Apple can hand data to law enforcement. Use Advanced Data Protection |
| **iCloud + Advanced Data Protection** | ✅ Good | E2EE — Apple can't read it |
| **Local backup (iTunes/Finder)** | ✅ Good | Encrypted backup with strong passphrase |
| **Google Backup** | ❌ Weak | Google has access |
| **Encrypted local backup** | ✅✅ Best | Seedvault (GrapheneOS) → external drive |
| **No backup** | ⚠️ Risky | Clean but you lose everything if phone dies |

### GrapheneOS — Seedvault

```
Settings → System → Backup → Seedvault
→ Encrypted, open-source backup to USB drive or Nextcloud
→ No Google account required
```

### iOS — Local Encrypted Backup

```
Finder (Mac) or iTunes (Windows) → Connect phone
→ "Encrypt local backup" → Set strong passphrase
→ This encrypts Health, passwords, and sensitive app data too
```

---

## 🔄 Open Source Replacements

| Instead of | Use | Platform |
|------------|-----|---------|
| Google Maps | **OsmAnd** / **Organic Maps** | Both |
| Chrome | **Firefox** / **Brave** / **Mull** | Both |
| Gmail | **Proton Mail** / **Tutanota** | Both |
| Google Photos | **Ente Photos** (E2EE) | Both |
| Google Authenticator | **Aegis** / **Raivo** / **Ente Auth** | Android/iOS |
| WhatsApp | **Signal** / **Session** / **SimpleX** | Both |
| Google Calendar | **Proton Calendar** / **Etar** | Both |
| Google Drive | **Proton Drive** / **Nextcloud** | Both |
| YouTube | **NewPipe** (no tracking) / **Invidious** | Android/Web |
| TikTok | Delete it. | — |
| Shazam | **SongRec** | Android |
| Google Keyboard | **OpenBoard** / **Heliboard** | Android |
| Samsung Keyboard | **OpenBoard** | Android |

---

## ⚠️ Common Mistakes

| Mistake | Fix |
|---------|-----|
| "Always" location access for most apps | Change to "While Using" or "Never" |
| iCloud Backup enabled without ADP | Disable or enable Advanced Data Protection |
| Bluetooth always on in public | Toggle off when not actively using it |
| Default browser with tracking (Chrome) | Switch to Firefox / Mull / Brave |
| WhatsApp / Telegram for sensitive conversations | Signal, Session, or SimpleX |
| Skipping system updates | Update immediately — patches fix active exploits |
| Same Google/Apple account on work and personal profiles | Separate accounts or work profile |
| SMS for 2FA | Switch to TOTP app or hardware YubiKey |
| Not auditing app permissions regularly | Check Privacy Dashboard / App Privacy Report monthly |
| Photo location sharing | Check camera settings — disable geotagging or strip EXIF before sharing |
| Charging via public USB ports (juice jacking) | Use your own charger or a USB data blocker |
| Weak PIN (1234, 0000, birthday) | Use alphanumeric passphrase |
| Auto-join public WiFi networks | Disable auto-join on all saved networks |

---

## ⚡ Quick Reference

```
OS         →  GrapheneOS (Pixel) or hardened iOS — nothing else
Advertising→  Delete IDFA / Advertising ID completely
Location   →  "While Using" only — never "Always" — audit every app
VPN        →  Always-on + kill switch — Mullvad / ProtonVPN / IVPN
DNS        →  Private DNS → dns.mullvad.net (Android/GrapheneOS)
WiFi       →  Randomized MAC per network — OFF when not in use
Bluetooth  →  OFF when not in use
Browser    →  Mull + uBlock Origin (Android) · Firefox Focus (iOS)
2FA        →  Aegis / Raivo TOTP — never SMS
Passwords  →  Bitwarden / KeePassDX — unique everywhere
Comms      →  Signal (trusted) · Session / SimpleX (anonymous)
Backup     →  Seedvault to USB (GrapheneOS) · Encrypted local (iOS)
Updates    →  Install immediately — security patches exist for a reason
2G         →  Disable 2G to prevent Stingray downgrade attacks
Physical   →  USB accessories blocked · Remote wipe configured
```

---

<div align="center">

> **Privacy on mobile is hard — that's why defaults are terrible.**
> These settings don't make you invisible.
> They raise the cost of tracking you to the point where passive, mass surveillance fails.

<br/>

<img width="100%" src="https://capsule-render.vercel.app/api?type=waving&color=gradient&customColorList=2,3,6&height=100&section=footer"/>

**[↑ Back to top](#-recommended-operating-systems)** · [Device Hardening Guide](https://github.com/cordinsanity/Device-hardening-guide) · [Revenge Plugins](https://github.com/cordinsanity/revenge-plugins)

</div>
