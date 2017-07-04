#include <LCD.h>
#include <LiquidCrystal_I2C.h>
#include <Wire.h> 

LiquidCrystal_I2C  lcd(0x27,2,1,0,4,5,6,7);

int analogPin= 9;
int raw= 0;
int Vin= 5;
float Vout= 0;
float R1= 1005;
float R2= 0;
float buffer= 0;

void setup() {
  lcd.begin (16,2); // for 16 x 2 LCD module
  lcd.setBacklightPin(3,POSITIVE);
  lcd.setBacklight(HIGH);
  pinMode(7, OUTPUT);  
}

void loop() {
  raw= analogRead(analogPin);
  if(raw) 
  {
    buffer= raw * Vin;
    Vout= (buffer)/1024.0;
    buffer= (Vin/Vout) -1;
    R2= R1 * buffer;
    lcd.home (); // set cursor to 0,0
    lcd.print("Vout: "); 
    lcd.print(Vout); 
    lcd.setCursor (0,1);
    lcd.print("R2: "); 
    lcd.print(R2); 
    if(R2 > 1000 && R2 < 2000)
    {
      digitalWrite(7, HIGH);
    }
    else
    {
      digitalWrite(7, LOW);
    }
  }
}
