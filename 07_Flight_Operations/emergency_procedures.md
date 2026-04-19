# Overview

This document describes emergency procedures for the TreatTurbine UAV operating on the S550 airframe with a Pixhawk 2.1 Cube Black running ArduPilot Copter 4.6.2. It covers the automatic failsafe behavior configured in ArduPilot and the pilot-initiated response for each failure mode. Operators should review this document and the committed ArduPilot parameter file (.param file link here!).

Pre-flight requirement: All procedures below assume the failsafe parameters in the committed parameter file are loaded and have been verified. Do not fly with a different parameter set without re-verifying these procedures against the new values.

# Failsafe Configuration Summary

Radio Failsafe (FS_THR_ENABLE, FS_THR_VALUE, FS_OPTIONS)

- FS_THR_ENABLE = 1 — enabled, action always Return to Launch (RTL)
- FS_THR_VALUE = 950 — throttle PWM below this value (held for the RC receiver's own timeout) triggers failsafe
- FS_OPTIONS = 16 — bitmask. Bit 4 (Release Gripper on Failsafe) is set. No "Continue in AUTO" or "Continue in GUIDED" bits are set, so the aircraft will exit AUTO/GUIDED and begin RTL on RC loss.

Battery Failsafe (BATT_FS_LOW_ACT, BATT_LOW_VOLT, BATT_CRT_VOLT, BATT_FS_CRT_ACT, BATT_LOW_TIMER, BATT_ARM_VOLT)

- BATT_ARM_VOLT = 13.6 V (3.40 V/cell on 4S) — minimum voltage to arm
- Stage 1 (low): BATT_LOW_VOLT = 13.2 V (3.30 V/cell), BATT_FS_LOW_ACT = 2 (RTL)
- Stage 2 (critical): BATT_CRT_VOLT = 12.0 V (3.00 V/cell), BATT_FS_CRT_ACT = 1 (LAND)
- BATT_LOW_TIMER = 10 s — sustained below-threshold duration before failsafe triggers (tolerates transient sag)
- BATT_CAPACITY = 6000 mAh (matches the OVONIC 4S pack)

GCS Failsafe (FS_GCS_ENABLE, FS_GCS_TIMEOUT)

- FS_GCS_ENABLE = 1 — enabled, action RTL
- FS_GCS_TIMEOUT = 5.0 s — MAVLink heartbeat must be absent for 5 seconds before failsafe triggers

EKF Failsafe (FS_EKF_ACTION, FS_EKF_THRESH)

- FS_EKF_ACTION = 1 (LAND) — on EKF variance exceeding threshold, the aircraft will land at current location
- FS_EKF_THRESH = 0.8 — default/relaxed threshold

Other Failsafes

- FS_VIBE_ENABLE = 1 — vibration failsafe enabled
- FS_CRASH_CHECK = 1 — crash detection enabled (disarms motors if the FC detects a crash)
- FS_DR_ENABLE = 2 — dead reckoning enabled if GPS is lost mid-flight
- FS_DR_TIMEOUT = 30 s

GeoFence (FENCE_ENABLE, FENCE_TYPE, FENCE_ACTION, FENCE_ALT_MAX, FENCE_RADIUS)

- FENCE_ENABLE = 1 — enabled
- FENCE_TYPE = 1 — altitude ceiling only. The circle bit (2) is not set, so FENCE_RADIUS is configured but not enforced.
- FENCE_ALT_MAX = 60.96 m (\~200 ft AGL)
- FENCE_RADIUS = 300 m — configured but inactive per FENCE_TYPE
- FENCE_ACTION = 0 — The fence reports a breach but takes no automatic action. Pilot must respond manually.
- FENCE_MARGIN = 2.0 m

Important Note: The FENCE_ACTION = 0 setting means the geofence will not automatically intervene on breach. If automatic RTL on breach is desired, change to FENCE_ACTION = 1.

RTL Behavior

- RTL_ALT = 2400 cm (24 m above ground level)
- RTL_LOIT_TIME = 3000 ms (3 s hover at home before landing)
- RTL_ALT_TYPE = 0 (altitude relative to home)
- RTL_CONE_SLOPE = 3.0

LAND Behavior

- LAND_SPEED = 50 cm/s (final descent speed)
- LAND_ALT_LOW = 1000 cm (10 m — altitude at which descent transitions to LAND_SPEED)
- LAND_REPOSITION = 1 (pilot can reposition during LAND)

RC Switch Options (auxiliary switch assignments)

- RC7_OPTION = 153 (Arm/Disarm toggle)

# Loss of RC Link

Symptoms:

- Transmitter shows "connection lost" or RC signal warning
- GCS telemetry shows RC channels frozen or FS flag set
- Aircraft enters failsafe flight mode (RTL) unexpectedly

Automatic response:  
FS_THR_ENABLE = 1 triggers RTL on loss of RC link. The aircraft climbs to RTL_ALT (24 m AGL), navigates to the launch point, loiters for 3 seconds, then lands at LAND_SPEED (0.5 m/s).

Pilot actions:

- Do not toggle the transmitter power off and on — repeated link drops confuse the failsafe logic
- Verify on the GCS map that the aircraft is executing RTL (flight mode indicator will change)
- Clear the landing zone of people and animals
- If RC link is regained during failsafe, do not fight the failsafe — let it complete RTL, then regain manual control only after the aircraft is over the home point
- Do not rearm until the root cause of the RC loss is identified

Common root causes to investigate post-incident:

- Antenna orientation on the transmitter or aircraft
- 2.4 GHz interference at the site (other transmitters)
- Low transmitter battery
- Physical obstructions at operational range

# Loss of Telemetry (GCS) Link

Symptoms:

- QGroundControl shows "communication lost" or MAVLink heartbeat timeout
- Telemetry map stops updating; aircraft attitude display freezes

Automatic response:  
FS_GCS_ENABLE = 1 triggers RTL after FS_GCS_TIMEOUT = 5 s of missing MAVLink heartbeat.

Pilot actions:

- The aircraft is NOT uncontrolled — RC link is independent of telemetry. Fly on RC.
- If in AUTO, consider switching to POSHOLD or STABILIZE on the flight mode switch to regain direct control
- Attempt to restore telemetry: check SiK radio LEDs, USB connection to GCS laptop, and power to the ground SiK unit
- Land manually if the aircraft is behaving predictably

Notes:

- Because both RC and GCS failsafes default to RTL, a simultaneous failure of both links will still result in an automatic RTL attempt

# Loss of Video Link

Symptoms:

- VRX shows no signal or static
- Capture card input goes black; livestream freezes or shows no signal

Automatic response: None. The video system is independent of the flight controller. The aircraft continues the mission.

Pilot actions:

- The aircraft is unaffected by video loss. Continue using RC input and the GCS map view.
- If video is required for the mission (e.g., donor-triggered drop confirmation), abort by initiating RTL from GCS
- Post-flight, diagnose using 09_troubleshooting.md, Video System section

# Flyaway

Definition: The aircraft is not responding to RC input and is not executing an expected failsafe — it is drifting, climbing, or holding position in a way that does not match any commanded mode.

Symptoms:

- Aircraft does not respond to stick input in a manual flight mode
- Aircraft continues moving after flight mode switch changes
- Position on the GCS map diverges from the expected flight path

Immediate pilot actions (in order):

1. Command RTL from GCS. There is no flight-mode-switch position assigned to RTL (confirmed from FLTMODE1–6), so this must be done via QGroundControl. If the aircraft has a valid GPS fix and the Pixhawk is still executing commands, it will begin to return.
2. If GCS RTL produces no response within \~5 seconds, command LAND from GCS. Landing at an uncontrolled location is preferable to a longer flyaway.
3. If neither command produces any response, and the aircraft is within visual range: Disarm via the Arm/Disarm switch on channel 7or disarm via throttle-stick gesture. Note: ArduPilot blocks in flight for modes like AUTO/RTL/LAND
4. If the aircraft is outside visual range and unrecoverable: note the last known GPS coordinates from telemetry. Do not attempt to chase. Report to the relevant authority.

Root-cause investigation (post-incident):

- GPS glitch or compass interference causing EKF divergence — check DataFlash log for EKF events, GPS HDOP spikes, and compass variance
- Poor compass calibration — recalibrate
- Parameter misconfiguration — diff the flight's parameters against the committed param file

# Low Battery Emergency

Symptoms:

- Telemetry shows battery voltage approaching BATT_LOW_VOLT (13.2 V)
- GCS reports "Battery Low" or "Battery Critical"

Automatic response:

- At BATT_LOW_VOLT = 13.2 V (3.30 V/cell), held for BATT_LOW_TIMER = 10 s: RTL (BATT_FS_LOW_ACT = 2)
- At BATT_CRT_VOLT = 12.0 V (3.00 V/cell): LAND at current location (BATT_FS_CRT_ACT = 1)

Pilot actions:

- Allow the automatic failsafe to execute
- If the RTL path is obstructed (e.g., trees between aircraft and home), immediately switch to LAND via GCS — do not let the battery reach 12.0 V while flying through obstacles
- Do not override battery failsafe to complete a treat drop. Land, swap the battery, and re-arm with a fresh pack.

Notes:

- OVONIC 4S 6000 mAh battery: nominal 14.8 V, fully charged 16.8 V
- The 10-second BATT_LOW_TIMER absorbs transient sags during aggressive throttle, but sustained low readings trigger RTL

# GPS / EKF Failure

Symptoms:

- GCS reports "GPS Glitch" or GPS HDOP spikes above \~2.0
- "EKF Variance" warning on telemetry
- Aircraft drifts horizontally in POSHOLD or AUTO despite stable stick input

Automatic response:  
FS_EKF_ACTION = 1 triggers LAND at current location when EKF variance exceeds FS_EKF_THRESH = 0.8. Dead reckoning is enabled (FS_DR_ENABLE = 2, FS_DR_TIMEOUT = 30 s) so the aircraft will continue for up to 30 seconds on inertial navigation before the EKF failsafe commits.

Pilot actions:

- Switch to STABILIZE (flight mode switch positions 1, 2, or 5) immediately — this removes dependence on GPS/EKF altogether
- Fly manually to a safe landing area
- Do not re-engage GPS-dependent modes (POSHOLD, AUTO) until the aircraft is on the ground and GPS status has been confirmed

Common causes:

- Compass interference from nearby metal, power lines, or the VTX/current-carrying traces too close to the magnetometer
- GPS antenna obscured by canopy, metal, or the operator's body

# Motor / ESC Failure

Symptoms:

- Aircraft yaws uncontrollably in one direction
- Telemetry shows one motor output saturated
- Audible tone change from one motor

Automatic response:  
The S550 is a hexacopter (six motors) and ArduPilot will attempt to maintain attitude with five motors, but compensation is limited without dedicated lost-motor firmware flags enabled. FS_CRASH_CHECK = 1 will disarm motors if the FC detects an actual crash.

Pilot actions:

- Switch to STABILIZE and attempt a controlled descent
- If the aircraft is unrecoverable, initiate LAND from GCS, or allow crash detection to trigger
- Do not attempt RTL — the added flight time under an unbalanced power train risks progressive failure of the remaining motors

Post-incident:

- Inspect all ESCs, motors, and wiring
- Replace failed components and bench-test before the next flight

# Geofence Breach

Symptoms:

- GCS reports "Fence breach"
- Aircraft altitude above FENCE_ALT_MAX = 60.96 m

Automatic response:  
None. FENCE_ACTION = 0 (Report Only). The breach is logged and announced on the GCS but the aircraft continues its current behavior.

Pilot actions:

- Manually reduce altitude via stick input (in STABILIZE/POSHOLD) or waypoint edit (in AUTO)
- If the aircraft cannot be brought back under the ceiling promptly, command RTL or LAND from GCS

# TreatTurbine Mechanism Malfunction in Flight

Symptoms:

- Dispenser does not actuate at commanded mission waypoint (no drop visible on video)

Automatic response: None. The flight controller continues the mission regardless of mechanism state.

Pilot actions:

- A failed drop is not a flight emergency. Continue the mission and attempt the next scheduled drop.
- If the mechanism is clearly jammed abort by commanding RTL from GCS
- Do not attempt to cycle the servo manually mid-flight unless explicitly planned. DO_SET_SERVO commands in a mission execute only between waypoints.

Post-flight diagnosis: see 09_troubleshooting.md, Dispenser Mechanism section.

# Post-Emergency Procedures

After any failsafe trigger or emergency event:

- Secure the aircraft: disarm, disconnect battery, move to a safe inspection area
- Retrieve flight logs: download the data log from the Pixhawk SD card or via MAVLink. The log contains all failsafe triggers, GPS/EKF state, RC input, and motor outputs.
- Retrieve video: Do not delete footage until post-incident review is complete.
- Document: record time, weather, battery voltage at start and end, flight mode history, and a narrative of the event
- Do not return to flight without identifying the root cause and verifying the fix — bench-test with props off, or in a low-risk flight profileDoes this need to stay?
What emergencies are we worried about?
