# SinemaRenk — Sinematik 60/30/10 Renk Analizörü

## 1. Giriş

Klasik sinematografide 60/30/10 renk dengesi kuralı, bir sahnedeki baskın, ikincil ve vurgu renklerinin göreceli alan oranlarını tanımlayan yerleşik bir kompozisyon ilkesidir. **SinemaRenk**, bu ilkeyi film karelerinde nicel olarak sınamak amaçıyla geliştirilmiş, tarayıcı tabanlı bir bilgisayarla görü uygulamasıdır. Yazılım, kullanıcının yüklediği anahtar kareleri analiz ederek baskın, ikincil ve vurgu renklerini piksel doluluk oranlarıyla birlikte raporlar ve bu oranları 60/30/10 hedefleriyle karşılaştırır.

## 2. Yazılımın Tanıtımı

SinemaRenk, tamamıyla kullanıcının kendi tarayıcısında çalışır; görseller herhangi bir sunucuya gönderilmez. Uygulama iki analiz yöntemi sunar:

1. **Hibrit Renk Tabanlı**
2. **CIELAB Tabanlı**

Kullanıcı, örnekleme yoğunluğu, isteğe bağlı gürültü azaltma (DMF) ve palet boyutu (3, 4 veya 5 renk) gibi parametreleri ayarlayabilir. Sonuçlar, her renk için hex kodu, HSV değerleri, piksel doluluk, hedef sapması ve ilişkili renk sıcaklığı (CCT) ile birlikte sunulur. Analiz sonuçları Excel (.xlsx) olarak dışa aktarılabilir; her görsel için ayrı ayrı veya toplu olarak, renk oranlarını görselleştiren PNG çıktıları üretilebilir.

## 3. Yöntem

### 3.1 Hibrit Renk Tabanlı

Hibrit Renk Tabanlı yöntem, HSL renk ailelerine dayalı ön sınıflandırmayı ve CIELAB uzayında algısal uzaklığa dayalı piksel atamasını birleştiren bileşik bir yaklaşımdır. Görüntü önce analiz hızını korumak amaçıyla küçültülür; isteğe bağlı Dominant-Pixels based Mean Filter (DMF) yerel piksel gürültüsünü azaltır. RGB pikselleri kırmızı, turuncu, sarı, yeşil, teal, mavi, mor, siyah ve beyaz/gri ailelerine ayrılır. Renkli ailelerde doygunluğu yüksek ve orta ışıklılıktaki piksellere öncelik verilerek temsilci renkler hesaplanır.

Temsilci renkler CIE 1976 L*a*b* uzayına dönüştürülür. Bu uzay, renk farklarını RGB'ye göre algısal olarak daha anlamlı değerlendirmeye yardımcı olur (Connolly & Fleiss, 1997). En büyük aile baskın renk olarak seçilir; ikincil ve vurgu renkleri, seçilmiş merkezlerden CIELAB uzaklığı bakımından yeterince ayrışan adaylardan belirlenir. Her piksel en yakın temsilciye atanır; bu işlem Voronoi tipi bir en-yakın-merkez bölgelemesi olarak yorumlanabilir (Lloyd, 1982). Düşük doygunluklu görüntülerde siyah, gri ve beyazın birleşmesini önlemek için CIELAB K-Means geri dönüşü uygulanır. Sonuçlar 60/30/10 hedefleriyle karşılaştırılır; ilişkili renk sıcaklığı (CCT) yalnızca yardımcı bir gösterge olarak sunulur (McCamy, 1992).

### 3.2 CIELAB Tabanlı

CIELAB Tabanlı yöntem, anahtar karelerdeki baskın renkleri veri temelli ve tekrarlanabilir biçimde belirlemek için K-Means kümeleme algoritmasını CIE 1976 L*a*b* uzayında uygular. Küme merkezleri K-Means++ ile başlatılır; bu yaklaşım, başlangıç merkezlerinin birbirinden uzak seçilmesini sağlayarak rastgele başlangıcın yol açabileceği kararlılık sorununu azaltır (Arthur & Vassilvitskii, 2007). Her piksel en yakın merkeze atanır; merkezler atanan piksellerin ortalama L*a*b* değeriyle güncellenir. İşlem, atamalar değişmeyene ya da en fazla 25 yinelemeye ulaşılana kadar sürer.

Her küme için iki temsil biçimi sunulur: "Ortalama" seçeneği küme merkezini RGB'ye dönüştürerek teorik ortalama rengi verir; "Gerçek Piksel" seçeneği ise merkeze CIELAB uzaklığı en küçük olan gerçek görüntü pikselini raporlar (Park & Jun, 2009). Kümeler piksel doluluğuna göre sıralanır; ilk üçü baskın, ikincil ve vurgu renkleri olarak atanır ve 60/30/10 hedefleriyle karşılaştırılır. K-Means'in renk nicelemedeki etkinliği Celebi (2011) tarafından ayrıntılı biçimde ele alınmıştır.

## 4. Geçerlilik ve Doğrulama

Geliştirilen yazılımın renk oranı çıktılarının geçerliliği iki tamamlayıcı yöntemle sınanmıştır. İlk olarak, gerçek renk oranı önceden bilinen sentetik referans görüntüler oluşturulmuştur. Bu amaçla Adobe Photoshop'ta, üç ana rengin (RGB: kırmızı, yeşil, mavi) 60/30/10 oranında yerleştirildiği bir renk çubuğu ile aynı orana sahip akromatik bir çubuk (siyah, gri, beyaz) üretilmiştir. Bu tür sentetik, kesin oranlı test görüntüleri, görüntü işleme algoritmalarının doğruluğunu değerlendirmek için yaygın kullanılan bir bilinen-cevap testi (known-answer test) yaklaşımıdır; gerçek dünya görüntülerinin aksine, sonuç önceden bilindiği için sistematik hataların doğrudan tespit edilmesine olanak tanır (Jannin ve ark., 2002). Yazılımın örnekleme yoğunluğu, DMF ayarı ve renk paleti sayısı gibi hassasiyet parametreleri, bu referans görüntülerin gerçek 60/30/10 oranını verecek biçimde ayarlanmış ve doğrulanmıştır.

