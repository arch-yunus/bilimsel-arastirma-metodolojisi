# 🔄 Yeniden Üretilebilirlik (Reproducibility) Kılavuzu

## 📌 Giriş ve Tanım
**Yeniden Üretilebilirlik (Reproducibility)**, bağımsız bir araştırma ekibinin, orijinal makalede sunulan yöntemleri, veri setini ve bilgisayar kodunu kullanarak **birebir aynı veya istatistiksel olarak eşdeğer sonuçlara ulaşabilmesi** durumudur.

Bilgisayar bilimleri ve yapay zeka alanında son yıllarda ciddi bir **"Yeniden Üretilebilirlik Krizi" (Reproducibility Crisis)** yaşanmaktadır. Yayınlanan birçok makaledeki başarı oranlarının, kodlar veya parametreler eksik paylaşıldığı için başka araştırmacılarca tekrar elde edilemediği görülmüştür. Bir çalışmanın tekrar edilemez olması, onun spekülasyondan ibaret kalmasına yol açar.

---

## 🔬 Yeniden Üretilebilirliği Sağlamanın 5 Temel Direği

Bir araştırmanın yeniden üretilebilir olması için aşağıdaki bileşenlerin eksiksiz olarak sunulması gerekir:

```text
┌────────────────────────────────────────────────────────┐
│ 1. KAYNAK KODU (Temiz, belgelenmiş ve sürüm kontrollü) │
├────────────────────────────────────────────────────────┤
│ 2. VERİ SETİ (Erişilebilir, sürümlendirilmiş ve açık)   │
├────────────────────────────────────────────────────────┤
│ 3. YAZILIM ORTAMI (Kütüphane sürümleri, Docker/Conda)   │
├────────────────────────────────────────────────────────┤
│ 4. HİPERPARAMETRELER (Yapılandırma dosyaları - YAML)   │
├────────────────────────────────────────────────────────┤
│ 5. RASTGELELİK KONTROLÜ (Seed sabitleme)                │
└────────────────────────────────────────────────────────┘
```

### 1. Kaynak Kodu Paylaşımı
*   Kodunuzu GitHub, GitLab veya Bitbucket gibi platformlarda açık kaynak olarak paylaşın.
*   Kodun nasıl çalıştırılacağını gösteren açık bir `README.md` kılavuzu hazırlayın.
*   Spagetti kodlardan kaçının; kodun modüler, okunabilir ve yorum satırlarıyla desteklenmiş olmasını sağlayın.

### 2. Veri Seti Erişilebilirliği
*   Kullanılan ham veriyi ve bu veriyi işlemek için kullanılan ön işleme (preprocessing) scriptlerini paylaşın.
*   Veri setini **Zenodo**, **Hugging Face**, **Kaggle** veya **OSF** gibi kalıcı dijital nesne tanımlayıcı (DOI) veren platformlarda arşivleyin.
*   *Not:* Eğer veri gizli/tıbbi ise, veri setinin yapısını temsil eden yapay (synthetic) bir veri seti paylaşarak kodun test edilebilmesini sağlayın.

### 3. Çalışma Ortamının İzolasyonu
Geliştirme yaptığınız bilgisayardaki kütüphane sürümleri ile kodu çalıştıracak diğer kişinin kütüphane sürümleri arasındaki farklar kodun hata vermesine neden olur.
*   **Python requirements.txt:** `pip freeze > requirements.txt`
*   **Conda Ortamı:** `conda env export > environment.yml`
*   **Docker Konteyneri:** İşletim sistemi seviyesinde bağımlılıkları sabitlemek için en güvenli yoldur.

### 4. Konfigürasyon ve Hiperparametre Dosyaları
Hiperparametreleri kodun içerisine gömmek yerine, dışarıdan okunabilen konfigürasyon dosyalarında (`.yaml`, `.json` veya `.ini`) tutun. Bu sayede araştırmacılar hangi hiperparametrelerle hangi sonucun alındığını kolayca izleyebilir.

---

## 💻 Rastgeleliğin Kontrolü: Seed Sabitleme

Yapay zeka modellerinin eğitiminde rastgele başlangıç değerleri kullanılır. Sonuçların birebir aynı çıkması için kullanılan tüm kütüphanelerdeki rastgele sayı üreteçleri (pseudo-random number generators) aynı çekirdek (seed) değerine sabitlenmelidir.

Aşağıdaki Python fonksiyonu, makine öğrenmesi projelerinde rastgeleliği kontrol altına almak için standart olarak kullanılmalıdır:

```python
import random
import os
import numpy as np
import torch

def seed_everything(seed: int = 42):
    """
    Tüm kütüphanelerdeki rastgele sayı üreteçlerini sabitleyerek 
    deneylerin yeniden üretilebilir olmasını sağlar.
    """
    random.seed(seed)
    os.environ['PYTHONHASHSEED'] = str(seed)
    np.random.seed(seed)
    
    # PyTorch için seed sabitleme
    torch.manual_seed(seed)
    torch.cuda.manual_seed(seed)
    torch.cuda.manual_seed_all(seed) # Çoklu GPU kullanılıyorsa
    
    # Deterministik GPU operasyonları (Hızı biraz düşürebilir ama kesinlik sağlar)
    torch.backends.cudnn.deterministic = True
    torch.backends.cudnn.benchmark = False

# Deney başlangıcında çağrılmalıdır:
seed_everything(42)
```

---

## 📋 Araştırma Yayınlama Öncesi Yeniden Üretilebilirlik Kontrol Listesi

- [ ] Kodlarınızı kendi bilgisayarınızdan tamamen bağımsız, temiz bir sanal makinede (veya başka bir bilgisayarda) sıfırdan çalıştırıp test ettiniz mi?
- [ ] Kullandığınız tüm kütüphanelerin tam sürüm numaralarını içeren `requirements.txt` veya `environment.yml` dosyasını depoya eklediniz mi?
- [ ] Kodunuzdaki tüm rastgele sayı üreteçleri (seed) sabitlenmiş mi?
- [ ] Veri setini indirme ve ön işleme adımlarını (veri temizleme, train/test bölme) tamamen otomatize eden scriptleri paylaştınız mı?
- [ ] Modelinizi eğitmek için kullanılan tüm hiperparametreleri (örneğin öğrenme oranı, batch size, epoch sayısı) belirttiniz mi?
- [ ] Makalede elde ettiğiniz grafik ve tabloları doğrudan üreten kod satırlarını veya analiz notebook'larını depoya dahil ettiniz mi?
