# Open FTMS Trainer Bridge

Turn a simple bike + cadence sensor into a **smart indoor trainer** that connects directly to your bike computer — **no proprietary apps, no locked ecosystems, no expensive hardware**.

---

## Motivation

I have mulitple bikes at home and a **traditional roller-based indoor trainer**. Mechanically, it works flawlessly — simple, reliable, and effective.

The problem?
It has **no way to connect to my bike computer**.

- My bike computer already shows **HR, cadence, laps, recording**
- I want to use the **same setup indoors and outdoors**
- While there are apps that can pair with HR and cadence sensors:
  - They don’t behave like a real trainer
  - They don’t integrate cleanly with bike computers
  - They leave many metrics and workflows behind

I didn’t want to:
- Change how I ride
- Buy an expensive smart trainer
- Depend on a proprietary app

So I built a small **FTMS simulator**.
This project reads data from a cadence sensor and **acts like a smart indoor trainer**, allowing my existing bike computer to connect without changing anything in my setup.

Now I can:
- Train indoors
- Use the same bike computer I use outdoors
- Record rides natively
- See cadence, speed, HR — all in one place

---

## What this project does

This project:

1. Reads **cadence** from a standard BLE cadence sensor
2. Calculates **speed**
3. Simulates a **smart trainer using FTMS**
4. Connects directly to your **bike computer**
5. Records rides exactly like outdoor sessions

From the bike computer’s perspective, it’s just another smart trainer.

---

## Simple flow diagram

      ┌────────────────────┐
      │  Cadence Sensor    │
      │  (BLE / CSC)       │
      └─────────┬──────────┘
                │
                ▼
      ┌────────────────────┐
      │  Laptop / SBC      │
      │  (Python)          │
      │                    │
      │  - Read cadence    │
      │  - Compute speed   │
      │  - Build FTMS data │
      └─────────┬──────────┘
                │
    ┌───────────┼──────────────────────┐
    │           │                      │
    ▼           ▼                      ▼
    ┌────────────┐ ┌──────────────┐ ┌────────────────┐
    │ macOS      │ │ Linux        │ │ ESP32          │
    │ (mock /    │ │ (BlueZ)      │ │ (recommended)  │
    │ experiment)│ │              │ │                │
    └──────┬─────┘ └──────┬───────┘ └──────┬─────────┘
           │              │                │
           ▼              ▼                ▼
            ┌─────────────────────────────┐
            │      Bike Computer          │
            │  (Garmin / Wahoo / etc.)    │
            │                             │
            │  - Speed                    │
            │  - Cadence                  │
            │  - HR (directly paired)     │
            │  - Recording                │
            └─────────────────────────────┘

   
## Core idea
**Use your bike computer indoors exactly the same way you use it outdoors.**

No workflow changes.  
No new apps.  
No proprietary hardware.

### Design principles

- FTMS logic is platform-agnostic
- Transport layers are isolated
- No BLE hacks in core code
- Easy to extend and debug

---

## Platform support

| Platform | Status | Notes |
|--------|------|------|
| macOS |  Experimental | Best for development & testing |
| Linux |  Supported | BlueZ FTMS GATT server |
| ESP32 |  Recommended | Stable, cheap, production-ready |

**Recommendation:**  
For real indoor training, use the **ESP32 transport**.

---

## Supported metrics

- Cadence (from sensor)
- Speed (computed)
- Heart Rate (paired directly to bike computer)
- Time, laps, recording (bike computer native)

Resistance / ERG mode is planned but not required for basic indoor rides.

---

## What this is *not*

- A full training platform
- A Zwift replacement
- A commercial smart trainer

It’s intentionally **minimal and open**.

---

## Why FTMS

FTMS (Fitness Machine Service) is the standard used by:

- Garmin
- Wahoo
- Zwift
- Most modern smart trainers

By speaking FTMS, this project:
- Works with existing devices
- Avoids vendor lock-in
- Stays future-proof

---

## License

MIT License  
Use it, modify it, ship it — just don’t pretend you wrote it 

---

## Closing note

This started as a simple personal problem:

> “I want my indoor rides to show up on my bike computer.”

If you already own a bike, a cadence sensor, and a working trainer — this project lets you keep using them, indoors and out.

---
