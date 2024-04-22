#define sensorPin  A0 // Flex Sensor is connected to this pin


#define PWMPin  6 // LED is attached to this Pin


float VCC = 5; // Arduino is powered with 5V VCC


float R2 = 10000; // 10K resistor is


float sensorMinResistance = 16700; // Value of the Sensor when its flat


float sensorMaxResistance = 18200; // Value of the Sensor when its bent at 90*


void setup() {


  Serial.begin(9600); // Initialize the serial with 9600 baud


  pinMode(sensorPin, INPUT); // Sensor pin as input


}


void loop() {


  int ADCRaw = analogRead(sensorPin);


  float ADCVoltage = (ADCRaw * VCC) / 1023; // get the voltage e.g (512 * 5) / 1023 = 2.5V


  float Resistance = R2 * (VCC / ADCVoltage - 1); // Calculate Resistance Value


  uint8_t ReadValue = map(Resistance, sensorMinResistance, sensorMaxResistance, 0, 255); // map the values 16700 to 0  18200 to 255


  analogWrite(PWMPin, ReadValue); // Generate PWM Signal


  // Print Debug Information


  Serial.print(Resistance);


  Serial.print("  ");


  Serial.println(ReadValue);


  delay(100);


}

