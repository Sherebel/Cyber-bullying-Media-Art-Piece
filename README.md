**ARDUINO**

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


**& PROCESSING**

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
