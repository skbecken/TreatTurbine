
## Flight Operations & Regulations

### Sequence of Events — Normal Flight

1. **Pre-Flight Setup**
   - Inspect the vehicle for physical damage, loose connections, and secure payload
   - Confirm battery is charged (>14.2V recommended)
   - Confirm GPS lock acquired (>8 satellites, HDOP <1.5)
   - Load and verify mission in QGC Plan View
   - Upload mission to vehicle and confirm waypoint count matches

2. **Arming & Takeoff**
   - Confirm airspace is clear of people, animals, and obstacles
   - Arm the vehicle in QGC
   - Set flight mode to **Auto (Mission)**
   - Initiate takeoff via the **Start Mission** slider in QGC
   - Monitor climb to mission altitude before proceeding

3. **Mission Execution**
   - Vehicle autonomously follows planned waypoints
   - Monitor telemetry (battery, GPS, altitude, speed) continuously in Fly View
   - Confirm servo actuation occurs at the correct waypoint
   - Maintain visual line of sight (VLOS) with the vehicle at all times

4. **Return & Landing**
   - Vehicle executes RTL after final waypoint
   - Monitor descent and confirm landing at home position
   - Disarm the vehicle in QGC after landing

5. **Post-Flight**
   - Inspect vehicle for damage
   - Download and review flight logs from QGC
   - Save the `.plan` mission file
   - Record flight in logbook (required under FAA Part 107)

---

### Desirable Flight Conditions 
**Wind:** The drone should _not_ be flown under windy conditions. 

**Altitude:** Per FAA Part 107, commercial drones should not be flown above 400 feet. 

**Distance to Animal:** An initial clearance of 100 feet should be used to start. The altitude can subsequently be lowered if the animals are undisturbed. The drone should approach no closer than 70 feet. 

> If animals exhibit stress behavior (running, vocalizing, scattering), immediately climb to a higher altitude or abort the mission and return to launch. Some wildlife areas require additional permits before any drone operations.

---

### FAA Part 107 Notes

- **Pilot Certification** — Remote Pilot Certificate (Part 107) required for all commercial operations
- **Registration** — All drones weighing >0.55 lbs must be registered with the FAA
- **Visual Line of Sight (VLOS)** — Pilot must maintain unaided visible line of sight with the vehicle at all times
- **Daylight Operations** — Flight permitted during daylight and civil night hours (with proper lighting)
- **Maximum Altitude** — 400 ft above ground level, or 400 ft above a structure if operating within 400 ft of it
- **Maximum Speed** — 100 mph groundspeed
- **Controlled Airspace** — Requires LAANC authorization or FAA Waiver before entering Class B/C/D/E airspace
- **People & Moving Vehicles** — Do not fly over moving vehicles or non-participating people without a waiver
- **Alcohol / Drugs** — No operations within 8 hours of alcohol consumption
- **Incident Reporting** — Report any accident causing >\$500 in damage or any injury to the FAA within 10 days
- **Flight Logbook** — Recommended to log all flights (date, location, duration, pilot, notes)

```
