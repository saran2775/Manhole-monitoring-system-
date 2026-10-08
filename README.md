# Manhole-monitoring-system-
The Manhole Monitoring System is an IoT-based system designed to monitor the condition of manholes and provide timely information about hazardous conditions. The system uses an ESP8266 microcontroller, soil moisture sensor, IR sensor, and RGB LED to detect changes and indicate the status of the manhole.
#define BLYNK_TEMPLATE_ID "YOUR_TEMPLATE_ID"
#define BLYNK_TEMPLATE_NAME "ManhoX"
#define BLYNK_AUTH_TOKEN "YOUR_AUTH_TOKEN"

#include <ESP8266WiFi.h>
#include <BlynkSimpleEsp8266.h>

char ssid[] = "YOUR_WIFI";
char pass[] = "YOUR_PASSWORD";

#define SOIL A0
#define IR D1

#define RED D5
#define GREEN D6
#define BLUE D7

void setup()
{
  Serial.begin(9600);

  pinMode(IR, INPUT);
  pinMode(RED, OUTPUT);
  pinMode(GREEN, OUTPUT);
  pinMode(BLUE, OUTPUT);

  Blynk.begin(BLYNK_AUTH_TOKEN, ssid, pass);
}

void loop()
{
  Blynk.run();

  int moisture = analogRead(SOIL);
  int ir = digitalRead(IR);

  Blynk.virtualWrite(V0, moisture);
  Blynk.virtualWrite(V1, ir);

  if (moisture > 700)
  {
    digitalWrite(RED, HIGH);
    digitalWrite(GREEN, LOW);
    digitalWrite(BLUE, LOW);
  }
  else if (moisture > 400)
  {
    digitalWrite(RED, LOW);
    digitalWrite(GREEN, LOW);
    digitalWrite(BLUE, HIGH);
  }
  else
  {
    digitalWrite(RED, LOW);
    digitalWrite(GREEN, HIGH);
    digitalWrite(BLUE, LOW);
  }

  delay(500);
}