İkinci aşamada, aynı referans çubukları ve ek test görselleri, çevrim içi olarak çalışan ve baskın renk yüzdesi ile üç renkli palet üreten bağımsız bir araç olan imageonline.io/dominant-colors üzerinden ayrıca analiz edilmiştir. Bağımsız bir yazılımla elde edilen sonuçların geliştirilen yazılımın çıktılarıyla karşılaştırılması, ölçüm aracının dışsal bir referansla ne ölçüde örtüştüğünü gösteren bir kriter geçerliliği sınamasıdır (Boita ve ark., 2021). Benzer bir doğrulama mantığı, K-Means tabanlı baskın renk çıkarımının farklı görüntü kümelerinde tutarlı ve düşük varyanslı sonuçlar verdiğini gösteren çalışmalarda da kullanılmıştır (Sánchez-Sánchez ve ark., 2020).

Bu iki bağımsız doğrulama adımı birlikte değerlendirildiğinde, yazılımın 60/30/10 renk oranlarını yeterli doğrulukla hesapladığı sonucuna varılmıştır. Bununla birlikte, bu doğrulama sınırlı sayıda kontrollü test görseline dayanmaktadır; farklı görüntü türleri, sıkıştırma biçimleri ve ışık koşulları için sistematik ve daha geniş kapsamlı bir doğrulama çalışması, yöntemin genellenebilirliğini güçlendirecektir.

## 5. Arayüz ve Kullanım

SinemaRenk arayüzü Türkçe ve İngilizce olmak üzere iki dilde çalışır. Ana ekranda yöntem seçimi, örnekleme yoğunluğu, DMF ve palet boyutu ayarları; sağ üst köşede ise sonuçları temizleme, Excel'e aktarma ve toplu analiz görseli oluşturma düğmeleri yer alır. "Yöntem" penceresi, her iki tekniğin akademik açıklamasını ve ilgili PDF dokümanlarını sunar; "Geçerlilik" penceresi ise yukarıda özetlenen doğrulama sürecini metin olarak sağlar. Her analiz sonucu, isteğe bağlı olarak fotoğrafın kendi sınırları içine bindirilmiş bir renk oranı barıyla (dikey veya yatay) PNG olarak dışa aktarılabilir.

## 6. Sınırlılıklar ve Gelecek Çalışmalar

Yazılımın doğrulaması, sınırlı sayıda kontrollü sentetik test görseli ve tek bir harici karşılaştırma aracına dayanmaktadır. Gelecekteki çalışmalarda, çeşitli görüntü türleri, sıkıştırma formatları ve aydınlatma koşullarını kapsayan daha geniş bir referans veri kümesiyle sistematik bir doğrulama yapılması önerilmektedir. Ayrıca CIEDE2000 gibi daha gelişmiş renk farkı formüllerinin uygulanması, algısal doğruluğu artırabilir.

## Kaynakça (APA 7)

Arthur, D., & Vassilvitskii, S. (2007). *k-means++: The advantages of careful seeding*. In *Proceedings of the Eighteenth Annual ACM-SIAM Symposium on Discrete Algorithms* (pp. 1027–1035). Society for Industrial and Applied Mathematics. https://doi.org/10.5555/1283383.1283494

Boita, J., ve ark. (2021). Validation of a candidate instrument to assess image quality in digital mammography. *European Journal of Radiology, 145*, Madde 110022. https://doi.org/10.1016/j.ejrad.2021.110022

Celebi, M. E. (2011). Improving the performance of k-means for color quantization. *Image and Vision Computing, 29*(4), 260–271. https://doi.org/10.1016/j.imavis.2010.10.002

Connolly, C., & Fleiss, T. (1997). A study of efficiency and accuracy in the transformation from RGB to CIELAB color space. *IEEE Transactions on Image Processing, 6*(7), 1046–1048. https://doi.org/10.1109/83.597279

Jannin, P., Fitzpatrick, J. M., Hawkes, D. J., Pennec, X., Shahidi, R., ve Vannier, M. W. (2002). Validation of medical image processing in image-guided therapy. *IEEE Transactions on Medical Imaging, 21*(12), 1445–1449. https://doi.org/10.1109/TMI.2002.806568

Lloyd, S. P. (1982). Least squares quantization in PCM. *IEEE Transactions on Information Theory, 28*(2), 129–137. https://doi.org/10.1109/TIT.1982.1056489

McCamy, C. S. (1992). Correlated color temperature as an explicit function of chromaticity coordinates. *Color Research & Application, 17*(2), 142–144. https://doi.org/10.1002/col.5080170211

Park, H.-S., & Jun, C.-H. (2009). A simple and fast algorithm for K-medoids clustering. *Expert Systems with Applications, 36*(2), 3336–3341. https://doi.org/10.1016/j.eswa.2008.01.039

Sánchez-Sánchez, C., ve ark. (2020). Dominant color extraction with K-means for camera characterization in cultural heritage documentation. *Remote Sensing, 12*(3), Madde 520. https://doi.org/10.3390/rs12030520

## Yazılım Atfı

Apaydın, C. (2026). SinemaRenk: Sinematik 60/30/10 Renk Analizörü [Bilgisayar yazılımı]. Zenodo. https://doi.org/10.5281/zenodo.22310061
