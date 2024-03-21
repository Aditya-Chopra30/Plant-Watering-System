#define trigPin 4 // Define the pin to which the trigger (Trig) of the ultrasonic sensor 
is connected 
#define echoPin 5 // Define the pin to which the echo (Echo) of the ultrasonic sensor 
is connected 
void setup() 
{ 
Serial.begin(115200); 
pinMode(trigPin, OUTPUT);
pinMode(echoPin, INPUT) 
}
void loop() 
{ 
long duration, distance; 
// Trigger the ultrasonic sensor 
digitalWrite(trigPin, LOW); 
delayMicroseconds(2); 
digitalWrite(trigPin, HIGH); 
delayMicroseconds(10); 
digitalWrite(trigPin, LOW); 
// Measure the duration of the pulse from the echo pin 
duration = pulseIn(echoPin, HIGH); 
// Calculate the distance in centimeters
distance = (duration * 0.034 / 2); 
Serial.print("Distance: "); 
Serial.print(distance); 
Serial.println(" cm"); 
delay(1000); // Delay between distance measurements 
}
