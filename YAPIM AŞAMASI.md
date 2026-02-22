# Akıllı Sera İklimlendirme ve Erken Uyarı Sistemi (Tarım 4.0 Prototipi)

Bu proje, Arduino platformu üzerinde geliştirilmiş; hassas sıcaklık takibi, otonom iklimlendirme kontrolü ve kritik durum alarm mekanizmalarını içeren bir **Mekatronik Sistem** çözümüdür.

## 🚀 Proje Hakkında
Sistem, seralardaki mikro-klimatik değişiklikleri gerçek zamanlı olarak takip eder. Belirlenen eşik değerlere göre aktif soğutma birimlerini (Fan) ve nem dengesi optimizasyonunu (Nemlendirici) otonom olarak yönetir. Kritik sıcaklık seviyelerinde ise sesli ve görsel feedback (Alarm) vererek kullanıcıyı uyarır.

## 🛠 Donanım Mimarisi
Proje, düşük maliyetli fakat yüksek verimli bileşenlerle optimize edilmiştir:
- **Mikrodenetleyici:** Arduino Uno R3
- **Sensör:** LM35 Hassas Sıcaklık Sensörü ($10mV/^\circ C$ Lineer Çıkış)
- **Aktüatör Kontrol:** 5V Optik İzoleli Röle Modülü
- **Çıkış Birimleri:** DC Fan, Ultrasonik Nemlendirici
- **Geri Bildirim:** Buzzer, Kırmızı & Yeşil LED
- **Güç Kaynağı:** 9V DC (Harici Aktüatör Beslemesi)

## ⚙️ Teknik Çalışma Prensibi
1. **Veri Akvizisyonu:** LM35'ten gelen analog veriler Arduino'nun 10-bit ADC birimi tarafından sayısal verilere dönüştürülür.
2. **Histerezis Denetimi:** Yazılımsal olarak tanımlanan $36^\circ C$ set değerinde sistem aktif soğutma fazına geçer.
3. **Fail-Safe Protokolü:** Sıcaklık $41^\circ C$ üzerine çıkarsa, sistem otomatik olarak "Acil Durum Modu"na geçer ve sesli/ışıklı alarm verir.
4. **Güç İzolasyonu:** Röle kullanımı sayesinde kontrol devresi ile güç devresi birbirinden elektriksel olarak izole edilmiştir.

## 🏗 Kurulum ve Çalıştırma
1. `Sera_Otomasyon.ino` dosyasını Arduino IDE ile açın.
2. Gerekli kütüphanelerin yüklü olduğundan emin olun.
3. Devre bağlantılarını şemaya uygun şekilde gerçekleştirin.
4. Kodu mikrodenetleyiciye yükleyin.

## 🔮 Gelecek Vizyonu
- **IoT Entegrasyonu:** ESP8266/ESP32 ile bulut tabanlı veri takibi.
- **HMI Geliştirme:** Anlık değerler için 16x2 I2C LCD ekran desteği.
- **Derin Tarım:** Toprak nem sensörü eklenerek otonom sulama entegrasyonu.


🔧 Teknik Kurulum ve Bağlantı Mantığı
LM35 Sıcaklık Sensörü (A0 Pini):

Bağlantı: Sol bacak 5V, sağ bacak GND ve orta bacak Arduino'nun A0 analog girişine bağlanır.

Mantık: Sensör her 1°C artış için 10mV voltaj üretir. Analog pin (A0) kullanıyoruz çünkü dijital pinler sadece "var/yok" diyebilirken, analog pin bu voltajı 1024 farklı parçaya bölerek hassas ısı ölçümü yapmamızı sağlar.

5V Röle Modülü (D7 Pini):

Bağlantı: Sinyal girişi (IN) D7 pinine bağlanır. Fanın enerji hattı bu röle üzerinden geçer.

Mantık: Arduino fanı doğrudan döndürecek güce sahip değildir. Röle burada bir "akıllı anahtar" görevi görür; Arduino'dan gelen düşük sinyalle harici pilin yüksek gücünü fana aktarır.

Görsel ve İşitsel Uyarı Sistemi (D8, D9, D10):

Yeşil LED (D8): Sistem aktif ve sıcaklık değerlerinin güvenli aralıkta (36°C altı) olduğunu belirtir.

Kırmızı LED (D9): Sıcaklık kritik eşik olan 41°C değerini aştığında yanarak tehlikeyi bildirir.

Buzzer (D10): Kritik ısı durumunda yüksek sesli uyarı vererek fiziksel müdahale gerekliliğini hatırlatır.

💡 Mühendislik ve Güvenlik Detayları
Ortak Şasi (Common Ground): Fan için kullanılan harici 9V pilin eksi kutbu ile Arduino'nun GND hattı birleştirilmiştir. Bu yapılmazsa devre tamamlanmaz ve sensör verileri hatalı okunur.

Histerezis (Tolerans Payı): Fan tam 36°C'de açılır ancak 35.5°C'ye düşene kadar kapanmaz. Bu 0.5°C'lik fark, rölenin sınır değerlerde sürekli açılıp kapanarak bozulmasını (chattering) engeller.

Optokuplör Koruması: Röle modülü üzerindeki izolasyon sayesinde, fan motoru çalışırken oluşan elektriksel gürültülerin Arduino'yu kilitlemesi veya resetlemesi önlenmiştir.
