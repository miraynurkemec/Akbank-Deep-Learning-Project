# Akbank-Deep-Learning-Project
Akbank Derin Öğrenme Bootcamp Projesi 
# Giriş

Bu proje, kraniyal manyetik rezonans (MRI) görüntülerinden **beyin tümörü sınıflandırması** yapmak üzere tasarlanmış, baştan uca bir **derin öğrenme çalışmasını** içerir. 

**Amaç;** görüntülerdeki yapısal ipuçlarını otomatik olarak yakalayarak **glioma, meningioma, pituitary** ve **no-tumor** sınıflarını güvenilir biçimde ayırt edebilen, **yeniden üretilebilir** ve **açıklanabilir** bir makine öğrenimi hattı (pipeline) sunmaktır. 

Proje, yalnızca bir model dosyası paylaşmakla kalmayıp; **verinin toplanmasından açıklanabilirliğe kadar tüm adımları** açıkça belgelendirir.

## Kullanılan Algoritmalar ve Yöntemler

* **Convolutional Neural Networks (CNN):**  
  * Temel evrişim katmanları (Conv2D, MaxPooling) ile öznitelik çıkarımı.  
  * Batch Normalization ve Dropout ile düzenlileştirme ve overfitting’in önlenmesi.  
  * Dense katmanlar ile sınıflandırma.

* **Veri Ön İşleme ve Artırma:**  
  * Görsellerin yeniden boyutlandırılması ve normalize edilmesi (0–1 aralığı).  
  * Veri artırma teknikleri: döndürme, kaydırma, yakınlaştırma, yatay çevirme.
  
* **Optimizasyon ve Eğitim Stratejileri:**  
  * `Adam` optimizasyon algoritması.  
  * Kayıp fonksiyonu: **Categorical Crossentropy**.
  * Callback’ler: **EarlyStopping** ve **ReduceLROnPlateau**.
  
* **Değerlendirme:**  
  * Accuracy, Precision, Recall, F1-score metrikleri.  
  * Confusion Matrix ile sınıflar arası karışmaların görselleştirilmesi.
  
* **Açıklanabilirlik (Explainability):**  
  * **Grad-CAM** ile modelin odaklandığı bölgelerin görselleştirilmesi.  
  * Alternatif olarak **Integrated Gradients** ile piksel önem haritaları.

# Metrikler

Bu projede elde edilen sonuçlar hem **sayısal metriklerle** hem de **görselleştirmelerle** detaylı olarak değerlendirilmiştir. Amaç, modelin yalnızca yüksek doğruluk sağlaması değil; aynı zamanda hangi sınıflarda güçlü, hangi sınıflarda zorlandığını da ortaya koymaktır.

---

## Eğitim ve Doğrulama Performansı

* **En iyi doğrulama doğruluğu (val_acc):** %93.08 (Modelin en iyi performansı)  
* **En iyi test doğruluğu (test_acc):** **%94.45**  
* **Test kaybı (test_loss):** **0.1544**  

Eğitim eğrileri incelendiğinde:  
* Eğitim doğruluğu istikrarlı bir şekilde artmıştır.  
* Doğrulama doğruluğu 10. epoch sonrasında plato yapmış ve erken durdurma ile en iyi model seçilmiştir.  
* Doğrulama kaybı 1.0 seviyesinin altında stabilize olmuştur; bu da **overfitting’in kontrol altında** olduğunu göstermektedir.  

---

## Sınıf Bazlı Sonuçlar

**Classification Report:**

| Sınıf       | Precision | Recall | F1-Score | Destek |
|-------------|-----------|--------|----------|--------|
| Glioma      | 0.92      | 0.98   | 0.95     | 324    |
| Meningioma  | 0.97      | 0.84   | 0.90     | 329    |
| No Tumor    | 0.99      | 0.97   | 0.98     | 400    |
| Pituitary   | 0.90      | 0.99   | 0.94     | 352    |

**Ortalama Sonuçlar:**
* Accuracy: **%94.45**  
* Macro Average: Precision 0.94 | Recall 0.94 | F1 0.94  
* Weighted Average: Precision 0.95 | Recall 0.94 | F1 0.94  

---

## Confusion Matrix Yorumları

Confusion Matrix incelendiğinde:  
* **Glioma:** %98 gibi çok yüksek recall değeri ile neredeyse tüm glioma vakaları doğru sınıflandırılmıştır. Precision değeri de oldukça yüksektir (0.92), bu da modelin hem kapsamda hem de kesinlikte başarılı olduğunu gösterir.  
* **Meningioma:** Precision değeri çok yüksektir (0.97), ancak recall (0.84) bu sınıflar arasında en düşük değerlerden biridir. Model bu sınıfta genel olarak güçlüdür.  
* **No Tumor:** Recall değeri çok yüksektir (0.97). Sağlıklı görüntülerin sınıflandırılmasında büyük ilerleme kaydedilmiştir ve F1 puanı %98'dir.  
* **Pituitary:** Çok yüksek recall değeri (%99) ve F1 puanı (%94) ile modelin en güçlü olduğu sınıflardan biri haline gelmiştir.  

