#include <LCD_I2C.h>
LCD_I2C lcd1(0x27,16,4);

int fault1 = 0;
int fault1fd = 0;

int fault2 = 0;
int fault2fd = 0;

int kd = 20000;


void setup() {

  delay(1000);

  Serial.begin(9600);
  lcd1.begin(16,4);
  lcd1.backlight();
  lcd1.clear();
  lcd1.setCursor ( 0, 0); 
  lcd1.print("Start");
  delay(2000);
  lcd1.clear();

   pinMode(10, OUTPUT);
   pinMode(2, OUTPUT);

   pinMode(12, INPUT);
   pinMode(4, INPUT);

   pinMode(11, OUTPUT);
   pinMode(3, OUTPUT);


   digitalWrite(11, LOW);
   digitalWrite(3, LOW);

   digitalWrite(10, LOW);
   digitalWrite(2, LOW);


   fault1 = 0;
   fault1fd = 0;

   fault2 = 0;
   fault2fd = 0;

}

void loop() 
{


  digitalWrite(12, LOW);
  digitalWrite(2, LOW);

  digitalWrite(11, LOW);
  digitalWrite(3, LOW);

  fault1 = 0;
  fault1fd = 0;

  fault2 = 0;
  fault2fd = 0;

  lcd1.clear();
  delay(1000);


  analogRead(A1);

    if(analogRead(A1) > 900)
    {

      Serial.print("Voltage 1 Ok: ");
      lcd1.setCursor ( 0, 3); 
      lcd1.print("No Fault");

      //Serial.println(analogRead(A1));
      delay(200);
      fault1 = 0;
      fault1fd = 0;
      delay(200);

    }else if(analogRead(A1) < 900){

      Serial.print("NO Voltage 1: Fault");
      //Serial.println(analogRead(A1));

      lcd1.clear();
      lcd1.setCursor ( 0, 0); 
      lcd1.print(" Sensor 1");
      lcd1.setCursor ( 0, 1);
      lcd1.print(" Voltage Wire ");
      lcd1.setCursor ( 0, 3); 
      lcd1.print("Fault Occur");


      delay(200);
      fault1 = 1;
      fault1fd = 1;
      delay(kd);

    }

  digitalRead(12);
    if(digitalRead(12) == LOW)
    {

      Serial.print("Gound 1 Ok");
      lcd1.setCursor ( 0, 3); 
      lcd1.print("No Fault");

      delay(200);
      fault1 = 0;
      fault1fd = 0;
      delay(200);

    }else if(digitalRead(12) == HIGH){

      Serial.print("No Gound 1: Fault");

      lcd1.clear();
      lcd1.setCursor ( 0, 0); 
      lcd1.print(" Sensor 1");
      lcd1.setCursor ( 0, 1);
      lcd1.print(" Gound Wire ");
      lcd1.setCursor ( 0, 3);
      lcd1.print("Fault Occur");

      delay(200);
      fault1 = 2;
      fault1fd = 2;
      delay(kd);

    }
    

  
  if(fault1 == 0 && fault1fd == 0)
  {

      analogRead(A0);
        
      if(analogRead(A0) < 100 && analogRead(A0)> 50){

        Serial.print("Day Time: ");
        Serial.println(analogRead(A0));

        lcd1.setCursor ( 0, 0); 
        lcd1.print(" Time : Day ");

        delay(200);

      }

      if(analogRead(A0) < 1025 && analogRead(A0)> 900){

        Serial.print("Night Time: ");
        Serial.println(analogRead(A0));

        lcd1.setCursor ( 0, 0); 
        lcd1.print(" Time : Night ");

        delay(200);

      }

      if(analogRead(A0) < 900 && analogRead(A0)> 100){

        Serial.print("NO Data: ");
        Serial.println(analogRead(A0));
        delay(200);
        fault1 = 3;
        delay(200);

      }
  }

  if(fault1 == 3 && fault1fd == 0)
  {

    digitalWrite(11, HIGH);
    delay(1500);

    analogRead(A0);
    delay(200);
    if(analogRead(A0)>900){

      fault1 = 4;
      fault1fd = 4;
      delay(300);
      digitalWrite(11, LOW);
      Serial.print("Change sensor 1");

      lcd1.clear();
      lcd1.setCursor ( 0, 0); 
      lcd1.print(" Sensor 1");
      lcd1.setCursor ( 0, 1);
      lcd1.print(" Change Sensor");
      lcd1.setCursor ( 0, 3);
      lcd1.print("Fault Occur");

      delay(kd);

    }else if(analogRead(A0) < 980){

      fault1 = 3;
      fault1fd = 3;
      delay(300);
      digitalWrite(11, LOW);
      Serial.print("Data Wire Fault");

      lcd1.clear();
      lcd1.setCursor ( 0, 0); 
      lcd1.print(" Sensor 1");
      lcd1.setCursor ( 0, 1);
      lcd1.print(" Data Wire ");
      lcd1.setCursor ( 0, 3);
      lcd1.print("Fault Occur");

      delay(kd);

    }

  }



  fault2 = 0;
  fault2fd = 0;


  analogRead(A3);

    if(analogRead(A3) > 900)
    {

      Serial.print("Voltage 2 Ok: ");
      //Serial.println(analogRead(A1));

      lcd1.setCursor ( 0, 3); 
      lcd1.print("No Fault");

      delay(200);
      fault2 = 0;
      fault2fd = 0;
      delay(200);

    }else if(analogRead(A3) < 900){

      Serial.print("NO Voltage 2: Fault");
      //Serial.println(analogRead(A1));

      lcd1.clear();
      lcd1.setCursor ( 0, 0); 
      lcd1.print(" Sensor 2");
      lcd1.setCursor ( 0, 1);
      lcd1.print(" Voltage Wire ");
      lcd1.setCursor ( 0, 3);
      lcd1.print("Fault Occur");

      delay(200);
      fault2 = 1;
      fault2fd = 1;
      delay(kd);

    }

  digitalRead(4);
    if(digitalRead(4) == LOW)
    {

      Serial.print("Gound 2 Ok");

      lcd1.setCursor ( 0, 3); 
      lcd1.print("No Fault");

      delay(200);
      fault2 = 0;
      fault2fd = 0;
      delay(200);

    }else if(digitalRead(4) == HIGH){

      Serial.print("No Gound 2: Fault");

      lcd1.clear();
      lcd1.setCursor ( 0, 0); 
      lcd1.print(" Sensor 2");
      lcd1.setCursor ( 0, 1);
      lcd1.print(" Gound Wire ");
      lcd1.setCursor ( 0, 3);
      lcd1.print("Fault Occur");


      delay(200);
      fault2 = 2;
      fault2fd = 2;
      delay(kd);

    }
    

  
  if(fault2 == 0 && fault2fd == 0)
  {

      analogRead(A2);
        
      if(analogRead(A2) < 780 && analogRead(A2)> 20){

        Serial.print("Air Quality: ");
        Serial.println(analogRead(A2));

        lcd1.setCursor ( 0, 1); 
        lcd1.print("Air Quality:");
        lcd1.setCursor ( 13, 1); 
        lcd1.print(analogRead(A2));

        delay(200);

      }
      if(analogRead(A2) > 780 && analogRead(A2)< 1025 || analogRead(A2)< 20 ){

        Serial.print("NO Data 2 : ");
        Serial.println(analogRead(A2));
        delay(200);
        fault2 = 3;
        delay(200);

      }
  }

  if(fault2 == 3 && fault2fd == 0)
  {

    digitalWrite(3, HIGH);
    delay(1500);

    analogRead(A2);
    delay(200);
    if(analogRead(A2)>900){

      fault2 = 4;
      fault2fd = 4;
      delay(300);
      digitalWrite(3, LOW);
      Serial.print("Change sensor 2");

      lcd1.clear();
      lcd1.setCursor ( 0, 0); 
      lcd1.print(" Sensor 2");
      lcd1.setCursor ( 0, 1);
      lcd1.print(" Change Sensor");
      lcd1.setCursor ( 0, 3);
      lcd1.print("Fault Occur");

      delay(kd);

    }else if(analogRead(A2) < 900){

      fault2 = 3;
      fault2fd = 3;
      delay(300);
      digitalWrite(3, LOW);
      Serial.print("Data Wire 2 Fault");

      lcd1.clear();
      lcd1.setCursor ( 0, 0); 
      lcd1.print(" Sensor 2");
      lcd1.setCursor ( 0, 1);
      lcd1.print(" Data Wire ");
      lcd1.setCursor ( 0, 3);
      lcd1.print("Fault Occur");

      delay(kd);

    }

  }

  delay(2000);
  lcd1.clear();
}
