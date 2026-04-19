# Overview

This document captures known failure modes of the TreatTurbine UAV system, their diagnostic signatures, and corrective actions. Entries are organized by subsystem.

Lessons learned from bench and field testing are collected at the end of the document.

# Dispenser Mechanism

Servo does not actuate at the commanded waypoint

- Likely cause: DO_CYCLE_SERVO used instead of DO_SET_SERVO; or SERVOn_FUNCTION for the target output is not set to 0 (Disabled); or the DO command is placed before the first navigation waypoint
- Fix: Use DO_SET_SERVO for precise one-shot actuation. Set the relevant SERVOn_FUNCTION to 0 so ArduPilot does not override the mission-controlled output. Ensure the DO command is placed between two navigation waypoints — DO commands execute between waypoints only.
- Prevention: Bench-test the full mission file with props off before every new mission load

Partial dispense (a second treat hangs at the exit but does not fall)

- Observed: during the 500-cycle endurance bench test.
- Root cause: the servo tester used for the endurance test drove the servo with a sudden (step) PWM change, without ramping. The abrupt actuation caused the divider to overshoot past the magnetic detent, which pushed the next compartment's treat partially through the exit. The second treat hung at the exit rather than falling — it was released on the next commanded actuation. This was non-catastrophic: no treat dropped at an unintended time, and the mechanism continued functioning.
- Implication: the failure mode is specific to the test rig's servo drive, not to the mechanism geometry. In flight, the servo is driven by ArduPilot's PWM output via DO_SET_SERVO, which may or may not reproduce the same abrupt motion profile. If the servo signal ramps (or if the servo's internal controller smooths the step), this failure mode may not appear in deployment.
- Fix: use a servo tester or driver that ramps the PWM signal rather than stepping it. For flight, verify the observed servo motion profile when triggered by DO_SET_SERVO — if it is abrupt, consider driving the servo through an intermediate waypoint or adjusting the servo's own control loop.
- Prevention: characterize the servo's angle-vs-time response under the actual flight-configured PWM command before deploying, not just its angle-vs-PWM static response

Mechanism jam

- Likely cause: treat lodged between divider and baseplate at the exit opening; debris in the gear ring; pawl deformed
- Fix (ground only): disassemble the stackable mechanism (no tools required), inspect the exit opening and gear ring, clear obstruction, reassemble
- Prevention: reject treats with sharp edges or volume exceeding the compartment capacity (202 cm³ per the V2 carousel in the FDR); inspect the gear ring every 100 cycles

Positional drift / divider misaligned with the exit

- Likely cause: one or more magnets dislodged from their pockets; a magnet embedded with flipped polarity during the paused-print step
- Fix: remove the divider, verify all 18 magnets are seated with correct alternating-attraction polarity between divider and baseplate, re-print the divider if the pockets are damaged
- Prevention: when pausing the print to embed magnets, verify polarity against a reference magnet before pressing each one into its pocket

Pawl deformation or wear

- Likely cause: repeated stress cycles exceeding the ASA-CF fatigue limit, or over-rotation driving the pawl past intended engagement
- Fix: replace the pawl (press-fit, no tools)
- Prevention: the FDR endurance test showed no visible pawl wear at 500 cycles. If wear appears earlier in field use, investigate servo overshoot or increased resistance in the gear ring.

Gear ring cracking or tooth damage

- Likely cause: TPU fatigue; cold environmental exposure reducing elasticity; contamination with abrasive debris
- Fix: replace the gear ring (snap-fit, no tools)
- Prevention: print the gear ring in TPU only — rigid filaments will not sustain the designed elastic deflection. Inspect teeth visually every 100 cycles.

# Flight Controller / Autopilot

DO_SET_SERVO command present in mission but does not fire

- Likely cause: (1) DO command placed before the first waypoint; (2) SERVOn_FUNCTION for the target output is not 0 (Disabled); (3) wrong output channel specified in the DO command
- Fix: restructure the mission so DO_SET_SERVO appears between two navigation waypoints; set SERVOn_FUNCTION = 0 for the target output; verify that the output channel corresponds to the physically wired AUX pin (AUX 1 = SERVO 9, AUX 2 = SERVO 10, ..., AUX 6 = SERVO 14)
- Prevention: test mission files in SITL or on the bench before field deployment; include a pre-flight checklist item to verify SERVOn_FUNCTION = 0 for the dispenser output

AUX pin numbering confusion

- Context: the Pixhawk 2.1 Cube Black has only 6 AUX outputs, mapped AUX 1–6 → SERVO 9–SERVO 14. References to "AUX 10" in online guides either mean SERVO 10 (= AUX 2) or refer to a different autopilot (e.g., Pixhawk 6X).
- Fix: use the ArduPilot SERVOn_FUNCTION naming (SERVO9–SERVO14) as the authoritative reference; treat "AUX n" from external sources as ambiguous unless the source explicitly defines it
- Prevention: keep the pin-to-channel mapping diagram in github/docs/ and reference it when wiring

Compass calibration fails or produces high offsets

- Likely cause: ferrous material on the airframe; calibration performed indoors near steel reinforcement; a TreatTurbine magnet placed too close during calibration
- Fix: recalibrate outdoors away from buildings and vehicles. Remove the TreatTurbine before compass calibration.
- Prevention: calibrate without the TreatTurbine installed, then verify behavior with the TreatTurbine attached

# Video System

No video at VRX on startup

- Likely cause: VTX and VRX not paired; mismatched firmware; loose camera-to-VTX ribbon cable
- Fix: run pairing procedure per the Walksnail Quickstart PDF; reseat the camera cable (the FPC connector is easy to dislodge)
- Prevention: pair both units after any firmware update

Video breakup or dropouts in flight

- Likely cause: co-channel interference on 5.8 GHz; multipath from the airframe; antenna damage from a prop strike
- Fix: change channel and re-pair; inspect and replace damaged antennas
- Prevention: scan for 5.8 GHz activity at the deployment site before flying; keep spare antennas on hand

VTX overheating / thermal throttling

- Likely cause: ground testing without airflow over the heatsink; high ambient temperature
- Fix: do not power the VTX for extended periods on the bench without a fan; verify it sits in prop wash during flight
- Prevention: during ground testing, run a desk fan over the VTX heatsink

# Power System

Voltage sag triggering false low-battery failsafe

- Likely cause: BATT_LOW_TIMER too short; LiPo internal resistance high due to age or damage
- Fix: set BATT_LOW_TIMER to \~10 seconds so transient sags during aggressive throttle do not trigger failsafe; retire batteries showing excessive sag
- Prevention: log battery voltage during an aggressive flight profile and inspect for sag behavior before committing to a field deployment

VTX unpowered despite battery connected

- Likely cause: broken or cold solder joint at the VTX power leads on the baseplate; damaged jumper wire
- Fix: inspect and resolder; verify continuity with a multimeter before flight
- Prevention: strain-relieve the VTX power leads; do not route them across the prop plane

# Communication

SiK telemetry shows intermittent link or drops at range

- Likely cause: antenna orientation mismatch between ground and air; 915 MHz interference; low transmit power setting
- Fix: orient ground and air antennas with matching polarization; set SiK transmit power to the maximum within local regulatory limits (27 dBm in the US)
- Prevention: range-test at the planned maximum flight distance before committing to the mission

# Lessons Learned from Testing

(Append entries as field deployment progresses. Initial entries from bench testing and design review:)

- Magnetic detents solve open-loop positioning errors. A pawl-and-gear mechanism alone is subject to servo overshoot and pawl slippage. Passive magnets at the divider-baseplate interface pull the carousel into exact alignment regardless of small servo angle errors. This is the key design insight that made an encoder-free open-loop actuator viable.
- DO_SET_SERVO is the correct mission command for precise one-shot servo control — not DO_CYCLE_SERVO. DO commands execute only between waypoints, and SERVOn_FUNCTION must be set to 0 (Disabled) for mission-controlled outputs.
- ESC LEDs on the DJI 420S are not programmable from ArduPilot. They are controlled by the ESC firmware, and changing them requires discontinued DJI tooling.
- GyroFlow is a post-processing tool, not a live-video tool. The Walksnail Avatar GT is used at Alveus for a low-latency livestream (VTX → VRX → Elgato capture → computer), so GyroFlow is not in the production path. It can be optionally applied to the VTX-recorded SD card footage for cleaner archived content.
- TPU gear ring elasticity is verified across 0–45 °C on the bench. The environmental test showed 100% positional indexing accuracy at 45 °C for 4 hours of cumulative soak. Performance above 45 °C has not been verified — if Texas ambient exceeds this sustained, re-verify gear ring stiffness before flight.
- Mechanism endurance exceeds the 416-cycle design life. 500 bench cycles produced 99.6% reliability with no visible wear on the pawl or gear ring. The two failures were non-catastrophic partial dispenses that resolved on the next actuation.
