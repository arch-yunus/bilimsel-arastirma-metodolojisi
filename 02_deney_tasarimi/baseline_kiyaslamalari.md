# ⚖️ Referans (Baseline) Kıyaslamaları

## 📌 Giriş ve Tanım
**Referans Model (Baseline)**, önerilen yeni bir yöntemin veya modelin ne kadar başarılı olduğunu objektif olarak ölçmek için kullanılan **başlangıç noktası veya mevcut standart çözümdür**.

Bilimsel araştırmalarda yeni bir yöntemin başarısını iddia etmenin tek yolu, o yöntemi uygun referans modellerle **adil (fair) ve bağlamsal** olarak kıyaslamaktır. Kıyaslama yapmadan sunulan hiçbir başarı metriğinin akademik veya sektörel geçerliliği yoktur.

---

## 🔬 Referans Model Çeşitleri

Bir araştırmada genellikle üç seviyeli referans grubu kurulmalıdır:

### 1. Sezgisel (Trivial / Heuristic) Referanslar
Problemi en basit mantıkla çözen yöntemlerdir.
*   *Örnek (Sınıflandırma):* Her zaman en çok görülen sınıfı tahmin eden model (Zero-Rule).
*   *Örnek (Zaman Serisi):* Yarının hava durumunun bugününkiyle tamamen aynı olacağını tahmin eden model (Naive Predictor).
*   *Amaç:* Geliştirdiğiniz karmaşık sistemin, hiçbir şey öğrenmeyen rastgele bir algoritmadan daha iyi çalıştığını kanıtlamak.

### 2. Geleneksel / Standart Referanslar
Yapay zeka literatüründe kabul görmüş, uygulaması kolay standart makine öğrenmesi algoritmalarıdır.
*   *Örnek:* Derin öğrenme tabanlı bir metin sınıflandırıcı öneriyorsanız; referans olarak **TF-IDF + Lojistik Regresyon** veya **Random Forest** modellerinin sonuçları verilmelidir.
*   *Amaç:* Önerdiğiniz karmaşık mimarinin getirdiği hesaplama maliyetinin, basit yöntemlere göre gerçekten anlamlı bir fark yaratıp yaratmadığını görmek.

### 3. Güncel En İyi Çözümler (State-of-the-Art - SOTA)
Literatürde aynı veri seti ve problem üzerinde en yüksek başarıyı elde etmiş güncel modellerdir.
*   *Amaç:* Yönteminizin dünya genelindeki güncel literatüre olan katkısını ispatlamak.

---

## ⚠️ Literatür Kıyaslamalarındaki Hileler ve Hatalar

Birçok araştırmacı, kendi modellerini daha iyi göstermek amacıyla farkında olarak veya olmayarak şu metodolojik hataları yapar:

### 1. Zayıflatılmış Referans (Weakening Baselines)
Kendi modelinin hiperparametrelerini (öğrenme oranı, katman sayısı, optimizer vb.) günlerce optimize ederken, kıyasladığı referans modeli varsayılan (default) parametrelerle veya yetersiz eğitimle bırakıp "Bizim modelimiz X modelini geride bıraktı" demek.
*   *Doğru Yaklaşım:* Referans modele de aynı hiperparametre arama bütçesi ve optimizasyon hakkı tanınmalıdır.

### 2. Veri Ön İşleme Farklılıkları (Data Discrepancy)
Referans modelin orijinal makalesindeki sonuçları doğrudan kopyalamak, ancak kendi modelinizi eğitirken veri setini temizlemek, veri artırımı yapmak veya farklı bir train/test bölünmesi kullanmak adil değildir.
*   *Doğru Yaklaşım:* Tüm modeller **tamamen aynı ön işlemden geçmiş veri** ve **aynı veri bölünmesi** üzerinde sıfırdan eğitilmeli veya test edilmelidir.

### 3. İstatistiksel Anlamlılığın İhmali
Kendi modelinizin başarımını $\%91.2$, referans modeli ise $\%91.0$ bulup doğrudan daha iyi olduğunuzu iddia etmek hatalıdır. Bu $\%0.2$'lik fark tamamen şans eseri (seed farkından) kaynaklanmış olabilir.
*   *Doğru Yaklaşım:* Anlamlılık testi (**t-test**, **Wilcoxon signed-rank test** vb.) uygulanarak p-değerinin ($p < 0.05$) altında olduğu gösterilmelidir.

---

## 🛠️ Adil Bir Kıyaslama Protokolü Nasıl Kurulur?

1.  **Açık Kaynak Kod Kullanımı:** Referans modellerin orijinal yazarları tarafından paylaşılan resmi kod depolarını kullanın.
2.  **Aynı Donanım ve Koşullar:** Modellerin çıkarım (inference) hızlarını kıyaslarken aynı GPU/CPU ortamını kullanın.
3.  **Çoklu Koşum ve Hata Çubukları (Error Bars):** Her modeli en az 5-10 kez farklı `seed` değerleriyle çalıştırıp sonuçları $mean \pm std$ olarak verin.
4.  **Açıklanmış Hiperparametreler:** Hem kendi modelinizin hem de referans modellerin hiperparametre optimizasyon aralıklarını makalenin ek (appendix) kısmında şeffafça paylaşın.

## 📋 Araştırmacılar İçin Kontrol Listesi

- [ ] Seçtiğiniz referans modeller güncel literatürü temsil ediyor mu?
- [ ] Referans modelleri eğitirken kullandığınız veri kümesi, eğitim/test bölünmesi ve metrikler kendi modelinizle birebir aynı mı?
- [ ] Kıyaslama yaptığınız modellerin hiperparametrelerini optimize etmek için adil bir bütçe (süre/deneme sayısı) ayırdınız mı?
- [ ] Elde ettiğiniz başarım farkının istatistiksel olarak anlamlı olduğunu (t-test vb. ile) doğruladınız mı?
