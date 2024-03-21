// Read data from rain sensor 
int rainValue = analogRead(rainPin);
// Read data from soil moisture sensor 
int soilMoistureValue = analogRead(soilMoisturePin); 
//Code Snippet for DHT11 Sensor: 
#include <DHT.h>
#define DHTPIN 2 // Define the pin to which the DHT11 sensor is connected 
#define DHTTYPE DHT22 // DHT22 sensor type 
DHT dht(DHTPIN, DHTTYPE); 
void setup() { 
Serial.begin(115200); 
dht.begin();} 
void loop() 
{ 
delay(2000); // Delay between sensor readings 
float humidity = dht.readHumidity(); 
float temperature = dht.readTemperature(); 
if (isnan(humidity) || isnan(temperature)) {
Serial.println("Failed to read from DHT sensor!"); 
return; 
} 
 Serial.print(humidity); 
Serial.print(temperature); 
}
