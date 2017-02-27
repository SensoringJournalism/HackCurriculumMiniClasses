# Detecting motion

### Sensing something visible

Journalism is about people, right? So let's figure out a way to detect a person in the room and then do something. 

## Arduino

**STEP 1** 



##STRETCH GOAL: Combine two sensors##

What if we just wanted to know what the temperature is when someone walks in the room? We can combine our temperature sensor and our PIR sensor. And we can merge the code. 


```
#include <dht.h>

dht DHT;


#define DHT22_PIN 5

int inputPin = 2;               // choose the input pin (for PIR sensor)
int pirState = LOW;             // we start, assuming no motion detected
int val = 0;                    // variable for reading the pin status
 
void setup()
{
    Serial.begin(9600);
    Serial.println("DHT TEST PROGRAM ");
    Serial.print("LIBRARY VERSION: ");
    Serial.println(DHT_LIB_VERSION);
    Serial.println();
    Serial.println("Type,\tstatus,\tHumidity (%),\tTemperature (C)");
    pinMode(inputPin, INPUT);     // declare sensor as input

}

void loop()
{
    val = digitalRead(inputPin);  // read input value
    if (val == HIGH) {            // check if the input is HIGH
    Serial.println("Motion detected!");

  // READ DATA
    Serial.print("DHT22, \t");
    int chk = DHT.read22(DHT22_PIN);
    switch (chk)
    {
        case DHTLIB_OK: 
            Serial.print("OK,\t"); 
            break;
        case DHTLIB_ERROR_CHECKSUM: 
            Serial.print("Checksum error,\t"); 
            break;
        case DHTLIB_ERROR_TIMEOUT: 
            Serial.print("Time out error,\t"); 
            break;
        default: 
            Serial.print("Unknown error,\t"); 
            break;
    }
    // DISPLAY DATA
    Serial.print(DHT.humidity, 1);
    Serial.print(",\t");
    Serial.println(DHT.temperature, 1);

    delay(5000);
    if (pirState == LOW) {
      // we have just turned on
      // We only want to print on the output change, not state
      pirState = HIGH;
    }
  } else {
    if (pirState == HIGH){
      // we have just turned of
      Serial.println("Motion ended!");
      // We only want to print on the output change, not state
      pirState = LOW;
    }
  }
}
```