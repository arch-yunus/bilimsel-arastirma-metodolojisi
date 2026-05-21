# 🍒 Cımbızlama Hatası (Cherry-picking)

## 📌 Tanım ve Kapsam
**Cımbızlama Hatası (Cherry-picking)**, bir araştırmacının veya geliştiricinin, hipotezini desteklemeyen ya da sisteminin zayıflığını gösteren verileri/deney sonuçlarını kasıtlı veya kasıtsız olarak görmezden gelip; sadece **hedefi destekleyen en başarılı, en parlak sonuçları raporlaması** durumudur. 

Akademik makalelerde ve sistem sunumlarında sıklıkla yapılan bu metodolojik hata, bilimin nesnellik ilkesine aykırıdır ve literatürde **sahte SOTA (State-of-the-Art)** iddialarının artmasına yol açar.

---

## 🔬 Cımbızlama Çeşitleri ve Bilgisayar Bilimlerindeki Yansımaları

Bilgisayar bilimleri ve yapay zeka araştırmalarında cımbızlama hatası genellikle şu şekillerde karşımıza çıkar:

### 1. En İyi "Seed" Sonucunu Raporlamak (Selective Reporting)
Derin öğrenme modelleri ağırlık başlangıçları (initialization) ve veri karıştırma (shuffling) işlemlerinde rastgele sayılara (seed) güvenir. Bir araştırmacı, modeli 20 farklı `seed` ile çalıştırıp bunlardan sadece en yüksek başarım veren **tek bir çalışmayı (run)** makalesine yazıp diğer 19 başarısız denemeyi gizliyorsa, bu bariz bir cımbızlamadır.
*   **Doğru Yaklaşım:** Ortalama (mean) ve standart sapma (standard deviation) değerlerini raporlamak (Örn: $\%89.2 \pm 0.4$).

### 2. P-Hacking ve Çoklu Test Problemi (Multiple Testing)
Yeterince fazla sayıda değişken üzerinde test yapıldığında, hiçbir anlamlı ilişki olmasa bile şans eseri (istatistiksel hata payı nedeniyle) bir tanesinin anlamlı çıkması kaçınılmazdır. İstatistiksel anlamlılık sınırı $\alpha = 0.05$ iken 20 farklı hipotez test edilirse, en az birinin şans eseri anlamlı çıkma olasılığı yaklaşık $\%64$'tür. Sadece bu şans eseri anlamlı çıkan sonucu yayınlamak istatistiksel bir cımbızlamadır.

### 3. Veri Sızıntısı ile Gelen Sahte Başarı (Data Leakage)
Eğitim setindeki bilgilerin test setine sızması (örneğin normalize etme işleminin tüm veri setine eğitim öncesi uygulanması) test sonuçlarını aşırı şişirir. Bu durumu fark etmesine rağmen raporlamayan veya araştırmayan araştırmacı cımbızlama hatasını sürdürmüş olur.

### 4. Sadece Seçilmiş Kalitatif Görselleri Sunmak
Görüntü işleme, üretken yapay zeka (GAN, LLM vb.) makalelerinde modelin ürettiği 1000 görselden en estetik ve hatasız 3 tanesini makaleye koyup, arkadaki 997 hatalı veya bozuk üretimi saklamak yaygın bir cımbızlama biçimidir.

---

## ⚠️ Akademik ve Sektörel Zararları

*   **Yeniden Üretilebilirlik Krizine Katkı:** Başka bir ekip aynı algoritmayı kendi verilerinde denediğinde vaat edilen yüksek başarıya asla ulaşamaz.
*   **Kaynak İsrafı:** Sektörde veya akademide, aslında çalışmayan yöntemlerin üzerine inşa edilen yeni projelerin hüsranla sonuçlanması.
*   **Bilimsel Güven Kaybı:** Yapay zeka ve sistem araştırmalarına duyulan güvenin sarsılması.

---

## 🛠️ Çözüm ve Önleme Stratejileri

Araştırmalarınızda cımbızlama ithamından kaçınmak ve bilimsel doğruluğu sağlamak için şu adımları izleyin:

### 1. Rastgele Deneylerin Raporlanması (Multi-run Evaluation)
Sisteminizi tek bir çalıştırma ile değerlendirmeyin. En az 5 veya 10 farklı rastgele başlangıç (seed) belirleyerek çalıştırın ve sonuçları standart sapmasıyla birlikte verin.

```python
# Kötü Uygulama (Cımbızlama):
best_accuracy = max(runs)  # Sadece bunu raporlamak

# İyi Uygulama (Bilimsel):
mean_accuracy = np.mean(runs)
std_dev = np.std(runs)
print(f"Model Accuracy: {mean_accuracy:.2f}% (+/- {std_dev:.2f}%)")
```

### 2. Dondurulmuş Test Setleri (Hold-out / Test Set Isolation)
Model geliştirme sürecinde (hiperparametre optimizasyonu, mimari denemeler) test setine asla dokunmayın. Test seti bir kilitli kutu gibi saklanmalı ve sadece **nihai model** değerlendirilirken **bir kez** açılmalıdır.

### 3. Hata Analizi (Failure Cases / Error Analysis)
Makalenizin veya raporunuzun bir bölümünü mutlaka **"Kısıtlamalar ve Hata Analizi" (Limitations & Error Analysis)** konusuna ayırın. Modelinizin hangi durumlarda başarısız olduğunu şeffaf bir şekilde gösterin. Başarısızlık senaryolarını analiz etmek, yönteminize olan bilimsel güveni azaltmaz aksine artırır.

### 4. Rastgele Seçilmiş Örnek Raporlama
Kalitatif (nitel) sonuçlar sunarken (örneğin modelin çevirdiği cümleler veya ürettiği resimler), "Rastgele seçilen 10 örnek" başlığı altında, önceden belirlenmiş bir kurala göre seçilen (örneğin ilk 10 test örneği) sonuçları gösterin.

## 📋 Araştırmacılar İçin Kontrol Listesi

- [ ] Raporladığınız metrikler tek bir başarılı denemenin mi yoksa birden fazla çalışmanın ortalaması mı?
- [ ] Modelinizin zayıf kaldığı senaryoları ve başarısızlık durumlarını da raporda belirttiniz mi?
- [ ] Hiperparametreleri optimize ederken test seti üzerinde "overfitting" yapmadığınızdan emin misiniz? (Doğrulama seti kullandınız mı?)
- [ ] Raporlanan başarı oranları, bağımsız bir araştırmacının kodunuzu çalıştırdığında alacağı beklenti değeriyle (expected value) uyuşuyor mu?
