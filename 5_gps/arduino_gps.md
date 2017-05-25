# Detecting location

### The GPS system is a modern marvel: Let's use it.

One of the key pieces of data we can have for any physical phenomenon is where did it happen. 

## Arduino

**STEP 1**

Gather your materials. You will need your Arduino, the Ulimate GPS Breakout, a u.FL connector and antenna and four jumper wires: red and black for power, orange and yellow for data.

**NOTE**: GPS signals are weak. If you're doing this in an interior room, you're unlikely to get a signal. Putting your antenna near a window or taking your project outside may be the only way get data on this one.   

**STEP 2** 

Wiring is relatively simple. There's only four wires, two of them are power. 

* Connect a red jumper wire from the VIN pin on the GPS breakout to the 5V pin on the Arduino. 
* Connect a black jumper wire from the GND pin to a GND pin on the Arduino. 
* Connect an orange jumper from the TX pin on the GPS breakout to  Digital Pin 3 (D3) on the Arduino.
* Connect a yellow jumper from the RX pin on the GPS breakout to Digital Pin 2 (D2) on the Arduino.

![arduino-gps-1](../images/arduino-gps-1.jpg)

![arduino-gps-2](../images/arduino-gps-2.jpg)
  
**STEP 3** 

To run the code, you need to add a library to the available libraries you have in your IDE. [You can get Adafruit's GPS library here](https://github.com/adafruit/Adafruit_GPS). Click the Clone or Download button, download the file, expand it, rename it to Adafruit_GPS and move it to your Arduino/libraries folder. You will have to restart your IDE to see the library appear. 

**STEP 4** 

Up to this point, we've been dealing with sensors with a minimum of data coming at you. It might be coming every few miliseconds, but it's just one number. The GPS breakout sends a LOT of data. Don't believe me? Let's run one of Adafruit's examples and see. 

In your Arduino IDE, go to File > Examples > Adafruit_GPS > parsing

The code is *extensively* commented, so we won't go over it here. But there's two things you have to know in order to see things working correctly. 

First, if you don't have the baud rate set correctly, you'll get gibberish, like this: 

![arduino-gps-3](../images/arduino-gps-3.png)

You set the baud rate on the bottom right. Set it to 115200 baud. 

Second issue, is if you aren't getting satellites -- like if you are inside -- then it will look like this:

![arduino-gps-4](../images/arduino-gps-4.png)

The giveaway is the line that says Fix: 0 quality 0. That means you have no satellite fixes, so the quality of the fix is zero. 

If you do venture outside, and you've got your antenna set up correctly, you'll see something like this:

![arduino-gps-5](../images/arduino-gps-5.png)


### Why did this work? 

The wiring in this case is simple -- you're using TX to send data TO the sensor and RX to receive data FROM the sensor. What isn't so simple is the code. The GPS chip receives a LOT of data, so the code written by Adafruit is pretty verbose. It does a lot of stuff we don't really need it to do -- it's an example designed to give you an idea of what all the library and the sensor can do. So what if we wanted to just cut it down to the minimum -- what if we just wanted to see the date, the  latitude and longitude in decimal degress? How do we do that? 


## STRETCH GOAL:

When it comes to combining other people's code and other people's ideas in code, there's a couple of things you need to think about. First -- do you have all the right libraries imported? Second -- is the code you are combining reading the right pins? Third -- if you're deleting things, are you deleting things that are getting referenced later? Did you delete something you need?

So to thin things and make it more readable, we're going to combine ideas in two different examples from Adafruit: the `GPS_HardwareSerial_Parsing` example and the `parsing` example we used before. Part of the verbosity -- and difference from what we've been doing -- in the parsing example is that it uses interrupt-based code instead of the loop based code we've been using all along. 

So we're going to move all of the GPS reading stuff into the loop and we're going to simplify the output. 

First, we need to set things up: Import libraries, set up the pins, tell the library what we're reading and setting the GPSECHO to false.

```
#include <Adafruit_GPS.h>
#include <SoftwareSerial.h>

SoftwareSerial mySerial(3, 2);

Adafruit_GPS GPS(&mySerial);
#define GPSECHO false

uint32_t timer = millis();

```
Now the setup, which is very familiar. We set the serial read at 115200 and the GPS read at 9600. The rest are commands we need to get the GPS set up and running. 

```
void setup()
{
  Serial.begin(115200);
  GPS.begin(9600);
  GPS.sendCommand(PMTK_SET_NMEA_OUTPUT_RMCGGA);
  GPS.sendCommand(PMTK_SET_NMEA_UPDATE_1HZ); // 1 Hz update rate
  GPS.sendCommand(PGCMD_ANTENNA);
  delay(1000);
}

```

Now lets loop what we need. Most of this comes from the `GPS_HardwareSerial_Parsing` example, with comments and a few Serial.prints removed. I've narrowed it down to just the data we want: The date and time, the location in decimal degrees and the number of satellites, which is a proxy for the quality of the location. One major differences is I've extended the reading time to 10 seconds instead of two. 

```
void loop() 
{
  char c = GPS.read();
  if (GPSECHO)
    if (c) Serial.print(c);
  if (GPS.newNMEAreceived()) {
    if (!GPS.parse(GPS.lastNMEA())) 
      return; 
  }

  if (timer > millis()) timer = millis();
     
  if (millis() - timer > 10000) {
    timer = millis(); // reset the timer
    Serial.print("\nTime: ");
    Serial.print(GPS.hour, DEC); Serial.print(':');
    Serial.print(GPS.minute, DEC); Serial.print(':');
    Serial.print(GPS.seconds, DEC); Serial.print('.');
    Serial.println(GPS.milliseconds);
    Serial.print("Date: ");
    Serial.print(GPS.day, DEC); Serial.print('/');
    Serial.print(GPS.month, DEC); Serial.print("/20");
    Serial.println(GPS.year, DEC);
    if (GPS.fix) {
      Serial.print("Location: ");
      Serial.print(GPS.latitudeDegrees, 6); Serial.print(GPS.lat);
      Serial.print(", ");
      Serial.print(GPS.longitudeDegrees, 6); Serial.println(GPS.lon);
      Serial.print("Satellites: "); Serial.println((int)GPS.satellites);
    }
  }
} 

```

Now, with GPS reading in the loop, we can combine this code with other code we've written to read a sensor and, as long as they aren't using the same pins, they'll behave similarly. 