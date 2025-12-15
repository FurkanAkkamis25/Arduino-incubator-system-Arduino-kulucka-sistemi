# 🐣 Arduino Akıllı Kuluçka Kontrol Sistemi (Smart Incubator)

Bu proje, Arduino Uno kullanarak geliştirilmiş; sıcaklık sabitleme, soğutma ve sesli alarm özelliklerine sahip bir **Otomatik Kuluçka Kontrol Sistemi** simülasyonudur.

![Devre Şeması](devre_semasi.png)

## 🌟 Özellikler
* **Hassas Sıcaklık Kontrolü:** Ortam sıcaklığını sürekli ölçer ve 0.5°C hassasiyetle tepki verir.
* **Ayarlanabilir Hedef:** Potansiyometre ile hedef sıcaklık 35°C - 40°C güvenli aralığında ayarlanabilir.
* **Otomatik Isıtma & Soğutma:**
    * Sıcaklık düştüğünde: **Isıtıcı (LED)** devreye girer.
    * Sıcaklık yükseldiğinde: **Fan (Motor)** devreye girer.
* **🚨 Acil Durum Alarmı:** Sıcaklık, belirlenen hedefi 2°C'den fazla aşarsa Buzzer sesli uyarı verir.
* **LCD Gösterge:** Anlık sıcaklık ve hedef sıcaklık değerleri 16x2 LCD ekranda görüntülenir.

## 🛠 Kullanılan Malzemeler
* Arduino Uno R3
* TMP36 Sıcaklık Sensörü
* LCD 16x2 Ekran
* DC Motor (Fan olarak) + NPN Transistör
* Kırmızı LED (Isıtıcı olarak)
* Piezo Buzzer (Alarm)
* 2x Potansiyometre (Biri ekran kontrastı, biri ısı ayarı için)
* Dirençler (220Ω, 1kΩ)

## 🚀 Kurulum ve Kullanım
1.  Devreyi şemada gösterildiği gibi kurun.
2.  `Kulucka_Sistemi.ino` dosyasını Arduino IDE ile açın.
3.  Kodu Arduino kartınıza yükleyin.
4.  Sisteme güç verin.
5.  Ayar düğmesini (Potansiyometre) kullanarak hedef sıcaklığı (Örn: 37.5°C) belirleyin.

## ⚙️ Kod Mantığı
Sistem, `loop` döngüsü içinde şu adımları izler:
1.  Sensörden sıcaklığı oku.
2.  Potansiyometreden hedef değeri oku.
3.  Değerleri LCD'ye yazdır.
4.  **Karar Ver:**
    * `Mevcut < Hedef - 0.5` -> Isıtıcı AÇ
    * `Mevcut > Hedef + 0.5` -> Fan AÇ
    * `Mevcut > Hedef + 2.0` -> **ALARM ÇAL!**

## 📷 Ekran Görüntüleri

/Users/furkanakkamis/Desktop/Arduino_kulucka_sistemi/devreSemasi.png/Ekran Resmi 2025-12-15 16.56.21.png
/Users/furkanakkamis/Desktop/Arduino_kulucka_sistemi/devreSemasi.png/Ekran Resmi 2025-12-15 16.56.36.png
/Users/furkanakkamis/Desktop/Arduino_kulucka_sistemi/devreSemasi.png/Ekran Resmi 2025-12-15 16.57.03.png
---
**Geliştirici:** [Furkan Akkamış]