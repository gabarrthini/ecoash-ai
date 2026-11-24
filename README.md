# ♻️ EcoAsh AI – Smart Boiler Ash Management Platform

A connected web platform for handling boiler ash from generation to reuse or authorized disposal: collection, storage, monitoring, transportation and value-added utilization.

> **Status:** software-only prototype. No IoT hardware is connected yet. Sensor values are manual entries or simulated and are labelled **DEMO DATA**.

## Problem

Boiler ash generated during operation must be handled, collected, transported and disposed of safely and economically. This project provides a systematic method for ash collection, storage, monitoring and identification of potential reuse applications, with less manual handling, better housekeeping and lower disposal cost.

## Three connected users

| User | Role | Main functions |
|---|---|---|
| **Thermal Plant** | Ash generator | Create batches (unique Batch ID + QR), Digital Ash Passport, link lab reports, silo and sensor monitoring, confirm dispatch |
| **Waste Management Industry (Admin)** | Central control centre | Approve plants and buyers, verify lab reports, marketplace and AI matching, approve orders (auto-reserves stock), schedule transport, record final reuse or disposal, analytics and reports |
| **Buyer** | Ash receiver | Submit ash requirements, browse verified batches, compare batches, request ash, confirm received quantity using the QR / Batch ID, report discrepancies |

All three share the same records and the same Batch ID.

## Workflow

1. Plant creates a batch → Batch ID and QR passport are generated
2. Plant links a laboratory report
3. Admin verifies the report → batch is listed in the marketplace
4. Buyer views batches and potential matches, then requests a quantity
5. Admin approves → quantity is reserved (overbooking is blocked)
6. Pickup is scheduled and the plant confirms dispatch
7. Buyer verifies the QR / Batch ID and confirms received quantity
8. Admin records final reuse or authorized disposal

## Features

- Password sign-in: choose user type first, then enter username and password
- New plant and buyer registrations are sent to the admin team for approval
- Role-based permissions in the data layer
- Batch passports with QR codes, movement history and downloadable quality certificates
- AI-style potential-match scoring (rule-based) between lab results and buyer requirements; matches are *potential* until the lab report is verified
- Stock tracking: Available, Reserved, Dispatched, Delivered, Reused, Disposed
- Notification centre for each user
- Analytics with CSV download and print / save as PDF
- Responsive dark industrial UI

## Run locally

No build step. Download the repository and open `index.html` in a browser.

## Deploy with GitHub Pages

1. Rename `ecoash-ai.html` to `index.html`
2. Repository → **Settings → Pages**
3. Source: *Deploy from a branch*, branch `main`, folder `/ (root)` → **Save**
4. Your site appears at `https://<username>.github.io/<repository>/`

## Demo accounts (sample data only)

| Role | Username | Password |
|---|---|---|
| Admin | `admin` | `Admin@123` |
| Thermal Plant | `tnp` | `Plant1@123` |
| Thermal Plant | `nct` | `Plant2@123` |
| Buyer | `ops@ultrablend.example` | `Buyer1@123` |
| Buyer (pending approval) | `info@greenbrick.example` | `Buyer2@123` |

These are demo credentials for sample data. Do not reuse them anywhere real.

## Future IoT integration (ESP32)

Planned sensors: load cell (ash weight), ultrasonic (silo level), temperature, dust, with an ESP32 sending readings over Wi-Fi.

The sensor layer is already separated in the code. A single function, `AshAPI.ingestReading(plantId, payload, source)`, validates readings, stores history and raises alerts. A server endpoint will call the same logic:

```
POST /api/v1/plants/{plantId}/readings
x-device-key: <per-plant secret>
{ "loadcell_kg": 0, "ultrasonic_level_pct": 0, "temp_c": 0, "dust_ugm3": 0 }
```

## Limitations

- Data is stored in the browser (localStorage), so it is not shared between devices
- Passwords are hashed in the browser and permissions are enforced client-side; production use needs a server, a database and real authentication
- Uploaded lab files are not stored (only the file name is recorded)
- QR verification is by entering the QR content or Batch ID; camera scanning is not implemented
- No real hardware is connected

## Roadmap

- [ ] Backend and database (shared data, server-side authentication and permissions)
- [ ] ESP32 + sensor integration and live readings
- [ ] Camera-based QR scanning
- [ ] Lab report file storage
- [ ] PDF report generation on the server

## Tech stack

HTML, CSS and vanilla JavaScript in a single file; [qrcodejs](https://cdnjs.com/libraries/qrcodejs) for QR codes.

## Project context

Developed for the iTNT (Tamil Nadu Technology Hub) problem statement on boiler ash management: ash quantity monitoring, collection, safe storage, transportation and suitable reuse or disposal.

## License

Add a license of your choice (for example MIT) before sharing publicly.
