# Water-Sensor_IoT
Creating a Water Sensor and integretingwith iot
#define Grove_Water_Sensor D3 // Attach Water sensor to Node MCU pin D3
#define LED 9 // Attach an LED to D4 (or use onboard LED)
#include <ESP8266WiFi.h> // ESP8266WiFi.h library
const char* ssid     = "Hehe";
const char* password = "haha1999";
const char* host = "api.thingspeak.com";
const char* writeAPIKey = "WPMLRCX7987EZ265";

void setup() {
   pinMode(Grove_Water_Sensor, INPUT); // The Water Sensor is an Input
   pinMode(LED, OUTPUT); // The LED is an Output
   // Initialize sensor
   Serial.begin(115200);
   //  Connect to WiFi network
   WiFi.begin(ssid, password);
   while (WiFi.status() != WL_CONNECTED) {
   delay(500);
   Serial.println(".");}
}

void loop() {
   if( digitalRead(Grove_Water_Sensor) == LOW) {
      digitalWrite(LED,HIGH);
   }else {
      digitalWrite(LED,LOW);
   }
// make TCP connections
  WiFiClient client;
  const int httpPort = 80;
  if (!client.connect(host, httpPort)) {
    return;
  }
  String url = "/update?key=";
  url+=writeAPIKey;
  url+="&field1=";
  url+=String(temperature);
  url+="&field2=";
  url+=String(humidity);
  url+="\r\n";
  //Serial.print("Temperature: ");
                            Serial.print(temperature);
                             Serial.print(" degrees Celcius, Humidity: ");
                             Serial.print(humidity);
                             Serial.println("%. Send to Thingspeak.");
  
  // Request to the server
  client.print(String("GET ") + url + " HTTP/1.1\r\n" +
               "Host: " + host + "\r\n" + 
               "Connection: close\r\n\r\n");
    delay(6000);   
}
