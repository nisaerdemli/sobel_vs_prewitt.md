# sobel_vs_prewitt.md# Kenar Tespiti Operatörleri Karşılaştırması: Sobel vs. Prewitt

Bu çalışma, dijital görüntü işlemede yaygın olarak kullanılan birinci dereceden türev tabanlı iki klasik kenar tespit operatörünün (**Prewitt** ve **Sobel**) teorik temellerini, gürültü toleranslarını ve görsel performanslarını karşılaştırmaktadır. Testler, yüksek frekanslı doku ve belirgin harç hatları barındıran gri tonlamalı bir tuğla duvar görüntüsü üzerinde gerçekleştirilmiştir.

---

## 1. Çekirdek (Kernel) Yapısı ve Matematiksel Temel

Her iki operatör de yatay ($G_y$) ve dikey ($G_x$) eksenlerdeki yoğunluk gradyanlarını hesaplamak için $3\times3$ konvolüsyon çekirdekleri kullanır.

### Prewitt Operatörü
Prewitt operatörü, gradyan yönüne dik eksende piksellere eşit ağırlık ($1, 1, 1$) vererek basit bir aritmetik ortalama alır:

$$G_x = \begin{bmatrix} -1 & 0 & 1 \\ -1 & 0 & 1 \\ -1 & 0 & 1 \end{bmatrix}, \quad G_y = \begin{bmatrix} -1 & -1 & -1 \\ 0 & 0 & 0 \\ 1 & 1 & 1 \end{bmatrix}$$

### Sobel Operatörü
Sobel operatörü, merkez satır veya sütuna daha fazla ağırlık ($2$) vererek türev alma işlemine yerel bir Gauss benzeri ağırlıklandırma ekler:

$$S_x = \begin{bmatrix} -1 & 0 & 1 \\ -2 & 0 & 2 \\ -1 & 0 & 1 \end{bmatrix}, \quad S_y = \begin{bmatrix} -1 & -2 & -1 \\ 0 & 0 & 0 \\ 1 & 2 & 1 \end{bmatrix}$$

---

## 2. Karşılaştırmalı Analiz

| Kriter | Prewitt Operatörü | Sobel Operatörü |
| :--- | :--- | :--- |
| **Ağırlık Dağılımı** | Homojen / Düzgün ($[-1, 0, 1]$) | Merkez odaklı / Ağırlıklı ($[-1, 0, 1]$ ve $[-2, 0, 2]$) |
| **Gürültüye Duyarlılık** | Yüksek; yüzey dokularını kenar sanabilir | Orta/Düşük; dahili yumuşatma etkisi vardır |
| **Kenar Kalınlığı & Kontrastı** | Daha ince, ancak gradyan büyüklüğü görece zayıf | Daha tok, belirgin ve yüksek kontrastlı kenarlar |
| **Hesaplama Maliyeti** | Çarpma gerektirmez (sadece toplama/çıkarma) | Merkez ağırlıkları için kaydırma/çarpma gerekir |

---

## 3. Test Görüntüsü Üzerindeki Gözlemler (Tuğla Duvar)

1. **Ön İşleme (Gauss Filtresi):** Her iki yöntemden önce uygulanan $3\times3$ Gauss filtresi, tuğla yüzeyindeki gözeneklerin türev aşamasında sahte kenar (gürültü) oluşturmasını engellemiştir.
2. **Yön Duyarlılığı:** 
   - $G_x$ ve $S_x$ filtreleri yalnızca dikey harç çizgilerini ve dikey çatlakları yakalamış; yatay hatları tamamen bastırmıştır.
   - $G_y$ ve $S_y$ filtreleri tuğla sıraları boyunca uzanan kesintisiz yatay hatları net bir şekilde ortaya çıkarmıştır.
3. **Performans Farkı:** Tuğla yüzeyindeki mikro çatlaklar ve taş pürüzleri Prewitt çıktısında daha fazla parazite neden olurken, Sobel operatörü merkez piksellere uyguladığı $2$ katsayısı sayesinde daha pürüzsüz ve sürekli harç sınırları üretmiştir.

---# Laplacian, Sobel ve Prewitt Kenar Operatörleri Karşılaştırması

## 1. Temel Teorik Farklar

* **Türev Derecesi:** 
  - **Sobel ve Prewitt:** Birinci dereceden türev (gradyan büyüklüğü) tabanlıdır. Yönsellik içerir ($G_x$ ve $G_y$). Kenarlar yerel parlaklık tepe noktaları (maksimumlar) olarak saptanır.
  - **Laplacian:** İkinci dereceden türev ($\nabla^2 f$) tabanlıdır. Kenarlar gradyanın sıfırdan geçtiği noktalar (**sıfır geçişi / zero-crossing**) ile belirlenir.
* **Yön Bağımsızlığı (İzotropik Yapı):** 
  - Sobel ve Prewitt filtreleri yönlüdür; yatay ve dikey kenarları ayrı ayrı hesaplayıp birleştirmek gerekir.
  - Laplacian skaler ve yön bağımsız (izotropik) bir operatördür; tek bir $3\times3$ çekirdekle tüm yönlerdeki kenarları aynı anda yakalar.

