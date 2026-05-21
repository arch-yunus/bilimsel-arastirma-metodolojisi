# 🪒 Ockham'ın Usturası (Occam's Razor)

## 📌 Giriş ve Tanım
**Ockham'ın Usturası (Lex Parsimoniae - Sadelik İlkesi)**, bilim felsefesinde kullanılan ve **"Aynı sonucu veren teoriler arasında en basit olanı tercih edilmelidir"** mantığına dayanan temel bir prensiptir. 14. yüzyılda yaşamış İngiliz filozof William of Ockham'a atfedilmiştir.

Latince orijinal ifadesiyle:
> *“Entia non sunt multiplicanda praeter necessitatem.” (Varlıklar gereklilik olmaksızın çoğaltılmamalıdır.)*

Bu ilke, bilgisayar bilimleri, yapay zeka ve sistem mühendisliğinde **gereksiz karmaşıklığı kesip atmak** için en önemli rehberlerden biridir.

---

## 🔬 Bilgisayar Bilimleri ve Makine Öğrenmesindeki Yeri

Makine öğrenmesinde Ockham'ın Usturası, doğrudan **Aşırı Uyum (Overfitting)** ve **Genellenebilirlik (Generalization)** kavramlarıyla ilişkilidir.

```text
Model Karmaşıklığı vs Hata Grafiği

Hata (Error)
  ^
  |      \             / (Overfitting / Karmaşık Model)
  |       \           / 
  |        \_________/  <-- Optimum Nokta (Basit ama Yeterli)
  |       (Underfitting)
  +--------------------------------------> Model Karmaşıklığı
```

### 1. Model Seçiminde Sadelik
Bir problemi çözmek için iki farklı modelimiz olduğunu varsayalım:
*   **Model A (Basit):** 5 öznitelikli (feature) bir Lojistik Regresyon modeli. Doğruluk oranı: $\%89.2$
*   **Model B (Karmaşık):** 50 katmanlı, milyonlarca parametreli bir Derin Sinir Ağı. Doğruluk oranı: $\%89.5$

Ockham'ın Usturası ilkesine göre, $\%0.3$'lük marjinal bir kazanç için devasa bir hesaplama, bakım ve açıklanamazlık maliyeti getiren Model B yerine **Model A** tercih edilmelidir. Model A daha az parametre içerdiği için yeni veriler karşısında daha kararlı (robust) davranacaktır.

### 2. Aşırı Parametrelendirmenin Cezalandırılması
İstatistikte model karmaşıklığını dengelemek amacıyla şu matematiksel metrikler kullanılır:

*   **Akaike Bilgi Kriteri (AIC):**
    $$AIC = 2k - 2\ln(L)$$
*   **Bayesçi Bilgi Kriteri (BIC):**
    $$BIC = k\ln(n) - 2\ln(L)$$

Burada $k$ parametre sayısını (karmaşıklık), $L$ ise modelin olabilirlik (likelihood) değerini (başarımını) temsil eder. Hem AIC hem de BIC, parametre sayısı arttıkça modeli cezalandırır. **En düşük AIC/BIC değerine sahip model en optimum modeldir.**

---

## 🛠️ Sistem Tasarımında Sadelik (KISS Prensibi)

Ockham'ın Usturası yazılım mühendisliğinde **KISS (Keep It Simple, Stupid - Basit Tut, Aptal!)** prensibi olarak hayat bulur.

*   **Aşırı Mühendislik (Overengineering):** İleride gerekebilir düşüncesiyle sisteme onlarca mikroservis, karmaşık önbellek katmanları ve kullanılmayan tasarım kalıpları (design patterns) eklemek.
*   **Sonuç:** Hata ayıklama (debugging) süresinin uzaması, sistem kararsızlığının artması ve bakım maliyetlerinin patlaması.
*   *Çözüm:* İhtiyacı karşılayan en minimal mimari ile yola çıkıp, darboğazlar oluştukça sistemi ölçeklemek.

---

## 🧠 Model Açıklanabilirliği (Explainable AI - XAI)

Karmaşık modeller (Black-Box / Kara Kutu) yüksek tahmin gücü sunsalar da kararlarının arkasındaki mantığı açıklayamazlar. Basit modeller (karar ağaçları, doğrusal modeller) ise doğrudan yorumlanabilir. Tıp, hukuk veya finans gibi kritik alanlarda, kararın *neden* verildiğini açıklayabilmek çoğu zaman $\%1$'lik ekstra bir doğruluk oranından çok daha değerlidir.

## 📋 Araştırmacılar İçin Kontrol Listesi

- [ ] Problemi çözmek için öncelikle en basit baseline yöntemleri (örn. Linear/Logistic Regression, Naive Bayes) denediniz mi?
- [ ] Önerdiğiniz modeldeki tüm parametrelerin ve katmanların gerçekten gerekli olduğunu ablasyon çalışmalarıyla kanıtladınız mı?
- [ ] Model seçiminde sadece doğruluk metriklerini mi kullandınız, yoksa AIC/BIC gibi karmaşıklığı cezalandıran istatistiksel kriterleri de göz önünde bulundurdunuz mi?
- [ ] Tasarladığınız yazılım mimarisinde "ileride lazım olur" diye eklediğiniz ama şu an aktif işlevi olmayan yapıları ayıkladınız mı?
