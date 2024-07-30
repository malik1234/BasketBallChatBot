# BasketBallChatBot
LLM project for Basketball
#include <Wire.h>
#include <Adafruit_GFX.h>
#include <Adafruit_SSD1306.h>

#define DEFAULT_OLED_ADDR 0x3C

#define DISPLAY1_ENABLE 2
#define DISPLAY2_ENABLE 3
#define DISPLAY3_ENABLE 4


#define SCREEN_WIDTH 128 // OLED display width, in pixels
#define SCREEN_HEIGHT 64 // OLED display height, in pixels
#define OLED_RESET     -1

// Create display objects
Adafruit_SSD1306 display1(SCREEN_WIDTH, SCREEN_HEIGHT, &Wire, OLED_RESET);
Adafruit_SSD1306 display2(SCREEN_WIDTH, SCREEN_HEIGHT, &Wire, OLED_RESET);
Adafruit_SSD1306 display3(SCREEN_WIDTH, SCREEN_HEIGHT, &Wire, OLED_RESET);

void setup() {
  Serial.begin(9600);

  pinMode(DISPLAY1_ENABLE, OUTPUT);
  pinMode(DISPLAY2_ENABLE, OUTPUT);
  pinMode(DISPLAY3_ENABLE, OUTPUT);

  digitalWrite(DISPLAY1_ENABLE, HIGH);
  digitalWrite(DISPLAY2_ENABLE, HIGH);
  digitalWrite(DISPLAY3_ENABLE, HIGH);

  initDisplay(display1, DISPLAY1_ENABLE);
  initDisplay(display2, DISPLAY2_ENABLE);
  initDisplay(display3, DISPLAY3_ENABLE);
  
  showMessage(display1, DISPLAY1_ENABLE, "Display 1!");
  showMessage(display2, DISPLAY2_ENABLE, "Display 2!");
  showMessage(display3, DISPLAY3_ENABLE, "Display 3!");
}

void loop() {

}

void initDisplay(Adafruit_SSD1306 &display, int powerPin) {
  digitalWrite(powerPin, LOW);
  delay(2000); 
  
  if(!display.begin(SSD1306_SWITCHCAPVCC, DEFAULT_OLED_ADDR)) {
    Serial.println(F("SSD1306 allocation failed"));
    for(;;);
  }
  
  display.clearDisplay();
  display.display();
  
  digitalWrite(powerPin, HIGH); //should be done in a loop afterwards to refresh images?
}

void showMessage(Adafruit_SSD1306 &display, int powerPin, const char *message) {
  digitalWrite(powerPin, LOW);
  delay(2000); 

  display.clearDisplay();

  display.setTextSize(1);      
  display.setTextColor(SSD1306_WHITE);  
  display.setCursor(0, 0);     
  display.print(message);
  display.display();

  digitalWrite(powerPin, HIGH);
}