---

## 2. Karşılaştırma Tablosu

| Kriter | Prewitt Operatörü | Sobel Operatörü | Laplacian Operatörü |
| :--- | :--- | :--- | :--- |
| **Türev Tipi** | 1. Derece Türev | 1. Derece Türev | 2. Derece Türev ($\nabla^2$) |
| **Yön Bağımlılığı** | Yönlü ($G_x$, $G_y$) | Yönlü ($S_x$, $S_y$) | Yönsüz (İzotropik - Tüm yönler) |
| **Gürültü Duyarlılığı** | Orta | Düşük / Orta (dahili Gauss ağırlığı) | **Çok Yüksek** (çift türev gürültüyü katlar) |
| **Kenar Kalınlığı** | Orta kalınlıkta | Belirgin ve tok | Çift çizgi etkisi (ince iç-dış hatlar) |
| **Ön İşleme İhtiyacı** | Önerilir | Önerilir | **Kesinlikle Zorunlu** (Gauss gerektirir) |

---

## 3. Test Görüntüsü Üzerindeki Gözlemler (Tuğla Duvar)

1. **Gürültüye Aşırı Tepki:** İkinci dereceden türev yüksek frekanslı parazitleri hızla büyüttüğü için, Laplacian filtresi tuğlaların pürüzlü yüzeyinde Sobel ve Prewitt'e kıyasla çok daha fazla kumlanma (parazit kenar) üretmiştir. Öncesinde uygulanan Gauss filtresi bu etkiyi hafifletmek için kritik bir rol oynamıştır.
2. **Çift Kenar Etkisi:** Sobel ve Prewitt geçiş noktasında tek ve net bir hat oluştururken; Laplacian ikinci türevin işaret değişimi nedeniyle parlaklıktan karanlığa geçiş sınırlarında çift çizgi (iç ve dış sınır) profili oluşturmuştur.
3. **Kullanım Senaryoları:**
   - **Sobel / Prewitt:** Yönlü sınırların çıkarılması, harç çizgilerinin netliği ve gürültü direnci gerektiren durumlar için çok daha uygundur.
   - **Laplacian:** Kapalı sınırların tek adımda yakalanması veya LoG (Laplacian of Gaussian) yapısında sıfır geçişi tespiti için tercih edilir.

## 4. Değerlendirme

- **Prewitt:** Donanım seviyesinde düşük işlem gücüne ihtiyaç duyulan veya sadece toplama/çıkarma işlemlerinin desteklendiği yalın sistemler için idealdir.
- ---

# 6. Madde: 7x7 Laplacian of Gaussian (LoG) Analiz ve Kıyaslama Raporu

## 1. LoG (Laplacian of Gaussian) Çekirdeği Nedir?
Standart Laplacian filtresi ikinci dereceden türev aldığı için görüntüdeki piksel gürültülerine ve mikro yüzey pürüzlerine aşırı tepki verir. **Laplacian of Gaussian (LoG)** ise Gauss yumuşatması ile Laplacian türevini tek bir çekirdekte matematiksel olarak birleştirir:

$$\text{LoG}(x, y) = -\frac{1}{\pi \sigma^4} \left( 1 - \frac{x^2 + y^2}{2\sigma^2} \right) e^{-\frac{x^2 + y^2}{2\sigma^2}}$$

Bu çekirdeğin kesiti şapka formunda olduğu için literatürde **Meksika Şapkası (Mexican Hat)** olarak da adlandırılır. $7\times7$ çekirdek boyutu, $\sigma = 1.0$ yayılımının kenarlarda sıfıra yumuşak bir şekilde sönümlenmesini sağlar.

---

## 2. Gauss Uygulanmış ve Uygulanmamış Filtrelerin Kıyaslaması

| Filtre | Gauss Ön İşleme | Çekirdek Boyutu | Gürültüye Hassasiyet | Kenar Görünümü |
| :--- | :--- | :--- | :--- | :--- |
| **Ham Laplacian** | Yok | $3\times3$ | **Çok Yüksek:** Tuğla yüzeyindeki tüm gözenekleri kenar zanneder. | Aşırı parazit, sahte beyaz noktacıklar. |
| **Gauss + Sobel** | Var ($3\times3$) | $3\times3$ | **Düşük:** Merkez ağırlığı ve ön yumuşatma gürültüyü çok iyi keser. | Tuğla harç çizgileri kalın, belirgin ve tek hatlı. |
| **7x7 LoG** | Dahili (Entegre) | $7\times7$ | **Düşük:** Gauss etkisi doğrudan çekirdeğe gömülüdür. | Yüzey parazitleri temizlenmiş, izotropik ve çift hatlı yapısal sınırlar. |

---

## 3. Test Sonuçları ve Değerlendirme

