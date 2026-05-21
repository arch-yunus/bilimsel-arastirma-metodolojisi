# 🔄 Çapraz Doğrulama (Cross-Validation)

## 📌 Giriş ve Tanım
**Çapraz Doğrulama (Cross-Validation)**, bir makine öğrenmesi modelinin hiç görmediği veriler üzerindeki genellenebilirlik performansını en doğru şekilde tahmin etmek ve aşırı uyumu (overfitting) tespit etmek için kullanılan istatistiksel bir yeniden örnekleme (resampling) yöntemidir.

Tek bir train/test bölünmesi yapmak yerine, veriyi farklı kombinasyonlarda parçalara bölerek tüm verinin hem eğitim hem de test süreçlerinde yer almasını sağlar.

---

## 🔬 Çapraz Doğrulama Yöntemleri

Farklı veri yapıları farklı doğrulama stratejileri gerektirir:

```text
K-Fold Çapraz Doğrulama Yapısı (K=5 için)
İterasyon 1: [ Test ] [ Train ] [ Train ] [ Train ] [ Train ]
İterasyon 2: [ Train ] [ Test ] [ Train ] [ Train ] [ Train ]
İterasyon 3: [ Train ] [ Train ] [ Test ] [ Train ] [ Train ]
İterasyon 4: [ Train ] [ Train ] [ Train ] [ Test ] [ Train ]
İterasyon 5: [ Train ] [ Train ] [ Train ] [ Train ] [ Test ]
```

### 1. K-Fold Çapraz Doğrulama
Veri seti rastgele ve eşit büyüklükte $K$ adet alt kümeye (fold) ayrılır. Sırasıyla her bir parça test seti olarak seçilirken geri kalan $K-1$ parça eğitim için kullanılır. $K$ adet eğitim ve değerlendirme sonucunun ortalaması nihai performans olarak raporlanır.
*   **Ne Zaman Kullanılır?** Veri kümesi dengeli ve zamandan/gruplardan bağımsız olduğunda. Genellikle $K=5$ veya $K=10$ seçilir.

### 2. Tabakalı Çapraz Doğrulama (Stratified K-Fold)
Her bir fold'un içindeki sınıf dağılım oranlarının, orijinal veri setindeki sınıf dağılım oranlarıyla neredeyse aynı olmasını garanti eder.
*   **Ne Zaman Kullanılır?** Sınıf dağılımı dengesiz olduğunda (Örn. %95 sınıf A, %5 sınıf B). Standart K-Fold kullanıldığında bazı fold'lara hiç sınıf B örneği düşmeyebilir.

### 3. Zaman Serisi Çapraz Doğrulama (Time-Series Split / Walk-Forward)
Gelecekteki verilerin geçmişi tahmin etmek için kullanılması (yani gelecek sızıntısı) mantıksal bir hatadır. Bu yüzden veri karıştırılmadan (shuffling olmadan) sıralı olarak bölünür. Eğitim seti sürekli genişlerken, test seti her zaman eğitim setinden kronolojik olarak daha ilerideki bir zaman dilimini kapsar.
*   **Ne Zaman Kullanılır?** Finansal veriler, hava durumu veya sensör verisi gibi zamana bağlı sıralı verilerde.

### 4. Gruplandırılmış Çapraz Doğrulama (Group K-Fold)
Aynı gruptan/kişiden gelen verilerin hem eğitim hem de test kümesinde yer almasını engeller.
*   *Örnek:* 50 farklı hastadan alınan tıbbi görüntülerimiz olsun. Eğer standart K-Fold yaparsak, Ahmet Bey'in 3 görüntüsü eğitimde, 1 görüntüsü testte yer alabilir. Model Ahmet Bey'in özel doku yapısını ezberleyip testte başarılı olabilir. Group K-Fold ile Ahmet Bey'in *tüm* görüntüleri ya sadece eğitimde ya da sadece testte yer alır.
*   **Ne Zaman Kullanılır?** Veri setinde hasta ID, kullanıcı ID gibi gruplayıcı öznitelikler olduğunda ve modelin *yeni kullanıcılara* nasıl genelleneceğini ölçmek istediğimizde.

