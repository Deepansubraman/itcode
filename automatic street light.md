#include <SoftwareSerial.h>

int sensorPin = A0; // select the input pin for the LDR

int sensorValue = 0; // variable to store the value coming from the sensor

int led = 3;

void setup() { // declare the ledPin as an OUTPUT:

pinMode(led, OUTPUT);

Serial.begin(9600); }

void loop()

{

Serial.println("Welcome to TechPonder LDR Tutorial");

sensorValue = analogRead(sensorPin);

Serial.println(sensorValue);

if (sensorValue < 100)

{

Serial.println("LED light on");

digitalWrite(led,HIGH);

delay(1000);

}

digitalWrite(led,LOW);

delay(sensorValue);

}
