# ⚡ Donanım Mimarisi ve Gömülü Sistemler Laboratuvarı

**Yazar:** Cengizhan Yalçın
**Konsept:** `sistemmimarisi.lab`

Bu depo, donanım temellerinden başlayarak gelişmiş sistem mimarisi, sinyal bütünlüğü ve haberleşme protokollerine kadar uzanan uygulamalı laboratuvar çalışmalarımın mühendislik kayıtlarını içermektedir. 

Çalışmaların temel felsefesi, elektronik bilgini yalnızca "devre elemanlarını tanıma" düzeyinde bırakmamak; bir cihazın analog girişinden güç katına, MCU ADC/DAC hattından haberleşme arayüzüne, PCB stackup yapısından EMC filtrelerine kadar tüm zinciri analiz edip tasarlayabilecek seviyeye çıkarmaktır.

## 🎯 Hedef Profil
Bu müfredatın sonunda ulaşılacak mühendislik yetkinlikleri:
-   12-24 V endüstriyel bir sensör düğümünün giriş korumasını, güç mimarisini, analog ön ucunu, MCU bağlantılarını ve haberleşme arayüzlerini tasarlayabilmek.
-   ADC hattındaki gürültünün kaynağını frekans domeninde düşünmek ve analog/dijital filtre kararlarını gerekçelendirmek.
-   Farklı parazitik sönümleme (RC, LC, RLC, aktif filtre, CMC, TVS) elemanlarının kullanım koşullarına hakim olmak.
-   PCB yerleşim kararlarını; stackup, return current, plane, via, decoupling ve sinyal bütünlüğü (SI) üzerinden verebilmek.
-   Osiloskop, multimetre, mantık analizörü ve SPICE ortamlarında ölçüm tabanlı hata ayıklamak.

## 🗺️ Öğrenim Yol Haritası (Syllabus)

Laboratuvar süreci 6 farklı yetkinlik seviyesi (L1-L6) ve 12 modülden oluşmaktadır.

| Seviye | Ana Soru | Çıktı |
| :--- | :--- | :--- |
| **L1 - Temel** | Devre neden böyle davranıyor? | Ölçüm, Ohm/Kirchhoff, RC/RL/RLC |
| **L2 - Analog** | Sinyali nasıl şartlandırırım? | Op-amp, aktif filtre, ADC sürme |
| **L3 - Sistem** | MCU ile analog dünyayı nasıl kontrol ederim? | ADC/DAC, timer, DMA, SPI/I2C/UART/CAN |
| **L4 - PCB** | Aynı devre PCB üzerinde neden bozuluyor? | Layout, return path, decoupling, SI |
| **L5 - Endüstriyel** | Ürün gürültü altında neden çalışmaya devam eder? | EMI/EMC, ESD, surge/EFT, filtreleme, test |
| **L6 - Tasarım Liderliği** | Bunu tekrar üretilebilir ürün haline nasıl getiririm? | DFM, DFT, BOM, revizyon, doğrulama |

### 📂 Tamamlanan ve Planlanan Modüller

### ✅ MODÜL 1: Elektrik Temelleri ve Laboratuvar Düşüncesi
**Durum:** Tamamlandı ([Modül 1 Raporuna Git](./Modul_01_Sinyal_ve_Olcum/README.md))

### ✅ MODÜL 2: Frekans Domeni ve Pasif Filtre Mimarisi
**Durum:** Tamamlandı ([Modül 2 Raporuna Git](./Modul_02_Frekans_Domeni/README.md))
-   **Hedefler:** Kompleks Empedans Analizi, RC/RLC Frekans Cevabı, Bode Diyagramı Pratiği, Kesim ve Rezonans Frekansları.
-   **Geçme Kriteri (MASTER):** Bir devrenin frekansa vereceği tepkiyi öngörebilmek ve filtre topolojisini (LP/HP/Band-Pass) sistem gereksinimine göre gerekçelendirebilmek.

### ✅ MODÜL 3: Aktif Yarı İletkenler, Anahtarlama ve Op-Amp Mimarisi
**Durum:** Tamamlandı ([Modül 3 Raporuna Git](./Modul_03_Aktif_Yari_Iletkenler/README.md))
-   **Hedefler:** P-N Eklemi (İleri Gerilim Düşümü), MOSFET Low-Side Switch, Endüktif Yük Koruması (Flyback), Op-Amp Negatif/Pozitif Geri Besleme (Amplifier & Schmitt Trigger).
-   **Geçme Kriteri (MASTER):** Mikrodenetleyici çıkış sinyallerini izole edip yükselterek güçlü endüstriyel yükleri güvenli şekilde anahtarlayabilmek ve sensör/okuma hatalarını donanımsal olarak engelleyebilmek.

### ✅ MODÜL 4: Aktif Filtreleme ve Analog Ön Uç (AFE) Tasarımı
**Durum:** Tamamlandı ([Modül 4 Raporuna Git](./Modul_04_Analog_On_Uc/README.md))
-   **Hedefler:** 4-20 mA Sensör Okuma Sinyal Zinciri, Sallen-Key Alçak Geçiren Filtre Topolojisi, Rail-to-Rail Op-Amp Seçimi, SPICE AC/Zaman Simülasyonları, ADC Kickback Gürültüsü ve Charge Bucket Koruması.
-   **Geçme Kriteri (MASTER):** Endüstriyel bir sensörden gelen elektriksel olarak kirli sinyali; ideal olmayan op-amp kısıtlarını, sızıntı akımlarını ve termal gürültüleri hesaba katarak SPICE ortamında donanımsal düzeyde filtreleyebilmek ve mikrodenetleyicinin ADC'sini sarsıntı yaratmadan güvenle sürebilmek.

-   [ ] **Modül 5:** Güç Elektroniği ve Güç Bütünlüğü
-   [ ] **Modül 6:** Gömülü Sistemi Donanımsal Olarak Kontrol Etme (ADC/DAC/DMA)
-   [ ] **Modül 7:** PCB Tasarımı: Şemadan Üretilebilir Karta
-   [ ] **Modül 8:** Signal Integrity ve Yüksek Kenar Hızlı Tasarım
-   [ ] **Modül 9:** EMC/EMI ve Donanımsal Filtreleme
-   [ ] **Modül 10:** Endüstriyel Haberleşme ve Korumalı I/O (RS-485/CAN)
-   [ ] **Modül 11:** Ölçüm, Doğrulama ve Hata Ayıklama (Bring-up Prosedürleri)
-   [ ] **Modül 12:** SPICE/Simülasyon ve Tasarım Doğrulama

## 🚀 Final Projesi: Endüstriyel Sensör / Edge Node
Tüm modüllerin entegrasyonu ile geliştirilecek 4 katmanlı, 24V nominal girişli, EMC/ESD korumalı, anti-aliasing filtreli, DMA tabanlı veri toplayan ve dijital filtreleme yapan RS-485/CAN haberleşmeli endüstriyel uç nokta (edge node) cihazı.