This is a code I am trying to write/compile in order to realize the concept that my group in my master studies, has created. We want to hit a pinata where a ADXL345 Accelerometer is placed, and this signal to display randomized images in Processing that contain hate comments and cyber bullying. It is a concept for an exhibition that we will be part of and will take place next week on wednesday.
This project includes an ADXL345 sensor (accelerometer), Arduino Uno R3 Board, Arduino IDE 2.0.3 and Processing 4.1.2.
I want Processing to display images randomly and continuously every time the values of the sensor that are received from the serial communication with the Arduino sketch, go x>5, x<-5, y.5, y.-5, z>1, z<-1. I don't understand where the problem is with my codes. The baud rate is the same. The images are contained in the same folder as the processing sketch. The Arduino sensortest's serial monitor is revealing the values changing as I move the sensor, so it means that the wiring is correct.

Can you help me correct the code?
These are the codes that I am using in Arudino and Processing.

**ARDUINO**
`
void setup() {
  // initialize serial communication at 9600 baud rate
  Serial.begin(9600);
}

void loop() {
  // send x, y, and z values over serial
  int x = analogRead(A0);
  int y = analogRead(A1);
  int z = digitalRead(2);
  Serial.println(x);
  Serial.println(",");
  Serial.println(y);
  Serial.println(",");
  Serial.println(z);
  delay(1000);
}
`
**& PROCESSING**
`
import processing.serial.*;
Serial mySerial;
PImage fragment;
int rand;

void setup() {
  size(1000, 500);
  rand = int(random(0,133)); 
  takerandomimage("frag_" + nf(rand, 3) + ".jpg"); 
  String portName = Serial.list()[0];
  mySerial = new Serial(this, portName, 9600);
}

void takerandomimage(String fn) {
   fragment = loadImage(fn); 
}

void draw() {
  background(255); //clears the screen
  if (fragment.width>0){ //check if image has been loaded
    String data = mySerial.readStringUntil('\n');
    if (data != null) {
      String[] values = split(data, ",");
      if (values.length == 3) {
        int x = int(values[0]);
        int y = int(values[1]);
        int z = int(values[2]);
        if (x > 5 || y > 5 || z > 0) {
          image(fragment, 0, 0);
        }
      }
    }
  }
}
`
