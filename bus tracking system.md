# itcode
#include <SoftwareSerial.h>
#include <TinyGPS++.h>

TinyGPSPlus gps;

SoftwareSerial gps_serial(2,3); 

double LATITUDE , LONGITUDE;
String latitude="", longitude="";
double SPEED;
String SMS;






void setup() {
  // put your setup code here, to run once:
    Serial.begin(9600);
    gps_serial.begin(9600);

    pinMode(A1,INPUT);
    pinMode(13,OUTPUT);
    digitalWrite(13, LOW);
    Serial.println(F("arduino with neo 6m gps module"));
    

}




String double_string_con(double input)
{
  String storag1 = "";
  int count=0, count2=0;
  String storag2="";
  char dot='.';
  String val_string="";

  
  storag2=(String)input;
  storag1= (String)(input*1000000);
  //Serial.println(b);
  //Serial.println(j);

  for(int i=0; i<6; i++)
    {
     
     if(storag2.charAt(i)==dot) break;
     count++;
     
    }

  for(int i=0; i<15; i++)
    {
     
     if(storag1.charAt(i)==dot) break;
     count2++;
     
    }
  //Serial.println(count2);
  
  for(int i=0; i<count2; i++)
    {
      if(i==count) 
           {
            val_string = val_string + dot ;
           }
           val_string = val_string + storag1.charAt(i);
    
    }
   
   count=0;
   count2=0;
   return val_string;
}




void loop() {
   
   while (gps_serial.available() > 0)  gps.encode(gps_serial.read());

   if (millis() > 5000 && gps.charsProcessed() < 10)
      {
        Serial.println(F("No GPS detected: check wiring."));
        while(true);
      }  
      
           
  
  int reading;
  reading= analogRead(A1);

  if(reading>800)
        {
          digitalWrite(13, HIGH);
      
          if (gps.location.isValid())
              {
                LATITUDE = gps.location.lat(), 6 ;
                latitude = double_string_con(LATITUDE);
                
                
                LONGITUDE = gps.location.lng(), 6 ;
                longitude = double_string_con(LONGITUDE);
          
                SPEED = gps.speed.kmph();

                
          
               
              }
      
          else
          {
            Serial.println(F("INVALID"));
          }
          
      
           SMS = "caution: Tlit sensor is activated !";       // this part create a msg for gsm module
           SMS = SMS + "\n";
           
           SMS = SMS + "Latitude: ";
           SMS = SMS + latitude;
           SMS = SMS + "\n";
           
           SMS = SMS + "Longitude: ";
           SMS = SMS + longitude;
           SMS = SMS + "\n";
           
           SMS = SMS + "Speed: ";
           SMS = SMS + (String)SPEED;
      
           SMS = SMS + "\n";
           SMS = SMS + "https://www.google.com/maps/search/?api=1&query=";
           SMS = SMS + latitude;
           SMS = SMS + ",";
           SMS = SMS + longitude;
           
      
           Serial.println(SMS);
         
      
           delay(2000);
           digitalWrite(13,LOW);
          }

  
  

}
