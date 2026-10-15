# 👥 MAD Practical 7 — JSON API + SQLite (Android)

![Platform](https://img.shields.io/badge/Platform-Android-3DDC84?logo=android&logoColor=white)
![Language](https://img.shields.io/badge/Language-Kotlin-7F52FF?logo=kotlin&logoColor=white)
![Database](https://img.shields.io/badge/Database-SQLite-003B57?logo=sqlite&logoColor=white)
![Min SDK](https://img.shields.io/badge/minSdk-26-blue)
![Target SDK](https://img.shields.io/badge/targetSdk-37-blue)

**Subject:** Mobile Application Development (MAD)
**Enrollment No.:** 25172022054
**Practical No.:** 7

---

## 📖 Overview

An Android app that **retrieves person data in JSON format from an internet API**, **stores it in a local SQLite database**, and **displays it in a RecyclerView**. Individual records can be deleted from the database, and the list can be refreshed from the API at any time.

If the device is offline (for example in a lab), the app automatically **falls back to a bundled `persons.json`** so it always has data to show.

## 🎯 Aim

To retrieve data in JSON format from an internet API, store it in an SQLite database, and display the stored records in the application.

## ✨ Features

- 🌐 Fetches JSON from an internet API using `HttpURLConnection` (primary URL + mirror URL)
- 📴 **Offline fallback** to `assets/persons.json` when the network is unreachable
- 🗄️ Stores records in **SQLite** through a custom `SQLiteOpenHelper`
- 📜 Shows records in a **RecyclerView** (name, phone, email, address)
- 🗑️ **Delete** a record — removed from both the database and the list
- 🔄 **Refresh button** — re-downloads the data and rewrites the database
- ⏳ Progress bar while loading and an **empty-state message** when there are no records
- 🧵 Network and database work runs on a **background thread**; UI is updated on the main thread
- 🔁 On the next launch, data is loaded straight from SQLite (no network call needed)

## 🧭 Data Flow

```
   Internet API (JSON)  ──fail──►  assets/persons.json (offline fallback)
          │                                   │
          └────────────► ApiService ◄─────────┘
                              │  parse JSON → List<Person>
                              ▼
                      DatabaseHelper (SQLite)
                              │  getAllPersons()
                              ▼
                  PersonAdapter ──► RecyclerView
```

## 🗃️ Database Schema

**Database:** `persons.db` **Table:** `persons`

| Column | Type | Notes |
|---|---|---|
| `id` | INTEGER | Primary key, auto-increment |
| `name` | TEXT | NOT NULL |
| `phone` | TEXT | |
| `email` | TEXT | |
| `address` | TEXT | |

## 📦 JSON Format

```json
[
  {
    "name": "Yesenia Yang",
    "phone": "+917983385148",
    "email": "yesenia_yang@gnu.ac.in",
    "address": "16 Sackett Street, Courtland, Michigan"
  }
]
```

## 🛠️ Tech Stack

| Component | Details |
|---|---|
| Language | Kotlin |
| UI | XML layouts, RecyclerView, Material Components |
| Networking | `HttpURLConnection` |
| JSON Parsing | `org.json` (`JSONArray`, `JSONObject`) |
| Local Storage | SQLite (`SQLiteOpenHelper`) |
| Concurrency | `kotlin.concurrent.thread` + `runOnUiThread` |
| Min SDK | 26 (Android 8.0) |
| Target SDK | 37 |
| Permissions | `INTERNET`, `ACCESS_NETWORK_STATE` |

## 📂 Project Structure

```
25172022054_mad_practical7/
├── app/src/main/
│   ├── java/com/new_app/a25172022054_mad_practical7/
│   │   ├── MainActivity.kt            # Loads data, refresh & delete handling
│   │   ├── model/Person.kt            # Data class
│   │   ├── network/ApiService.kt      # API fetch, JSON parsing, offline fallback
│   │   ├── db/DatabaseHelper.kt       # SQLite CRUD operations
│   │   └── adapter/PersonAdapter.kt   # RecyclerView adapter
│   ├── assets/persons.json            # Offline fallback data
│   ├── res/
│   │   ├── layout/                    # activity_main.xml, item_person.xml
│   │   ├── drawable/                  # icons and shape backgrounds
│   │   └── values / values-night/     # strings, colors, themes (light + dark)
│   └── AndroidManifest.xml
├── gradle/                            # Wrapper + libs.versions.toml
├── build.gradle.kts
└── settings.gradle.kts
```

## 🚀 Getting Started

### Prerequisites
- Android Studio (latest stable recommended)
- JDK 17 or above
- Emulator / device with Android 8.0 (API 26) or higher
- Internet connection (optional — the app works offline using the bundled JSON)

### Run the project

```bash
# 1. Clone the repository
git clone https://github.com/Prince-Prajapati-Maheshbhai/25172022054_mad_practical7.git

# 2. Open in Android Studio and let Gradle sync

# 3. Select an emulator / device and click ▶ Run
```

### How to use the app
1. On the first launch the database is empty, so the app downloads the JSON and saves it to SQLite.
2. Records appear in the list; a toast shows whether they came from the **Internet API** or the **offline fallback**.
3. Tap the 🗑️ icon on a card to delete that record from SQLite.
4. Tap the 🔄 button to re-fetch the data and rebuild the database.

## 📚 Concepts Covered

- Making HTTP requests and handling response codes and timeouts
- Parsing a JSON array into model objects
- SQLite with `SQLiteOpenHelper`, `ContentValues` and transactions
- CRUD operations (insert, read, delete) on a local database
- RecyclerView, ViewHolder and a custom adapter
- Background threads and updating the UI safely
- Reading files from the `assets` folder
- Light and dark theme resources

## 👤 Author

**Prince Prajapati**

Enrollment No.: 25172022054
