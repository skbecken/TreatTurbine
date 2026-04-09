## Electrical Connection

Wiring the servo is easier done than said. For our system, we connected the power and ground the radio receiver and the signal to the AUX2 PWM pin on the Pixhawk. This is an extremely non standard setup and for good reason. 

The Pixhawk does not supply 5V power on its auxiliary pins in order to prevent a rogue servo from drawing too much power and browning out the entire flight controller. By pulling power from the receiver which, in turn, pulls power from the Pixhawk, we have circumvented this intentional safety decision. This decision was not taken lightly, but was deemed acceptable based on the expected maximum power draw of the servo.

*Note: The AUX 2 plug corresponds to AUX10/SERVO10 in the Ardupilot software. This is because Servos 1-8 correspond to the motor outputs*


## Ardupilot Configuration

There are two primary modes that I operate the servo in:

1. RC_PASSTHRU sets the servo position directly proportional to the switch, slider, or stick input on the radio. This is useful for troubleshooting and manual control. If SERVO10 is set to RC_PASSTHRU then channel 10 on your radio will control the servo.
2. Disabled is the default state. In this state the servo can be controlled by the SET_SERVO mission waypoint. This is how the aircraft should be set up during autonomous missions. 
