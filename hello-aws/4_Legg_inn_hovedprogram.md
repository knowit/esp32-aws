# Hello AWS: Legg inn hovedprogram


## Legg til biblioteker
Åpne ```platformio.ini``` og legg til nye bilblioteker slik:
```properties
lib_deps = 
	arduino-libraries/ArduinoHttpClient@^0.4.0
	bblanchon/ArduinoJson@^6.19.4
	256dpi/MQTT@2.4.8
```

## Legg inn hovedprogram
Åpne ```main.cpp``` og erstatt innholdet med:
```cpp
#include "secrets.h"
#include <WiFiClientSecure.h>
#include <MQTTClient.h>
#include <ArduinoJson.h>
#include "WiFi.h"

// The MQTT topics that this device should publish/subscribe
#define AWS_IOT_PUBLISH_TOPIC "esp32/pub"
#define AWS_IOT_SUBSCRIBE_TOPIC "esp32/sub"

WiFiClientSecure net = WiFiClientSecure();
MQTTClient client = MQTTClient(256);

void messageHandler(String &topic, String &payload)
{
  Serial.println("Message received.\nTopic: " + topic + "\nPayload:\n" + payload);
  StaticJsonDocument<200> doc;
  deserializeJson(doc, payload);
  const char *message = doc["message"];
  const char *value = doc["value"];
  Serial.print("Message: ");
  Serial.println(message);
  Serial.print("Value: ");
  Serial.println(value);
}

void connectAWS()
{
  WiFi.mode(WIFI_STA);
  WiFi.begin(WIFI_SSID, WIFI_PASSWORD);

  Serial.print("Connecting to Wi-Fi. SSID: ");
  Serial.print(WIFI_SSID);
  while (WiFi.status() != WL_CONNECTED)
  {
    delay(500);
    Serial.print(".");
  }
  Serial.println();
  Serial.println("OK - Connected to Wifi");

  // Configure WiFiClientSecure to use the AWS IoT device credentials
  net.setCACert(AWS_CERT_CA);
  net.setCertificate(AWS_CERT_CRT);
  net.setPrivateKey(AWS_CERT_PRIVATE);

  // Connect to the MQTT broker on the AWS endpoint we defined earlier
  client.begin(AWS_IOT_ENDPOINT, 8883, net);

  // Create a message handler
  client.onMessage(messageHandler);

  Serial.print("Connecting to AWS IOT");
  while (!client.connect(THINGNAME))
  {
    Serial.print(".");
    delay(100);
  }
  Serial.println();

  if (!client.connected())
  {
    Serial.println("AWS IoT Timeout!");
    return;
  }

  // Subscribe to a topic
  client.subscribe(AWS_IOT_SUBSCRIBE_TOPIC);

  Serial.println("OK - Connected to AWS IoT");
}

void publishMessage()
{
  StaticJsonDocument<200> doc;
  doc["time"] = millis();
  doc["analog_32"] = analogRead(32);
  char jsonBuffer[512];
  serializeJson(doc, jsonBuffer); // print to client

  client.publish(AWS_IOT_PUBLISH_TOPIC, jsonBuffer);
}

void setup()
{
  Serial.begin(115200);
  connectAWS();
}

void loop()
{
  publishMessage();
  client.loop();
  delay(1000);
}
```

Gå videre til: [Konfigurasjon og sertifikater](./5_Konfigurasjon_og_sertifikater.md)
