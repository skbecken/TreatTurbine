## Quickstart

1. Start by installing the latest version of QGroundControl from <https://qgroundcontrol.com>
2. Enter the settings by pressing on the Q in the top left then Application Settings > Maps and change to Google Maps (we have found they provide the most up to date satellite imagery of the providers)
3. Connect your computer via either SiK radio or USB cable to your flight controller 
4. Wait for the purple “Disconnected - Click to manually connect” message to change
   1. If you’re on MacOS it should work right out of the box. If on Windows, you may need to install a serial driver to see a connection 
5. Congrats, you are now ready to configure parameters via Q > Vehicle Configuration or to create a flight plan with Q > Plan flight
6. If you are setting up an aircraft for the first time, make sure you flash firmware (as outlined in 03 Ardupilot setup) and your parameter list
![[intro-to-Q.png]]
## What is QGroundControl?

QGroundControl (QGC) is one of several open source [ground control station](https://ardupilot.org/plane/docs/common-choosing-a-ground-station.html) (GCS) software packages that works with Ardupilot. Each of these GCS programs communicates over the MavLink protocol (which can also be used with Python and other programmatic methods) if more custom behavior is required. 
While Ardupilot runs everything onboard the aircraft, the GCS sends and receives information. It is important to note that **no information is stored in the GCS**. This means that once a drone is set up, all of the configuration (parameters and flight plans) are stored onboard the craft unless saved out to a .plan or .param file. These files **must be backed up** so that a crash or lost aircraft does not include loss of data. It also means that the data in QGroundControl is not important, and the pilot is free to move between different computers with QGC installed. 

## Why QGroundControl?

Our team chose QGroundControl because it works across MacOS, Linux, Windows, Android, and (allegedly, though we never got it working) iOS.

In our very brief experience, it felt polished and easy to use (at least as compared to Mission Planner). It is also under active development; over the course of our work, multiple significant bugs were resolved. If you’re following our documentation, we recommend QGroundControl as well because it will be better documented and supported by our team. 