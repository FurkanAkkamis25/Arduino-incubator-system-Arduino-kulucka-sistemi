#include <LiquidCrystal.h>

// --- PIN AYARLARI ---
LiquidCrystal lcd(12, 11, 5, 4, 3, 2);

const int sensorPin = A0;    // TMP36 Sıcaklık Sensörü
const int ayarPin = A1;      // Hedef Sıcaklık Düğmesi
const int isiticiPin = 8;    // Kırmızı LED (Isıtıcı)
const int fanPin = 9;        // DC Motor (Fan - Soğutucu)
const int buzzerPin = 7;     // ALARM (Piezo Hoparlör) - YENİ!

// --- KULUÇKA AYARLARI ---
float tolerans = 0.5;        // Normal çalışma aralığı
float alarmSiniri = 2.0;     // Hedefi kaç derece geçerse alarm çalsın?

void setup() {
  pinMode(isiticiPin, OUTPUT);
  pinMode(fanPin, OUTPUT);
  pinMode(buzzerPin, OUTPUT); // Buzzer bir çıkış elemanıdır
  
  lcd.begin(16, 2);
  lcd.print("Sistem Aktif");
  delay(1000);
  lcd.clear();
}

void loop() {
  // 1. MEVCUT SICAKLIĞI OKU
  int sensorDeger = analogRead(sensorPin);
  float voltaj = sensorDeger * 0.0048828125; 
  float sicaklik = (voltaj - 0.5) * 100.0;

  // 2. HEDEF SICAKLIĞI BELİRLE (35-40 Arası)
  int potDeger = analogRead(ayarPin);
  float hedefSicaklik = map(potDeger, 0, 1023, 350, 400) / 10.0; 

  // 3. EKRANA YAZDIR
  lcd.setCursor(0, 0);
  lcd.print("Isi:");
  lcd.print(sicaklik);
  lcd.print("C ");

  lcd.setCursor(0, 1);
  lcd.print("Hdf:");
  lcd.print(hedefSicaklik);
  lcd.print("C ");

  // --- ALARM KONTROLÜ (YENİ BÖLÜM) ---
  // Eğer sıcaklık, hedeften 2 derece daha yüksekse -> ALARM ÇAL!
  if (sicaklik >= hedefSicaklik + alarmSiniri) {
    lcd.setCursor(14, 1);
    lcd.print("!!"); // Ekrana ünlem koy
    
    // Kesik kesik "Düt Düt" sesi
    tone(buzzerPin, 1000); // 1000 Hz ses ver
    delay(100);
    noTone(buzzerPin);     // Sus
    delay(100);
    // Not: Buradaki delay sistemi biraz yavaşlatabilir ama uyarı için gereklidir.
  } 
  else {
    noTone(buzzerPin); // Tehlike yoksa sus
    lcd.setCursor(14, 1);
    lcd.print("  ");   // Ünlemi sil
  }

  // 4. TERMOSTAT KONTROLÜ
  if (sicaklik < hedefSicaklik - tolerans) {
    digitalWrite(isiticiPin, HIGH); 
    digitalWrite(fanPin, LOW);
  }
  else if (sicaklik > hedefSicaklik + tolerans) {
    digitalWrite(isiticiPin, LOW);
    digitalWrite(fanPin, HIGH);
  }
  else {
    digitalWrite(isiticiPin, LOW);
    digitalWrite(fanPin, LOW);
  }

  // Alarm varken delay kullanıldığı için buradaki genel beklemeyi kısalttık
  delay(100); 
}