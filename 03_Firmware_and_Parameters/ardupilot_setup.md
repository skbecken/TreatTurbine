## What does Ardupilot do?
Ardupilot is the operating system running on the flight controller. It is responsible for all of the onboard decision-making that takes place on the drone. Ardupilot is the reason the drone can continue to function even **without** a connection to the ground station or radio controller and configuration is incredibly important. 

## Flashing your version of Ardupilot
Flashing happens by connecting your flight controller to your ground control station and flashing your desired version of Ardupilot. While the latest version is generally recommended, we used version 4.6.2 and recommend that you follow along. Steps are below:
1. Acquire your firmware file (arducopter-4-6-2.apj is located in this directory), ensuring you have the correct version, **and that it matches your flight controller**. We have the correct version for the Pixhawk 2.4.8
2. Download and set up your ground control station (we recommend QGroundControl)
3. Navigate to Q > Vehicle Configuration > Firmware
4. Connect your flight controller via USB (we **strongly** advise against using a radio connection for this step)
5. Select ChibiOS and your downloaded .apj firmware file
6. Hit flash, and wait for the process to complete before restarting your flight controller

## Customization and setup
An Ardupilot configuration is defined entirely by a parameter list. This details everything from the orientation of the motors to the failsafe behavior. This parameter list is stored in plain text, generally with the file extension .params. Since they store very specific critical information, its vital that your parameter list is matched to both your Ardupilot **version and physical hardware**. 

If you are following along with our exact steps and hardware, you will be able to copy our entire parameter list (stored in this directory) to your flight controller. This is done by connecting your flight controller, navigating to Q > Vehicle Configuration > Parameters > Tools > Upload. If you do this, it is even more important that you follow all of the steps in the before_first_flight checklist. 