#include <Wire.h>
#include <Adafruit_GFX.h>
#include <Adafruit_SSD1306.h>

#define SCREEN_WIDTH 128
#define SCREEN_HEIGHT 64
#define OLED_RESET    -1

Adafruit_SSD1306 display(SCREEN_WIDTH, SCREEN_HEIGHT, &Wire, OLED_RESET);

const char* lyrics[] = {
  "    sudah terbiasa",
  "       Terjadi",
  "        Tante",
  "  Teman teman datang",
  "        Ketika",
  "         Lagi",
  "     Butuh saja",
  "        Coba",
  "        kalau",
  "      lagi susah",
  "     mereka semua",
  " menghilaaaaannngggg",
  " apakah spek standar",
  "     seperti ini",
  "      yang para",
  " pemirsa inginkan???",
  "      Tanteee....."
};

// waktu tampil (bukan durasi antar lirik!)
unsigned long lyricTimes[] = {
  0,
  1500,
  2500,
  3500,
  4500,
  5500,
  6000,
  7500,
  7900,
  9000,
  10200,
  12100,
  13500,
  14500,
  15000,
  15500,
  16500
};

const int numLines = sizeof(lyrics) / sizeof(lyrics[0]);
int currentLine = 0;
unsigned long startTime;

void setup() {
  Wire.begin();
  display.begin(SSD1306_SWITCHCAPVCC, 0x3C);
  display.clearDisplay();
  display.setTextSize(1);
  display.setTextColor(SSD1306_WHITE);
  startTime = millis();
}

void loop() {
  unsigned long currentTime = millis() - startTime;

  if (currentLine < numLines && currentTime >= lyricTimes[currentLine]) {
    display.clearDisplay();
    display.setCursor(0, 20);
    display.println(lyrics[currentLine]);
    display.display();
    currentLine++;
  }

  // Reset untuk looping setelah semua lirik tampil
  if (currentLine >= numLines) {
    delay(3000);  // jeda 3 detik sebelum mulai lagi (opsional)
    currentLine = 0;
    startTime = millis();  // mulai ulang waktu
  }
}
