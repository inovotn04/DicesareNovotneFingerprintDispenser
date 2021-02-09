# DicesareNovotneFingerprintDispenser
```C++

#include <Servo.h>


#include <Adafruit_Fingerprint.h> //Adding fingerprint sensor libraries
#include <SoftwareSerial.h>

Servo myServo;

SoftwareSerial mySerial(2, 3); // Do I know what's happening here? No. Does it work? Yes.

const int servopin = 9;


Adafruit_Fingerprint finger = Adafruit_Fingerprint(&mySerial);
int fingerprintID = 0;
String IDname; // A bunch of initialization for fingerprint sensor which I only understand very vaguely but a fingerprint sensor is a complicated thing so I don't blame myself
// for using the wonderful feature of ctrl v + ctrl c

void setup() {
    Serial.begin(9600); // Starting the serial monitor, we all know this
    finger.begin(57600); // Starting the fingerprint sensor, don't... ask me why it runs at this speed because I don't know
    
   
    
    if (finger.verifyPassword()) {
      Serial.println("Found fingerprint sensor!");
  } else {                                                        // Checking to see if the fingerprint sensor is there :smile:
      Serial.println("Did not find fingerprint sensor :(");
     while (1) { delay(1); }
  }
}

void loop() {
    fingerprintID = getFingerprintIDez(); // Checks fingerprint to see if correct
    delay(50);
    if(fingerprintID == 1 || fingerprintID == 2){
      Serial.println("wow what a nice finger, so tasty");
      myServo.write(150);
      delay(500);            // does a nice old turn if it's correct
      myServo.write(90);
  }
}
    int getFingerprintIDez() {
   uint8_t p = finger.getImage();
    if (p != FINGERPRINT_OK)  return -1;
  
   p = finger.image2Tz();
   if (p != FINGERPRINT_OK)  return -1;       //gets the fingerprint ID, once again not sure the exact details as to how it works but it's uh yeah it works 

   p = finger.fingerFastSearch();
   if (p != FINGERPRINT_OK)  return -1;
}
```
