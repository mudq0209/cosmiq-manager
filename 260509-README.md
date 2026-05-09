# 🤿 COSMIQ+ Manager

An unofficial web-based manager for the **Deepblu COSMIQ+** dive computer — built for divers who still use this watch after Deepblu shut down their servers and discontinued app support.

**🌐 Live App: [mudq0209.github.io/cosmiq-manager](https://mudq0209.github.io/cosmiq-manager/)**

> No installation required. Works directly in Chrome on Android or desktop.  
> Connect your Google Drive to sync your dive logs and photos across all your devices.

---

## ✨ Features

### 🌊 Dive Log Download
- Download all dive logs directly from the watch via Bluetooth
- Automatically scans all 30 dive slots and downloads unsynced dives
- Full **depth profile** with interactive chart visualization
- Dive stats: max depth, average depth, duration, min/avg water temperature
- Data is saved locally and synced to your Google Drive

### 📸 Photos
- Upload photos to each dive log entry
- Photos with EXIF timestamps are **automatically placed on the depth chart** at the correct depth and time
- Tap photo markers on the depth chart to view the photo taken at that exact depth
- Set a **cover photo** for each dive card
- Photos are stored in your Google Drive (`COSMIQ Manager` folder) — no storage limits

### ✏️ Dive Log Editor
Tap any dive card to open the full detail view and edit:
- Location and country
- Dive type (Scuba / Freedive)
- Purpose: Fun / Training / Photography / Research
- Site features: Reef / Wreck / Wall / Cave / Drift / Night
- Conditions: weather, air temperature, wave height, current, visibility
- Gear: air mix (O₂%), tank start/end pressure, suit type and thickness
- Personal notes
- ⭐ Star rating (1–5)

### ☁️ Google Drive Sync
- Connect your own Google Drive account — **your data stays in your Drive, not on any third-party server**
- Dive logs and photos sync across all your devices (phone, tablet, desktop)
- Bidirectional sync: changes on any device are merged automatically
- Data saved in `My Drive / COSMIQ Manager / dives.json`
- Photos saved as individual files in the same folder
- Works offline — syncs when you next tap the Sync button

### 📤 Export
- Export all dive logs as **JSON** (full data including depth samples, photos metadata)
- Export as **CSV** (summary: date, depth, duration, temperature)

### ⚙️ Watch Settings
- **Time sync** — sync watch clock to your phone's current time
- Default dive mode (Scuba / Freedive / Gauge)
- Display units (Metric / Imperial)
- Date display format (current date / last dive date)
- Screen timeout duration
- Backlight intensity (1–5)
- Power-saving mode (< 5% battery)
- Environment mode (Normal / High Salinity / Altitude > 300m)
- **Scuba:** safety factor, max PPO2, air mix, depth alarm, time alarm
- **Freedive:** max time alarm, 6 independent depth alarms

---

## 📱 Requirements

- **Chrome on Android or desktop** — Web Bluetooth API is required
- Safari and Firefox are **not supported**
- Bluetooth enabled on your device
- Deepblu COSMIQ+ dive computer

---

## 🚀 How to Use

### Setup
1. Open **[mudq0209.github.io/cosmiq-manager](https://mudq0209.github.io/cosmiq-manager/)** in Chrome
2. Bookmark the URL for easy access
3. Connect your Google Drive (tap **Connect** in the Saved Dives section) to enable cross-device sync

### Downloading Dive Logs
1. Turn on your COSMIQ+ and tap **Connect to COSMIQ+**
2. Select your watch from the Bluetooth list
3. Go to the **🌊 Dive Log** tab
4. Tap **Download All Unsynced Dives**
5. The app scans all 30 slots and downloads new dives automatically
6. Each dive takes about 20–30 seconds — the watch needs a short rest between downloads
7. After downloading, tap **Sync** to save everything to Google Drive

### Adding Photos to a Dive
1. Tap any dive card to open the detail view
2. Scroll to the **Photos** section and tap **＋**
3. Select photos from your phone's camera roll
4. Photos with EXIF timestamps matching the dive time are automatically positioned on the depth chart
5. Tap a **teal dot** on the depth chart to view the photo taken at that depth
6. Tap any thumbnail → **Set as Cover** to use it as the dive card's cover image

### Syncing Across Devices
1. On Device A: download dives and tap **Sync** — data uploads to your Drive
2. On Device B: open the app, tap **Connect** (Google Drive), tap **Sync** — data downloads automatically
3. Any edits (location, notes, photos, ratings) are merged on next sync

### Syncing Time
1. Connect your watch
2. Go to **General** tab
3. Tap **Sync Now** — the watch clock is set to your phone's current time

### Exporting Data
1. In the **Dive Log** tab, tap **Export JSON** or **Export CSV**
2. The file downloads to your device

---

## 🔧 Technical Notes

### BLE Protocol (Reverse Engineered)
Communication uses the **Nordic UART Service** (UUID: `6e400001-b5a3-f393-e0a9-e50e24dcca9e`).

Key commands discovered through reverse engineering:

| Command | Function |
|---------|----------|
| `0x20` | Time sync (BCD decimal-as-hex format, len=0x0C) |
| `0x21–0x3F` | Dive slot status query (slots 1–30) |
| `0x41` | Download dive header (date, start time) |
| `0x43` | Download full dive profile (depth + temperature samples) |
| `0x5B` | Read/write screen timeout, date display, units |
| `0x5C` | Read/write dive mode, depth alarm, time alarm, air mix |
| `0x5D` | Read/write safety factor, PPO2, freedive alarms |
| `0x5F` | Read/write environment mode, backlight, power-saving |
| `0x60` | Read/write additional freedive depth alarms |

**Dive Profile Format (`0x43`):** Returns depth samples at 20-second intervals as alternating temperature (×0.1 °C) and pressure (mbar) values. Depth is calculated as `(pressure_mbar − 1013) / 100` metres.

**Checksum Rule:** All write commands follow `CMD + target = 0xFE`, with the exception of time sync which uses target `0xE0`.

### Google Drive Storage
Data is stored in your own Google Drive under `My Drive / COSMIQ Manager /`:
- `dives.json` — all dive records, settings and photo metadata
- `photo_{diveId}_{photoId}.jpg` — individual photo files

The app uses the `drive.file` scope — it can only access files it creates and cannot read any other files in your Drive.

### Local Storage
Dive data and photo cache are also stored in your browser's `localStorage` for offline access. Clearing browser data will remove the local cache — your Drive copy remains intact.

---

## 🙏 Credits

- Based on [cosmiq5-web](https://github.com/blue-notes-robot/cosmiq5-web) by [@blue-notes-robot](https://github.com/blue-notes-robot) — original BLE settings protocol for COSMIQ Gen5
- Dive log download protocol independently reverse engineered from COSMIQ+ BLE communication
- Built with vanilla HTML/CSS/JavaScript — zero dependencies

---

## ⚠️ Disclaimer

This is an **unofficial, community-built tool**. It is not affiliated with Deepblu or any watch manufacturer. Use at your own risk. Always verify your dive computer settings before diving. The developers are not responsible for any issues arising from use of this software.

---

## 📄 License

MIT License — free to fork, modify and share.
