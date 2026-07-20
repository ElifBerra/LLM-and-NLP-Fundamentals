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

## Ekran Görüntüleri

<img width="733" height="242" alt="image" src="https://github.com/user-attachments/assets/368d9dd5-d2e3-4ab9-9405-c9c5f8fcf34d" />

● Hugging Face'in Transformer kütüphanesini indirdik.
<img width="582" height="422" alt="image" src="https://github.com/user-attachments/assets/6aad7f66-6d2a-4918-9c8f-87a774c1f48a" />

 ● Kelimeler beklediğimden çok daha küçük parçalara bölündü. Örneğin model, "Yapay" kelimesini bile bütün olarak tanıyamayıp ['Ya', '##pa', '##y'] şeklinde 3 ayrı alt parçaya; "öğrenmesi" kelimesini ise ['ö', '##ğ', '##ren', '##mesi'] şeklinde 4 parçaya ayırdı. Modelin subword seviyesinde en küçük harf kombinasyonlarına kadar indiğini gördüm.
<img width="476" height="386" alt="image" src="https://github.com/user-attachments/assets/f592e755-2131-405c-97e5-1dcb6cede382" />

● Çok dilli (multilingual) büyük modellerin eğitim veri setlerinin (corpus) ezici bir çoğunluğu İngilizce metinlerden oluşur. Bu nedenle İngilizce kelimelerin neredeyse tamamı modelin sözlüğünde (Vocabulary) tek parça halinde yer alırken, Türkçe kelimeler hem eklemeli dil yapımız hem de veri setlerindeki azınlık durumu nedeniyle sürekli kök ve ek parçalarına (##) bölünmek zorunda kalır.
<img width="647" height="212" alt="image" src="https://github.com/user-attachments/assets/1cb01e71-78ae-408e-92a4-6cf167a50e1d" />

● Model "Çekoslovakya" ülkesini bile tek parça tanıyamamış; onu bile ç, ##eko, ##sl, ##ova, ##ky diye 5 ayrı parçaya bölmüş! Kelimenin geri kalan eklerini ise (##alı, ##la, ##ştı...) dildeki en sık geçen harf kombinasyonlarına göre ayırarak toplamda tam 14 parçaya bölmüş.

 ● Parçalanan eleman sayısı 14 olmasına rağmen len(tok.encode(zor_kelime)) sonucunun 16 çıkma sebebi, 101 ([CLS]) ve en sondaki 102 ([SEP]) özel sinyal ID'leridir.

 ● 1. Context WindowAçısından Anlamı:
● Hafızanın Hızlı Dolması: Büyük dil modellerinin tek seferde işleyebileceği maksimum bir token limiti (Context Window) vardır.
● Türkçe metinler, bu örnekte canlı canlı gördüğün gibi kelime bütünlüğüyle tanınmayıp sürekli en küçük hece ve ek parçalarına (##) bölündüğü için, İngilizceye kıyasla 3-4 kat daha fazla token harcar.
● Bu durum, modelin kısa süreli hafızasını çok daha agresif bir şekilde tüketir. Örneğin; İngilizce bir dökümanın 10.000 kelimesini modele tek seferde pürüzsüzce okutabilirken, aynı bilginin Türkçe karşılığı alt parçalara çok bölündüğü için belki de 3.000 kelimede hafıza limitine (Context Window) çarparak tıkanacaktır.

● 2. API Maliyeti Açısından Anlamı:
● Ekonomik Dezavantaj (Maliyet Tuzağı): OpenAI (GPT serisi), Anthropic (Claude) veya Mistral gibi ticari yapay zeka sağlayıcılarının tamamı seni yazdığın karakter veya kelime sayısına göre değil; arka planda harcanan "Token Sayısı" üzerinden faturalandırır.
● Ekranda gördüğün gibi, Türkçede tek bir kelime bile kendi içinde 14 farklı tokene parçalanıp maliyet çarpanını katlayabiliyor.
●Dolayısıyla, bir yazılım mimarı olarak production ortamına kod yazarken, tamamen aynı anlama gelen bir prompt/girdi sisteminin Türkçe sürümü arka planda devasa token dizileri ürettiği için, projenin operasyonel bulut bütçesi İngilizcesine kıyasla her zaman çok daha pahalıya patlayacaktır.

<img width="474" height="239" alt="image" src="https://github.com/user-attachments/assets/8d64db3c-9cd0-43ed-aa4c-aebac43433bb" />

● PyTorch motoru kurduk.

<img width="251" height="91" alt="image" src="https://github.com/user-attachments/assets/fc1be5cc-86a3-4846-9ed9-9d4cbeb29baa" />
● sentence-transformers Kütüphanesi Kurulumunu yaptık.
<img width="342" height="362" alt="image" src="https://github.com/user-attachments/assets/25e80c64-bb53-467b-8545-e7bc747e1102" />

● Çalıştırdığım kodun çıktısında vektörün boyutu 384 olarak göründü.
→ Girdiğim "Bugün hava çok güzel." cümlesi, embedding modeli tarafından 384 adet ondalıklı sayıdanoluşan tek bir diziye (vektöre) dönüştürülmüştür. Geometrik açıdan bakarsak; bu metin artık 384 boyutlu bir koordinat uzayında tek bir nokta koordinat olarak temsil edilmektedir.
→ Tek tek bakıldığında hiçbir sayı tek başına anlamlı veya okunabilir bir bilgi sunmaz. Örneğin ilk sayının 0.2483785 olması bize "hava" ya da "güzel" kelimesine dair tek başına hiçbir şey ifade etmez.
→Dil modellerinde anlamsal öz; tek bir sayının değerinde değil, o 384 sayının birlikte oluşturduğu benzersiz örüntüde, dağılımda ve yönelimde saklıdır.
<img width="371" height="121" alt="image" src="https://github.com/user-attachments/assets/6d7ba18e-eaed-4fae-b5c6-73f97f0ce0e4" />

● İster tek kelimelik bir metin girilsin ister uzun bir cümle, model her iki durumda da dışarıya tam olarak aynı uzunlukta, yani 384 boyutlu bir liste fırlatır.
Eğer metinlerin uzunluğuna göre çıkan vektörlerin boyutları değişseydi, onları birbiriyle matris çarpımlarına sokamazdık. İki farklı metnin anlamsal yakınlığını ölçebilmemiz, onları matematiksel olarak karşılaştırabilmemiz ve aynı veritabanında saklayabilmemiz için boyutların tamamen sabit uzunlukta olması zorunludur.

<img width="369" height="301" alt="image" src="https://github.com/user-attachments/assets/19c0e7ea-c66b-4ba8-bc34-36cfbfb60c86" />

● Kedi-Pisi cümleleri arasındaki ortak tek bir kelime bile yok. Buna rağmen kosinüs benzerliği skoru 0.5932 gibi yüksek bir oranda anlam yakınlığı verdi. Borsa cümlesiyle olan alakasız skor ise sıfıra yakın (0.0185) çıktı.
→Klasik arama mantığı (keyword search) olsaydı bu iki cümlenin ortak kelimesi olmadığı için benzerlik skoru 0 çıkardı. Ancak embedding modeli, metinleri kelime karakteri olarak değil, anlamsal uzaydaki vektör doğrultuları olarak eşleştirir. "Kedi-pisi", "bahçe-çimen" ve "uyumak-şekerleme yapmak" kavramları uzayda birbirine çok yakın yönlere baktığı için, sistem ortak kelime olmasa bile anlamsal yakınlığı pürüzsüzce yakalamıştır.

<img width="355" height="164" alt="image" src="https://github.com/user-attachments/assets/b7dddcb2-2c4a-49a3-9518-2f6b701c2769" />

● (a) Farklı Kelimeler / Aynı Anlam Skoru: 0.8725 "Hekim" ve "Doktor" cümleleri arasında neredeyse hiçbir ortak kelime olmamasına rağmen model anlamsal benzerliği kusursuzca yakalamış ve %87.2 gibi çok yüksek bir skor fırlatmış. Anlamsal yakınlığın klasik kelime eşleşmesinden farkını burada netçe görebiliyoruz.
● (b) Aynı Kelimeler / Farklı Anlam Skoru: 0.9911 İşte az önce bahsettiğimiz embedding modellerinin en büyük zafiyeti ve kör noktası! "Köpek kediyi kovaladı" ile "Kedi köpeği kovaladı" cümleleri mantıksal olarak birbirinin taban tabana zıttı olmasına rağmen model bunlara %99.1 oranında (neredeyse tamamen aynı cümlelermiş gibi) benzerlik skoru verdi.
→Standart embedding modelleri kelimelerin genel "konusunu", "ortamını" ve "anlamlarını" harika yakalar; iki cümlede de kedi, köpek ve kovalama eylemi geçtiği için onları uzayda yan yana konumlandırır. Ancak cümle içindeki özne-nesne ilişkisini, mantıksal yönü ve sıra detaylarını (syntax) yakalamakta kör kalabilir. Bu yüzden production mimarilerinde sadece embedding uzayına güvenilmez, tam kelime eşleşmesini de koruyan Hibrit Arama (Hybrid Search) yapıları tercih edilir. 

<img width="309" height="285" alt="image" src="https://github.com/user-attachments/assets/9e147f70-48b4-48a0-b193-6796fc4b734e" />

● Grafikteki renk skalasına bakarsak, değerler 1.0'a yaklaştıkça mavi renk belirgin şekilde koyulaşıyor. "Kedi" ve "Bir pisi" cümlelerinin kesişimi 0.59 değeriyle orta koyulukta bir mavi almış. "Borsa" ve "Dolar" cümlelerinin kesişimi ise kendi aralarında pozitif bir yönde. Hayvan cümleleri ile ekonomi cümlelerinin kesiştiği pencereler ise tamamen beyaza yakın (örneğin 0.019, 0.024 gibi sıfıra çok yakın değerler) kalarak anlamsal olarak tamamen ayrıştıklarını kanıtlıyor.
● Sol üstten sağ alta doğru inen çapraz hattın (köşegen) tamamen en koyu mavi renkle 1 olmasının sebebi, her cümlenin matriste kendisiyle kıyaslanmasıdır. Bir vektörün kendisiyle olan yönü ve doğrultusu tamamen aynı olduğu için kosinüs benzerliği değeri maksimum sınır olan 1.0 çıkar. 

<img width="371" height="373" alt="image" src="https://github.com/user-attachments/assets/78760f80-c92c-46cc-802f-1390d8705e1c" />

● Model, cümlelerin hangi konudan geldiğini hiçbir etiket (label) olmadan, sadece anlamsal uzaydaki yönlerine bakarak pürüzsüzce ayrıştırmayı başardı. 

<img width="647" height="106" alt="image" src="https://github.com/user-attachments/assets/ab9a5170-142b-429f-a628-5d2ece26171c" />
● scikit-learn Kurulumu

<img width="360" height="522" alt="image" src="https://github.com/user-attachments/assets/e23d9ab2-3027-4d51-8ce1-f255bbed962f" />
● K-Means'in cümleleri doğru konulara tam olarak doğru ayırdığını söyleyemeyiz. Algoritma kedi, pisi ve köpek cümlelerini başarıyla aynı gruba toplarken, tamamen farklı bir finans konusu olan "Borsa bugün yüzde üç düştü." cümlesini de bu gruba dahil etmiştir. Buna karşılık "Dolar kuru sabah yükseldi." cümlesini tek başına ayrı bir kümeye koymuştur. Ancak modele hangi cümlenin hangi konudan olduğuna dair hiçbir label vermememize rağmen veriyi kendi içinde sayısal yakınlıklara göre gruplaması, Unsupervised Learning’in net bir çalışmasıdır. 
● Görsel düzlemde net bir ayrışma fark ediliyor. Hayvan temalı 3 cümle grafiğin sağ tarafında yatay bir hat üzerinde kümelenirken, ekonomi temalı iki cümle (borsa ve dolar) grafiğin sol tarafında alt alta konumlanarak kendi içlerinde ayrı bir blok oluşturmuş durumdalar. 

<img width="366" height="457" alt="image" src="https://github.com/user-attachments/assets/5c9b6e81-62e9-4a77-a719-0075ad1b9fa0" />
● n_clusters = 2  Durumu ([0 0 0 0 1]):
"Dolar kuru" kırmızı renkle tek başına bir küme olurken, geriye kalan 4 cümle (Borsa dahil) mor renkle tek bir kümede toplanmış. Verideki 2 ana konsepti (Hayvan-Ekonomi) yakalamaya en yakın çalışan, yani varyansı en geniş hatlarıyla bölmeye çalışan temel yapı budur.
● n_clusters = 3  Durumu ([2 0 0 1 0]):
Algoritma "Borsa" ve "Kedi" cümlelerini tek başlarına birer küme haline getirmiş; "Dolar", "Pisi" ve "Köpek" cümlelerini ise aynı kümeye sıkıştırmış. Hayvan ve ekonomi kavramları matematiksel olarak tamamen birbirine girmiş durumda.
● n_clusters = 4  Durumu ([2 0 0 1 3]):
Burada zaten toplamda 5 cümlemiz olduğu için sistem neredeyse her noktayı ayrı bir küme yapmış: "Borsa" , "Kedi" , "Dolar" , "Köpek" şeklinde dağılıyor. Sadece "Pisi" ve "Köpek" aynı renkte kalmış.

→ Bu veri kümesi için teorik ve mantıksal olarak en uygun küme sayısı 2 (n_clusters=2)'dir.

→ Nedeni: Elimizdeki metinler temelde sadece 2 farklı konsept alanına (Finans ve Hayvanlar) aittir. k=3 ve k=4 yapıldığında, veri seti çok küçük (toplam 5 cümle) olduğu için algoritma anlamsız alt bölünmeler yapmakta, "Kedi" ile "Borsa"yı tamamen alakasız cümlelerle aynı sepetlere koymaktadır. Küçük veri gruplarında küme sayısını artırmak gürültüyü artırır. Dolayısıyla konsept bütünlüğünü en azından makro düzeyde korumaya çalışan en kararlı sayı 2'dir.

● Not: PCA 384 boyutu 2'ye indirirken bilgi kaybeder. Grafik, gerçeğin basitleştirilmiş bir gölgesi — yine de örüntüyü görmeye yeter.

→ Matematiksel Arka Plan (384 → 2):
Modelin ürettiği embedding vektörleri normalde 384 boyutlu devasa bir hiper-uzayda yaşar. Lineer cebirsel bir yöntem olan PCA (Temel Bileşenler Analizi), bu 384 eksendeki bilgiyi en iyi temsil edecek şekilde seçtiği en baskın 2 yeni eksene (Temel Bileşen 1 ve Temel Bileşen 2) izdüşürür.
→ Neden "Basitleştirilmiş Bir Gölge"?
384 boyuttaki geometrik uzaklıkları ve ince anlamsal ilişkileri sadece 2 boyuta sıkıştırdığımızda, verinin içindeki birçok varyans ve detay kaçınılmaz olarak kaybolur. Tıpkı 3 boyutlu elinin duvara yansıyan gölgesinin, elinin derinliğini, rengini veya parmaklarının tam yerleşimini tam olarak gösterememesi gibidir. Grafikte "Borsa" cümlesinin hayvan kümesine yakın durması ya da k=3, 4 yapıldığında kümelerin birbirine karışması tamamen bu sıkışmadan kaynaklanan bir illüzyondur.
→ "Yine de Örüntüyü Görmeye Yeter" Ne Demek?
Her ne kadar veri ve detay kaybetsek de, PCA en yüksek varyansı korumaya çalıştığı için verinin genel hatlarını, büyük gruplanmaları ve zıt kutupları (örneğin hayvan cümlelerinin sağda, ekonomi cümlelerinin solda kümelenme eğilimini) insan gözünün algılayabileceği bir düzleme indirger.
→ Yüksek boyutlu verilerle çalışırken görselleştirme yapmak, gerçeğin mükemmel bir resmini sunmaz; sadece basitleştirilmiş bir haritasını verir. Kümeleme ve benzerlik hesaplamaları her zaman arka plandaki orijinal yüksek boyutlu (384) uzayda yapılmalı, 2 boyutlu grafikler (PCA) ise sadece örüntüleri kabaca gözlemlemek ve sezgi geliştirmek için bir araç olarak kullanılmalıdır. 








