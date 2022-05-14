# IR_Remote_Control

This was done for the ECE 387 Midterm chip analysis. This includes interfacing with the IR Range sensor.
This repo will include the library that needs to be added, C code along with the Arduino sketch as well as the video of the skematic working.

![image](https://user-images.githubusercontent.com/92557357/164129747-47c8e864-e0c1-4527-aa56-03834894b939.png)

![image](https://user-images.githubusercontent.com/92557357/164129774-00589031-a636-4ab5-9f51-88805c83de9c.png)

# How the sensor works

The way this sensor works is based on the principle of the reflection of light. As seen in the above diagrams, the sensor has a transmitter and a receiver. It sends an IR signal and after it reflects from the surface, the receiver collects the signal and depedning on how far away the object is, the more or less of the signal is received by the receiver. This is then converted to an analog electric signal of a voltage that ranges from 0 to 5V. Using a linear electricity to distance equation (the max range is 80cm which returns close to 0V) we can calculate the distance an object is away from the sensor. The code is an infinite loop which returns the distance calculated.

# How the code works

```
#include <SharpIR.h>
#define IRPin A0
#define model 1080
int distance_cm;
```

We include the SharpIR header file which contains the various functions that we can use. This is provided by the maker of the sensor, SHARP. We then set up the IR sensor according to the diagram you see below. It is set to the A0 port which returns an analog signal depending on the distance. Since the library is for a large variety of devices, we set the model to the specific device we end up using. We declare an integer which is used to store the distance. This distance is calculated using an average of 25 samples.

```
SharpIR mySensor = SharpIR(IRPin, model);

void setup() {
  Serial.begin(9600);
}
```

We create an object of the SharpIR class so that we can use the functions that entail that library. For our purposes it will be getting the distance from the user. This distance is not instantaneous and an average of 25 samples along with the inital start up of the sensor which takes 25 to 30 ms. We also set up the Serial monitor since this is where we will be the looking at the distance of an object. 

```
void loop() {
  distance_cm = mySensor.distance();
  Serial.print("Average Distance with 25 samples: ");
  Serial.print(distance_cm);
  Serial.println(" centimeters");
  delay(500);
}
```
The loop that runs infinitely is used to get the distance using the distance method. We print this distance and proceed to wait for half a second so as not to flood the serial monitor.

# Hardware needed

* Arduino
* Breadboard
* 10 meu-F or higher electrolytic capacitor
* wires

# Setup
![image](https://user-images.githubusercontent.com/92557357/164130168-2c5f0d53-a882-4ae4-a396-1ba7cbdb6d49.png)
After this we upload the given code after having added the library and then test out the sensor

# YouTube Link
https://youtu.be/_KAkkrMKvxA
