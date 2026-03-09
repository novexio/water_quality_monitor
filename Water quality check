#include <Wire.h>
#include <Adafruit_GFX.h>
#include <Adafruit_SSD1306.h>

// OLED display configuration
#define SCREEN_WIDTH 128
#define SCREEN_HEIGHT 64
#define OLED_RESET -1
Adafruit_SSD1306 display(SCREEN_WIDTH, SCREEN_HEIGHT, &Wire, OLED_RESET);

// Pin definitions
#define TDS_PIN 34      // Analog pin connected to TDS sensor
#define BUZZER_PIN 25   // Active buzzer pin

float tdsValue = 0.0;

void setup() {

  Serial.begin(115200);

  pinMode(BUZZER_PIN, OUTPUT);
  digitalWrite(BUZZER_PIN, LOW);

  // Initialize OLED display
  if(!display.begin(SSD1306_SWITCHCAPVCC, 0x3C)) {
    Serial.println("OLED initialization failed");
    while(true); 
  }

  display.clearDisplay();
  display.setTextSize(1);
  display.setTextColor(SSD1306_WHITE);

  display.setCursor(0,0);
  display.println("Water Quality Monitor");
  display.setCursor(0,20);
  display.println("Initializing...");
  display.display();

  delay(2000);
}

void loop() {

  int analogValue = analogRead(TDS_PIN);

  // Convert analog value to voltage
  float voltage = analogValue * (3.3 / 4095.0);

  // Convert voltage to approximate TDS value (ppm)
  tdsValue = voltage * 500;  

  Serial.print("Analog Value: ");
  Serial.print(analogValue);
  Serial.print("  TDS: ");
  Serial.print(tdsValue);
  Serial.println(" ppm");

  // OLED Display Output
  display.clearDisplay();

  display.setCursor(0,0);
  display.println("Water Quality");

  display.setCursor(0,20);
  display.print("TDS: ");
  display.print(tdsValue);
  display.println(" ppm");

  // Water Quality Status
  display.setCursor(0,40);

  if(tdsValue > 500) {
    display.println("Status: Poor Water");
    digitalWrite(BUZZER_PIN, HIGH);
  } 
  else {
    display.println("Status: Good Water");
    digitalWrite(BUZZER_PIN, LOW);
  }

  display.display();

  delay(1000);
}
