# 🏠 Konut Fiyat Tahmini — Doğrusal Regresyon

Bu projede, konut özelliklerinden yararlanarak **konut fiyatlarını tahmin etmek** amacıyla doğrusal regresyon yöntemleri uygulanmıştır.

Çalışmada veri keşfi ve temizliğinden başlayarak basit ve çoklu doğrusal regresyon modelleri kurulmuş; modeller **R², RMSE, MAE, VIF, çapraz doğrulama ve artık analizi** kullanılarak değerlendirilmiştir.

Ayrıca hedef değişkendeki sağa çarpıklığın etkisini incelemek amacıyla **logaritmik dönüşüm** uygulanmıştır.

---

## 📊 Veri Seti

Çalışmada `Housing.csv` veri seti kullanılmıştır.

* **Gözlem sayısı:** 545 konut
* **Başlangıç değişken sayısı:** 13
* **Hedef değişken:** `price`
* **Eksik değer:** Yok
* **Tekrarlanan kayıt:** Yok

### Değişkenler

| Değişken           | Açıklama                             |
| ------------------ | ------------------------------------ |
| `price`            | Konut fiyatı — hedef değişken        |
| `area`             | Konut/arsa alanı (ft²)               |
| `bedrooms`         | Yatak odası sayısı                   |
| `bathrooms`        | Banyo sayısı                         |
| `stories`          | Kat sayısı                           |
| `mainroad`         | Ana yola cepheli olma durumu         |
| `guestroom`        | Misafir odası                        |
| `basement`         | Bodrum                               |
| `hotwaterheating`  | Sıcak su ısıtma sistemi              |
| `airconditioning`  | Klima                                |
| `parking`          | Otopark kapasitesi                   |
| `prefarea`         | Tercih edilen bölgede bulunma durumu |
| `furnishingstatus` | Eşya durumu                          |

---

## 🛠️ Kullanılan Teknolojiler

* Python
* Pandas
* NumPy
* Matplotlib
* Seaborn
* Scikit-learn
* SciPy

---

## 🔍 Analiz Süreci

Proje aşağıdaki adımlardan oluşmaktadır:

1. Veri setinin yüklenmesi ve incelenmesi
2. Eksik ve tekrarlanan verilerin kontrolü
3. Kategorik değişkenlerin kodlanması
4. Keşifçi Veri Analizi (EDA)
5. Korelasyon analizi
6. Basit doğrusal regresyon
7. Çoklu doğrusal regresyon
8. VIF ile çoklu bağlantı kontrolü
9. 5-katlı çapraz doğrulama
10. Artık (residual) analizi
11. Logaritmik hedef dönüşümü
12. Modellerin karşılaştırılması

---

## 📈 Keşifçi Veri Analizi

`price` ile en yüksek pozitif korelasyona sahip değişkenlerden bazıları:

| Değişken          | Korelasyon |
| ----------------- | ---------: |
| `area`            |      0.536 |
| `bathrooms`       |      0.518 |
| `airconditioning` |      0.453 |
| `stories`         |      0.421 |
| `parking`         |      0.384 |

Konut fiyatı dağılımında **pozitif çarpıklık** tespit edilmiştir.

**Price skewness:** `1.209`

Bu nedenle çalışmanın ilerleyen bölümünde hedef değişkene logaritmik dönüşüm uygulanmıştır.

---

## 1️⃣ Basit Doğrusal Regresyon

İlk modelde yalnızca `area` değişkeni kullanılarak konut fiyatı tahmin edilmiştir.

Model denklemi:

```text
price = 2,512,254.26 + 425.73 × area
```

Buna göre diğer faktörler dikkate alınmadığında, alanın 1 ft² artması modelde yaklaşık **426 birimlik fiyat artışı** ile ilişkilidir.

### Test Sonuçları

| Metrik |     Sonuç |
| ------ | --------: |
| R²     |     0.273 |
| RMSE   | 1,917,104 |
| MAE    | 1,474,748 |

Alan tek başına konut fiyatındaki değişkenliğin yaklaşık %27'sini açıklayabilmektedir.

---

## 2️⃣ Çoklu Doğrusal Regresyon

İkinci modelde konutun mevcut özellikleri birlikte kullanılmıştır.

### Test Sonuçları

| Metrik |         Sonuç |
| ------ | ------------: |
| R²     |     **0.653** |
| RMSE   | **1,324,507** |
| MAE    |   **970,043** |

Basit modelden çoklu modele geçildiğinde test R² değeri:

```text
0.273 → 0.653
```

olarak yükselmiştir.

Bu sonuç, konut fiyatının yalnızca alanla değil; banyo sayısı, klima, kat sayısı, otopark ve diğer özelliklerle birlikte değerlendirilmesinin tahmin performansını önemli ölçüde artırdığını göstermektedir.

