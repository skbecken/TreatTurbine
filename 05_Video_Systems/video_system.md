# System Overview

The video system is essential for broadcasting treat delivery to the wolfdogs! Our team mounted an Avatar GT camera system (VTX, camera, and antennas) to the S550 frame using the provided camera mount. On the camera mount we attached a 3D printed TPU part to fix the camera in place and allow quick changes to orientation (landscape or portrait) and pitch angle. The transmitter and receiver operate on the 5.8 GHz band. Their placement relative to the other RF components on the airframe is described in the Installation section below.

# Components

Purchased: Avatar GT kit components (Video Transmitter - VTX, Camera, Antennas, Video Receiver - VRX) and VRX Power Supply

- VTX, Camera and Antenna Combo: https://www.racedayquads.com/products/walksnail-avatar-gt-kit-20x20-25x25-vtx-w-starlight-hd-pro-camera?aff=2
- VRX: https://www.racedayquads.com/products/walksnail-avatar-vrx-combo-choose-version?keyword=walksnail vrc

3D Printed: Camera Cage, TPU Camera Mount

- Camera Cage Onshape
- TPU Camera Mount Onshape

# Configuration

VTX:  
Instructions on how to update the VTX firmware can be found in the pdf for avatar gt (link to pdf in github).

VRX:  
The pairing procedure with VTX can be found the the Quickstart PDF (link on github). The VRX

- HDMI output setup
- Instructions on how to pair the VTX and VRX can be found here: https://www.caddxfpv.com/pages/download-center (add pdf for avatar gt stuff to github directly)

# Installation

The camera is screwed into the camera cage, which allows for easy reorientation. The cage lines up with the mounting holes on the side of the camera. To change the camera angle once it's in the mount, screws must be able to enter from all four sides of the camera — but the camera itself only has mounting holes on two sides. To work around this, we embedded nuts into the other two sides of the cage during 3D printing (pausing the print to place the nuts). This adds effective mounting points on all four sides and makes orientation changes easy. The TPU camera mount has one hole for fixed mounting plus a slider for angling the camera. This part attaches to the underside of the camera mounting plate. We chose TPU with the intent that it would dampen in-flight vibration for cleaner video. 

(ADD pictures of Camera cage, Camera mount)

The VTX is mounted on the camera mounting bracket provided in the S550 drone kit. We adjusted the bracket so the VTX sits underneath two of the propellers to maximize airflow over the VTX heatsink. The antennas are zip tied to opposite arms to prevent contact with the propellers.

(Add photos of vtx and antennas, DIAGRAM SHOWING SIGNAL FROM OTHER RF DEVICES)

RF separation:

- 5.8 GHz VTX antennas are positioned at least (DISTANCE) from the 2.4 GHz RC receiver
- 915 MHz SiK telemetry radio antenna is mounted at (LOCATION)
- GPS module is mounted on a stalk above the airframe to separate it from the VTX RF field and from current-carrying traces (relevant to the internal compass as well as RF isolation)

The VTX has an input voltage range of 11.1V - 25.2 V. With the voltage range we were able to power the VTX by soldering jumper cables to the terminals used to power the motor ESCs on the S550 baseplate. (Add photo of this soldering job!).

(WIRING DIAGRAM HERE)
