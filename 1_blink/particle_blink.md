# Blinking a light

### The Hello World of Electronics

One of the simplest things you can do with electronics is to make an LED blink on and off. 

## Particle Core/Electron

Blinking a light on a Particle Core or Electron could not be any simpler. 

1. Plug in the board to your USB board. 
2. Boot up the [Particle IDE](https://build.particle.io/build/) (see Basics about claiming your Particle board and connecting it to the IDE).
3. Copy this code into a blank sketch
4. Click the lightning bolt (flash) and send the code to the board. 

The code: 

```
int led = D7;

void setup() {
  pinMode(led, OUTPUT);
}

void loop() {
  digitalWrite(led, HIGH);   
  delay(1000);
  digitalWrite(led, LOW);
  delay(5000);
}

```

### Taking it further###

**How would you use an external LED instead of the internal?**

That LED is awfully tiny, and kind of a pale blue. So let's use our own LED and walk through the basics of a circuit while we're at it. 

We need a breadboard, so grab yours. Press the pins of your Particle board into the breadboard -- it doesn't matter where, so long as the board straddles the center channel. To do this, we'll also need a red and black jumper wire and an LED.

![particle-blink-1](../images/particle-blink-1.jpg)


Let's take a look at the LED. It's a very simple device that emits light. You'll notice one wire is longer than the other -- that's the positive, or anode, wire. The shorter one is the negative, or cathode, wire. 

**STEP 1**



