# IR_Remote_Control

This was done for the ECE 387 Midterm chip analysis. This includes interfacing with the IR Range sensor.
This repo will include the library that needs to be added, C code along with the Arduino sketch as well as the video of the skematic working.

![image](https://user-images.githubusercontent.com/92557357/164129747-47c8e864-e0c1-4527-aa56-03834894b939.png)

When the position of the object changes, the angle of the reflected beam and the position of the spot on the PSD changes as well. See point A and point B in the image below.

![image](https://user-images.githubusercontent.com/92557357/164129774-00589031-a636-4ab5-9f51-88805c83de9c.png)

# How the sensor works

The way this sensor works is based on the principle of the reflection of light. As seen in the above diagrams, the sensor has a transmitter and a receiver. It sends an IR signal and after it reflects from the surface, the receiver collects the signal and depedning on how far away the object is, the more or less of the signal is received by the receiver. This is then converted to an analog electric signal of a voltage that ranges from 0 to 5V. Using a linear electricity to distance equation (the max range is 80cm which returns close to 0V) we can calculate the distance an object is away from the sensor. The code is an infinite loop which returns the distance calculated. The IR distance sensor uses a beam of infrared light to reflect off an object to measure its distance. The distance is calculated using triangulation of the beam of light. The sensor consists of an IR LED and a light detector or PSD (Position Sensing Device). When the beam of light gets reflected by an object, the reflected beam will reach the light detector and an ‘optical spot’ will form on the PSD.

The sensor has a built-in signal processing circuit. This circuit processes the position of the optical spot on the PSD to determine the position (distance) of the reflective object. It outputs an analog signal which depends on the position of the object in front of the sensor.

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

# How to read an IR sensor

IR distance sensors output an analog signal, which changes depending on the distance between the sensor and an object. From the datasheet, you can see that the output voltage of the SHARP GP2Y0A21YK0F ranges from 2.3 V when an object is 10 cm away to 0.4 V when an object is 80 cm away. The graph also shows why the usable detection range starts at 10 cm. Notice that the output voltage of an object that is 2 cm away is the same as the output voltage for an object that is 28 cm away. The usable detection range, therefore, starts after the peak at roughly 10 cm or 2.3 V.

![image](https://user-images.githubusercontent.com/92557357/168440150-9a701410-2307-45e3-b402-02049413f693.png)

The graph also shows the drawback of these sensors, the response is non-linear. In other words, a big change in the output voltage does not always correspond to a big change in range. In order to determine the distance between the sensor and an object, you need to find a function that converts the output voltage into a range value.

This can be done using MS Excel and results in the following formula for distances > 10cm:

Distance (cm) = 29.988 X POW(Volt , -1.173)

This is the function that is used in the SharpIR library, which we will be using later. Note that this function is based on data from the SHARP datasheet only. The output characteristics of the sensor will vary slightly from sensor to sensor so you might get inaccurate readings.

If you want to improve the accuracy of your readings, you can try to measure and plot many data points in Excel and fit a curve through these points. Once you have a new function for your specific sensor, you will need to change the formula used in the SharpIR.cpp file.

# Specifications

| Feature       | Spec          |
| ------------- | ------------- |
| Op voltage    | 4.5 to 5.5V   |
| Op current    | 30 mA         |
| Range         | 10 to 80 cm   |
| Output        | Analog        |
| Dimensions    | 29x13x13 mm   |


# Hardware needed

* Arduino
* Breadboard
* 10 meu-F or higher electrolytic capacitor
* wires

# Wiring
![image](https://user-images.githubusercontent.com/92557357/164130168-2c5f0d53-a882-4ae4-a396-1ba7cbdb6d49.png)
These type of distance sensors tend to be a bit noisy, so it is recommended to add a capacitor between Vcc and GND. The datasheet suggests a capacitor of 10 µF or more (I used 220 µF). Connect the positive lead of the capacitor to the Vcc wire connection and the negative lead to the GND wire connection (see picture). Capacitors are often marked with a stripe which indicates the negative lead. The positive lead is often longer then the negative lead.

If your sensor comes with different colored wires, be sure to check the pinout below. The Vo pin is connected to the analog in of the Arduino (A0)

![image](https://user-images.githubusercontent.com/92557357/168440551-0d2ec53e-218f-4374-8629-39253bbe37c2.png)


# YouTube Link
https://youtu.be/_KAkkrMKvxA

# Conclusion
This project has been a supremely learning experience. It is amazing sensor that I was able to use to craft this circuit and test it to its limits using regular as well as reflexive paper. Having the oppurtunity to deep dive into a sensor like this was a blast!
