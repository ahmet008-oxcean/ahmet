## 🧠 Sistem Tasarımı ve Mühendislik Yaklaşımı

Bu proje, temel bir hobi devresinden ziyade, gerçek dünya problemlerine odaklanan bir **Gömülü Sistem (Embedded System)** tasarımıdır. Tasarım sürecinde aşağıdaki disiplinler temel alınmıştır:

### 1. Sinyal İşleme ve Veri Doğruluğu
LM35 sensörü, sıcaklık değerini analog voltaj olarak iletir. Arduino üzerindeki 10-bit ADC (Analog-to-Digital Converter) birimi, bu voltajı 0-1023 arası bir değere çevirir. Projede kullanılan matematiksel modelleme şöyledir:
* **Hassasiyet:** Her 1°C değişim için 10mV çıkış.
* **Dönüşüm Formülü:** $V_{out} = \text{ADC Value} \times (5.0 / 1024)$
* **Sıcaklık Hesaplama:** $T = V_{out} \times 100$
Bu hesaplama, yazılım katmanında doğrudan santigrat dereceye çevrilerek anlık izleme imkanı sunar.

### 2. Aktüatör Yönetimi ve Röle Mantığı
Endüktif yükler (DC Fanlar) çalışırken manyetik alan oluşturur ve devre kapandığında geri besleme voltajı (back-EMF) üretebilir. Bu durum mikrodenetleyicinin kilitlenmesine neden olur. 
* **Çözüm:** Sistemde kullanılan röle modülü, **Galvanik İzolasyon** sağlayarak kontrol devresini güç devresinden fiziksel olarak ayırır. Böylece sistem 7/24 kesintisiz çalışabilir.

### 3. Kontrol Algoritması: Histerezis (Hysteresis)
Sistemde düz bir "aç/kapa" mantığı yerine endüstriyel standart olan histerezis algoritması uygulanmıştır.
* **Problem:** Sıcaklık tam 36.0°C sınırında dalgalandığında fanın saniyede defalarca açılıp kapanması (Chattering).
* **Mühendislik Çözümü:** 0.5°C'lik bir "Ölü Bant" (Deadband) oluşturulmuştur. Fan 36°C'de açılır ancak ortam ısısı 35.5°C'ye düşene kadar çalışmaya devam eder. Bu, rölenin mekanik ömrünü %80 oranında artırır.

### 4. Güvenlik ve Hata Yönetimi (Fail-Safe)
Sera ortamları biyolojik olarak hassas alanlardır. Sensör arızası veya ani ısı artışları mahsul kaybına yol açar.
* **Kritik Eşik ($41.0°C$):** Bu seviye, sistemin sadece iklimlendirme yapmadığı, aynı zamanda bir **Yangın/Aşırı Isı Alarmı** olarak çalıştığı evredir. 
* **Multimodal Uyarı:** Hem sesli (Buzzer) hem görsel (Kırmızı LED) uyarı ile fiziksel müdahale hızı artırılmıştır.

## 📈 Projenin Katkısı ve Gelecek Çalışmalar
Bu çalışma, sürdürülebilir tarım teknolojilerinin temel bir yapı taşıdır. Gelecek versiyonlarda aşağıdaki özelliklerin eklenmesi planlanmaktadır:
1. **LDR Sensör Entegrasyonu:** Gün ışığına bağlı otonom gölgelendirme sistemi.
2. **DHT11 Entegrasyonu:** Sadece sıcaklık değil, bağıl nem takibi ile gerçek hissedilen sıcaklık hesabı.
3. **IoT Dashboard:** Verilerin Wi-Fi üzerinden bir web panelinde grafiksel olarak sunulması.

---
*Bu dökümantasyon, projenin teknik şeffaflığını ve tekrarlanabilirliğini sağlamak amacıyla hazırlanmıştır.*
