# Modül 2: Frekans Domeni, Empedans ve Pasif Filtre Mimarisi

## 📌 Modül Özeti ve Mühendislik Hedefi
Bu modül, elektronik komponentlerin zaman domenindeki (şarj/deşarj) davranışlarından frekans domenindeki (empedans) davranışlarına geçişi inceler. Direncin frekansa karşı sabit kalırken, kondansatör ve bobinin frekansa duyarlı (bukalemun) reaksiyonları analiz edilmiştir. Hedef, endüstriyel tasarımlarda sinyal şartlandırma ve gürültü bağışıklığı için doğru pasif filtre (Low-Pass, High-Pass, RLC Notch) mimarilerini kurabilmektir.

---

## 🔬 Deney 1: RC Alçak Geçiren (Low-Pass) Filtre ve -3dB Noktası

**Amaç:** Kondansatörün yüksek frekanslarda düşük empedans gösterme özelliğinden yararlanarak, devrenin "Kesim Frekansını" (fc) tespit etmek.
**Kurulum:** PWM sinyal kaynağı, R (4.61 kOhm) seri bağlantı, C (0.47 uF) toprağa paralel bağlantı.
**Teorik Kesim Frekansı (fc = 1 / 2πRC):** 73.45 Hz

**Frekans Cevabı (Bode) Gözlemleri:**
*   **10 Hz (Düşük Frekans):** Kondansatör açık devre gibi davrandı. Sinyal kayıpsız (~5.0V) çıkışa ulaştı.
*   **75 Hz - 260 Hz (Geçiş Bölgesi):** Sinüs dalgaları için teorik ezilme noktası olan ~74 Hz, kare dalganın (PWM) şarj dinamikleri nedeniyle direnç gösterdi. Sinyalin 3.5V seviyesine gerilemesi ( -3dB çöküşü) ~260 Hz bandında fiziksel olarak doğrulandı.
*   **1 kHz (Yüksek Frekans):** Kondansatör kısa devreye yaklaştı, genlik 2.85V'a kadar ezilerek sönümlendi.

