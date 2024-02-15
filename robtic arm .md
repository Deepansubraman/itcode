//Arduino Robotic Arm Control Spftware
//Robotocs & Energy
//March, 2020
#include <Servo.h>
#include <string.h>
Servo servo_0,servo_1,servo_2,servo_3,servo_4,servo_5;  // create servo objects to control a servo
String received = "";
String servo[6] = {"000", "000", "000", "000", "000", "000"};
int pos[6] = { 90, 120, 90, 120, 120, 0 };
int dump;
char c;
void dataHandle(String line)
{
  int servoNum;
  int lineIndex = 0;
  int valueIndex = 0;
  for(servoNum = 0; servoNum < 6; servoNum++)
  {
    while(line[lineIndex] != ' ' && line[lineIndex] != 'e')     
     {
      servo[servoNum][valueIndex] = line[lineIndex];
      valueIndex++;
      lineIndex++;
     }
     lineIndex++;
     pos[servoNum] = (servo[servoNum].toInt() / pow(10, 3 - valueIndex));
     valueIndex = 0;
  }
}
void armPosUpdate()
{
  servo_0.write(((float)180/270)*pos[0]);
  servo_1.write(((float)180/270)*pos[1]);
  servo_2.write(((float)180/270)*pos[2]);
  servo_3.write(((float)180/270)*pos[3]);
  servo_4.write(((float)180/270)*pos[4]);
  servo_5.write(((float)180/270)*pos[5]);
}
String readServoPositions()
{
  int i;
  String poses = "";
  pos[0] = ((float)servo_0.read()*1.5); //270/180 = 1.5
  pos[1] = ((float)servo_1.read()*1.5);
  pos[2] = ((float)servo_2.read()*1.5);
  pos[3] = ((float)servo_3.read()*1.5);
  pos[4] = ((float)servo_4.read()*1.5);
  pos[5] = ((float)servo_5.read()*1.5);
  for(i = 0; i<6; i++)
  {
    poses += String(String(pos[i]) + " ");
  }
  return poses;
}
void servoStatus()
{
  int i;
  for (i = 0; i < 6; i++) 
  {
    Serial.println(String("Servo " + String(i) + ": " + String(pos[i]) + '\n'));
    delay(10);
  }
}
void setup() 
{
  pinMode(13, OUTPUT);
  digitalWrite(13, LOW);
  servo_0.attach(3);
  servo_1.attach(5);
  servo_2.attach(6); 
  servo_3.attach(9);
  servo_4.attach(10);
  servo_5.attach(11);
  armPosUpdate();
  Serial.begin(115200); 
  Serial.write("start");
}
void loop()
 {     
      if(Serial.available()>0)
       {
          delay(20);
          do
          {
            c = Serial.read();
            if (c == 'u') 
            {
              Serial.println(String("Update: " + readServoPositions()));
              delay(10);
              break;
            }
            else received += c;
          }
          while (c != 'e');
          while(Serial.available()>0) dump = Serial.read(); // dump buffer
          if(c != 'u' && received != "")
          {
            Serial.println(String("Received: " + received));
            dataHandle(received);     
            armPosUpdate();
            received = "";
          }
       }      
}
