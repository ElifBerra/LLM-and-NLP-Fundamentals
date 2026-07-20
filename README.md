# Semantic Text Analysis: Tokenization, Embeddings & Unsupervised Clustering

Bu proje, Doğal Dil İşleme (NLP) ve Büyük Dil Modellerinin (LLM) temel yapı taşları olan **Tokenization**, **Text Embeddings**, **Semantic Similarity** ve **Gözetimsiz Öğrenme (Unsupervised Learning)** kavramlarını pratik uygulamalarla incelemek amacıyla geliştirilmiş bir laboratuvar çalışmasıdır.

Proje kapsamında kelime ve cümlelerin anlamsal uzaydaki sayısal dinamikleri incelenmiş, çok boyutlu embedding vektörleri kümelenerek görselleştirilmiştir.

---

## 🚀 Proje Kapsamı ve Analizler

### 1. Tokenization & Dil Karşılaştırmaları
* **Subword Tokenization:** Kelimelerin eklemeli dil yapılarına göre (özellikle Türkçe gibi dillerde) nasıl alt kelime birimlerine ayrıldığı analiz edilmiştir.
* **Maliyet ve Context Penceresi:** Türkçe ve İngilizce metinlerin token tüketim miktarları kıyaslanmış; dil modellerinin yerel dillerdeki tokenlaştırma agresifliği ve bunun bağlam penceresine etkileri yorumlanmıştır.

### 2. Sentence Embeddings & Anlamsal Benzerlik
* `sentence-transformers` kullanılarak metinler sabit **384 boyutlu** vektör uzayına izdüşürülmüştür.
* Eş anlamlı fakat farklı kelimeler içeren cümlelerin anlamsal yakınlığı **Cosine Similarity (Kosinüs Benzerliği)** metriği ile ölçülmüştür.
* **Embedding Sınırları:** *"Köpek kediyi kovaladı"* ve *"Kedi köpeği kovaladı"* gibi özne-nesne yer değişimi içeren cümlelerin yüksek benzerlik skoru vermesi üzerinden, standart embedding modellerinin sözdizimsel (syntax) yön körlüğü tartışılmıştır.

### 3. Benzerlik Matrisi & Heatmap Görselleştirme
* Çoklu cümle havuzlarının birbiriyle olan anlamsal ilişkileri `seaborn` ısı haritası (`heatmap`) kullanılarak görselleştirilmiştir.
* Farklı konu başlıklarının (Yazılım, Yemek, Uzay) anlamsal uzayda nasıl belirgin mavi adacıklar oluşturarak kümelendiği doğrulanmıştır.

### 4. Boyut İndirgeme & Kümeleme (K-Means + PCA)
* 384 boyutlu embedding vektörleri, herhangi bir etiket (label) olmaksızın **K-Means** algoritması ile gözetimsiz olarak kümelenmiştir.
* **PCA (Temel Bileşenler Analizi)** mimarisi kullanılarak yüksek boyutlu uzay, insan gözünün algılayabileceği 2 boyuta indirgenmiştir.
* Boyut sıkıştırma esnasında yaşanan **bilgi kaybının (Information Loss)** kümeleme grafiğindeki yansımaları ve görsel sapmaları analiz edilmiştir.

---

## 🛠️ Kullanılan Teknolojiler

* **Dil:** Python 3.x
* **NLP & Vektörleştirme:** `sentence-transformers` (Model: `paraphrase-multilingual-MiniLM-L12-v2`)
* **Makine Öğrenmesi:** `scikit-learn` (`KMeans`, `PCA`)
* **Veri Görselleştirme:** `matplotlib`, `seaborn`