---

## Genel Yorum

* Modelin **en güçlü olduğu sınıf:** No Tumor ve Pituitary (yüksek F1/Recall).  
* Modelin **en zayıf olduğu sınıf:** Meningioma (diğer sınıflara göre en düşük Recall).  
* **Overfitting:** Eğitim/validasyon eğrileri arasındaki fark kabul edilebilir düzeyde. Data augmentation ve dropout ile overfitting büyük ölçüde engellenmiştir.  
* **Genel başarı:** **%94.45** test doğruluğu, bu alan için oldukça güçlü bir performans göstergesidir.  

---

## Çıkarımlar

1. **Klinik anlam:** Model, tüm tümör türleri (Glioma, Meningioma, Pituitary) ve sağlıklı (No Tumor) görüntülerin tespitinde çok yüksek başarı göstermektedir.  
2. **Zayıf nokta:** Meningioma sınıfında, diğer sınıflara göre nispeten daha az kapsamlıdır.  
3. **Geliştirme fırsatları:**  
   * Transfer learning (EfficientNet, ResNet).  
   * Veri artırma tekniklerinin çeşitlendirilmesi.  
   * Class-weight veya focal loss ile dengesiz sınıfların desteklenmesi.  

---
# Sonuç ve Gelecek Çalışmalar

Bu proje kapsamında, beyin tümörlerinin MRI görüntülerinden sınıflandırılması için **CNN tabanlı bir derin öğrenme modeli** geliştirilmiştir. Yapılan deneyler sonucunda:

* Model, test setinde **%94’ün üzerinde** doğruluk elde ederek, tüm sınıflarda çok güçlü bir performans sergilemiştir.  
* **Confusion Matrix** ve sınıf bazlı metrikler incelendiğinde, modelin performansı tüm sınıflarda tutarlı ve yüksektir.  
* **Grad-CAM** ve **Eigen-CAM** ile görselleştirmeler, modelin karar verirken gerçekten tümör bölgelerine odaklandığını göstermiştir. Bu, klinik uygulamalar için güven artırıcı bir faktördür.  

---

## Gelecek Çalışmalar

Projenin bootcamp sonrası gelişim potansiyeli oldukça yüksektir. Gelecekte şu adımlar planlanabilir:

1. **Arayüz Geliştirme**  
   * Kullanıcı dostu bir web arayüzü (ör. Streamlit veya Flask) eklenerek, modelin gerçek zamanlı olarak test edilmesi sağlanabilir.  
   * Doktorlar veya öğrenciler için “MRI yükle → tahmin al → Grad-CAM görselleştir” akışı kurulabilir.  

2. **Gerçek Zamanlı Veri Toplama**  
   * Modelin farklı hastanelerden veya açık veri kaynaklarından gelecek yeni MRI görüntüleriyle sürekli güncellenmesi sağlanabilir.  
   * Böylece model, dinamik ve sürekli öğrenen bir yapıya kavuşur.  

3. **Transfer Learning ve Daha İleri Modeller**  
   * EfficientNet, ResNet veya Vision Transformer gibi daha güçlü mimarilerle performans artırılabilir.  
   * 3D CNN modelleri ile tek kesit yerine hacimsel (3D) MRI verileri işlenebilir.  

4. **Veri Dengesini İyileştirme**  
   * “No Tumor” sınıfında daha yüksek başarı elde etmek için daha geniş ve çeşitli sağlıklı MRI verileri toplanabilir.  
   * Class-weight, focal loss veya SMOTE gibi yöntemler ile dengesiz veri seti sorunları çözülebilir.  

5. **Kariyer ve Teknoloji Yönelimi**  
   * Bu proje, yapay zekâ ve sağlık teknolojileri kesişiminde güçlü bir öğrenme deneyimi sağlamaktadır.  
   * Gelecekte **AI in Healthcare**, **Explainable AI (XAI)** ve **Medical Imaging** alanlarında derinleşmek, hem akademik hem profesyonel kariyer için yol gösterici olacaktır.  

---

## Genel Değerlendirme

Bu çalışma, bootcamp sürecinde öğrenilenlerin somut bir çıktıya dönüştürülmesi açısından önemli bir kilometre taşıdır.  
Projeyi ilerletmek için atılacak her yeni adım (arayüz, gerçek zamanlı veri, daha gelişmiş modeller) hem teknik derinliği artıracak hem de projeyi gerçek dünyaya daha yakın hale getirecektir.  

# Linkler
https://www.kaggle.com/code/capswinther/akbank-deep-learning-brain-tumor-mriclassification
