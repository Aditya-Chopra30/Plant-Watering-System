#define BLYNK_TEMPLATE_ID "TMPL37oE1wKQc"
#define BLYNK_TEMPLATE_NAME "Smart Plant Monitoring System"
#define BLYNK_AUTH_TOKEN "K4Sg6iIhtuC8yfHtMd-qsVlvtfps8KYq"
#include <Wire.h>
#include <WiFi.h>
#include <LiquidCrystal_I2C.h>
#include <DHT.h>
#include <Ticker.h>
#define rainAnalog 39
#define soilMoisturePin 34
#define soilMoisturePin2 36
#define TRIG_PIN 12
#define ECHO_PIN 13
#define RELAY_PIN0 15
#define RELAY_PIN1 4
#define RELAY_PIN2 16
#define RELAY_PIN3 18
#define my_pin 17
#define DHTPIN 5
#define DHTTYPE DHT22 
Ticker t_hTicker; 
Ticker lcdTicker; 
Ticker soilTicker; 
Ticker rainTicker; 
Ticker tankTicker; 
String data = ""; 
char auth[] = "K4Sg6iIhtuC8yfHtMd-qsVlvtfps8KYq"; 
char ssid[] = "Madara Uchiha"; 
char pass[] = "sasukeuchiha"; 
int Relay0state = HIGH; 
int Relay1state = HIGH; 
int Relay2state = HIGH; 
int Relay3state = HIGH; 
WiFiServer server(80); 
WiFiClient client; 
LiquidCrystal_I2C lcd(0x27, 20, 4); 
DHT dht(DHTPIN, DHTTYPE); 
void setup() {
 Serial.begin(9600); 
 delay(1000); 
 WiFi.mode(WIFI_STA); 
 WiFi.begin(ssid, pass); 
 Serial.println("\nConnecting"); 
 while(WiFi.status() != WL_CONNECTED){
 Serial.print("."); 
 delay(100);
 }
 Serial.print("Local ESP32 IP: "); 
 Serial.println(WiFi.localIP()); 
 Wire.begin(); 
 
 pinMode(RELAY_PIN0, OUTPUT); 
 digitalWrite(RELAY_PIN0, HIGH); 
 pinMode(RELAY_PIN1, OUTPUT); 
 digitalWrite(RELAY_PIN1, HIGH); 
 pinMode(RELAY_PIN2, OUTPUT); 
 digitalWrite(RELAY_PIN2, HIGH); 
 pinMode(RELAY_PIN3, OUTPUT); 
 digitalWrite(RELAY_PIN3, HIGH); 
 server.begin(); 
 lcd.init(); 
 lcd.backlight(); 
 lcd.setCursor(0,1); 
 lcd.print(" Temp Humid Tank "); 
 pinMode(TRIG_PIN, OUTPUT); 
 pinMode(ECHO_PIN, INPUT); 
 pinMode(my_pin, OUTPUT); 
 dht.begin(); 
 
 t_hTicker.attach(5, t_h); 
 lcdTicker.attach(15, lcdtimer); 
 soilTicker.attach(1,Soil_Sensor ); 
 rainTicker.attach(1, Rain_Sensor); 
 tankTicker.attach(0.1, tank); 
}
void lcdtimer(){
 lcd.setCursor(0,0); 
 lcd.print(" "); 
 lcd.setCursor(0,1); 
 lcd.print(" Temp Humid Tank "); 
 lcd.setCursor(14,2); 
 lcd.print(" "); 
 lcd.setCursor(0,3); 
 lcd.print(" "); 
}
void t_h() { 
 lcd.setCursor(0,1); 
 lcd.print(" Temp Humid Tank "); 
 digitalWrite(my_pin, LOW); 
 digitalWrite(my_pin, HIGH); 
 delay(2000); 
 float humidity = dht.readHumidity(); 
 float temperature = dht.readTemperature(); 
 delay(5000); 
 if (isnan(humidity) || isnan(temperature)) {
 Serial.println("Failed to read from DHT sensor."); 
 } else {
 lcd.setCursor(0, 2); 
 lcd.print(temperature); 
 lcd.print("% "); 
 lcd.setCursor(7, 2); 
 lcd.print(humidity); 
 lcd.print("% "); 
 }
 digitalWrite(my_pin, LOW); 
}
void Soil_Sensor() {
 int soilMoistureValue = 0; 
 soilMoistureValue = analogRead(soilMoisturePin); 
 int soilMoistureValue2 = analogRead(soilMoisturePin2); 
 Serial.println(soilMoistureValue2); 
 lcd.setCursor(0, 0); 
 lcd.print("Moist1::2:"); 
 if (soilMoistureValue <= 2200) {
 lcd.print("100%::"); 
 } else if (soilMoistureValue > 2200 && soilMoistureValue <= 2800) {
 lcd.print("050%::"); 
 } else {
 lcd.print("000%::"); 
 }
 soilMoistureValue2 = map(soilMoistureValue2, 0, 4095, 0, 100); 
 soilMoistureValue2 = (soilMoistureValue2 - 100) * -1;
 if (soilMoistureValue2 == 4095) {
 lcd.print("000%"); 
 Blynk.virtualWrite(V4, 0); 
 } else if (soilMoistureValue2 < 4095){
lcd.print("100%"); 
 }
 lcd.print(soilMoistureValue2);
 
}
void Rain_Sensor() {
 int Value_2 = analogRead(rainAnalog); 
 if (Value_2 < 4095) {
 lcd.setCursor(0, 3); 
 lcd.print("Rain:"); 
 lcd.print("Yes"); 
 } else {
 lcd.setCursor(0, 3); 
 lcd.print("Rain:"); 
 lcd.print("No "); 
 }
}
void tank() {
 long duration, distance; 
 digitalWrite(TRIG_PIN, LOW); 
 delayMicroseconds(2); 
 digitalWrite(TRIG_PIN, HIGH); 
 delayMicroseconds(10); 
 digitalWrite(TRIG_PIN, LOW); 
 duration = pulseIn(ECHO_PIN, HIGH); 
 distance = (duration / 2) / 29.1; 
 int final_d = 12 - distance; 
 lcd.setCursor(14, 2); 
 lcd.print(final_d); 
 lcd.print("cm"); 
}
BLYNK_WRITE(V1) {
 Relay0state = param.asInt(); 
 digitalWrite(RELAY_PIN0, Relay0state); 
}
void switch0() {
 Relay0state = !Relay0state; // Toggle the state
 digitalWrite(RELAY_PIN0, Relay0state); 
}
BLYNK_WRITE(V7) {
 Relay1state = param.asInt(); 
 digitalWrite(RELAY_PIN1, Relay1state);
}
void switch1() {
 Relay1state = !Relay1state; // Toggle the state
 digitalWrite(RELAY_PIN1, Relay1state); 
 Blynk.virtualWrite(V7, Relay1state); 
}
BLYNK_WRITE(V8) {
 Relay2state = param.asInt(); 
 digitalWrite(RELAY_PIN2, Relay2state); 
}
void switch2() {
 Relay2state = !Relay2state; // Toggle the state
 digitalWrite(RELAY_PIN2, Relay2state); 
}
BLYNK_WRITE(V9) {
 Relay3state = param.asInt(); 
 digitalWrite(RELAY_PIN3, Relay3state); 
}
void switch3() {
 Relay3state = !Relay3state; // Toggle the state
 digitalWrite(RELAY_PIN3, Relay3state); 
}
void loop(){
 Serial.println("Button pressed in Blynk app"); 
 client = server.available(); 
 if (!client)
 return; 
 data = checkClient(); 
 Serial.print("Received data: "); 
 Serial.println(data); 
 if (data == "device1on")
 {
 switch0(); 
 }
 else if (data == "device1off")
 {
 switch0(); 
 }
 else if (data == "device2on")
 {
 switch1(); 
 }
 else if (data == "device2off")
 {
 switch1(); 
 }
 else if (data == "device3on")
 {
 switch2(); 
 }
 else if (data == "device3off")
 {
 switch2(); 
 }
 else if (data == "device4on")
 {
 switch3(); 
 }
 else if (data == "device4off")
 {
 switch3(); 
 }
}
String checkClient()
{
 while (!client.available())
 delay(1); 
 String request = client.readStringUntil('\r'); 
 request.remove(0, 5); 
 request.remove(request.length() - 9, 9); 
 return request; 
}  
