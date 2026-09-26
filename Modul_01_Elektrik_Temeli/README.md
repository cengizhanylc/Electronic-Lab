# Modül 1: Elektrik Temeli ve Laboratuvar Düşüncesi

## 📌 Modül Özeti ve Hedefleri
Bu modülün temel amacı, gerilim, akım, güç ve empedans kavramlarını kağıt üzerinden çıkarıp ölçüm cihazlarının (multimetre, osiloskop) fiziksel sınırlarıyla birlikte analiz etmektir[cite: 2]. Zaman sabiti (step response), frekans domeni kavramları ve cihaz iç dirençlerinin devreye olan yükleme etkileri deneysel olarak doğrulanmıştır[cite: 2].

> **Geçme Kriteri (MASTER):**
> *Bir devreyi breadboard üzerinde kurarken, “şu elemanı buraya taktım” yerine “bu düğümün empedansı nedir, akım nereden dönüyor, ölçüm cihazı devreyi ne kadar yüklüyor?” diye düşünebilmek[cite: 2].* **(BAŞARIYLA TAMAMLANDI)**

---

## 🔬 Deney 1: Enstrümantasyon Yüklemesi (Probe Loading)

**Teori:** Ölçüm cihazlarının (özellikle giriş empedansı ~1 M$\Omega$ olan el tipi cihazların) yüksek dirençli düğümlere paralel bağlanması, eşdeğer direnci değiştirerek okunan voltajın çökmesine (loading effect) neden olur[cite: 2].

**Kurulum & İcra:**
9V DC kaynak, $R_1$ (4.61 k$\Omega$) ve $R_2$ (2.16 k$\Omega$) kullanılarak bir voltaj bölücü kurulmuş ve ölçüm cihazının sisteme dahil olduğu andaki sapma kaydedilmiştir.

| Parametre | Değer | Not |
| :--- | :--- | :--- |
| **Batarya (Yüklü Durum)** | 8.92V | Yüksüz durumdaki 9V gerilimin sisteme bindiği anki çöküşü. |
| **Teorik Beklenti ($V_{R2}$)** | 2.85V | $8.92 \times \frac{2.16}{6.77}$ formülü ile hesaplanmış ideal düğüm voltajı. |
| **Gerçek Ölçüm (Multimetre)** | 2.83V | Cihazın devreden akım çekmesiyle oluşan ~0.02V'luk donanımsal hata payı. |

![Ölçüm Tablosu](./assets/IMG_0264.jpg)
*(Görsel: Çökme gerilimi ve multimetre iç direncinin matematiksel ispatı)*

**Sistem Mimarisi Çıkarımı:** Bu fiziksel fenomen, özellikle bir ESP32'nin ADC pinleri ile yüksek empedanslı bir analog sensör (örn: toprak nem sensörü) okunurken karşılaşılacak "yanlış ölçüm" sorunlarının temel simülasyonudur. Sensörün vereceği voltaj, MCU bağlandığı an çökecektir.

---

## 🔬 Deney 2: Zaman Domeninde RC Karakteristiği ve Step Response

**Teori:** Kapasitörlerin fiziksel davranışı ve enerji depolama süreçleri nedeniyle, sisteme uygulanan ani gerilim değişimleri (step response) üstel bir $e^{-t/\tau}$ şarj/deşarj eğrisine dönüşür[cite: 2]. 

**Kurulum & İcra:**
PWM modülü kullanılarak 20 Hz, %50 duty cycle ayarında donanımsal bir basamak (step) sinyali üretilmiştir. Sinyal, $R$ (4610 $\Omega$) ve $C$ (0.47 $\mu$F) filtresinden geçirilerek FNIRSI DSO-152 osiloskobu ile düğüm (node) üzerinden incelenmiştir.

*   **Matematiksel Zaman Sabiti ($\tau$):** $4610 \times (0.47 \times 10^{-6}) \approx 2.16\text{ milisaniye}$.
*   **Osiloskop Doğrulaması:** Eğrinin, tepe noktasının (Vmax) %63.2'sine ulaşması yatay eksende tam olarak ~2.2 ms (1 Time/Div karesinden hafif fazla) olarak fiziksel dünyada saptanmıştır.

**Sistem Mimarisi Çıkarımı:** Gelecekte tasarlanacak PCB'lerde, veri yollarının (I2C, SPI) uzunluğuna bağlı oluşan parazitik kapasitanslar, jilet gibi olması gereken veri paketlerini (1 ve 0'ları) bu deneydeki gibi yavaşlayarak "yüzgeç" formuna dönüştürecektir. İletişim kopmaları ve CRC hatalarının kök nedeni bu kapasitif gecikmedir.

---

## 🔬 Deney 3: Frekans Domeni Sinyalleri ve AC/DC Coupling

**Teori:** Sinyali zaman ve frekans domeninde düşünmeye giriş yapılmış[cite: 2], sistemdeki anlık AC değişimler ve sabit DC zemin karakteristikleri izole edilerek test edilmiştir.

**Kurulum & İcra:**
Fonksiyon jeneratörü ile üretilen 100 Hz, 1 kHz ve 10 kHz kare dalgalar osiloskoba aktarılarak DC ve AC Coupling modlarında çapraz analize tabi tutulmuştur[cite: 2].
*   **DC Coupling Modu:** Sinyalin 0V ile 5V arasında (pozitif bölgede) seken stabil hali gözlemlenmiştir.
*   **AC Coupling Modu:** Osiloskobun içindeki fiziksel seri kapasitörün devreye girmesiyle DC zemin tamamen bloke olmuş; devre bir yüksek geçiren filtreye (High-Pass Filter / Türev Alıcı) dönüşerek yalnızca kare dalganın ani yükseliş ve düşüşlerini "iğne (spike) dalgalar" halinde ekrana yansıtmıştır[cite: 2].

**Sistem Mimarisi Çıkarımı:** Bir donanım debug sürecinde güç hatlarını (örneğin Buck Converter çıkışlarını) salt DC voltaj kademesinde okumak, alttaki anahtarlama gürültülerini gizler. İşlemcinin kararsız çalışmasına veya donmasına sebep olan güç dalgalanmaları (ripple) ancak AC kuplaj ile tespit edilebilir.

---
**Durum:** Modül 1 kapsamında hedeflenen cihaz limitleri, empedans farkındalıkları ve sinyal deformasyonu analizleri uygulamalı olarak laboratuvarda ispatlanmış olup "MASTER" seviyesine[cite: 2] ulaşılmıştır.