# ⚡ Donanım Mimarisi ve Gömülü Sistemler Laboratuvarı

**Yazar:** Cengizhan Yalçın
**Konsept:** `sistemmimarisi.lab`

Bu depo, donanım temellerinden başlayarak gelişmiş sistem mimarisi, sinyal bütünlüğü ve haberleşme protokollerine kadar uzanan uygulamalı laboratuvar çalışmalarımın mühendislik kayıtlarını içermektedir. 

Çalışmaların temel felsefesi, elektronik bilgini yalnızca "devre elemanlarını tanıma" düzeyinde bırakmamak; bir cihazın analog girişinden güç katına, MCU ADC/DAC hattından haberleşme arayüzüne, PCB stackup yapısından EMC filtrelerine kadar tüm zinciri analiz edip tasarlayabilecek seviyeye çıkarmaktır[cite: 2].

## 🎯 Hedef Profil
Bu müfredatın sonunda ulaşılacak mühendislik yetkinlikleri[cite: 2]:
*   12-24 V endüstriyel bir sensör düğümünün giriş korumasını, güç mimarisini, analog ön ucunu, MCU bağlantılarını ve haberleşme arayüzlerini tasarlayabilmek[cite: 2].
*   ADC hattındaki gürültünün kaynağını frekans domeninde düşünmek ve analog/dijital filtre kararlarını gerekçelendirmek[cite: 2].
*   Farklı parazitik sönümleme (RC, LC, RLC, aktif filtre, CMC, TVS) elemanlarının kullanım koşullarına hakim olmak[cite: 2].
*   PCB yerleşim kararlarını; stackup, return current, plane, via, decoupling ve sinyal bütünlüğü (SI) üzerinden verebilmek[cite: 2].
*   Osiloskop, multimetre, mantık analizörü ve SPICE ortamlarında ölçüm tabanlı hata ayıklamak[cite: 2].

## 🗺️ Öğrenim Yol Haritası (Syllabus)

Laboratuvar süreci 6 farklı yetkinlik seviyesi (L1-L6) ve 12 modülden oluşmaktadır[cite: 2].

| Seviye | Ana Soru | Çıktı |
| :--- | :--- | :--- |
| **L1 - Temel** | Devre neden böyle davranıyor? | Ölçüm, Ohm/Kirchhoff, RC/RL/RLC[cite: 2] |
| **L2 - Analog** | Sinyali nasıl şartlandırırım? | Op-amp, aktif filtre, ADC sürme[cite: 2] |
| **L3 - Sistem** | MCU ile analog dünyayı nasıl kontrol ederim? | ADC/DAC, timer, DMA, SPI/I2C/UART/CAN[cite: 2] |
| **L4 - PCB** | Aynı devre PCB üzerinde neden bozuluyor? | Layout, return path, decoupling, SI[cite: 2] |
| **L5 - Endüstriyel** | Ürün gürültü altında neden çalışmaya devam eder? | EMI/EMC, ESD, surge/EFT, filtreleme, test[cite: 2] |
| **L6 - Tasarım Liderliği** | Bunu tekrar üretilebilir ürün haline nasıl getiririm? | DFM, DFT, BOM, revizyon, doğrulama[cite: 2] |

### 📂 Tamamlanan ve Planlanan Modüller

*   [x] **Modül 1:** Elektrik Temeli ve Laboratuvar Düşüncesi[cite: 2] (*[Detaylı Rapor](./Modul_01_Elektrik_Temeli/README.md)*)
*   [ ] **Modül 2:** Frekans Domeni ve Pasif Filtrelerin Temeli[cite: 2]
*   [ ] **Modül 3:** Diyotlar, BJT/MOSFET ve Analog Devre Blokları[cite: 2]
*   [ ] **Modül 4:** Aktif Filtreleme ve Analog Sinyal İşleme[cite: 2]
*   [ ] **Modül 5:** Güç Elektroniği ve Güç Bütünlüğü[cite: 2]
*   [ ] **Modül 6:** Gömülü Sistemi Donanımsal Olarak Kontrol Etme (ADC/DAC/DMA)[cite: 2]
*   [ ] **Modül 7:** PCB Tasarımı: Şemadan Üretilebilir Karta[cite: 2]
*   [ ] **Modül 8:** Signal Integrity ve Yüksek Kenar Hızlı Tasarım[cite: 2]
*   [ ] **Modül 9:** EMC/EMI ve Donanımsal Filtreleme[cite: 2]
*   [ ] **Modül 10:** Endüstriyel Haberleşme ve Korumalı I/O (RS-485/CAN)[cite: 2]
*   [ ] **Modül 11:** Ölçüm, Doğrulama ve Hata Ayıklama (Bring-up Prosedürleri)[cite: 2]
*   [ ] **Modül 12:** SPICE/Simülasyon ve Tasarım Doğrulama[cite: 2]

## 🚀 Final Projesi: Endüstriyel Sensör / Edge Node
Tüm modüllerin entegrasyonu ile geliştirilecek 4 katmanlı, 24V nominal girişli, EMC/ESD korumalı, anti-aliasing filtreli, DMA tabanlı veri toplayan ve dijital filtreleme yapan RS-485/CAN haberleşmeli endüstriyel uç nokta (edge node) cihazı[cite: 2].