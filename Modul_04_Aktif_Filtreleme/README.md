# Modül 4: Aktif Filtreleme, Analog Ön Uç (AFE) ve Sinyal Şartlandırma

## 📌 Modül Özeti ve Mühendislik Hedefi
Bu modül, sahadaki endüstriyel sensörlerden (örn: 4-20 mA basınç transmiterleri) gelen elektriksel olarak gürültülü ve uyumsuz sinyalleri, mikrodenetleyicinin (ESP32/STM32) ADC birimine "tertemiz ve güvenli" bir şekilde aktarmayı inceler. Temel hedef; sinyali voltaja çevirmek, voltaj sıçramalarından korunmak, yüksek frekanslı parazitleri (50 Hz şebeke, motor sürücü gürültüleri) donanımsal olarak ezmek ve ADC'nin örnekleme vuruntusuna (kickback) karşı sistemi sarsılmaz kılmaktır.

---

## 🛠️ Tasarım Analizi 1: Akım-Gerilim Dönüşümü ve Donanımsal Koruma

**Amaç:** Mikrodenetleyicilerin doğrudan okuyamadığı 4-20 mA endüstriyel akım sinyalini, ADC'nin 0-3.3V referans aralığına uygun, güvenli bir voltaj sinyaline dönüştürmek.

**Gerekli Donanımsal Elemanlar ve Topoloji:**
1.  **Şönt (Shunt) Direnci (Örn: $120 \Omega$):** Akım döngüsüne seri bağlanır. Ohm kanunu gereği $20 mA$ maksimum akımda üzerinde $2.40 V$ gerilim düşümü yaratır. Bu değer 3.3V beslemeli bir sistem için güvenli okuma aralığıdır. Toleransı çok düşük (%0.1) seçilmelidir.
2.  **TVS (Transient Voltage Suppressor) Diyot:** Sinyal hattı ile toprak arasına paralel bağlanır. Sensör kablosunun bir anten gibi topladığı yüksek voltajlı endüstriyel sıçramaları (Surge/EFT) op-amp'a ulaşmadan toprağa kısa devre edip sistemi korur.
3.  **Akım Sınırlayıcı Seri Direnç (Örn: $1 k\Omega$):** TVS diyot ile op-amp arasına yerleştirilir. TVS'nin kesemediği artık sızıntıların op-amp giriş pinlerini (ESD diyotlarını) yakmasını engeller.

---

## 🔬 Tasarım Analizi 2: Sallen-Key Aktif Alçak Geçiren Filtre (10 Hz)

**Amaç:** Sinyaldeki faydalı düşük frekanslı sensör verilerini (0-10 Hz) kayıpsız geçirirken, 50/60 Hz şebeke vınlamasını ve yüksek frekanslı anahtarlama gürültülerini pasif bir filtreye kıyasla çok daha keskin (-40 dB/decade) bir şekilde bastırmak.

**Altın Kurallar (Komponent Seçimi "Tatlı Noktası"):**
1.  **Önce Kondansatör (C) Seçilir:** Kaliteli (NP0/C0G) ve fiziksel sınırları makul olan $1 nF$ ile $1 \mu F$ arası bir değer sabitlenir. (PCB parazitiklerinden etkilenmemesi için $pF$ seviyesinden kaçınılır).
2.  **Sonra Direnç (R) Hesaplanır:** Direnç değeri $1 k\Omega$ ile $100 k\Omega$ arasına düşmelidir. $100 k\Omega$'dan büyük dirençler devasa **Termal Gürültü (Johnson Noise)** ve op-amp bias sızıntılarından dolayı **DC Ofset** hatası yaratır. Küçük dirençler ise op-amp'ı aşırı akım çekmeye zorlar.
3.  **Rail-to-Rail Çıkış (RRO) Zorunluluğu:** 3.3V tekil beslemede (Single-Supply) sıradan bir op-amp çıkışı 2.0V'da tıkanır. 2.40V'a çıkacak sensör sinyalinin tepe noktalarının **kırpılmaması (clipping)** için RRO özellikli op-amp şarttır.

**Topoloji ve SPICE Simülasyon Çıkarımları:**
*   **Negatif Geri Besleme (Buffer):** Çıkış (OUT) pini her zaman (-) inverting pine dönmelidir. Yanlışlıkla (+) pine dönen bir geri besleme, sistemi analog filtre olmaktan çıkarıp kilitli bir karşılaştırıcıya (Schmitt Trigger) dönüştürür (Simülasyonda sinyalin çökme sebebi).
*   **DC Ofset (Sanal Toprak):** Tek kaynaklı (0-3.3V) AC analizlerde sinyalin zemine (0V) çarpıp tıkanmaması için, test sinyali çalışma aralığının tam ortasına (örn: $1.65 V$) yükseltilmelidir.
*   **Bode Diyagramı Davranışı:** $R = 15.9 k\Omega$ ve $C = 1 \mu F$ ile kurulan Sallen-Key ağında, 10 Hz noktasında sinyal -3 dB seviyesine kırılır; 100 Hz ve ilerisinde ise parazitler mikrodenetleyicinin göremeyeceği kadar küçültülür.

---

## 🛠️ Tasarım Analizi 3: ADC Sürücü Katı (Charge Bucket)

**Amaç:** Mükemmel filtrelenmiş analog sinyali, mikrodenetleyici ADC'sinin okuma (Sample & Hold) anında hattan çektiği vahşi ve anlık akım darbelerinden (Kickback Noise) korumak.

**Topoloji ve Mekanizma:**
*   Op-amp çıkışı ile MCU pini arasına seri bir direnç (Örn: $33 \Omega$) ve pinden toprağa paralel bir kondansatör (Örn: $1 nF$) yerleştirilir.
*   **Kondansatörün Görevi (Charge Bucket):** Filtreleme yapmak için değil, ADC'nin örnekleme anında ihtiyaç duyduğu ani akımı karşılayan bir "şarj deposu" olarak çalışır. Op-amp'ın bu yüksek frekanslı akım talebine yetişemeyip voltajı çökertmesini (Ringing/Dalgalanma) engeller.
*   **Direncin Görevi:** Op-amp çıkışını doğrudan kapasitif bir yüke bağlamak op-amp'ı osilasyona (kararsızlığa) iter. Buradaki izolasyon direnci, kapasitif yükü op-amp'tan gizler ve sinyaldeki çınlamaları sönümler.