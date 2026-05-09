# 🤿 COSMIQ+ Manager

An unofficial web-based manager for the **Deepblu COSMIQ+** dive computer, built for divers who still use this watch after Deepblu shut down their servers and app support.

**🌐 Live App: [mudq0209.github.io/cosmiq-manager](https://mudq0209.github.io/cosmiq-manager/)**

> No installation required. Works directly in Chrome on Android or desktop.

---

## ✨ Features

### 🌊 Dive Log
- **Download all dive logs** directly from the watch via Bluetooth
- Automatically scans all 30 dive slots and downloads unsynced dives
- Full **depth profile** with mini chart visualization
- Dive stats: max depth, duration, min/avg temperature
- Locally saved in your browser — data persists across sessions

### 📸 Photos
- Upload photos to each dive log entry
- Photos with EXIF timestamps are **automatically placed on the depth chart**
- Set a **cover photo** for each dive
- Tap photo markers on the depth chart to view the photo taken at that depth

### ✏️ Dive Log Editor
- Edit dive location, country, dive type (Scuba / Freedive)
- Log dive purpose, site features (reef, wreck, wall, cave, drift, night)
- Record conditions: weather, air temp, waves, current, visibility
- Record gear: air mix, tank pressure, suit type and thickness
- Add personal notes
- ⭐ Star rating

### 📤 Export
- Export all dive logs as **JSON** (full data including depth samples)
- Export as **CSV** (summary data, compatible with spreadsheets)

### ⚙️ Watch Settings
- **Time sync** — sync watch clock to your phone's time
- Default dive mode (Scuba / Freedive / Gauge)
- Display units (Metric / Imperial)
- Date display format
- Screen timeout
- Backlight intensity (1–5)
- Power-saving mode
- Water / altitude environment mode
- Scuba: safety factor, PPO2, air mix, depth alarm, time alarm
- Freedive: max time alarm, 6 depth alarms

---

## 📱 Requirements

- **Android or desktop Chrome** (Web Bluetooth is required; Safari and Firefox are not supported)
- Bluetooth enabled on your phone/computer
- COSMIQ+ dive computer

---

## 🚀 How to Use

### First Time Setup
1. Open **[mudq0209.github.io/cosmiq-manager](https://mudq0209.github.io/cosmiq-manager/)** in Chrome
2. Bookmark this URL — your dive data is saved to this browser session
3. Turn on your COSMIQ+ watch

### Downloading Dive Logs
1. Tap **Connect to COSMIQ+** and select your watch from the list
2. Go to the **🌊 Dive Log** tab
3. Tap **Download All Unsynced Dives**
4. The app will scan all 30 dive slots and download new dives automatically
5. Each dive takes about 20–30 seconds to download (watch needs rest between dives)
6. Once complete, tap any dive card to view details and add photos/notes

### Syncing Time
1. Connect your watch
2. Go to **General** tab
3. Tap **Sync Now** — the watch clock will be set to your phone's current time

### Adding Photos to a Dive
1. Open a dive log entry (tap the card)
2. Scroll to the **Photos** section
3. Tap **＋** to upload photos from your phone
4. If photos have EXIF timestamps matching the dive time, they appear as dots on the depth chart
5. Tap a dot on the chart to view the photo taken at that depth
6. Tap any thumbnail → **Set as Cover** to use as the dive card cover image

### Exporting Data
1. In the **Dive Log** tab, tap **Export JSON** or **Export CSV**
2. The file will be downloaded to your phone

---

## 🔧 Technical Notes

### BLE Protocol (Reverse Engineered)
This app communicates with the COSMIQ+ using the **Nordic UART Service** (UUID: `6e400001-b5a3-f393-e0a9-e50e24dcca9e`).

Key commands discovered through reverse engineering:

| Command | Function |
|---------|----------|
| `0x20` | Time sync write |
| `0x21–0x3F` | Dive slot status query (slots 1–30) |
| `0x43` | Download full dive profile (depth + temperature) |
| `0x41` | Download dive header (date, time) |
| `0x5B–0x60` | Read/write watch settings |

The dive profile (`0x43`) returns depth samples at **20-second intervals**, encoded as alternating temperature (×0.1 °C) and pressure (mbar) values. Depth is calculated as `(pressure_mbar - 1013) / 100` metres.

### Data Storage
All dive data is stored in your browser's **localStorage** under the URL `mudq0209.github.io`. Clearing browser data will remove saved dives — use Export JSON to back up your data.

---

## 🙏 Credits

- Based on [cosmiq5-web](https://github.com/blue-notes-robot/cosmiq5-web) by [@blue-notes-robot](https://github.com/blue-notes-robot) — original BLE settings protocol for COSMIQ Gen5
- Dive log protocol independently reverse engineered from the COSMIQ+ BLE communication
- Built with vanilla HTML/CSS/JavaScript, no dependencies

---

## ⚠️ Disclaimer

This is an **unofficial, community-built tool**. It is not affiliated with Deepblu. Use at your own risk. Always verify your dive computer settings before diving. The developers are not responsible for any issues arising from use of this software.

---

## 📄 License

MIT License — feel free to fork, modify and share.
