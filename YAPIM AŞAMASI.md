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

## 🔌 Bağlantı Şeması (Pinout)
| Bileşen | Pin |
| :--- | :--- |
| **LM35** | A0 |
| **Röle (IN)** | D7 |
| **Yeşil LED** | D8 |
| **Kırmızı LED** | D9 |
| **Buzzer** | D10 |

## 🏗 Kurulum ve Çalıştırma
1. `Sera_Otomasyon.ino` dosyasını Arduino IDE ile açın.
2. Gerekli kütüphanelerin yüklü olduğundan emin olun.
3. Devre bağlantılarını şemaya uygun şekilde gerçekleştirin.
4. Kodu mikrodenetleyiciye yükleyin.

## 🔮 Gelecek Vizyonu
- **IoT Entegrasyonu:** ESP8266/ESP32 ile bulut tabanlı veri takibi.
- **HMI Geliştirme:** Anlık değerler için 16x2 I2C LCD ekran desteği.
- **Derin Tarım:** Toprak nem sensörü eklenerek otonom sulama entegrasyonu.

---
*Bu proje modern tarım teknolojilerine (Tarım 4.0) giriş niteliğinde bir prototiptir.*
