# ✂️ Ablasyon Çalışmaları (Ablation Studies)

## 📌 Giriş ve Tanım
**Ablasyon Çalışması (Ablation Study)**, karmaşık bir yapay zeka mimarisinin, makine öğrenmesi modelinin veya yazılım sisteminin **belirli bileşenlerinin, katmanlarının veya özelliklerinin sistemden tek tek çıkarılarak** veya devre dışı bırakılarak sistemin genel performansının nasıl değiştiğini inceleyen kontrollü deneylerdir.

Tıp biliminden (özellikle nörolojiden, bir beyin bölgesinin hasar gördüğünde hangi fonksiyonların kaybedildiğini araştırma yönteminden) ödünç alınan bu terim, bilgisayar bilimlerinde **"nedensel katkı kanıtı"** sunmanın en güçlü yoludur.

---

## 🔬 Ablasyon Çalışmaları Neden Hayatidir?

Modern yapay zeka sistemleri birçok farklı bileşenden (veri artırma teknikleri, aktivasyon fonksiyonları, normalizasyon katmanları, kayıp fonksiyonları, dikkat mekanizmaları vb.) oluşur. Eğer yeni geliştirdiğiniz bir modelin literatürdeki diğer modellerden daha iyi çalıştığını iddia ediyorsanız, şu kritik soruyu cevaplamanız gerekir:

> *"Sistemin başarısı gerçekten önerdiğiniz yeni temel modülden mi kaynaklanıyor, yoksa sisteme eklediğiniz standart bir ince ayardan (LR Warmup, Dropout, LayerNorm vb.) mı?"*

Ablasyon çalışmaları olmaksızın yapılan bir çalışma:
*   **Gereksiz Karmaşıklığı Gizler:** Aslında sisteme hiçbir katkısı olmayan, hatta performansı düşüren boşuna eklenmiş modülleri fark edemezsiniz.
*   **Literatürü Yanıltır:** Diğer araştırmacılar sizin önerdiğiniz ana fikir yerine, sistemin yan bileşenlerinin getirdiği avantajı asıl yenilik zannedebilir.

---

## 🛠️ Adım Adım Ablasyon Deneyi Tasarımı

Etkili bir ablasyon çalışması tasarlamak için şu adımları izleyin:

1.  **Tam Modelin Tanımlanması (Full System / Proposed):** Önerdiğiniz, tüm bileşenleri aktif olan en güçlü model varyasyonunu referans alın.
2.  **Bileşenlerin Belirlenmesi:** Sistemin performansını etkileyebilecek tüm bağımsız modülleri, hiperparametreleri ve teknikleri listeleyin.
3.  **Tekil Çıkarma (One-at-a-time Ablation):** Her deneyde *sadece bir bileşeni* sistemden çıkarın ve diğer tüm parametreleri (seed, öğrenme oranı, batch size) sabit tutarak modeli tekrar eğitin.
4.  **Kümülatif Ekleme (Cumulative / Bottom-Up):** En temel halden (baseline) başlayarak bileşenleri tek tek ekleyin ve her eklemenin marjinal katkısını ölçün.

---

## 📊 Örnek Ablasyon Çalışması Tablosu

Bir derin öğrenme makalesinde ablasyon çalışmalarını sunmanın en standart yolu **Ablasyon Tablosu** hazırlamaktır. Aşağıda bir bilgisayarlı görü (Computer Vision) projesine ait örnek bir tablo yer almaktadır:

| Model ID | Değişiklik / Konfigürasyon | Çıkarılan/Değişen Bileşen | Doğruluk (Accuracy %) | Çalışma Süresi (ms/im) | Gözlem / Yorum |
|---|---|---|---|---|---|
| **M0** | **Önerilen Model (Full)** | *Hiçbiri* | **92.4** | 24.5 | En iyi performans |
| M1 | w/o Attention | Dikkat Mekanizması Çıkarıldı | 88.1 (-4.3) | 18.2 | Dikkat katmanı performansa ciddi katkı sağlıyor. |
| M2 | w/o Data Augmentation | Veri Artırma Devre Dışı | 89.5 (-2.9) | 24.5 | Model overfitting yapmaya başladı. |
| M3 | w/o Layer Normalization | Katman Normalizasyonu Çıkarıldı | 84.2 (-8.2) | 23.8 | Gradyan patlaması/sönümlenmesi yaşandı. |
| M4 | w/o Skip Connections | Artık Bağlantılar Çıkarıldı | 78.9 (-13.5) | 24.1 | Derin ağlarda optimizasyon çöktü. |

*(w/o = without / -sız, -siz)*

---

## 💻 Python / PyTorch ile Teorik Kod Şablonu

Aşağıdaki kod şablonu, model içerisindeki modüllerin parametrik olarak nasıl kapatılıp açılabileceğini ve ablasyon denemelerinin nasıl yönetilebileceğini göstermektedir:

```python
import torch
import torch.nn as nn

class ResearchModel(nn.Module):
    def __init__(self, use_attention=True, use_dropout=True):
        super(ResearchModel, self).__init__()
        self.use_attention = use_attention
        self.use_dropout = use_dropout
        
        self.backbone = nn.Sequential(
            nn.Linear(512, 256),
            nn.ReLU(),
            nn.Dropout(0.5) if use_dropout else nn.Identity()
        )
        
        if self.use_attention:
            self.attention = nn.Linear(256, 256) # Basit dikkat simülasyonu
            
        self.classifier = nn.Linear(256, 10)
        
    def forward(self, x):
        x = self.backbone(x)
        if self.use_attention:
            attn_weights = torch.softmax(self.attention(x), dim=-1)
            x = x * attn_weights
        return self.classifier(x)

# --- Deney Yönetimi (Ablation Controller) ---
deneyler = {
    "Full Model (Proposed)": {"use_attention": True, "use_dropout": True},
    "Ablated: No Attention": {"use_attention": False, "use_dropout": True},
    "Ablated: No Dropout":   {"use_attention": True, "use_dropout": False},
}

for isim, config in deneyler.items():
    model = ResearchModel(**config)
    print(f"Eğitiliyor: {isim} -> Parametre Sayısı: {sum(p.numel() for p in model.parameters())}")
    # train_and_evaluate(model)
```

## 📋 Araştırmacılar İçin Kontrol Listesi

- [ ] Modelinizin literatüre sunduğu "ana yeniliği" net olarak izole ettiniz mi?
- [ ] Önerdiğiniz yenilik haricindeki tüm iyileştirici ekstraları (özel optimizasyonlar, gelişmiş veri ön işleme) kapatarak denemeler yaptınız mı?
- [ ] Ablasyon deneylerinde tüm ortak parametreleri (learning rate, epochs, optimizer, seed) tamamen sabit tuttunuz mu?
- [ ] Sonuçları sadece başarı yüzdesi bazında değil, hesaplama maliyeti (flops, parametre sayısı, inference süresi) bazında da karşılaştırdınız mı?
