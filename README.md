#include <Wire.h>
#include <LiquidCrystal_I2C.h>
#include <DHT.h>

// Khai bao chan cam bien
#define DHTPIN 4
#define DHTTYPE DHT22

// Khoi tao DHT22
DHT dht(DHTPIN, DHTTYPE);

// Khoi tao LCD I2C
LiquidCrystal_I2C lcd(0x27, 16, 2);

void setup() {
  Serial.begin(115200);

  // Khoi dong cam bien
  dht.begin();

  // Khoi dong LCD
  Wire.begin(21, 22);
  lcd.init();
  lcd.backlight();

  lcd.setCursor(0, 0);
  lcd.print("ESP32 + DHT22");

  lcd.setCursor(0, 1);
  lcd.print("Dang khoi dong");

  delay(2000);
  lcd.clear();
}

void loop() {

  // Doc nhiet do va do am
  float nhietDo = dht.readTemperature();
  float doAm = dht.readHumidity();

  // Kiem tra loi doc cam bien
  if (isnan(nhietDo) || isnan(doAm)) {
    lcd.clear();
    lcd.setCursor(0, 0);
    lcd.print("Loi cam bien!");
    Serial.println("Loi doc DHT22");

    delay(2000);
    return;
  }

  // Hien thi tren Serial Monitor
  Serial.print("Nhiet do: ");
  Serial.print(nhietDo);
  Serial.print(" C | Do am: ");
  Serial.print(doAm);
  Serial.println(" %");

  // Hien thi tren LCD
  lcd.setCursor(0, 0);
  lcd.print("Nhiet do: ");
  lcd.print(nhietDo, 1);
  lcd.print((char)223);
  lcd.print("C  ");

  lcd.setCursor(0, 1);
  lcd.print("Do am: ");
  lcd.print(doAm, 1);
  lcd.print("%   ");

  delay(2000);
}
