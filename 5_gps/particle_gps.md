# Detecting location

### The GPS system is a modern marvel: Let's use it.

One of the key pieces of data we can have for any physical phenomenon is where did it happen. 

## Particle

**STEP 1**

Gather your materials. You will need your Particle, the Ulimate GPS Breakout, a u.FL connector and antenna and four jumper wires: red and black for power, orange and yellow for data.

**NOTE**: GPS signals are weak. If you're doing this in an interior room, you're unlikely to get a signal. Putting your antenna near a window or taking your project outside may be the only way get data on this one.   

**STEP 2** 

Wiring is relatively simple. There's only four wires, two of them are power. 

* Connect a red jumper wire from the VIN pin on the GPS breakout to the VIN pin on the Particle. 
* Connect a black jumper wire from the GND pin to a GND pin on the Particle. 
* Connect an orange jumper from the TX pin on the GPS breakout to the RX pin on the Particle.
* Connect a yellow jumper from the RX pin on the GPS breakout to the TX pin on the Particle.

![arduino-gps-1](../images/arduino-gps-1.jpg)

![arduino-gps-2](../images/arduino-gps-2.jpg)
  
**STEP 3** 


**STEP 4** 



### Why did this work? 

The wiring in this case is simple -- you're using TX to send data TO the sensor and RX to receive data FROM the sensor. What isn't so simple is the code. The GPS chip receives a LOT of data, so the code written by Adafruit is pretty verbose. It does a lot of stuff we don't really need it to do -- it's an example designed to give you an idea of what all the library and the sensor can do. So what if we wanted to just cut it down to the minimum -- what if we just wanted to see the date, the  latitude and longitude in decimal degress? How do we do that? 


## STRETCH GOAL:
