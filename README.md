
//1-Sera Otomatik Kontrol Ek:

#include <LiquidCrystal_I2C.h>
#include <Wire.h>
#include "DHT.h"
#include "LiquidCrystal_I2C.h"
LiquidCrystal_I2C lcd(0x27, 16,2);
#define DHTPIN 5
#define DHTTYPE DHT11
DHT sensor(DHTPIN, DHTTYPE);
#define hava A3

int temiz_hava;
int role=7;
int nemsensor=A0;
int sondeger=450;
int havalandirma = 9;
int fan = 8;
int isi = 4;

void setup() {
pinMode(role, OUTPUT);
pinMode(nemsensor, INPUT);
digitalWrite(role,HIGH);
Serial.begin(9600);
pinMode(hava, OUTPUT);
digitalWrite(hava, HIGH);
lcd.begin();
sensor.begin();
pinMode(fan, OUTPUT);
digitalWrite(fan, HIGH);
pinMode(havalandirma, OUTPUT);
digitalWrite(havalandirma, HIGH);
pinMode(isi, OUTPUT);
digitalWrite(isi, HIGH); }
void loop() {
int nem = analogRead(nemsensor);
Serial.println(nem);
temiz_hava=digitalRead(hava);
Serial.println(temiz_hava);
delay(100);

// Havadaki CO2 değeri
if (temiz_hava==0) {
digitalWrite(havalandirma,HIGH);
delay(50000); }
else {
digitalWrite(havalandirma,LOW);
delay(5000); }

//Nem değeri sondeger 'den yüksekse role 5s LOW yap 0.5s HIGH yap.
if(nem >= sondeger) {
digitalWrite(role,LOW);
delay(5000);
digitalWrite(role,HIGH);
delay(500); }
else {
digitalWrite(role,HIGH); }
lcd.clear();
float t = sensor.readTemperature();
float n = sensor.readHumidity ();
if (isnan(t)) {
lcd.print("Hata");
delay(1000);
return; }
lcd.setCursor(0,0);
lcd.print("Derece:");
lcd.print(t);
lcd.print(" C");
//lcd.print(n);
// Sıcaklık değeri t derecesi 20'nin altına düşerse isi pinini LOW yap değilse HIGH yap.
if (t < 20){
digitalWrite(isi, LOW);
delay(100); }
else{
digitalWrite(isi, HIGH); }
// Sıcaklık t 28 derecenin üstüne çıkarsa fan pinini LOW yap değilse HIGH yap.
if (t > 28){
digitalWrite(fan, LOW);
lcd.setCursor(0,1);
lcd.print("Fan on ");
lcd.print("Nem:%");
lcd.print(n);
delay(100); }
else {
digitalWrite(fan, HIGH);
lcd.setCursor(0,1);
lcd.print("Fan off ");
lcd.print("Nem:%");
lcd.print(n);
delay(100); }
delay(500);
}