---

## 🔄 Cross-Validation

Modelin tek bir train/test ayrımına bağımlı olup olmadığını değerlendirmek için **5-katlı çapraz doğrulama** uygulanmıştır.

| Model           | Ortalama CV R² |
| --------------- | -------------: |
| Basit Regresyon |          0.226 |
| Çoklu Regresyon |      **0.632** |

Çoklu regresyon modelinin farklı veri bölmelerinde de benzer performans göstermesi modelin genellenebilirliği açısından olumlu bir sonuçtur.

---

## 🔗 VIF Analizi

Bağımsız değişkenler arasındaki çoklu bağlantıyı (multicollinearity) kontrol etmek amacıyla **Variance Inflation Factor (VIF)** analizi uygulanmıştır.

En yüksek VIF değeri:

```text
1.67
```

olarak bulunmuştur.

Bu nedenle modelde ciddi bir **multicollinearity problemi gözlenmemiştir.**

---

## 📊 Özelliklerin Etkisi

Farklı ölçeklerdeki değişkenlerin ham katsayılarını doğrudan karşılaştırmak yanıltıcı olabileceğinden özellikler standartlaştırılarak katsayılar ayrıca incelenmiştir.

Analizde özellikle:

* `area`
* `bathrooms`
* `airconditioning`
* `stories`

değişkenlerinin fiyat tahmininde güçlü katkılar sağladığı görülmüştür.

`bedrooms` değişkeninin fiyatla ham korelasyonu pozitif olmasına rağmen, diğer değişkenler kontrol edildiğinde bağımsız katkısının daha sınırlı olduğu görülmektedir.

Bu durum, **korelasyonun tek başına özellik önemi olarak yorumlanmaması gerektiğini** göstermektedir.

---

## 📉 Artık Analizi

Çoklu regresyon modelinin tahmin hataları ayrıca incelenmiştir.

Artık grafiklerinde fiyat seviyesi arttıkça hata yayılımının değiştiği gözlemlendiğinden hedef değişkene logaritmik dönüşüm uygulanarak alternatif bir model kurulmuştur.

---

## 🔬 Logaritmik Dönüşüm

Hedef değişken:

```python
np.log(price)
```

şeklinde dönüştürülerek model yeniden eğitilmiştir.

Çarpıklık:

```text
price      : 1.209
log(price) : 0.140
```

seviyesine düşmüştür.

### Log Model Sonuçları

| Model               |        R² |          RMSE |         MAE |
| ------------------- | --------: | ------------: | ----------: |
| Çoklu Regresyon     |     0.653 |     1,324,507 |     970,043 |
| Log Çoklu Regresyon | **0.658** | **1,314,648** | **960,123** |

Log dönüşümü R² değerinde sınırlı bir artış sağlasa da artıkların yayılımını daha homojen hale getirmiştir.

---

## 🏆 Sonuç

Çalışmada üç farklı yaklaşım karşılaştırılmıştır:

| Model                       |   Test R² |
| --------------------------- | --------: |
| Basit Doğrusal Regresyon    |     0.273 |
| Çoklu Doğrusal Regresyon    |     0.653 |
| Log-Hedefli Çoklu Regresyon | **0.658** |

Sonuçlara göre konut fiyatlarını tahmin etmek için yalnızca alan değişkenini kullanmak yeterli değildir.

Birden fazla konut özelliğinin birlikte kullanıldığı model tahmin performansını önemli ölçüde artırmıştır. Logaritmik hedef dönüşümü ise performansta küçük bir iyileşme sağlamasının yanında model varsayımlarının daha iyi karşılanmasına katkıda bulunmuştur.

Modelin açıklayamadığı fiyat farklılıklarının bir kısmının veri setinde bulunmayan **konum, bina yaşı ve kat düzeyi gibi değişkenlerden** kaynaklanabileceği düşünülmektedir.

---

## 📁 Proje Yapısı

```text
konut-fiyat-regresyon/
│
├── dogrusal_regresyon_konut.ipynb
├── Housing.csv
└── README.md
```

---

## ▶️ Çalıştırma

Projeyi klonladıktan sonra gerekli Python kütüphanelerini yükleyin:

```bash
pip install pandas numpy matplotlib seaborn scikit-learn scipy
```

Ardından:

```bash
jupyter notebook
```

komutuyla notebook'u çalıştırabilirsiniz.

---

## 🎯 Projenin Amacı

Bu çalışma, doğrusal regresyonun yalnızca model kurma aşamasını değil; **veri hazırlama, keşifçi veri analizi, model değerlendirme, multicollinearity kontrolü, cross-validation, residual analizi ve hedef dönüşümü** gibi temel makine öğrenmesi adımlarını uygulamalı olarak göstermek amacıyla hazırlanmıştır.
