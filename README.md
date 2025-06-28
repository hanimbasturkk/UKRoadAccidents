# Birleşik Krallık Trafik Kazaları: Keşifsel Veri Analizi (EDA)

## Projeye Genel Bakış

Bu proje, 2005 ile 2014 yılları arasındaki Birleşik Krallık (BK) karayolu kaza verileri üzerinde kapsamlı bir Keşifsel Veri Analizi (EDA) gerçekleştirmektedir. Projenin temel amacı, kaza oluşumları ve şiddeti ile trafik akışı, zaman, coğrafi alanlar (kentsel/kırsal) ve hava koşulları gibi çevresel faktörler arasındaki desenleri, korelasyonları ve anahtar içgörüleri ortaya çıkarmaktır.

## Cevap Aranan Temel Sorular

Bu analiz, aşağıdaki temel sorulara yanıt bulmayı hedeflemektedir:

1.  **Trafik akışı**, karayolu kazalarının sıklığını ve şiddetini nasıl etkilemektedir?
2.  **Kentsel ve kırsal alanlar** arasındaki kaza karakteristikleri açısından temel farklılıklar nelerdir?
3.  **Günün saati ve hava koşulları** gibi diğer çevresel faktörler, kaza oluşumlarını nasıl etkilemektedir?

## Veri Kaynakları

Proje, iki ana veri setini kullanmaktadır:

1.  **BK Karayolu Güvenliği Verileri - Kazalar (2005-2014):** Kayıtlı her bir karayolu kazası hakkında konum, zaman, şiddet, yol koşulları ve daha fazlasını içeren detaylı bilgiler sunar.
2.  **BK Karayolu Trafik Sayımları (AADF - Yıllık Ortalama Günlük Akış):** BK genelindeki çeşitli noktalara ait ortalama günlük trafik akışı verilerini sağlar ve bu verilerin kaza analiziyle entegrasyonuna olanak tanır.

## Metodoloji

1.  **Veri Yükleme ve Birleştirme:**
    * Kaza verileri ve trafik sayım verileri Pandas DataFrame'lerine yüklenir.
    * Trafik akışı verileri (`AllMotorVehicles`), `Year` ve coğrafi yakınlık (Boylam/Enlem) temelinde `cKDTree` kullanılarak en yakın komşu araması ile kaza verileriyle stratejik olarak birleştirilir. Bu adım, her kaza kaydının en yakın ilgili trafik akışı ölçümüyle ilişkilendirilmesini sağlar.
2.  **Veri Temizleme ve Ön İşleme:**
    * Eksik değerlerin yönetimi (yüksek oranda eksik değere sahip alakasız sütunların çıkarılması, koordinatların temizlenmesi).
    * Özellik mühendisliği: Zaman tabanlı analizler için `Date` ve `Time` sütunlarından `Year`, `Month`, `Day_of_Week_Name` ve `Hour` gibi özelliklerin çıkarılması.
    * Veri tutarlılığının ve doğru veri tiplerinin sağlanması.
3.  **Keşifsel Veri Analizi (EDA):**
    * **Zamansal Analiz:** Ortalama trafik akışı ve kaza şiddetiyle ilişkilerini inceleyerek, yıllar ve günün farklı saatlerindeki kaza eğilimlerinin görselleştirilmesi.
    * **Coğrafi Analiz:** Kentsel ve kırsal alanlar arasındaki kaza sayıları ve şiddetleri ile ilgili trafik akışlarının karşılaştırılması.
    * **Şiddet Analizi:** Kaza şiddeti ile ortalama trafik akışı arasındaki korelasyonun incelenmesi.
    * **Çevresel Faktör Analizi:** Çeşitli hava koşullarının kaza oluşumları üzerindeki etkisinin analizi.
    * Kapsamlı veri görselleştirme için `matplotlib` ve `seaborn` kütüphanelerinin kullanılması.

## Temel Bulgular (İçgörülerin Özeti)

Gerçekleştirilen EDA, BK karayolu kazalarına dair birkaç önemli içgörüyü ortaya koymuştur:

* **Genel Azalma:** 2005'ten 2012'ye kadar hem toplam kaza sayılarında hem de ortalama trafik akışında genel bir düşüş eğilimi gözlenmiştir. Bu durum, yol güvenliğindeki olası iyileşmeleri ve/veya seyahat alışkanlıklarındaki değişimleri düşündürmektedir.
* **Günün Saatinin Etkisi:** En yüksek kaza sayıları işe gidiş-geliş saatlerinde (07:00-09:00 ve 15:00-18:00) meydana gelirken, en **şiddetli** kazalar genellikle trafik yoğunluğunun az olduğu gece geç saatlerde (00:00-05:00) gerçekleşmektedir. Bu durum, geceleyin daha düşük trafik hacminin daha yüksek hızları teşvik edebileceğini ve kazaların daha kritik sonuçlara yol açabileceğini düşündürmektedir.
* **Kentsel ve Kırsal Farklılıklar:** Kentsel alanlar, daha yüksek nüfus ve trafik yoğunluğu nedeniyle genel olarak önemli ölçüde daha fazla kazaya ev sahipliği yapmaktadır. Ancak, ortalama trafik akışları benzer olmasına rağmen **kırsal alanlardaki kazalar ortalama olarak daha şiddetlidir.** Bu durum, kırsal bölgelerdeki daha yüksek hız limitleri, daha az aydınlatma ve muhtemelen daha yavaş acil müdahale süreleri gibi faktörlere işaret etmektedir.
* **Trafik Akışı ve Şiddet Paradoksu:** Trafik akışı ve kaza şiddeti arasında ilginç bir ters ilişki bulunmaktadır. Yüksek trafik akışı (sıkışık alanlar) daha fazla sayıda hafif kazayla ilişkilidir. Buna karşılık, daha düşük trafik akışı (potansiyel olarak daha hızlı yollar) daha az sayıda ancak daha ciddi veya ölümcül kazalarla bağlantılıdır.
* **Hava Koşulları:** Kazaların çoğu "rüzgarsız güzel hava" koşullarında meydana gelmektedir, çünkü bu en yaygın hava tipidir. Ancak, "rüzgarsız yağmurlu hava" kaza riskini önemli ölçüde artırmaktadır. Aşırı olumsuz koşullarda (örn. yoğun kar, sis) kaza sayıları daha düşüktür, bu durum muhtemelen seyahatlerin azalması veya sürücülerin artan dikkatiyle açıklanabilir.
