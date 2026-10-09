# Modül 3: Aktif Yarı İletkenler, Anahtarlama ve Op-Amp Mimarisi

## 📌 Modül Özeti ve Mühendislik Hedefi
Bu modül, elektriğe yön ve şart koşabildiğimiz aktif yarı iletken bileşenleri (Diyotlar, BJT, MOSFET, Op-Amp) inceler. Temel hedef; zayıf lojik sinyalleri (örn: 3.3V ESP32 pinleri) kullanarak devasa endüstriyel yükleri (motor, röle, valf) anahtarlayabilmek ve ortamdaki elektriksel gürültülere karşı sistemi donanımsal düzeyde (Flyback, Schmitt Trigger) korumaktır.

---

## 🔬 Deney 1: Diyotlar ve P-N Eklemi "Geçiş Ücreti" ($V_f$)

**Amaç:** Elektrik akımının tek yönlü geçişine izin veren diyotların, sistem mimarisindeki voltaj düşümü maliyetini (Forward Voltage Drop) hesaplamak ve ters kutuplama davranışını gözlemlemek.

**Topoloji ve Sonuçlar:**
*   **İleri Kutuplama (Forward Bias):** 5V güç kaynağı, 1k$\Omega$ direnç ve 1N4007 (silisyum) diyot seri bağlandı. Osiloskop ölçümünde diyot üzerinde tam **~0.7V** gerilim düşümü (V_f) gözlemlendi. Geriye kalan 4.3V dirence aktarıldı.
*   **Ters Kutuplama (Reverse Bias):** Diyot ters çevrildiğinde (katot kaynağa doğru), P-N eklemi açık devreye dönüştü. Direnç üzerindeki gerilim **0V** olarak ölçüldü; sistemin ters akıma karşı tam bir blokaj sağladığı kanıtlandı.

**Sistem Mimarisi Çıkarımı:** Seri bağlanan diyotlar mantık (lojik) sinyallerinde (örn. 3.3V) ciddi voltaj erimesine neden olur. Bu tarz korumalarda 0.7V yerine, ~0.2V düşüme sahip **Schottky Diyotlar** tercih edilmelidir. Dış dünyaya açılan pinlerde yüksek gerilim sıçramalarına karşı kalkan görevi gören diyotlara ise **TVS (Transient Voltage Suppressor)** denir.

---

## 🛠️ Tasarım Analizi 1: MOSFET ile Endüstriyel "Low-Side Switch"

**Amaç:** Bir mikrodenetleyicinin (MCU) 3.3V çıkış sinyali ile 24V ve yüksek akım çeken endüktif bir yükü (Örn: DC Motor, Röle) güvenli şekilde anahtarlamak.

**Gerekli Donanımsal Koruma Elemanları:**
1.  **Gate Direnci (Örn: 100 $\Omega$):** MOSFET'in gate ucu devasa bir kapasitördür. MCU pini "HIGH" olduğunda oluşacak anlık aşırı şarj akımını (kısa devre etkisini) sınırlar ve MCU pinini yanmaktan kurtarır.
2.  **Pull-Down Direnci (Örn: 10 k$\Omega$):** MCU reset (boot) anındayken veya pin tanımlanmadığında (Floating/Yüzen pin durumu), MOSFET gate'ine sızacak çevresel parazitleri (hayalet voltajları) toprağa deşarj eder. Motorun kendi kendine çalışmasını engeller.
3.  **Flyback (Serbest Geçiş) Diyotu:** Endüktif yük (motor/bobin) enerjisi kesildiğinde manyetik alan çöker ve zıt yönde devasa bir voltaj (Zıt EMK) üretir. Motora ters yönde (katodu 24V'a bakacak şekilde) bağlanan bu diyot, o yıkıcı voltaj şokunu (-300V) MOSFET'e ulaşmadan sönümler. Sistem güvenliği için **kritiktir.**

---

## 🔬 Tasarım Analizi 2: İşlemsel Yükselteçler (Op-Amp) Mimarisi

**Amaç:** Zayıf (milivolt seviyesindeki) sensör sinyallerini işlenebilir lojik seviyelere yükseltmek (Amplifier) ve sinyal parazitlerini yenerek donanımsal karar algoritmaları kurmak (Comparator/Schmitt Trigger).

**Altın Kurallar:**
1.  Op-Amp girişleri (+ ve -) sonsuz empedansa sahiptir, sensörden akım çekmez.
2.  Negatif geri beslemede Op-Amp, (+ ve -) giriş voltajlarını eşitlemek için çıkış voltajını ayarlar.

### 1. Sinyal Yükseltme (Non-Inverting Amplifier)
*   **Topoloji:** Sensör (+) girişte. Çıkış, R2 ve R1 direnç bölücü ağıyla (-) girişine (Negatif Geri Besleme) bağlanır.
*   **Davranış:** Op-Amp eşitleme prensibini uygulamak için çıkışa çok daha yüksek bir voltaj basar.
*   **Endüstriyel Kullanım:** Load cell, termokupl gibi zayıf analog çıkış veren sensörleri MCU'nun ADC aralığına (0-3.3V) eşitlemek için kullanılır. (Kazanç formülü: $1 + (R2 / R1)$).

### 2. Gürültü Bağışıklığı (Schmitt Trigger / Histerezis)
*   **Problem:** Geri beslemesiz (açık çevrim) saf bir karşılaştırıcıda, sensör sinyalindeki minik dalgalanmalar rölenin çılgınlar gibi açılıp kapanmasına (Chattering) yol açar.
*   **Çözüm:** Çıkış, (+) girişine dirençle bağlanarak **Pozitif Geri Besleme** uygulanır. Bu sayede açma ve kapama için tek bir sınır (threshold) yerine, aralarında bir "ölü bölge (hysteresis band)" olan alt ve üst sınırlar oluşturulur.
*   **Endüstriyel Kullanım:** Kirli, parazitli ve yavaş değişen sensör okumalarında veya buton basımlarında "tertemiz" lojik kararlar almak için sistemleri kararlı kılar. Lojik çiplerin donanımsal kalbidir.