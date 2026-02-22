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

🎓 Akıllı Sera Sistemi: Adım Adım Kurulum ve Teknik Mantık RehberiBu bölüm, sistemin donanım bileşenlerini nasıl birleştireceğinizi ve bu bağlantıların arkasındaki mühendislik nedenlerini açıklar.🔌 Donanım Bağlantı Tablosu (Pinout)BileşenArduino PiniBağlantı AmacıNeden Bu Pin?LM35 (Sinyal)A0Sıcaklık verisini okumak.Sıcaklık sürekli değişen (analog) bir veri olduğu için Analog-Digital dönüştürücü (ADC) pinine ihtiyaç duyar.Röle Modülü (IN)D7Fanı açıp kapatmak.Dijital bir anahtarlama sinyali (0 veya 1) göndererek fanın enerjisini yönetmek için.Yeşil LEDD8"Sistem Güvenli" uyarısı.Isı normal değerlerdeyken görsel onay sağlamak için.Kırmızı LEDD9"Kritik Isı" uyarısı.Isı 41°C'yi geçtiğinde acil durum görseli oluşturmak için.BuzzerD10Sesli Alarm.Tehlikeli durumda operatörü işitsel olarak uyarmak için.🧐 Hangi Bileşeni Neden Bağlıyoruz? (Detaylı Analiz)1. LM35 Sıcaklık Sensörü (A0 Pini)Bağlantı: Sensörün üç bacağı vardır; sol bacak 5V, sağ bacak GND, orta bacak ise A0'a gider.Neden: LM35, her 1 derecelik artış için 10 milivolt (mV) gerilim üretir. Arduino'nun A0 pini bu çok küçük voltaj değişimlerini algılayarak sayısal verilere dönüştürür. Diğer dijital pinler sadece "var" veya "yok" diyebilirken, A0 "ne kadar sıcak?" sorusuna cevap verebilir.2. 5V Röle Modülü (D7 Pini)Bağlantı: Rölenin VCC ve GND uçları Arduino'ya, IN ucu D7'ye bağlanır. Fanın artı ucu ise rölenin "Normalde Açık" (NO) terminalinden geçer.Neden: Arduino'nun pinleri sadece 5V ve çok düşük akım verir, bu da fanı döndürmeye yetmez. Röle burada bir "Akıllı Şalter" görevi görür. Arduino'dan gelen küçük sinyalle, harici 9V pilden gelen büyük gücü fan için serbest bırakır.3. Uyarı Sistemi (LED ve Buzzer - D8, D9, D10)Bağlantı: Her bir bileşen kendi dijital pinine bağlıdır. Devreye 220 ohm direnç eklenerek bileşenlerin aşırı akımdan yanması önlenir.Neden: Bir sistemin sadece çalışması yetmez; durumunu kullanıcıya bildirmesi gerekir.Yeşil LED: Sistemin aktif ve sıcaklığın 36°C'nin altında olduğunu gösterir.Kırmızı LED & Buzzer: Sıcaklık 41°C gibi kritik bir eşiği geçtiğinde devreye girer. Bu, ortamda bir yangın riski veya klima arızası olabileceğini bildiren bir **"Güvenlik Katmanı"**dır.⚠️ Önemli Kurulum Notları (Öğrenenler İçin)Ortak Şasi (Common Ground): Eğer fan için harici bir 9V pil kullanıyorsanız, pilin eksi (-) ucu ile Arduino'nun GND ucu mutlaka birbirine bağlanmalıdır. Aksi halde devre tamamlanmaz ve sinyaller havada kalır.Histerezis (Tolerans): Kodun içinde fanın 36°C'de açılıp tam 36°C'de kapanmaması gerekir. 35.5°C'ye kadar beklemesi, rölenin saniyede onlarca kez "çıt çıt" yaparak bozulmasını engeller.İzolasyon: Röle modülünün optokuplörlü (ışıkla yalıtımlı) olması, fan motorundan gelecek elektrik sıçramalarının Arduino'yu yakmasını veya resetlemesini önler.
