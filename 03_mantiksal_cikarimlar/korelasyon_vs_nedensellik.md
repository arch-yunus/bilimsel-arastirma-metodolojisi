# 🔗 Korelasyon vs. Nedensellik (Correlation vs. Causality)

## 📌 Giriş ve Tanım
Bilimsel araştırmalarda en sık yapılan mantıksal hatalardan biri **Korelasyon (İlişki)** ile **Nedensellik (Sebep-Sonuç İlişkisi)** kavramlarının karıştırılmasıdır. 

> *“Cum hoc ergo propter hoc” (Bununla birlikte, dolayısıyla bundan dolayı)*

İki değişkenin değerlerinin eşzamanlı olarak artması veya azalması (korelasyon), bu değişkenlerden birinin diğerinin ortaya çıkmasına sebep olduğunu (nedensellik) **kanıtlamaz**. İstatistiksel eşzamanlılık, bir mekanizma açıklaması sunamaz.

---

## 🔬 Kavramsal Ayrımlar

### 1. Korelasyon (İlişki)
İki veya daha fazla değişken arasındaki doğrusal ilişkinin yönünü ve gücünü gösteren istatistiksel bir ölçüdür (Örn: Pearson Korelasyon Katsayısı, $r$). Korelasyon sadece **gözlemseldir** ve yönü belirsizdir. Eğer $A$ ve $B$ korelasyonel ise:
*   $A$, $B$'ye neden oluyor olabilir.
*   $B$, $A$'ya neden oluyor olabilir.
*   Hiçbir ilişki olmayıp, tamamen şans eseri olabilir.
*   Üçüncü bir gizli değişken ($C$), her ikisine de neden oluyor olabilir.

### 2. Nedensellik (Causality)
Bir değişkenin (Neden / Bağımsız Değişken), başka bir değişkenin değerini (Sonuç / Bağımlı Değişken) doğrudan etkilediği ve değiştirdiği durumdur. Nedensellik ilişkisinde zaman sırası (neden, sonuçtan önce gelmelidir) ve bir etki mekanizması (fiziksel, biyolojik veya mantıksal yolak) bulunmalıdır.

---

## ⚠️ Karıştırıcı Değişkenler (Confounding Variables) ve Sahte Korelasyonlar

### Karıştırıcı Değişken (Confounder) Nedir?
Hem bağımsız değişkeni hem de bağımlı değişkeni etkileyen, dolayısıyla aralarında aslında olmayan sahte bir sebep-sonuç ilişkisi varmış gibi görünmesine neden olan üçüncü değişkenlerdir.

```text
       [ C: Sıcak Hava ] (Karıştırıcı)
          /         \
         v           v
  [ A: Dondurma ]  [ B: Boğulma Vakaları ]
```

*   **Klasik Örnek:** Dondurma satışları ($A$) ile boğulma vakaları ($B$) arasında güçlü bir pozitif korelasyon vardır. Dondurma yiyenlerin boğulduğunu iddia etmek nedensellik hatasıdır. Aradaki karıştırıcı değişken **Sıcak Hava ($C$)**’dır. Sıcak hava hem dondurma tüketimini hem de denize giren insan sayısını artırır.

### Bilgisayar Bilimlerinden Bir Örnek
Bir yazılım ekibi, yeni geliştirdikleri veri tabanı indeksleme algoritmasını ($A$) devreye aldıktan sonra veri tabanı sorgu yanıt sürelerinin ($B$) yarı yarıya düştüğünü gözlemliyor. Ekip, algoritmanın bu düşüşe neden olduğunu iddia ediyor. 
Ancak, algoritmanın devreye alındığı saatlerde sistem üzerindeki kullanıcı trafiği ($C$) de düşmüştür. Burada trafik ($C$) kontrol edilmeden, algoritmanın ($A$) sorgu sürelerini ($B$) kısalttığı iddia edilemez.

---

## 🛠️ Nedensellik Nasıl İspatlanır?

İstatistikte korelasyondan nedenselliğe geçebilmek için kullanılan temel yöntemler şunlardır:

### 1. Rastgele Kontrollü Deneyler (RCT) & A/B Testleri
Nedenselliği kanıtlamanın altın standardıdır. Denekler/sistemler tamamen rastgele iki gruba ayrılır:
*   **Kontrol Grubu:** Mevcut sistemle devam eder (müdahale yapılmaz).
*   **Deney Grubu:** Test edilmek istenen yeni özelliğe maruz bırakılır.
*   Rastgeleleştirme (Randomization), tüm olası karıştırıcı değişkenlerin ($C$) her iki gruba da eşit dağılmasını sağlar, böylece gruplar arasındaki tek fark yapılan müdahale olur.

### 2. Nedensel Çıkarım (Causal Inference) Yöntemleri
Doğal veya gözlemsel verilerde rastgeleleştirme yapmak mümkün değilse (örneğin sigara içmenin insan üzerindeki etkisini test etmek için insanlara zorla sigara içirilemeyeceği için), şu istatistiksel teknikler kullanılır:
*   **Propensity Score Matching (Eğilim Skoru Eşleştirme):** Benzer özelliklere (yaş, cinsiyet, kronik hastalıklar) sahip kişileri eşleştirerek gruplar oluşturma.
*   **Instrumental Variables (Araç Değişkenler):** Karıştırıcı değişkenlerle ilişkisi olmayan ancak bağımsız değişkeni etkileyen harici bir değişken bulma.
*   **Difference-in-Differences (Farkların Farkı):** Müdahale öncesi ve sonrası gelişim trendlerini kontrol grubuyla kıyaslama.

## 📋 Araştırmacılar İçin Kontrol Listesi

- [ ] İki değişken arasında bulduğunuz ilişkiyi raporlarken "sebep olur", "yol açar", "tetikler" gibi nedensel kelimeler yerine "ilişkilidir", "korelasyon gösterir" gibi ifadeler mi kullandınız?
- [ ] Sisteminizdeki performans artışını açıklarken, ortamdaki diğer tüm dış etkenleri (örn. arka plan trafiği, CPU sıcaklığı, işletim sistemi yükü) sabitlediğinizi veya kontrol ettiğinizi kanıtladınız mı?
- [ ] Bulgularınızda A/B testi veya kontrollü bir deney tasarımı kullandınız mı? Eğer kullanamadıysanız, olası karıştırıcı değişkenleri (confounders) tartışma bölümünde belirttiniz mi?
- [ ] Bulduğunuz korelasyonun şans eseri (sahte korelasyon) olmadığını göstermek için yeterli örneklem büyüklüğüne ve istatistiksel anlamlılığa ulaştınız mı?
