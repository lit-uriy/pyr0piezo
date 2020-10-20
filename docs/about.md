## What is this thing?

TL;DR: It's a litte circuit that turns your actual nozzle into a z-probe.

This is a Z-Probe Sensor for 3D Printers that utilizes a piezoelectric element (commonly a 20-27mm piezo disk used in musical pickups) to detect when the nozzle of the printer touches the bed. There have been other Z-Probes in the past that do this, but this is the first to incorporate an onboard microcontroller that automatically calibrates and balances the circuit. Previous piezo Z-Probes all required manual tuning to some extent, usually with tiny hard-to-use trimpots. Due to the mechanical and electrical characteristics of this type of sensor, it has several advantages over other Z-Probes:

- There are no offsets to configure or adjust
- It can be configured to emulate either a standard endstop, or any active-high Z-Probe
- It's not dependent on a specific type of bed or hotend
- The builtin signal filtering reduces the amount of false driggers dramatically compared to other piezo sensors.
- It's FAST. Delay between the piezo being actuated and the z-trigger signal being sent to the printer's controller is ***just 26 nanoseconds***

### Is there a catch?

Well, yeah a couple. The sensor requires introducing a small amount of physical instability into the construction of the 3D printer. Something needs to move, even a small fraction of a millimeter. This can be something like the bed being pushed down slightly on it's springs, or adding a hinge and a tensioning spring to the print head. There's innumerable ways to make this work, and I've been working hard with my beta testers to find the best possible method combining as much stability as possble with the highest sensitivity. The other catch is that any oozing filament will skew the leveling results, so you have to decide to either assume there will always be ooze and use a small z-offset, or add a nozzle wipe/clean manuver to your startup gcode before bed leveling.

### How do I use it?

I designed the sensor to connect to a 3D Printer's controller like any other endstop or Z-Probe. The sensor also includes an i2c interface that allows the 3D Printer's controller to change parameters on the fly during a print or before a fast move. Marlin support is already incoming, and I'm working on getting integration through Klipper and RepRap.

There's also a sketch included with the github source that can be installed on an Arduino Uno to act as a USB-I2C bridge to set parameters for now until 3d printer controller firmware adopts the new protocol.

The piezo element is mounted somewhere on the 3D Printer in such a way that it undergoes mechanical stress when the nozzle touches the bed. So far there have been three distinct mounting schemes that appear to work well:

- On the extruder assembly
- Under the Print Bed
- In the case of CoreXY or other Gantry Kinematic systems, on either end of the gantry

#### Features:

- Self-calibrating (no more fiddling with tiny potentiometers!)
- Ultra-precise z-height measurements
- Zero offset (The nozzle itself is the sensor!)
- Compatible with ALL surface types
- No plugging in removable sensors for leveling
- 5v or 3.3v signal output
- Can be configured for active high *or* active low signal (endstop vs probe input)
- Tunable over UART / I2C

#### FFC Cable Chain Extra Features:

- FFC Cable chain for a clean connection between print head and controller
- Onboard switchable DC buck converter for 12v or 5v fan operation
- LED Feedback on all PWM components
 Standalone version available for drop-in installation

Credit must be given to precisionpiezo.co.uk for getting me started on this project and giving me a place to start. I did build a version of the FFC cable chain based on their electrical designs but found the calibration of the circuit to be very fiddly, as the range of value on the potentiometers that was acceptable was very narrow.

I've since started from scratch using my own BOM and designs, while including an onboard microcontroller to handle auto-calibration. Given that every 3D printer is different, I wanted this sensor to be as easy and stable as other sensors available on the market, but with the increased performance of a piezo sensor.