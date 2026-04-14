
# 02 - Manufacturing & Assembly Instructions

## What you can find here

- Printing instructions for all TreatTurbine components
- Magnet embedding guidance for the baseplate and divider
- Post-processing steps for the servo pawl
- Full assembly instructions and wire routing guidance

Note: All CAD files are accessible via the provided Onshape links. The instructions below reflect the methods used by Team 223. Adjustments may be needed depending on your specific printer model and drone platform.

##

## CAD Model Modifications

| Part | Modification Required |
|------|----------------------|
| Baseplate | None |
| Divider | None |
| Servo Pawl | None |
| Gear Ring | None |
| Mounting Plate | Top hole pattern must be adjusted to match your specific drone model |

##

## Printing Instructions

### Baseplate

> ⚠️ **Printer Compatibility Warning:** This part requires a mid-print pause for magnet embedding. Confirm that your printer supports mid-print pausing with positional recalibration before attempting this print. Not all printers support this feature.

1. Begin printing the baseplate
2. The print will pause automatically at a predefined layer to expose the magnet holes
3. Insert magnets into **every other hole** to distribute magnetic forces evenly
4. **All magnets must be oriented in the same magnetic direction** — verify by checking which sides attract and which repel before embedding
5. Once all magnets are correctly placed, resume the print to completion

> ⚠️ **Critical:** Incorrect magnet orientation will prevent the baseplate from locking properly with the divider.

##

### Divider

1. The divider can be printed in a single, uninterrupted process
2. After printing, insert magnets into **every other hole**, following the same alternating pattern used in the baseplate
3. Orient the divider magnets such that they **attract** to the corresponding magnets in the baseplate
4. Because these magnets are not encapsulated by printed material, **tape over each magnet** to prevent them from falling out during handling and assembly

##

### Servo Pawl

1. Print the servo pawl normally
2. After printing, locate the **original attachment arm** included with the SG90 servo motor *(see Figure X below)*
3. Cut the attachment arm so that only the **top circular snap-fit section** remains — the cut does not need to be precise, the goal is simply to remove most of the arm length
4. Press-fit the trimmed circular piece into the servo pawl with the **servo-snap side facing down** — this allows the servo pawl to snap directly onto the SG90 motor

> *Figure X — SG90 Servo Motor Attachment Arm*
> `[Insert Figure X here]`

##

### Gear Ring

- Print using **TPU filament only** — this material's flexibility is required for proper interfacing with the servo pawl
- No post-processing steps required

##

## Assembly Instructions

1. Stack all components in the order shown in *Figure XX* below

> *Figure XX — Full Assembly Stack Order*
> `[Insert Figure XX here]`

2. Route the servo motor wires through the **holes in the baseplate on the side opposite the servo pawl** *(see Figure XXX below)* — this allows the servo motor's power and signal wires to reach the drone platform above
3. Mount the servo motor to the baseplate using **two M2.5 screws**

> *Figure XXX — Wire Routing Diagram*
> `[Insert Figure XXX here]`

##

## Checklist

- [ ] Confirm printer supports mid-print pause with positional recalibration before printing the baseplate
- [ ] Verify magnet polarity before embedding in both the baseplate and divider
- [ ] Tape over magnets in the divider to secure them during assembly
- [ ] Adjust the mounting plate hole pattern to match your specific drone model
- [ ] Gear ring must be printed in TPU — no substitutions
- [ ] Servo motor wires must be routed through the correct baseplate holes before final assembly
