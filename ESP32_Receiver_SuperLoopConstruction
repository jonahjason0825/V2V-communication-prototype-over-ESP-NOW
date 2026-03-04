#include <Arduino.h>
#include <WiFi.h>
#include <esp_now.h>
#include <SPI.h>
#include <Wire.h>
#include <Adafruit_GFX.h>
#include <Adafruit_SSD1306.h>
//init section
int buzz=15;
#define LED 18
#define SCREEN_WIDTH 128 // OLED display width, in pixels
#define SCREEN_HEIGHT 64 // OLED display height, in pixels
#define OLED_RESET     -1 // Reset pin # (or -1 if sharing Arduino reset pin)
#define SCREEN_ADDRESS 0x3C
Adafruit_SSD1306 display(SCREEN_WIDTH, SCREEN_HEIGHT, &Wire, OLED_RESET);
//structure section for message passing
// Must match ESP8266 struct exactly
typedef struct proximity_warning {
  char a[32];
} proximity_warning;

proximity_warning warning;


void OnDataRecv(const uint8_t *mac, const uint8_t *data, int len) {
  memcpy(&warning, data, sizeof(warning));

  Serial.println("Received:");
  Serial.print("Status: "); Serial.println(warning.a);
  Serial.println("-------------------");
 
}

void setup() {
  Serial.begin(9600);
  delay(1000);

  // SSD1306_SWITCHCAPVCC = generate display voltage from 3.3V internally
  if(!display.begin(SSD1306_SWITCHCAPVCC, SCREEN_ADDRESS)) {
    Serial.println(F("SSD1306 allocation failed"));
    for(;;); // Don't proceed, loop forever
  }
  WiFi.mode(WIFI_STA);

  if (esp_now_init() != ESP_OK) {
    Serial.println("ESP-NOW init failed");
    return;
  }

  esp_now_register_recv_cb(OnDataRecv);

  Serial.println("ESP32 ESP-NOW Receiver ready");
  pinMode(LED, OUTPUT);
  pinMode(buzz, OUTPUT);
  display.display();
  delay(2000); // Pause for 2 seconds
  // Clear the buffer
  display.clearDisplay();
  display.setTextSize(1);
  display.setTextColor(SSD1306_WHITE);
}

void loop() {
  digitalWrite(LED, LOW);
  digitalWrite(buzz, LOW);
  
   if (strcmp(warning.a, "VEHICLE CLOSE AHEAD! STOP!") == 0) {
    display.clearDisplay();
    display.setTextSize(1); // Draw 2X-scale text
    display.setTextColor(SSD1306_WHITE);
    display.setCursor(10, 1);
    display.println(F("VEHICLE CLOSE AHEAD!"));
    display.setCursor(10, 10);
    display.println(F("STOP!"));
    display.display();      // Show initial text
    delay(100);
    digitalWrite(LED, HIGH);
    digitalWrite(buzz, HIGH);
    delay(300);
    digitalWrite(LED, LOW);
    digitalWrite(buzz, LOW);
    delay(300);
   }
   else{
    display.clearDisplay();
    display.setTextSize(1); // Draw 2X-scale text
    display.setTextColor(SSD1306_WHITE);
    display.setCursor(10, 1);
    display.println(F("All clear..."));
    display.setCursor(10, 10);
    display.println(F("No cause for alarm"));
    display.display();      // Show initial text
    delay(100);
   }
}
