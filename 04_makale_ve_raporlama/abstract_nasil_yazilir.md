# ✍️ Bilimsel Makale Özeti (Abstract) Nasıl Yazılır?

## 📌 Giriş ve Önem
Bir bilimsel makalenin **Özeti (Abstract)**, çalışmanızın vitrinidir. Editörler, hakemler ve diğer araştırmacılar genellikle makalenizin tamamını okuyup okumamaya sadece özet kısmına bakarak karar verirler. 

İyi bir özet, çalışmanın özünü en fazla **150-250 kelime** arasında, hiçbir gereksiz detay barındırmadan, doğrudan ve anlaşılır bir şekilde sunmalıdır.

---

## 🔬 5-Cümle Kuralı (The 5-Sentence Rule)

Etkili ve akıcı bir özet yazmak için önerilen en güçlü yapısal kılavuz **5-Cümle Metodu**’dur. Her bir cümle, çalışmanın farklı bir yapı taşını temsil eder:

```text
┌─────────────────────────────────────────────────────────┐
│ 1. Giriş / Bağlam (Geniş çerçeve ve önem)              │
├─────────────────────────────────────────────────────────┤
│ 2. Problem Tanımı (Çözülmek istenen spesifik zorluk)    │
├─────────────────────────────────────────────────────────┤
│ 3. Önerilen Yöntem (Geliştirilen özgün çözüm veya metot)│
├─────────────────────────────────────────────────────────┤
│ 4. Sayısal Bulgular (Elde edilen net ve ölçülebilir veri)│
├─────────────────────────────────────────────────────────┤
│ 5. Katkı ve Önem (Çalışmanın literatüre genel etkisi)   │
└─────────────────────────────────────────────────────────┘
```

### 1. Giriş ve Bağlam (Context)
*   **Soru:** Araştırdığınız konu genel olarak nedir ve neden önemlidir?
*   *Örnek:* Derin öğrenme tabanlı nesne tespiti modelleri, otonom sürüş sistemlerinin güvenliği için kritik bir rol oynamaktadır.

### 2. Problem Tanımı (Problem Statement)
*   **Soru:** Çözmek istediğiniz spesifik problem nedir ve mevcut literatür neden bu konuda yetersiz kalmaktadır?
*   *Örnek:* Ancak, mevcut modeller yoğun yağış veya sis gibi zorlu hava koşullarında yüksek yanlış negatif oranlarına sahiptir.

### 3. Önerilen Yöntem (Proposed Methodology)
*   **Soru:** Bu problemi çözmek için ne yaptınız? Önerdiğiniz özgün yaklaşım nedir?
*   *Örnek:* Bu çalışmada, zorlu hava koşullarındaki nesneleri tespit etmek amacıyla çoklu ölçekli dikkat mekanizmasına sahip yeni bir derin CNN mimarisi (WeatherNet) önerilmiştir.

### 4. Sayısal Bulgular ve Sonuçlar (Key Findings)
*   **Soru:** Deneyler sonucunda ne buldunuz? (Burada "başarımızı artırdık" gibi belirsiz ifadeler yerine **net sayısal oranlar** verilmelidir).
*   *Örnek:* Yaygın olarak kullanılan KITTI veri kümesi üzerinde yapılan testlerde WeatherNet, mevcut SOTA modellerine kıyasla ortalama hassasiyeti (mAP) $\%8.4$ artırarak $\%91.2$ seviyesine ulaştırmıştır.

### 5. Katkı ve Önem (Impact / Conclusion)
*   **Soru:** Bu çalışmanın literatüre veya uygulamaya katkısı nedir? Neden önemlidir?
*   *Örnek:* Geliştirilen bu yöntem, otonom araçların olumsuz hava koşullarındaki algılama güvenilirliğini artırmaya önemli bir katkı sağlamaktadır.

---

## 📊 Kötü vs. İyi Özet Karşılaştırması

### ❌ Kötü Özet Örneği (Belirsiz ve Sayısalsız)
> "Bu makalede derin öğrenme modellerinin görüntü sınıflandırma performansı araştırılmıştır. Yeni bir model geliştirdik. Bu modelimiz diğer modellerden daha iyi çalışmaktadır. Sonuçlar modelimizin doğruluğunun arttığını göstermiştir. Bu çalışma gelecekteki çalışmalara ışık tutacaktır."
*   **Neden Kötü?** Hangi model? Hangi veri seti? Ne kadar artış sağlandı? Hangi problem çözüldü? Tamamen havada kalan, hiçbir bilimsel değer taşımayan bir metindir.

###  İyi Özet Örneği (Net, Sayısal ve Yapılandırılmış)
> "Derin öğrenme modelleri tıbbi görüntü analizinde yaygın olarak kullanılsa da, sınırlı veri setlerinde eğitildiklerinde aşırı uyum (overfitting) göstermektedirler. Bu çalışmada, veri kıtlığı problemini çözmek amacıyla kontrastif öğrenme tabanlı yeni bir veri artırma yöntemi önerilmiştir. Önerilen yöntem, 500 göğüs röntgeni görüntüsünden oluşan kısıtlı bir veri setinde test edilmiştir. Deney sonuçları, yöntemimizin temel ResNet-50 modelinin F1-skorunu $\%76.5$'ten $\%87.1$'e çıkardığını ve eğitim süresini $\%15$ kısalttığını göstermiştir. Bu çalışma, kısıtlı klinik veri kümeleriyle çalışan derin öğrenme modellerinin güvenilirliğini artırmaktadır."

---

## 💡 Özet Yazımında Dikkat Edilmesi Gereken İpuçları

*   **Geçmiş Zaman Kullanımı:** Yapılan deneyleri anlatırken geçmiş zaman (past tense / edilgen çatı - *önerilmiştir, test edilmiştir*) kullanın.
*   **Kısaltmalardan Kaçının:** Çok yaygın olanlar (örneğin DNA, CPU) hariç, özet içinde ilk kez geçen kısaltmaları açık haliyle yazın.
*   **Atıf Yapmayın:** Özet içinde literatürdeki diğer çalışmalara (Örn: Ahmet ve ark., 2021) atıfta bulunulmaz. Özet bağımsız bir doküman gibi okunmalıdır.
*   **Gereksiz Ayrıntıları Silin:** Donanım modelleri, kod kütüphanesi sürümleri veya ara işlem adımları özette yer almaz; bunlar metodoloji (methodology) kısmına aittir.

## 📋 Araştırmacılar İçin Kontrol Listesi

- [ ] Özetiniz 5-Cümle kuralındaki adımları (Bağlam, Problem, Metot, Bulgular, Katkı) sırasıyla içeriyor mu?
- [ ] Özetin içinde en az bir veya iki tane somut, sayısal bulgu (örn: başarım oranları, yüzdesel kazançlar) yer alıyor mu?
- [ ] Kelime sayısı hedef derginin/konferansın sınırları içinde mi? (Genellikle < 250 kelime)
- [ ] Özetin içinde hiçbir literatür atfı (citation) veya metodolojik detay boğulması bulunmadığından emin misiniz?
