/*
  Quetzal Takımı X3 Aracı - Deneyap Kart Kontrol Kodu
  Bu kod, Raspberry Pi web sunucusundan seri port üzerinden gelen komutları dinler
  ve L298N motor sürücüsü kullanarak iki motorlu aracı kontrol eder.

  Son Güncelleme: Tüm sunucu özellikleriyle tam uyumlu.
*/

// --- Pin Tanımlamaları ---
// Bu pinleri kendi L298N bağlantınıza göre değiştirebilirsiniz.

// Sol Motor Kontrol Pinleri
const int sol_motor_ileri_pin = D2 ; // IN1
const int sol_motor_geri_pin  = D3; // IN2

// Sağ Motor Kontrol Pinleri
const int sag_motor_ileri_pin = D0; // IN3
const int sag_motor_geri_pin  = D1; // IN4

// Not: L298N üzerindeki ENA ve ENB pinlerinde jumper'ların takılı
// olduğundan emin olun. Bu, motorların tam hızda çalışmasını sağlar.

void ileri_git(int solHiz, int sagHiz);
void geri_git(int hiz = 50);
void sola_don(int hiz = 90);
void saga_don(int hiz = 90);
void motorlari_durdur();

void setup() {
  // Seri haberleşmeyi başlat. Baud rate, Python koduyla aynı olmalı (9600).
  Serial.begin(9600);
  while (!Serial); // Seri portun açılmasını bekle.

  // Seri monitöre hazır olduğumuza dair bir mesaj gönderelim.
  Serial.println("======================================");
  Serial.println("Quetzal X3 Araç Kontrol Kartı Hazır.");
  Serial.println("Sunucudan komut bekleniyor...");
  Serial.println("======================================");
  
  // Tüm motor kontrol pinlerini çıkış (OUTPUT) olarak ayarla.
  pinMode(sol_motor_ileri_pin, OUTPUT);
  pinMode(sol_motor_geri_pin, OUTPUT);
  pinMode(sag_motor_ileri_pin, OUTPUT);
  pinMode(sag_motor_geri_pin, OUTPUT);

  // Güvenlik için başlangıçta tüm motorları durdur.
  motorlari_durdur();
}

void loop() {
  // USB (seri port) üzerinden okunacak veri var mı diye kontrol et.
  if (Serial.available() > 0) {
    // Yeni satır karakterine ('\n') kadar gelen veriyi bir String olarak oku.
    // Python sunucumuz her komutun sonuna '\n' eklediği için bu yöntem güvenilirdir.
    String komut = Serial.readStringUntil('\n');
    
    // Gelen komutun başındaki/sonundaki olası boşlukları veya karakterleri temizle.
    komut.trim(); 

    // Gelen komutu doğrulama ve hata ayıklama için seri monitöre yazdır.
    Serial.print("Sunucudan Komut Alındı -> ");
    Serial.println(komut);

    // Gelen komuta göre ilgili motor fonksiyonunu çağır.
    if (komut == "MANUEL_ILERI") {
      ileri_git(50, 50);
    } 
    else if (komut == "MANUEL_GERI") {
      geri_git(50);
    }
    else if (komut == "MANUEL_SOL") {
      sola_don(90);
    }
    else if (komut == "MANUEL_SAG") {
      saga_don(90);
    }
    // MANUEL_DUR, GOREVI_DURDUR ve ACIL_DURDUR komutlarının hepsi aracı durdurur.
    else if (komut == "MANUEL_DUR" || komut == "ACIL_DURDUR" || komut == "GOREVI_DURDUR") {
      motorlari_durdur();
    }
    else {
      // Beklenmedik bir komut gelirse, seri monitöre bilgi yazdır.
      Serial.print("Bilinmeyen komut alındı: '");
      Serial.print(komut);
      Serial.println("'");
    }
  }
}

// === MOTOR KONTROL FONKSİYONLARI ===

// Aracı ileri hareket ettirir
void ileri_git(int solHiz, int sagHiz) {
  if (solHiz >= 0) {
    analogWrite(sol_motor_ileri_pin, solHiz);
    analogWrite(sol_motor_geri_pin, 0);
  } else {
    analogWrite(sol_motor_ileri_pin, 0);
    analogWrite(sol_motor_geri_pin, -solHiz);
  }

  if (sagHiz >= 0) {
    analogWrite(sag_motor_ileri_pin, sagHiz);
    analogWrite(sag_motor_geri_pin, 0);
  } else {
    analogWrite(sag_motor_ileri_pin, 0);
    analogWrite(sag_motor_geri_pin, -sagHiz);
  }
}

void geri_git(int hiz) {
  analogWrite(sol_motor_ileri_pin, 0);
  analogWrite(sol_motor_geri_pin, hiz);
  analogWrite(sag_motor_ileri_pin, 0);
  analogWrite(sag_motor_geri_pin, hiz);
}

void sola_don(int hiz) {
  analogWrite(sol_motor_ileri_pin, 0);
  analogWrite(sol_motor_geri_pin, hiz);
  analogWrite(sag_motor_ileri_pin, hiz);
  analogWrite(sag_motor_geri_pin, 0);
}

void saga_don(int hiz) {
  analogWrite(sol_motor_ileri_pin, hiz);
  analogWrite(sol_motor_geri_pin, 0);
  analogWrite(sag_motor_ileri_pin, 0);
  analogWrite(sag_motor_geri_pin, hiz);
}

void motorlari_durdur() {
  analogWrite(sol_motor_ileri_pin, 0);
  analogWrite(sol_motor_geri_pin, 0);
  analogWrite(sag_motor_ileri_pin, 0);
  analogWrite(sag_motor_geri_pin, 0);
}
