# Derin Öğrenme Bootcamp - Intel Image Classification
Ad Soyad : Rabia Okutan 

## Projenin Amacı

Intel Image Classification veri seti, temel bir CNN modeli kullanılarak sınıflandırma yapmak amacıyla eğitilmiştir.

## Veri Seti Hakkında Bilgi

Bu projede, Intel Image Classification veri seti kullanılmıştır. Veri seti; farklı coğrafi bölgeleri temsil eden 6 sınıftan oluşan görüntüleri içermektedir:

* Mountain (Dağ)
* Street (Sokak)
* Building (Bina)
* Forest (Orman)
* Glacier (Buzul)
* Sea (Deniz) 

Veriler toplamda 3 klasörde yer almaktadır.Bu klasörlerden seg_pred klasörü projede kullanılmasına gerek görülmemiştir. 

Klasörlerden:seg_train ve seg_test klasörleri, sınıf isimlerine göre alt klasörlere ayrılmıştır.

* seg_train klasöründeki veriler, modelin eğitilmesi amacıyla kullanılmıştır.
  Bu klasördeki veriler:
  %80 oranında eğitim seti (11.230 örnek)
  %20 oranında doğrulama seti (2.804 örnek) olarak ayrılmıştır.

* seg_test klasöründeki 3.000 görsel ise, modelin test verisiyle değerlendirilmesinde kullanılmıştır.

## Kullanılan Yöntemler:

### Veri Hazırlama Yöntemleri
* Veri Ön İşleme:Görselleri aynı boyuta getirme (ör. 150×150, 224×224).
* Normalizasyon (piksel değerlerini 0–1 veya -1–1 aralığına ölçeklemek).
* Veri Artırma (Data Augmentation): ImageDataGenerator fonksiyonunu kullanarak Veri Artırma işlemi yapılmıştır. Böylece eğitim verileri çeşitlendirilmiştir.Bu durum , modelin daha farklı örneklerle , kapsamlı verilerle eğitilmesini sağlamıştır. Bu yöntem, modelin ezberlemesini(overfittingi) engellemek,genelleme yeteneğini arttırmak için kullanılmıştır.
 
### Modelin Eğitilmesinde Kullanılan Yöntemler

Temel Katmanlar ile CNN yapısı kurulmuştur.
 
* Convolutional Layer:Evrişim katmanıdır. Görsellerden özellik çıkarır.Hiperparemetre optimizasyonu bölümünde bu katmandan 1 tane daha eklenmiş ve modelin başarısı artmıştır.

* Pooling Layer (MaxPooling, AveragePooling): Özellik haritalarını küçültür ve gereksiz bilgiyi atar.

* Dropout: Overfitting’i önlemek için nöronları rastgele kapatmıştır.

* Dense Layer(Fully Connected):Görselden çıkarılan özellikleri alıp sınıf tahmini yapan son karar katmanıdır.

* Aktivasyon Fonksiyonları
ReLU → ara katmanlarda
Softmax → sınıflandırma katmanında
kullanılmıştır.

* Optimizasyon Algoritmalarından : Adam yöntemi .

## Proje Süreci Adımları 

### 1. Kütüphanelerin Yüklenmesi ve Veri Setinin Tanımlanması
Gerekli Python kütüphaneleri (NumPy, Matplotlib, TensorFlow, Keras vb.) yüklendi. Intel Image Classification veri seti tanımlanmış ve klasör yapısı incelenmiştir.

### 2. Veri Ön İşleme
Görseller normalize edildi (rescale=1./255)

Eğitim ve doğrulama verileri %80–%20 oranında ayrılmıştır. (validation_split=0.2)

Görseller 150x150 boyutuna yeniden boyutlandırılmıştır.

### 3. Veri Artırma
ImageDataGenerator ile eğitim verisi çeşitlendirilmiştir.

Amaç: modelin ezberlemesini önlemek ve genelleme yeteneğini artırmak

### 4. Modelin Eğitilmesi

CNN mimarisi kurulmuştur: 3 evrişim bloğu + Dense katman

Dropout ile regularization sağlanmıştır.

Model Adam optimizer ile 0.001 öğrenme oranı ile  10 epoch boyunca eğitilmiştir.

Eğitim ve doğrulama doğruluk/kayıp grafikleri oluşturulmuştur.

### 5. Modelin Test Verisi ile Değerlendirilmesi

seg_test klasöründeki 3.000 görsel ile modelin başarımı test edilmiştir.

Tahmin sonuçları incelendi, sınıflandırma doğruluğu analiz edilmiştir.

### 6.  Hiperparametre Optimizasyonu

Öğrenme oranı arttırılmıştır. (learning_rate=0.005)
Katman sayısı arttırılmıştır.(Eklenen katmana , dropout=0.5 de eklenmiştir) 

### 7. Performans Karşılaştırması
İlk model ile optimize edilmiş modelin doğruluk skorları grafikle karşılaştırılmıştır.

## Sonuç 

Model oluşturulurken :
* Veri artırma ile aşırı öğrenme riski azaltılmak istenmiştir.
  
* Model eğitiminde batch size 32 olarak seçilmiştir; bu, her iterasyonda 32 örnek üzerinden ağırlıkların güncellenmesini sağlayarak eğitim süresini kısa tutarken modelin genelleme performansını korumasına yardımcı olmuştur.

* Confusion Matrix ile modelin hangi sınıfları tespit etmede fazlaca hata yaptığı belirtilmiştir.
  
*İlk eğitilen modelde test doğruluğu %72.9 iken hiperparemetre optimizasyonu kapsamında:
  Öğrenme oranı 0.001’den 0.0005’e düşürülmüş
  Katman sayısı 3’ten 4’e çıkarılmıştır Bu iyileştirmeler doğruluk skorunu artırmıştır.

Proje sonucunda , CNN mimarisi ile eğitilmiş, sınıf tahmininde doğruluğu %82 olan bir model elde edilmiştir. 

## Gelecek Çalışmalar 

Modelin doğruluk oranı arttırabilir.Farklı hiperparemetreler ile model test edilebilir. Gelecek çalışmalarda modelde optimizasyon yapılması beklenmektedir.

Ayrıca modele bazı sınıfların (glacier,building) tespitinde doğruluğun oldukça düşük olduğu görülmüştür.Özellikle bu sınıflar için model nasıl daha iyi eğitilir diye çalışmalar yapılabilir.

## Kaggle Linki

(Kod çıktıları dosyayı kaydederken silindi . Kod çıktıları dosyalar kısmındaki  github nootbookunda bulunmaktadır.)
https://www.kaggle.com/code/rabiiaokutan/derin-renme-bootcamp-intel-image-classification







