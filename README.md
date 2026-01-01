# MetroGuardian 🚆

**A High-Reliability, Offline-First Transit Alarm for Global Commuters.**

MetroGuardian is a specialized Android utility designed to solve the "Deep Sleeper" problem on trains and metros. Unlike generic map apps, MetroGuardian uses a fail-safe, sensor-fusion approach to ensure you never miss your stop, even in areas with zero cellular data or GPS signal.

## 🚀 The Problem
Current navigation apps fail commuters in three ways:
1. **The Tunnel Gap:** GPS dies underground, freezing location updates.
2. **The Battery Drain:** Constant high-frequency GPS pings kill the battery.
3. **The Data Vacuum:** Many regions (including 90% of Indian Metros) do not publish official GTFS data, making them "invisible" to apps like Google Maps.

## 🛠 The "Zero-Failure" Engineering Approach
MetroGuardian doesn't just "ping" GPS; it uses a **Multimodal Sensor Fusion** strategy:

- **GPS Geofencing:** Primary trigger for above-ground Indian Railways and suburban lines.
- **Adaptive GPS Polling:** Battery-optimized logic that scales polling frequency based on distance to destination (5 min -> 1 min -> 20 sec).
- **Overshoot Logic:** Mathematical safeguard that triggers the alarm if the distance to the station starts increasing before a "near" ping was registered (crucial for fast express trains).
- **Foreground Service Architecture:** Utilizes a persistent Android Foreground Service to bypass OS-level background restrictions and "Doze" mode.

## 🏗 MVP Tech Stack
- **Platform:** Native Android (Kotlin)
- **Data:** Offline-First JSON (Cleaned OSM Data)
- **Location:** FusedLocationProviderClient
- **Algorithm:** Haversine Great-Circle Distance
- **Architecture:** MVVM + Foreground Services

## 📋 Features
- **Global Coverage:** Powered by OpenStreetMap station nodes.
- **Offline-First:** No API calls required during transit; works in airplane mode.
- **Critical Alerts:** Bypasses "Silent" and "Do Not Disturb" modes to ensure the user wakes up.
- **Zero Configuration:** No accounts, no logins, no tracking. Select station -> Start Alert.

## 📂 Project Structure (MVP)
```text
├── assets/
│   └── stops_cleaned.json      # Validated offline station database
├── src/main/kotlin/
│   ├── ui/                     # Minimalist B&W Search & Setup UI
│   ├── service/
│   │   └── LocationService.kt  # The "Heart" (Foreground Service)
│   ├── engine/
│   │   └── DistanceMath.kt     # Haversine & Overshoot logic
│   └── data/
│       └── StationProvider.kt  # Local JSON parser & search index