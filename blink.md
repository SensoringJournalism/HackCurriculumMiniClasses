# Blinking a light

### The Hello World of Electronics

One of the simplest things you can do with electronics is to blink an LED. 


## Arduino

Blinking a light on a Arduino could not be any simpler. You just plug in the board and upload this code to the board. 

```
*/
  Blink
  Turns on an LED on for one second, then off for one second, repeatedly.
 
  This example code is in the public domain.
 */
 
// Pin 13 has an LED connected on most Arduino boards.
// give it a name:

int led = 13;

// the setup routine runs once when you press reset:

void setup() {                
  // initialize the digital pin as an output.
  pinMode(led, OUTPUT);
}

// the loop routine runs over and over again forever:
void loop() {
  digitalWrite(led, HIGH);   // turn the LED on (HIGH is the voltage level)
  delay(1000);               // wait for a second
  digitalWrite(led, LOW);    // turn the LED off by making the voltage LOW
  delay(1000);               // wait for a second   
             // wait for a second   
}

```

### Taking it further###

How would you use an external LED instead of the internal?

### Stretch goal###

How could you make multiple LEDs blink at the same time?

## Spark Core/Electron

## Raspberry Pi