---

## ⚠️ En Yaygın Hata: Çapraz Doğrulama Öncesi Bilgi Sızıntısı

Veri bilimcilerin en sık düştüğü hata, **veri ön işleme (preprocessing)** işlemlerini çapraz doğrulamadan *önce* tüm veri setine uygulamaktır.

*   **Hatalı Akış:** Tüm veri setinin ortalamasını alıp boşlukları doldurmak (imputation), tüm veri setine göre normalizasyon (scaling) yapmak ve ardından K-Fold başlatmak. Bu durumda test fold'larının bilgisi eğitim fold'larına sızar ve doğrulama performansı yapay olarak yüksek çıkar.
*   **Doğru Akış:** Normalizasyon parametreleri (mean, std) sadece **eğitim fold'u** üzerinden hesaplanmalı ve elde edilen parametreler test fold'una uygulanmalıdır.

---

## 💻 Python ile Uygulama Örnekleri

Aşağıdaki scriptte farklı çapraz doğrulama şemalarının nasıl kodlanacağı gösterilmektedir:

```python
import numpy as np
from sklearn.model_selection import KFold, StratifiedKFold, TimeSeriesSplit
from sklearn.datasets import make_classification
from sklearn.preprocessing import StandardScaler
from sklearn.linear_model import LogisticRegression
from sklearn.pipeline import Pipeline

# 1. Sentetik Veri Oluşturma
X, y = make_classification(n_samples=1000, n_classes=2, weights=[0.8, 0.2], random_state=42)

# 2. Doğru İş Akışı (Pipeline Kullanımı - Bilgi Sızıntısını Önler)
pipeline = Pipeline([
    ('scaler', StandardScaler()),
    ('classifier', LogisticRegression())
])

# 3. K-Fold Çapraz Doğrulama
kf = KFold(n_splits=5, shuffle=True, random_state=42)
kf_scores = []
for train_idx, test_idx in kf.split(X):
    X_train, X_test = X[train_idx], X[test_idx]
    y_train, y_test = y[train_idx], y[test_idx]
    
    pipeline.fit(X_train, y_train)
    kf_scores.append(pipeline.score(X_test, y_test))

print(f"K-Fold Accuracy (5 folds): {np.mean(kf_scores):.4f} (+/- {np.std(kf_scores):.4f})")

# 4. Stratified K-Fold
skf = StratifiedKFold(n_splits=5, shuffle=True, random_state=42)
skf_scores = []
for train_idx, test_idx in skf.split(X, y):
    X_train, X_test = X[train_idx], X[test_idx]
    y_train, y_test = y[train_idx], y[test_idx]
    
    pipeline.fit(X_train, y_train)
    skf_scores.append(pipeline.score(X_test, y_test))

print(f"Stratified K-Fold Accuracy: {np.mean(skf_scores):.4f} (+/- {np.std(skf_scores):.4f})")
```

## 📋 Araştırmacılar İçin Kontrol Listesi

- [ ] Veri setiniz zamana bağlı mı? Eğer öyleyse rastgele karıştırma (shuffling) içeren doğrulama yöntemlerinden kaçındınız mı?
- [ ] Verinizde aynı bireye/gruba ait birden fazla kayıt var mı? Eğer varsa `GroupKFold` kullandınız mı?
- [ ] Normalizasyon, boyut azaltma (PCA) veya eksik veri tamamlama gibi ön işlemleri her bir fold için ayrı ayrı (tercihen `Pipeline` kullanarak) yaptınız mı?
- [ ] Raporladığınız başarı metriklerinin sadece ortalamasını değil, standart sapmasını da belirttiniz mi?
