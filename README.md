# Snapmaker U1 External NFC Filament Reader

## Project Preview

This project adds external NFC filament-tag reading to the **Snapmaker U1 with the BigTreeTech PopStation Mini**.

The goal is to allow the U1 to read official Snapmaker filament spool tags when using external PopStation Mini feeders.

The system has now been successfully bench-tested across **all four U1 filament feeders**.

> **Status:** Working prototype.  
> Installation photos, mounting documentation, and public release files are still being prepared.

---

## What It Does

The system reads official Snapmaker filament NFC tags and sends the decoded filament information to the Snapmaker U1.

Information decoded from the spool can include:

- Material type
- Material subtype
- Filament color
- SKU
- Diameter
- Spool weight
- Filament length
- Drying temperature
- Drying time
- Recommended nozzle temperature range
- Bed temperature
- First-layer nozzle temperature
- Other-layer nozzle temperature
- Manufacturing date
- Official Snapmaker tag status
- NFC card UID

The information is then associated with the correct U1 filament feeder.

---

## Four-Feeder Support

The current system uses:

- **2 × ESP32-S3 controllers**
- **4 × PN532 NFC readers**
- **1 NFC reader per filament feeder**

The configuration is:

| U1 Feeder | Controller | NFC Reader |
|---|---|---|
| Feeder 1 | ESP1 | Reader 1 |
| Feeder 2 | ESP1 | Reader 2 |
| Feeder 3 | ESP2 | Reader 3 |
| Feeder 4 | ESP2 | Reader 4 |

### ESP1

ESP1 handles:

**Feeder 1 → Reader 1**

**Feeder 2 → Reader 2**

Current tested firmware:

**ESP1 v31**

### ESP2

ESP2 handles:

**Feeder 3 → Reader 3**

**Feeder 4 → Reader 4**

Current tested firmware:

**ESP2 v32**

---

## How It Works

One of the important design decisions in this project is separating **filament presence** from **filament identification**.

### The U1 determines filament presence

The Snapmaker U1's existing feeder sensors determine whether filament is physically loaded.

### NFC determines filament identity

The external PN532 readers determine which Snapmaker spool is associated with that feeder.

In other words:

**U1 feeder sensor = Is filament loaded?**

**NFC reader = What filament is it?**

This avoids relying on continuous NFC detection to determine whether filament is physically present.

---

## U1 Feeder Mapping

The integration maps the four readers to the U1 as follows:

| Feeder | U1 feeder object | U1 filament channel |
|---|---|---|
| 1 | `filament_feed left / extruder0` | `filament_detect.info[0]` |
| 2 | `filament_feed left / extruder1` | `filament_detect.info[1]` |
| 3 | `filament_feed right / extruder2` | `filament_detect.info[2]` |
| 4 | `filament_feed right / extruder3` | `filament_detect.info[3]` |

---

## Current Testing

The system has been bench-tested with all four feeders.

Testing has confirmed:

- Reader 1 → Feeder 1
- Reader 2 → Feeder 2
- Reader 3 → Feeder 3
- Reader 4 → Feeder 4
- Official Snapmaker spool decoding
- Filament metadata transferred to the correct U1 channel
- U1 feeder sensors control loaded/empty state
- Filament information clears when the corresponding U1 feeder becomes empty

---

## Installation

Physical installation is the next stage of the project.

Installation photographs will be added showing:

**[PHOTO — Completed installation]**

**[PHOTO — Feeder 1 / Reader 1]**

**[PHOTO — Feeder 2 / Reader 2]**

**[PHOTO — Feeder 3 / Reader 3]**

**[PHOTO — Feeder 4 / Reader 4]**

**[PHOTO — ESP32-S3 controller installation]**

**[PHOTO — PN532 wiring]**

---

## Hardware

The prototype currently uses:

- Snapmaker U1
- BigTreeTech PopStation Mini
- 2 × ESP32-S3 development boards
- 4 × PN532 NFC modules
- Official Snapmaker NFC-tagged filament spools

Additional wiring and mounting information will be documented after the permanent installation is completed.

---

## A Note About PN532 Reliability

During development, intermittent NFC authentication failures were traced in part to the electrical connection between the PN532 reader and ESP32.

A solid ground connection proved particularly important.

A PN532 may initialize successfully while still experiencing unreliable tag authentication if its electrical connections are marginal.

Detailed wiring and troubleshooting information will be included with the release documentation.

---

## Firmware

The currently tested ESP32 firmware versions are:

**ESP1 v31 — Feeders 1 & 2**

**ESP2 v32 — Feeders 3 & 4**

The U1-side integration is also working in the current development system.

Because the U1-side integration interacts with software on the printer, the public release will include compatibility checks designed to prevent installation against an unknown or changed U1 firmware version.

---

## Project Status

**Four-feeder NFC integration: Bench tested and working**

**Physical installation: Pending**

**Installation photos: Pending**

**Public installer: In development**

**Public release package: In development**

More documentation and installation photographs will be added as the project moves from the bench setup to the permanent installation.

---

## Disclaimer

This is an independent community project.

It is not an official Snapmaker or BigTreeTech product and is not affiliated with or endorsed by Snapmaker or BigTreeTech.
