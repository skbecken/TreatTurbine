
# QGroundControl Mission Setup — Basic Manual

> **Software Version:** QGroundControl v4.x+
> **Applies To:** Fixed-wing, Multirotor, VTOL (ArduPilot / PX4)

---

## Table of Contents
1. [[#1-interface-overview|Interface Overview]]
2. [[#2-connecting-your-vehicle|Connecting Your Vehicle]]
3. [[#3-opening-the-plan-view|Opening the Plan View]]
4. [[#4-creating-a-basic-waypoint-mission|Creating a Basic Waypoint Mission]]
5. [[#5-waypoint-parameters-explained|Waypoint Parameters Explained]]
6. [[#6-mission-start--return-settings|Mission Start & Return Settings]]
7. [[#7-uploading-the-mission|Uploading the Mission]]
8. [[#8-starting-the-mission|Starting the Mission]]
9. [[#9-monitoring-during-flight|Monitoring During Flight]]
10. [[#10-saving--loading-missions|Saving & Loading Missions]]

---

## 1. Interface Overview

When QGroundControl opens, you will see the **Fly View** by default.

| UI Element | Description |
|---|---|
| **Fly View** | Live map + telemetry during flight |
| **Plan View** | Where you build missions |
| **Vehicle Status Bar** | Battery, GPS, flight mode, arming state |
| **Instrument Panel** | Speed, altitude, heading, etc. |

---

## 2. Connecting Your Vehicle

### Via USB / Telemetry Radio
1. Plug in your vehicle or telemetry radio into computer
2. QGC will **auto-detect** most connections
3. A green **Connected** indicator appears in the top toolbar

---

## 3. Opening the Plan View

Click the **Plan** icon in the top toolbar (grid/map icon).

You will see:
- A blank map centered on your last known location or GPS position
- A **Mission toolbar** on the left side
- A **Mission item list** on the right side

---
## 4. Creating a Basic Waypoint Mission

### Step 1 — Set the Takeoff Point
- Click **Plan Tools (left sidebar) → Takeoff**
- Click on the map where takeoff should occur
- A **Takeoff** item `(T)` appears

> For multirotor, QGC will automatically add a takeoff command. For fixed-wing, a launch altitude is required.

### Step 2 — Add Waypoints
- Click the **Waypoint tool** (pin icon) in the left sidebar
- Click on the map to place **Waypoint 1**, **Waypoint 2**, etc., which is where the drone will execute its various commands
- A numbered marker and connecting line appear on the map

### Step 3 — Add Servo Actuation Command
At a waypoint where the servo should actuate to drop the treat from the TreatTurbine, add a `DO_SET_SERVO` command:

1. Click the waypoint where actuation should occur
2. In the **command panel**, click **Add Command → DO_SET_SERVO**
3. Set the following parameters:

| Parameter | Value | Description |
|---|---|---|
| **Servo Number** | `e.g. 9` | The output channel the servo is connected to |
| **PWM** | `1100` | Servo start position (disengaged) |
| **PWM** | `1500` | Servo actuated position (engaged) |

> A PWM of `1100` corresponds to the disengaged position and `1500` corresponds to the actuated position. Adjust these values if your servo requires a different range.

### Step 4 — Add a Return to Launch (RTL)
- Click **Plan Tools → Return to Launch**
- Place it after your last waypoint
- The vehicle will fly home and land automatically

---

## 5. Waypoint Parameters Explained

When you click a waypoint on the map or in the list, a **parameter panel** appears on the right.

### Key Parameters

| Parameter | Description | Typical Value |
|---|---|---|
| **Altitude** | Height above takeoff point (relative) or sea level (absolute) | `30–120 m` |
| **Altitude Mode** | Relative / Absolute / Terrain Following | `Relative` (most common) |
| **Speed** | Waypoint-specific speed (if supported by firmware) | `5–15 m/s` |
| **Hold Time** | Seconds to loiter at this waypoint | `0 s` (no hold) |
| **Accept Radius** | How close the vehicle must get to proceed | `5 m` |
| **Pass Through** | Skip stopping at this waypoint | Off |

### Waypoint Actions (Commands)

Click the **Command** dropdown to change what happens at a waypoint:

| Command | Use Case |
|---|---|
| `WAYPOINT` | Fly to location |
| `LOITER_TIME` | Circle for X seconds |
| `LOITER_TURNS` | Circle N times |
| `DO_CHANGE_SPEED` | Change speed mid-mission |
| `DO_SET_CAM_TRIGG_DIST` | Trigger camera every N meters |
| `LAND` | Land at this point |
| `RETURN_TO_LAUNCH` | Go home |

---

## 6. Mission Start & Return Settings

### Mission Start Item
At the top of the right-side list, click **Mission Start** to set:
- **Flight speed** (default for the whole mission)
- **Planned home position** (where RTL flies back to)

---

## 7. Uploading the Mission

Once your mission is complete:

1. **Review** the total distance and estimated flight time shown at the bottom of the screen
2. Click the **Upload Required** button (top right of Plan View) — turns blue when changes are pending
3. Confirm the upload — QGC sends all waypoints to the vehicle
4. Status changes to **Mission items uploaded**

Note: You must Upload a mission after every edit to the plan

---

## 8. Starting the Mission

Switch to **Fly View**.

### Pre-Flight Checklist
- [ ] Battery charged (>14.2V recommended)
- [ ] GPS lock acquired (>8 satellites, HDOP <1.5)
- [ ] Home position set
- [ ] Mission uploaded and confirmed
- [ ] Airspace clearance confirmed
- [ ] Failsafes configured

### Arm & Start
1. **Arm the vehicle** — Slide to arm or use your transmitter
2. Set mode to **Auto** (Mission mode) on your transmitter, **OR** in QGC tap the mode indicator → **Mission**
3. Click the **Takeoff / Start Mission** slider in QGC

---

## 9. Monitoring During Flight

| Display | What to Watch |
|---|---|
| **Map** | Vehicle position vs. planned path |
| **Altitude Graph** | Current vs. planned altitude |
| **Battery Widget** | Remaining capacity and estimated return capacity |
| **Next Waypoint** | Which WP is active and distance to it |
| **Instrument Panel** | Airspeed, groundspeed, heading |

### Intervention Options

| Action | How |
|---|---|
| **Pause Mission** | Tap Pause — vehicle holds position |
| **Resume** | Tap Resume — continues from current waypoint |
| **RTL** | Tap RTL button — overrides mission |
| **Land Now** | Tap Land — descends immediately |
| **Take Control** | Switch transmitter to Manual/Stabilize |

---

## 10. Saving & Loading Missions

### Save a Mission
- Plan View → **File icon (☰) → Save to File**
- Saves as a `.plan` file (JSON format)

### Load a Mission
- Plan View → **File icon (☰) → Open File**
- Select your `.plan` file

---
