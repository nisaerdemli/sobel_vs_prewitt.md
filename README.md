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
- **Sobel:** Yapısındaki yerel Gauss yumuşatması sayesinde gürültülü ve pürüzlü yüzeylerde kenarları çok daha tutarlı ve kesintisiz tespit eder. Pratikte çoğu bilgisayarlı görü uygulamasında Prewitt'e kıyasla daha yaygın tercih edilir.
