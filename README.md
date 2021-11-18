

#include <LiquidCrystal.h> 
#include<CapacitiveSensor.h>
 
CapacitiveSensor capSensor = CapacitiveSensor (7, 8);// capacitive rain sensor
 
LiquidCrystal lcd(12, 11, 5, 4, 3, 2); 
int SensorValue = 0;
int LedGreen = 10;       //LDR led
const int LDR = A1;      //LDR Pin
int LedRed = 13;           //Temperature led
const int sensorPin = A0;  //TMP Pin
const float baselineTemp = 24.0; 
 
int trigPin = 6;
int echoPin = 9;
int LedYellow = A2;       //Ultrasonic sensor LED
 
int threshold = 1000;
int LedBlue = A3;     //Rain sensor LED
 
void setup() 
{ 
 
  lcd.begin(16, 2); 
  Serial.begin(9600); 
  pinMode(LedGreen, OUTPUT); //ldr
  pinMode(LedRed, OUTPUT);  //temp
  pinMode(trigPin, OUTPUT);
  pinMode(echoPin, INPUT);
  pinMode(LedYellow, OUTPUT); //Ultra-s
  pinMode(LedBlue, OUTPUT);  //Rain-s
} 
 
void loop() { 
 
 
 
  int sensorVal = analogRead(sensorPin); 
  float voltage = (sensorVal / 1024.0) * 5.0; 
  float temperature = (voltage - .5) * 100; 
 
    lcd.setCursor(0,0);         //LCD Printing
    lcd.print("Degree C = "); 
    lcd.print(temperature); 
  delay(1000); 
 
Serial.print(temperature);     //Serial Monitor Printing
Serial.println("C");
 
  SensorValue = analogRead(LDR);
  delay (5);
  Serial.print("sensor = ");
  Serial.print(SensorValue);
 
  
if (temperature > baselineTemp)   //TMP
{ 
digitalWrite(LedRed, HIGH); 
  delay(50); 
} 




else 
{ 
digitalWrite(LedRed, LOW); 
  delay(50);
}
 
 
 
if (SensorValue < 100)  //LDR
{
digitalWrite(LedGreen, HIGH);
Serial.println(" Green is turn ON");
} 
else 
{15
digitalWrite(LedGreen, LOW);
Serial.println(" Green is turn OFF");
}
  delay(1000); // Delay a little bit to improve simulation performance
 
  long duration, distance;    // Ultra Sonic Sensor
 
  digitalWrite(trigPin, LOW); 
  delayMicroseconds(2); 
  digitalWrite(trigPin, HIGH);
  delayMicroseconds(10); 
  digitalWrite(trigPin, LOW);
  duration = pulseIn(echoPin, HIGH);
  distance = (duration/2) / 29.1;
  if (distance < 20) {
    digitalWrite(LedYellow, HIGH); 
}
  else {
    digitalWrite(LedYellow, LOW);
  }
  if (distance >= 200 || distance <= 0){
    Serial.println("Out of range");
    digitalWrite(LedYellow, LOW);
  }
  else {
    Serial.print(distance);
    Serial.println(" cm");
  }
  delay(500);
 
long sensorValue = capSensor.capacitiveSensor(30);
Serial.println(sensorValue);
 
if (sensorValue > threshold) { 
digitalWrite(LedBlue, HIGH);
}
else{
digitalWrite(LedBlue, LOW);
}
delay(100);
}