1. **Gauss Filtresi Olmadan Laplacian:** İkinci türev doğrudan ham pikseller üzerinde çalıştığında tuğlaların pürüzlü dokusu baskın çıkmış ve asıl harç kenarları bu kumlanmanın içinde kaybolmuştur. Bu durum, ikinci türev tabanlı filtrelerde yumuşatmanın kesinlikle zorunlu olduğunu göstermektedir.
2. **7x7 LoG Başarısı:** Çekirdeğin kendi içindeki Gauss bileşeni sayesinde ekstra bir ön işleme gerek kalmadan yüzey pürüzleri filtrelenmiş; harç çizgileri ve tuğla sınırları tüm yönlerde (izotropik olarak) temiz bir şekilde yakalanmıştır.
3. **Sobel ile Farkı:** Sobel gradyan büyüklüğünü tek ve tok bir çizgi olarak sunarken, LoG ikinci dereceden türevin sıfır geçişi (zero-crossing) karakteristiğinden ötürü sınırların iki yanında çift hat oluşturmuştur.
4. ---

# 7. Madde: Canny Kenar Algoritması ve Genel Algoritma Kıyaslaması

## 1. Canny Kenar Algoritmasının Çalışma Prensibi
Canny algoritması tek bir filtreleme işleminden ibaret olmayıp, çok aşamalı (multi-stage) bir kenar tespit mimarisidir:
1. **Gauss Yumuşatma:** Gürültüleri filtrelemek için kullanılır.
2. **Gradyan Hesaplama:** Sobel çekirdekleriyle yoğunluk türevleri ($G_x, G_y$) ve yön açıları ($\theta$) bulunur.
3. **Maksimum Olmayanların Bastırılması (Non-Maximum Suppression):** Gradyan yönündeki yerel tepe noktası olmayan pikseller sıfırlanarak kalın kenarlar **1 piksel genişliğe (iskelete)** inceltilir.
4. **Histeresis Çift Eşikleme (Hysteresis Thresholding):** İki eşik değeri ($T_{low}, T_{high}$) kullanılarak zayıf ve güçlü kenarlar ayırt edilir; güçlü kenarlarla bağlantısı olmayan izole gürültüler elenir.

---

## 2. Kapsamlı Karşılaştırma Tablosu

| Kriter | Prewitt | Sobel | 7x7 LoG | OpenCV Canny |
| :--- | :--- | :--- | :--- | :--- |
| **Kenar Kalınlığı** | Kalın / Gradyanlı | Kalın / Tok | Çift Çizgi Profili | **Tam 1 Piksel (İnce İskelet)** |
| **Gürültüye Direnç** | Orta | İyi | Yüksek | **En Yüksek** (Histeresis filtrelemesi) |
| **Doğruluk / Konumlandırma** | Orta | İyi | Zayıf (Köşe bozulmaları) | **Mükemmel** (Tam sınır tespiti) |
| **Çıktı Türü** | Gri tonlamalı gradyan | Gri tonlamalı gradyan | Çift işaretli gradyan | **İkili (Binary: 0 veya 255)** |
| **Hesaplama Karmaşıklığı** | Düşük (Tek filtre) | Düşük (Tek filtre) | Orta ($7\times7$ çekirdek) | Yüksek (Çok aşamalı pipeline) |
| **Çalışma Zamanı (Performans)** | Çok Hızlı (~1-2 ms) | Çok Hızlı (OpenCV C optimizasyonu ile ~0.5-1 ms) | Hızlı (~1-2 ms) | Hızlı (Optimize C++ altyapısı sayesinde ~1-2 ms) |

---

## 3. Deneysel Gözlemler ve Sonuç Değerlendirmesi

1. **Kenar Keskinliği ve Kalitesi:**
   - Prewitt, Sobel ve LoG operatörleri piksel yoğunluk gradyanı ürettiğinden kenarlar kalın, bulanık ve gri tonlamalı bir hat halinde kalır; kenarın kesin sınırını belirlemek için harici eşikleme gerekir.
   - Canny ise NMS (Non-Maximum Suppression) adımı sayesinde harç çizgilerini ve tuğla kenarlarını tam **1 piksel kalınlığında, temiz ve ikili (siyah-beyaz)** olarak üretmiştir.
2. **Çalışma Zamanı Analizi:**
   - Canny matematiksel olarak en karmaşık algoritma olmasına rağmen, OpenCV'nin C/C++ seviyesindeki SIMD ve çok çekirdek optimizasyonları sayesinde milisaniye mertebesinde tamamlanmaktadır.
3. **Genel Çıkarım:**
   - Hızlı gradyan hesaplaması veya donanım kısıtlı gömülü sistemler için **Sobel** yeterli ve dengeli bir çözümdür.
   - Yüksek doğruluk, gürültüsüz kenar iskeleti ve nesne tanıma/segmentasyon gibi ileri seviye bilgisayarlı görü görevleri için **Canny** açık ara en üstün algoritmadır.
- **Sobel:** Yapısındaki yerel Gauss yumuşatması sayesinde gürültülü ve pürüzlü yüzeylerde kenarları çok daha tutarlı ve kesintisiz tespit eder. Pratikte çoğu bilgisayarlı görü uygulamasında Prewitt'e kıyasla daha yaygın tercih edilir.
