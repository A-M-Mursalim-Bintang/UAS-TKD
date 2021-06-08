#include <DS3231.h>
#include <Wire.h>
#include <LiquidCrystal_I2C.h>

LiquidCrystal_I2C lcd(0x27, 16, 2);

// Init the DS3231 using the hardware interface
DS3231 rtc(SDA, SCL);

int button = 2;
int count = 1;

void setup() {

  // Setup Serial connection
  Serial.begin(115200);
  // Uncomment the next line if you are using an Arduino Leonardo
  //while (!Serial) {}

  // Initialize the rtc object
  rtc.begin();

  // The following lines can be uncommented to set the date and time
    rtc.setDOW(MONDAY);     // Set Day-of-Week to SUNDAY
    rtc.setTime(8, 5, 0);     // Set the time to 12:00:00 (24hr format)
    rtc.setDate(6, 7, 2021);   // Set the date to January 1st, 2014

  // initialize the lcd
  lcd.begin();
  // Print a message to the LCD.
  lcd.backlight();

  pinMode(button, INPUT);

}

void loop() {
  if (digitalRead(button) == HIGH) {
    count++;

    if (count == 1) {
      lcd.clear();
      delay(5);

      // Send Temp
      Serial.print("Temperature: ");
      Serial.print(rtc.getTemp());
      Serial.println(" C");
      

      lcd.setCursor(0, 1);
      lcd.print("Temp: ");
      lcd.setCursor(7, 1);
      lcd.print(rtc.getTemp());
      

      // Send time
      Serial.println(rtc.getTimeStr());
      lcd.setCursor(0, 0);
      lcd.print("Time:  ");
      lcd.print(rtc.getTimeStr());
      delay(3000);

    }

    else {
      lcd.clear();
      delay(5);
      // Send Day-of-Week
      Serial.print(rtc.getDOWStr());
      Serial.println(" ");
      lcd.setCursor(0, 0);
      lcd.print(rtc.getDOWStr());
//      delay(500);

      Serial.println(rtc.getDateStr());
      lcd.setCursor(0, 1);
      lcd.print("Date: ");
      lcd.print(rtc.getDateStr());
      delay(3000);

      count = 0;
    }

  }
}
