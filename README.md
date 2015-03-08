
# ShuttleCP: Jog Dial for ChiliPeppr

This code is meant to allow button, jog, and shuttle events from a Contour ShuttleXpress controller to flow to the Serial Port JSON server typically used with ChiliPeppr driven CNC machines. It was originally developed to run on a RaspberryPi but could probably run on any linux box.

Interface to Shuttle Contour Xpress based on "Contour ShuttlePro v2 interface" by Eric Messick.


## Configuration instructions:

1. Edit shuttlecp.c and update the following items at the top of the file:

```
#define SPJS_HOST     "localhost"        // Hostname where SPJS is running
#define SPJS_PORT     "8989"             // Port for SPJS
#define CYCLE_TIME_MICROSECONDS 100000   // time of each main loop iteration
#define MAX_FEED_RATE 1500.0   // (unit per minute - initially tested with millimeters)
#define OVERSHOOT     1.06     // amount of overshoot for shuttle wheel
```

2. Make sure ChiliPeppr is already connected to the SPJS and opened the 
connection to the board, as this utility sends commands assuming that CP
has already opened the port at the correct baud rate.


## Build instructions:
(written for Raspbian, but should work for others)

1. Ensure openssl dev libraries are installed:

```
 sudo apt-get install libssl-dev build-essential
```

2. Download the nopoll library from here: http://www.aspl.es/nopoll/downloads/ (I'm currently using version 0.2.7.b164 because I got a strange linking error when I tried to compile 0.2.8.b184 for some odd reason.  Feel free to try both).  Extract, configure, make, and install the nopoll library with the usual:

```
 ./configure
 make
 sudo make install
```

3. Take a git clone of the wiringPi utility and use their included build script to build and install the library. Instructions are here: http://wiringpi.com/download-and-install/

4. Go into the ShuttleCP directory and run "make". Make sure you've edited any items from the "Configuration" section above first.

```
 make
```

## Running:

1. First, make sure SPJS is already running and ChiliPeppr has already opened a connection to your machine.  I run SPJS on the same raspberryPi that I run the ShuttleCP utility on.

2. Start up shuttlecp with an argument that is the device interface for your ShuttleXpress.  Mine is /dev/input/by-id/usb-Contour_Design_ShuttleXpress-event-if00 Use sudo so that wiringPi has access to GPIO pins:

```
 sudo ./shuttlecp /dev/input/by-id/usb-Contour_Design_ShuttleXpress-event-if00
```

Or, you can use the "./shuttle" script, which effectively does the same thing.


