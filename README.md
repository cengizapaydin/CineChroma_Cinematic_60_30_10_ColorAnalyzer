# CineChroma: Cinematic 60/30/10 Color Analyzer

**SinemaRenk / CineChroma** — Film karelerindeki renk dağılımını klasik sinematografi kuralı olan **60/30/10** oranına göre nicel olarak analiz eden, tamamen tarayıcı tabanlı, bilingual (TR/EN) bir bilgisayarla görü aracıdır.

🔗 Repository: [github.com/cengizapaydin/CineChroma\_Cinematic\_60\_30\_10\_ColorAnalyzer](https://github.com/cengizapaydin/CineChroma_Cinematic_60_30_10_ColorAnalyzer)
📦 Zenodo DOI: [10.5281/zenodo.22310061](https://doi.org/10.5281/zenodo.22310061)

\---

## 📖 İçindekiler

* [Amaç](#-amaç)
* [Özellikler](#-özellikler)
* [Ekran Görüntüleri](#-ekran-görüntüleri)
* [Yöntemler](#-yöntemler)
* [Kurulum](#-kurulum)
* [Kullanım](#-kullanım)
* [Depo Yapısı](#-depo-yapısı)
* [Geçerlilik ve Doğrulama](#-geçerlilik-ve-doğrulama)
* [Atıf (Citation)](#-atıf-citation)
* [Lisans](#-lisans)
* [İletişim](#-iletişim)

\---

## 🎯 Amaç

Klasik sinematografide **60/30/10 renk dengesi kuralı**, bir sahnedeki baskın (%60), ikincil (%30) ve vurgu (%10) renklerin göreli alan oranlarını tanımlayan yerleşik bir kompozisyon ilkesidir. CineChroma, bu ilkeyi **film karelerinde nicel olarak sınamak** amacıyla geliştirilmiştir.

Yazılım şunları yapar:

* Kullanıcının yüklediği anahtar kareleri (key frame) analiz eder.
* Baskın, ikincil ve vurgu renklerini **piksel doluluk oranlarıyla** birlikte hesaplar.
* Bu oranları **60/30/10 hedefleriyle** karşılaştırıp sapmayı raporlar.
* Akademik çalışma, tez, makale veya rapor hazırlayan araştırmacılar için tekrarlanabilir, veri temelli bir ölçüm sağlar.

## ✨ Özellikler

* 🌐 **Tamamen tarayıcı tabanlı** — kurulum gerektirmez, görseller hiçbir sunucuya gönderilmez (gizlilik dostu).
* 🇹🇷 / 🇬🇧 **Çift dilli arayüz** (Türkçe / İngilizce), tek tıkla dil değişimi.
* 🎨 **İki farklı analiz yöntemi**: Hibrit Renk Tabanlı ve CIELAB Tabanlı (K-Means).
* 🎛️ Ayarlanabilir **örnekleme yoğunluğu**, isteğe bağlı **DMF gürültü azaltma**, **3/4/5 renk paleti** boyutu.
* 📊 Her renk için **hex kodu, HSV değerleri, piksel doluluğu, hedef sapması ve ilişkili renk sıcaklığı (CCT)**.
* 📁 **Excel (.xlsx)** olarak toplu sonuç dışa aktarma (kare bazlı, film bazlı, otomatik yorum metinleri).
* 🖼️ Her görsel için **analiz görseli (PNG)** oluşturma — renk oranı barı fotoğrafın üzerine bindirilmiş biçimde.
* 📂 Dosya adlarını `FilmAdi\_000000.jpg` biçiminde tutarsanız, kareler **otomatik olarak film bazında gruplanır**.
* 📚 Arayüz içinden erişilebilen **Yöntem** (akademik açıklama + indirilebilir PDF'ler) ve **Geçerlilik** (doğrulama süreci) pencereleri.

## 🖼️ Ekran Görüntüleri

|Türkçe Arayüz|English Interface|
|-|-|
|!\[Ekran 1 - TR](Ekran1TR.png)|!\[Screen 1 - EN](Ekran1EN.png)|
|!\[Ekran 2 - TR](Ekran2TR.png)|!\[Screen 2 - EN](Ekran2EN.png)|

> Not: Görsellerin bu README'de doğru görüntülenmesi için depoya `Ekran1TR.jpg`, `Ekran2TR.jpg`, `Ekran1EN.jpg`, `Ekran2EN.jpg` dosyalarının da yüklenmiş olması gerekir.

## 🔬 Yöntemler

### 1\. Hibrit Renk Tabanlı (Hybrid Perceptual Palette)

HSL renk ailelerine dayalı ön sınıflandırma ile CIELAB uzayında algısal uzaklığa dayalı piksel atamasını birleştirir. Renkli ailelerde doygunluğu yüksek ve orta ışıklılıktaki piksellere öncelik verilir; düşük doygunluklu görüntülerde CIELAB K-Means geri dönüşü uygulanır. *(Connolly \& Fleiss, 1997; Lloyd, 1982; McCamy, 1992)*

### 2\. CIELAB Tabanlı (K-Means)

CIE 1976 L\*a\*b\* uzayında K-Means++ ile başlatılan kümeleme algoritmasını kullanır. Her küme için "Ortalama" veya "Gerçek Piksel" (K-Medoids) temsil seçeneği sunulur; kümeler piksel doluluğuna göre sıralanıp 60/30/10 hedefiyle karşılaştırılır. *(Arthur \& Vassilvitskii, 2007; Celebi, 2011; Park \& Jun, 2009)*

Her iki yöntemin ayrıntılı akademik açıklaması ve APA 7 kaynakçası, uygulama içindeki **Yöntem** penceresinden ve depodaki PDF dosyalarından erişilebilir.

## ⚙️ Kurulum

Kurulum gerekmez. CineChroma, tek bir HTML dosyası olarak çalışır:

1. Depoyu indirin veya klonlayın:

```bash
   git clone https://github.com/cengizapaydin/CineChroma\_Cinematic\_60\_30\_10\_ColorAnalyzer.git
   ```

2. `CineChroma-v14.4.html` dosyasını herhangi bir modern tarayıcıda (Chrome, Edge, Firefox) açın.

İnternet bağlantısı yalnızca Excel dışa aktarma kütüphanesinin (SheetJS/xlsx, CDN üzerinden) yüklenmesi için gereklidir; görsel analizinin kendisi tamamen yerel olarak (tarayıcınızda) yapılır.

## 🚀 Kullanım

1. **Analiz yöntemini seçin**: Hibrit Renk Tabanlı veya CIELAB Tabanlı.
2. **Parametreleri ayarlayın**: örnekleme yoğunluğu, DMF gürültü azaltma, renk paleti boyutu (3/4/5).
3. **Görselleri sürükleyip bırakın** veya tıklayıp seçin (birden fazla görsel aynı anda yüklenebilir).
4. Her görsel için otomatik olarak hesaplanan **Baskın / İkincil / Vurgu** renklerini, piksel doluluk oranlarını ve hedeften sapmayı inceleyin.
5. İsterseniz her kare için **Analiz Görseli (PNG)** oluşturun.
6. Tüm sonuçları **Excel (.xlsx)** olarak dışa aktarın (kare bazlı özet, film bazlı ortalama sapma, otomatik yorum metinleri dahil).
7. Üstteki **Yöntem** ve **Geçerlilik** butonlarından yöntemlerin akademik dayanağını ve doğrulama sürecini görüntüleyin/indirin.

## 📁 Depo Yapısı

```
CineChroma\_Cinematic\_60\_30\_10\_ColorAnalyzer/
├── CineChroma-v14.4.html                          # Ana uygulama (çalıştırılabilir dosya)
├── README.md                                       # Bu dosya
├── SinemaRenk-Sinematik-60-30-10-Renk-Analizoru.md # Kitap bölümü / akademik açıklama (TR)
├── CineChroma-Cinematic-60-30-10-Color-Analyzer.md # Kitap bölümü / akademik açıklama (EN)
├── Hibrit Renk Analizi.pdf                         # Yöntem 1 açıklaması (TR)
├── Hybrid Color Analyse\_EN.pdf                     # Yöntem 1 açıklaması (EN)
├── CIELAB Tabanlı Renk Analizi.pdf                 # Yöntem 2 açıklaması (TR)
└── CIELAB Based K-Means.pdf                        # Yöntem 2 açıklaması (EN)
```

## ✅ Geçerlilik ve Doğrulama

Yazılımın renk oranı çıktılarının geçerliliği iki tamamlayıcı yöntemle sınanmıştır:

1. **Bilinen-cevap testi**: Gerçek oranı önceden bilinen sentetik referans görüntüler (Adobe Photoshop'ta üretilmiş 60/30/10 oranlı renkli ve akromatik çubuklar) ile parametreler doğrulanmıştır.
2. **Kriter geçerliliği**: Aynı referans görüntüler, bağımsız bir üçüncü taraf aracıyla (imageonline.io/dominant-colors) ayrıca analiz edilip sonuçlar karşılaştırılmıştır.

Ayrıntılı doğrulama metni, uygulama içindeki **Geçerlilik** penceresinden görüntülenebilir/indirilebilir.

## 📌 Atıf (Citation)

Bu yazılımı akademik bir çalışma, rapor veya yayında kullananların aşağıdaki kaynağa atıf vermesi rica edilir:

**Türkçe:**

> Apaydın, C. (2026). \*SinemaRenk: Sinematik 60/30/10 Renk Analizörü\* \[Bilgisayar yazılımı]. Zenodo. \[https://doi.org/10.5281/zenodo.22310061](https://doi.org/10.5281/zenodo.22310061)

**English:**

> Apaydın, C. (2026). \*CineChroma: Cinematic 60/30/10 Color Analyzer\* \[Computer software]. Zenodo. \[https://doi.org/10.5281/zenodo.22310061](https://doi.org/10.5281/zenodo.22310061)

Bu atıf bilgisi, yazılımın kendi arayüzünde de ("Yazılım Atfı" / "Software Citation" kutusu) kullanıcılara gösterilmektedir.

## ⚖️ Lisans

Bu proje şu anda açık kaynak olarak, **kullananların atıf vermesi koşuluyla** herkesin özgürce kullanabileceği şekilde paylaşılmaktadır.

**Önerilen lisans:** [**Creative Commons Attribution 4.0 International (CC BY 4.0)**](https://creativecommons.org/licenses/by/4.0/deed.tr)

Bunu öneriyorum çünkü:

* ✅ Herkes yazılımı **ücretsiz kullanabilir, kopyalayabilir, değiştirebilir ve dağıtabilir** (istediğiniz erişilebilirlik hedefiyle uyumlu).
* ✅ Kullanan kişi/kurum **yasal olarak atıf vermek zorundadır** (istediğiniz asgari koruma budur — MIT/Apache gibi lisanslarda atıf yalnızca "nezaket" düzeyinde kalır, hukuken bağlayıcı değildir).
* ✅ Akademik yazılımlar ve Zenodo'da DOI ile arşivlenen araçlar için yaygın ve tanınan bir tercihtir.
* ⚠️ Not: CC BY 4.0 esasen kod için değil, daha çok içerik/veri/araştırma çıktıları için tasarlanmıştır; yazılım kodu için topluluk bazen **MIT + "lütfen atıf yapın" notu** kombinasyonunu tercih eder. Ancak bu ikincisi hukuken atfı zorunlu kılmaz. Atfı gerçekten şart tutmak istiyorsanız **CC BY 4.0** daha güvenli bir seçimdir.

Lisansı netleştirmek için depoya bir `LICENSE` dosyası eklemenizi öneririm.

## 👤 İletişim

**Öğr. Gör. Dr. Cengiz Apaydın**
GitHub: [@cengizapaydin](https://github.com/cengizapaydin)

Sorular, hata bildirimleri ve öneriler için lütfen bir [GitHub Issue](https://github.com/cengizapaydin/CineChroma_Cinematic_60_30_10_ColorAnalyzer/issues) açın.