**Sistem Mimarisi Çıkarımı:** Bu topoloji, ESP32-S3 gibi mikrodenetleyicilerin ADC pinleri ile analog sensör okuması yaparken hayati önem taşır. Donanımsal "Anti-aliasing" filtresi olarak kurulan bu yapı, ortamdaki Wi-Fi veya motor fırça gürültülerini (yüksek frekans) toprağa yutarken, asıl sensör verisini (DC'ye yakın düşük frekans) MCU'ya kayıpsız ulaştırır.

---

## 🔬 Deney 2: Ayna Etkisi ve RC Yüksek Geçiren (High-Pass) Filtre

**Amaç:** Sinyal yolundaki DC (0 Hz) ve düşük frekanslı bileşenleri bloke edip, sadece anlık AC değişimlerin geçişine izin vermek.
**Kurulum:** Topoloji tersine çevrildi; C (0.47 uF) sinyal yoluna seri, R (4.61 kOhm) toprağa paralel bağlandı.

**Frekans Cevabı (Bode) Gözlemleri:**
*   **10 Hz (Düşük Frekans):** Sinyal, kondansatörün "DC Blokaj" duvarına çarptı ve genlik 0.5V'a kadar sönümlenerek geçit verilmedi.
*   **1 kHz (Yüksek Frekans):** Empedansın düşmesiyle yol açıldı ve sinyal 2.9V genliğine şahlanarak çıkışa ulaştı. Dalga formu, osiloskobun "AC Coupling" modundaki iğne (spike) dalgalarına dönüştü.

**Sistem Mimarisi Çıkarımı:** Sensör veya mikrofon sinyallerinin üzerine binen istenmeyen DC offset voltajlarını silmek için kullanılır. Sinyal yoluna seri bağlanan kondansatör, DC zemini tamamen engellerken yüksek frekanslı titreşim verilerini işlemciye geçirir.

---

## 🔬 Analiz 3: RLC Seri Rezonans ve Endüstriyel Frenleme

**Amaç:** Bobin (L) ve Kondansatörün (C) zıt kutuplu empedans tepkilerinin, spesifik bir frekansta birbirini sönümleyerek (rezonans) maksimum enerji transferine izin vermesini analiz etmek.
**Sanal Kurulum:** 100 Ohm Direnç, 10 mH Bobin ve 0.47 uF Kondansatör seri olarak bağlandı.

**Rezonans ve Frekans Dinamikleri:**
*   **Düşük Frekans (Örn: 100 Hz):** Kondansatör (C) yüksek empedans göstererek sinyali bloke etti.
*   **Yüksek Frekans (Örn: 10 kHz):** Bobin (L), manyetik Lenz tepkisi (zıt EMK) göstererek yüksek hızlı değişimi frenledi ve sinyali bloke etti.
*   **Rezonans Frekansı (f0 = 2.32 kHz):** L ve C'nin ürettiği ters kuvvetler birbirini tam olarak eşitledi. Devre içi reaktif direnç sıfırlandı ve sinyal %100 genlikle kayıpsız geçiş yaptı.

**Endüstriyel Uygulama (Araç Kontrol & Otomasyon):** 
Bu dinamik iki farklı stratejide kullanılır:
1.  **Güç Filtreleme:** KiCad üzerinde anahtarlamalı (Buck) güç kaynağı tasarlanırken, sinyal yoluna seri Bobin (L) ve paralel Kondansatör (C) atılır. DC akım bobinden kayıpsız geçerken, 100 kHz'lik anahtarlama gürültüleri bobinin manyetik duvarına çarpıp elenir.
2.  **Notch (Çentik) Filtreleri:** Fabrika ortamlarındaki CAN Bus veya Modbus hatlarında, 50 Hz'lik devasa şebeke/motor gürültüsünü engellemek için f0 = 50 Hz ayarlı RLC filtreleri sinyal ile GND arasına paralel bağlanır. Devre sadece 50 Hz'i toprağa yutarak haberleşme sinyallerini kirlilikten korur.


## 📐 Endüstriyel Tasarım Notları: Gerçek Komponent Davranışları

Fiziksel breadboard ölçümlerimizin ötesinde, endüstriyel PCB tasarımına (KiCad vb.) geçerken ideal formüllerin dışına çıkmamıza neden olan iki kritik donanım gerçeği incelenmiştir.

### 1. Parazitik Etkiler: ESR, ESL ve Öz-Rezonans (SRF)
İdeal bir hesaplamada, kondansatörün empedansı frekans arttıkça sonsuza dek sıfıra (kısa devreye) doğru düşmelidir. Ancak gerçek komponentlerin iç yapısında her zaman parazitik bir direnç (ESR - Eşdeğer Seri Direnç) ve parazitik bir bobin etkisi (ESL - Eşdeğer Seri İndüktans) saklıdır[cite: 1]. 

Frekans çok yüksek seviyelere (MHz-GHz bandına) ulaştığında, formüldeki bu gizli ESL devreye girer. Kondansatör bir noktadan sonra (SRF - Self Resonant Frequency) kapasitif filtreleme yeteneğini kaybederek tamamen bir bobin gibi davranmaya ve gürültüyü geçirmeye başlar[cite: 1].

**Sistem Mimarisi Çıkarımı (Decoupling):** İşlemcilerin (örn. STM32, ESP32) güç pinlerine filtreleme yaparken sadece bir adet büyük 10 $\mu$F kondansatör takıp geçemeyiz. Büyük kondansatörlerin ESL değeri yüksek olduğundan yüksek frekanslı anahtarlama gürültülerini kaçırırlar. Bu yüzden işlemcinin dibine fiziksel olarak çok küçük kılıflı, düşük ESL'ye sahip 100 nF veya 10 nF seramik kondansatörleri paralel bağlayarak frekans yanıtını dengeleriz.

### 2. Ferrit Boncuk (Ferrite Bead) ve RF Bastırma Sezgisi
Standart bobinler (Inductor) yüksek frekanslı gürültüye karşı manyetik bir duvar örer ve gürültü enerjisini devrede tutarak geri yansıtır. Ancak bir devrede yüksek frekanslı (RF) kirliliği tamamen yok etmek istediğimizde standart bobin yerine **Ferrit Boncuk (Ferrite Bead)** kullanılır[cite: 1]. 

Ferrit boncuklar, düşük frekanslarda normal bir tel gibi davranırken, yüksek frekanslı parazit akımlarına maruz kaldıklarında rezistif (dirençsel) bir özellik gösterirler. Yani gürültüyü engellemekle kalmaz, gürültü enerjisini doğrudan kendi üzerlerinde **ısıya çevirerek sistemden silerler**[cite: 1]. Yüksek frekanslı (HF) diferansiyel gürültüleri yutmak için idealdirler[cite: 1].

**Sistem Mimarisi Çıkarımı (Analog/Dijital İzolasyon):** İşlemcinin dijital güç hatlarındaki (DVDD) karmaşanın, hassas ADC okumaları yapan analog güç hatlarına (AVDD) sızmasını engellemek için bu iki bölge arasına ferrit boncuk yerleştirilir. Standart bir LC filtresindeki gibi tehlikeli çınlama (ringing) sorunlarına yol açmadan analog bölgeyi pürüzsüz tutar. Filtreler sadece gürültüyü yok eden parçalar değil, empedansı şekillendiren ağlardır[cite: 1].

---
*Sonuç: Modül 2 kapsamındaki frekans cevapları, rezonans, ESR/ESL parazitikleri ve ferrit boncuk gibi RF bastırma tekniklerinin teorik/pratik temelleri atılmıştır.*[cite: 1]
---
*Bu laboratuvar modülü, sinyalin hızına karşı donanımın verdiği fiziksel tepkileri kanıtlamış ve devre topolojilerinin rastgele değil, frekans hedeflerine göre tasarlandığını teyit etmiştir.*