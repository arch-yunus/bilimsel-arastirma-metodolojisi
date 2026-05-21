# 🎯 Doğruluk Paradoksu (Accuracy Paradox)

## 📌 Giriş ve Tanım
**Doğruluk Paradoksu (Accuracy Paradox)**, bir makine öğrenmesi sınıflandırma modelinin yüksek bir doğruluk (accuracy) değerine sahip olmasına rağmen, pratikte tamamen yararsız ve hatta tehlikeli derecede başarısız olması durumudur. 

Bu durum, özellikle **dengesiz veri kümelerinde (imbalanced datasets)** —yani bir sınıfın örnek sayısının diğer sınıflardan ezici çoğunlukta olduğu durumlarda— ortaya çıkar.

---

## 🔬 Matematiksel Arka Plan

Doğruluk (Accuracy), doğru tahmin edilen örneklerin toplam örnek sayısına oranıdır:

$$\text{Doğruluk (Accuracy)} = \frac{TP + TN}{TP + TN + FP + FN}$$

Burada:
*   **TP (True Positive):** Pozitif olarak tahmin edilen ve gerçekte de pozitif olanlar.
*   **TN (True Negative):** Negatif olarak tahmin edilen ve gerçekte de negatif olanlar.
*   **FP (False Positive):** Pozitif tahmin edilen ama gerçekte negatif olanlar (Tip I Hata).
*   **FN (False Negative):** Negatif tahmin edilen ama gerçekte pozitif olanlar (Tip II Hata).

### Dengesiz Veri Kümesi Senaryosu
Diyelim ki elimizde **nadir rastlanan bir hastalık (örneğin kanser)** teşhisi için toplanmış 10.000 kişilik bir hasta veri seti var:
*   **Gerçek Sağlıklı Kişiler (Negatif Sınıf):** 9.900 kişi (%99)
*   **Gerçek Kanser Hastaları (Pozitif Sınıf):** 100 kişi (%1)

Hiçbir makine öğrenmesi modeli veya tıbbi bilgi içermeyen, sadece **herkese "Sağlıklı" (Negatif) teşhisi koyan** son derece ilkel bir "tahminci" (Zero-Rule Classifier) tasarlayalım. Bu ilkel tahmincinin performans metrikleri şöyle olacaktır:
*   **TP = 0** (Hiçbir hastayı doğru teşhis edemedi)
*   **TN = 9.900** (Tüm sağlıklı kişileri doğru bildi)
*   **FP = 0** (Hiçbir sağlıklı kişiye hasta demedi)
*   **FN = 100** (Tüm hastaları gözden kaçırdı ve sağlıklı dedi)

$$\text{Doğruluk} = \frac{0 + 9900}{0 + 9900 + 0 + 100} = \frac{9900}{10000} = \%99$$

**Paradoks:** Modelimiz **%99 doğruluk** oranına sahiptir ancak hayati öneme sahip kanser hastalarının **hiçbirini (%0)** tespit edememiştir. Akademik bir yayında sadece "%99 doğruluk elde ettik" ifadesinin kullanılması bu anlamda son derece yanıltıcıdır.

---

## 🛠️ Çözüm Metotları ve Alternatif Metrikler

Doğruluk metriğinin tek başına yetersiz kaldığı durumlarda aşağıdaki metrikler ve yöntemler kullanılmalıdır:

### 1. Hata/Karmaşıklık Matrisi (Confusion Matrix)
Modelin tahminlerinin dağılımını gösteren tablodur. Hatayı sadece oran olarak değil, tip olarak (Tip I vs Tip II) görmemizi sağlar.

| Gerçek / Tahmin | Tahmin: Pozitif (Hasta) | Tahmin: Negatif (Sağlıklı) |
|---|---|---|
| **Gerçek: Pozitif (Hasta)** | **True Positive (TP)** | **False Negative (FN)** (Kritik Hata) |
| **Gerçek: Negatif (Sağlıklı)** | **False Positive (FP)** | **True Negative (TN)** |

### 2. Kesinlik (Precision)
Pozitif olarak tahmin edilenlerin ne kadarının gerçekten pozitif olduğunu gösterir. Yanlış alarm (FP) maliyetinin yüksek olduğu durumlarda önemlidir.

$$\text{Precision} = \frac{TP}{TP + FP}$$

### 3. Duyarlılık (Recall / Sensitivity)
Gerçek pozitiflerin ne kadarının yakalanabildiğini gösterir. Kanser teşhisi gibi kaçırmanın (FN) ölümcül olduğu durumlarda en kritik metriktir.

$$\text{Recall} = \frac{TP}{TP + FN}$$

### 4. F1-Score
Precision ve Recall metriklerinin harmonik ortalamasıdır. Sınıf dengesizliği olan durumlarda en güvenilir tekil ölçüttür.

$$\text{F1-Score} = 2 \times \frac{\text{Precision} \times \text{Recall}}{\text{Precision} + \text{Recall}}$$

---

## 💻 Python ile Uygulamalı Örnek

Aşağıdaki kod bloğunda, dengesiz bir veri setinde "sürekli çoğunluk sınıfı tahmin eden" bir modelin doğruluk metriğinin nasıl aldattığını ve diğer metriklerin bu durumu nasıl açığa çıkardığını görebilirsiniz:

```python
import numpy as np
from sklearn.metrics import accuracy_score, precision_score, recall_score, f1_score, confusion_matrix

# 1. Dengesiz veri kümesi simülasyonu (%99 Sağlıklı [0], %1 Hasta [1])
np.random.seed(42)
y_true = np.concatenate([np.zeros(9900), np.ones(100)])

# 2. Tembel/Hileci Model (Her duruma 0/Negatif tahmini yapan)
y_pred = np.zeros(10000)

# 3. Metriklerin Hesaplanması
accuracy = accuracy_score(y_true, y_pred)
precision = precision_score(y_true, y_pred, zero_division=0)
recall = recall_score(y_true, y_pred, zero_division=0)
f1 = f1_score(y_true, y_pred, zero_division=0)
conf_matrix = confusion_matrix(y_true, y_pred)

print("--- Tembel Model Sonuçları ---")
print(f"Doğruluk (Accuracy) : {accuracy:.4f} (Yanıltıcı derecede yüksek!)")
print(f"Kesinlik (Precision): {precision:.4f} (Gerçek performansı gösteriyor)")
print(f"Duyarlılık (Recall)  : {recall:.4f} (Kritik hastaların yakalanma oranı)")
print(f"F1-Score            : {f1:.4f}")
print("\nKarmaşıklık Matrisi:")
print(conf_matrix)
```

## 📋 Araştırmacılar İçin Kontrol Listesi

- [ ] Veri setinizdeki sınıf dağılımlarını analiz ettiniz mi? (Örn. Sınıf oranları 1:10 veya daha yüksek mi?)
- [ ] Makalenizde/Raporunuzda doğruluğun (Accuracy) yanında mutlaka **F1-Score**, **Precision** ve **Recall** değerlerini raporladınız mı?
- [ ] Modelinizin **False Negative (FN)** ve **False Positive (FP)** değerlerini içeren Karmaşıklık Matrisini (Confusion Matrix) sundunuz mu?
- [ ] Karşılaştırma yaptığınız diğer literatür çalışmalarının metrik tercihlerini inceleyerek dengeli bir kıyaslama yaptınız mı?
