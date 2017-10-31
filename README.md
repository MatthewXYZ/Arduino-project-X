# Arduino-project-X
//Library
#include <LiquidCrystal.h> 

//Constant output/input pins
const int pinDig=13,pinRed=10,pinBlue=9,pinGreen=8,inPin=0,rs=12,re=11,db4=5,db5=4,db6=3,db7=2;
const int buttonPin=1;
//State
int buttonState = 0;

const int MAX=33,MIN=24;//Min and Max constant for temperature

LiquidCrystal lcd(rs,re,db4,db5,db6,db7); //LCD connection

void setup() {
  //Input/output pins
  lcd.begin(16,2);
  pinMode(pinDig,OUTPUT);
  pinMode(pinGreen,OUTPUT);
  pinMode(pinRed,OUTPUT);
  pinMode(pinBlue,OUTPUT);
  pinMode(buttonPin,INPUT);
  
}
//Function for temperature reading
float tempC()
{
  int value = analogRead(inPin); 
  lcd.setCursor(0,1);
  float millivolts = (value / 1024.0) * 5000; 
  float celsius = (millivolts-500) / 10;
  return celsius;
}
void loop() {
  
  if(digitalRead(buttonPin)==1){ //Switch state on button press
    if(buttonState==1)
  		buttonState = 0;
    else
      	buttonState = 1;
  }
  float temp = tempC();
  lcd.clear();
  if(buttonState==0){ // Show temp. readings if state=0
    lcd.setCursor(0,0);
    lcd.print(temp);
    lcd.print("'C");
    lcd.setCursor(0,1);
   //TODO:: Add heather code!!
    if(temp > MAX){ //ventilation ON(untill further change) and RGB led glows RED
      digitalWrite(pinDig,HIGH); 
      digitalWrite(pinRed,1);
      digitalWrite(pinBlue,0);
      digitalWrite(pinGreen,0);
      lcd.print("TEMP:HIGH");
    }
    else if(temp < MIN){ //ventilation OFF(untill further change) and RGB led glows BLUE and heather turns on untill min temp. is reached
      digitalWrite(pinDig,LOW); 
      digitalWrite(pinBlue,1);
      digitalWrite(pinRed,0);
      digitalWrite(pinGreen,0);
      lcd.print("TEMP:LOW");
    }
    else{ //RGB led glows GREEN and heather turns off
      digitalWrite(pinGreen,1);
      digitalWrite(pinBlue,0);
      digitalWrite(pinRed,0);
      lcd.print("TEMP:NORM");
    }
  }
  //Add humidity sensor code
  else{ //If state is 1 show humidity readings on LCD
    lcd.setCursor(0,0);
  	lcd.print("HUMIDITY");
    lcd.setCursor(0,1);
    lcd.print("Sensor???");
  }
  delay(500);
}
