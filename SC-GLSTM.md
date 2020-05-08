### SC-GLSTM 
SC-GLSTM algoritmasına dair ([**Forecasting Trajectory and Behavior of Road-Agents Using Spectral Clustering in Graph-LSTMs**](https://obj.umiacs.umd.edu/gamma-umd-website-imgs/pdfs/autonomousdriving/spectralcows_full.pdf)) calışmalar bu repo'da toplanmıştır.

SC-GLSTM projesine **https://gamma.umd.edu/spectralcows** üzerinden de ulaşılabilir. 

Kodun alındığı repo: **https://github.com/rohanchandra30/Spectral-Trajectory-and-Behavior-Prediction**

State of the Art'a eriştikleri bir takım veri setleri ve challenge'lar:
- Trajectory Prediction on Apolloscape 
- Trajectory Prediction on Argoverse
- Trajectory Prediction on Lyft Level 5

Sonuçlar metre yerine normalized RMSE değerleri cinsinden verilmiştir.

<p align="center">
<img src="figures/predict.png" width="260">
<img src="figures/results.gif" width="266">
<img src="figures/behavior.gif" width="260">
</p>

## Nasıl Çalıştırılır? 
  
Python 3.7 versiyonu üzerinde geliştirilmiştir.
### Yükleme İşlemleri
1. Conda cevresi oluşturulur:
  `conda env create -f env.yml`
2. Çevre aktive edilir:
  `conda activate sc-glstm`
3. İlgili kaynaklar indirilir:
  `python setup.py`

### Kullanım

* Gerçeklenen metodun sunduğu bir veya iki stream üzerine kurulu modelleri calıştırmak için:
  1. `cd ours/`
  2. `python main.py`
  3. Bir stream'den iki stream'e geçmek için, `main.py` dosyasındaki `s1` değişkenini True/False olarak değiştirmek gerekir.
  4. Modeli değiştirmek için, `main.py`'da bulunan  `DATA` ve `SUFIX` değişkenleri güncellenir.
* EncDec metodu karşılaştırmasını çalıştırmak için:  
  1. `cd comparison_methods/EncDec/`
  2. `python main.py`
  3. Modeli değiştirmek için, `main.py`'da bulunan  `DATA` ve `SUFIX` değişkenleri güncellenir.
* GRIP metodu karşılaştırmasını çalıştırmak için: 
  1. `cd comparison_methods/GRIP/`
  2. `python main.py`
  3. Modeli değiştirmek için, `main.py`'da bulunan  `DATA` ve `SUFIX` değişkenleri güncellenir.
* TraPHic/SC-LSTM metodu karşılaştırmasını çalıştırmak için: 
  1. `cd comparison_methods/traphic_sconv/`
  2. `python main.py`
  3. Modeli ve metodları değiştirmek için, `main.py`'da bulunan `DATASET` ve `PREDALGO` değişkenleri güncellenir. 

### Karşılaştırma Maksadıyla Gerçeklenen Trajectory Prediction Metodlarının Listesi

* [**TraPHic: Trajectory Prediction in Dense and Heterogeneous Traffic Using Weighted Interactions**, CVPR'19](https://gamma.umd.edu/researchdirections/autonomousdriving/traphic/)
* [**Convolutional Social Pooling for Vehicle Trajectory Prediction**, CVPRW'18](https://arxiv.org/pdf/1805.06771.pdf)
* [**Social GAN: Socially Acceptable Trajectories with Generative Adversarial Networks**, CVPR'18](https://arxiv.org/pdf/1803.10892.pdf)
* [**GRIP: Graph-based Interaction-aware Trajectory Prediction**, ITSC'19](https://arxiv.org/pdf/1907.07792.pdf)
    * SC-GLSTM algorıtmasını gerçekleyen geliştiriciler, GRIP yaklaşımına dair kodun official implementation'i olmadığından replica'sını ürettiklerini ifade etmektedirler. Official Implementation aşağıdaki linkten sağlanacaktır: https://github.com/xincoder/GRIP

### Kullanılan Veri Setleri 
* [**Argoverse**](https://www.argoverse.org/data.html) (input length: 20 & output length: 30)
* [**Apolloscape**](http://apolloscape.auto/trajectory.html) (input length: 6 & output length: 10)
* [**Lyft Level 5**](https://level5.lyft.com/dataset/)  (input length: 20 & output length: 30)

### Veri Hazırlama Süreci
Raw data'dan veri elde etme sürecine dair adımlar burada özetlenmiştir.

#### Resmi Websitesi'nden İndirdikten Sonra Veri Setinin Formatlanması:
* `data_processing/format_apolloscape.py` kodu çalıştırılarak ApolloScape verisi istenen formata kavuşturulur. 
* `data_processing/format_lyft.py` kodu çalıştırılarak Lyft verisi istenen formata kavuşturulur.
* Run `data_processing/generate_data.py` kodu çalıştırılarak Argoverse Trajectory verisi istenen formata kavuşturulur.

#### Formatlanmış Verinin İlgili Veri Yapısı İçin Hazırlanması:
* `data_processing/data_stream.py` kodu kullanılarak stream1 and stream2 için input verisi oluşturulur. 
* `data_processing/behaviors.py` kodundaki `generate_adjacency()` fonksiyonu kullanılarak yakınlık matrisleri oluşturulur.
* Veriyi network'e aktarmadan evvel yapılması gereken bir işlem: `data_processing/behaviors.py` kodundaki `add_behaviors_stream2()`  `add_behaviors_stream2()` fonksiyonu kullanılarak davranış label'ları stream2 verisine eklenir. Ancak sonrasında network'e stream2 çıktısı sağlanmalıdır.

### Görselleştirme

* `data_processing/behaviors.py` kodundaki `plot_behaviors()` fonksiyonu kullanılarak agent'ların davranışları görselleştirilebilir.

## SC-GLSTM Mimarisi

<p align="center">
<img src="figures/network.png">
</p>
Network Mimarisi görselinde, i'inci road-agent icin Trajectory ve Behavior Prediction gösterilmedektedir (Traffic Graphs kısmındaki kırmızı daire). Input'ta, T saniyelik geçmişe dair uzaysal koordinatların yanı sıra traffic-graph'ların  ilk T traffic-graph'a tekabül eden eigenvector'leri de barınmaktadır (yeşil dikdörtgenler, her bir yeşil tonu eigenvector'un index'ini temsil etmektedir). Hakiki loss fonksiyonunu regularize etmek ve uzun vadeli tahmin kalitesini artırma maksadıyla yeni loss fonksiyonu üzerinde backpropagation işlemi gerçekleştirmek için, Stream 2 blogundan edinilen tahmin edilmiş eigenvector'lerin üzerinde Spectral clustering tekniği icra edilir.  

## Diğer Modellerle Kıyaslamalar
Trajectory Prediction metodlarına dair Average Displacement Error (ADE) ve Final Displacement Error (FDE) hata sonuçları raporlanmıştır. Düşük skorlar daha iyiyi niteler ve kalın yazılmış sonuçlar SOTA sonuçlardır. Makalenin yazarları, en düşük ADE/FDE sonuçlarına erişmenin yanı sıra, uzun vadeli tahmin sonuçlarındaki hata paylarını da 5 kat düşürdüklerini iddia etmektedirler. Asteriks (∗) sembolu tahmin süresince yakınsayamayan metodları vurgular. Bu kısımdaki sonuçlar metre cinsindendir.

![comparison of our methods with other methods](figures/compare.png)

## Değerlendirme Sonuçları

### Trajectory Prediction Sonucu
![Trajectory Prediction Result](figures/spectral_cluster_regularization.png)

Nitel Analiz: Tahmin edilen trajectory, Ground Truth trajectory (cyan noktalar ile yeşil rota çizgisi) ile kıyaslanmaktadır. Tahmin süresi 5 saniyedir. Figürde görülen her bir kırmızı bölge tahmin edilmiş Bivariate Gaussian Distribution'ı *N(µ∗, σ∗, ρ∗)* temsil eder. 
### Behavior Prediction Sonuçları

<p align="center">
  <img src="figures/behaviors.png">
</p>
Sırasıyla Lyft, Argoverse, and Apolloscape veri setlerinden seçilen birer videoda bulunan tüm road-agent'lar için 3 adet davranış sınıflandırması yapılmıştır: overspeeding (mavi), neutral (yesil), ve braking (kirmizi). y-ekseni θ' parametresini, x-ekseni road-agent'ları göstermektedir. 

### Veri Setlerine Göre RMSE Sonuçları 
Tahmin penceresi Lyft ve ApolloScape veri setleri için 5 saniye, Argoverse veri seti içinse 3 saniyedir. Sırasıyla 30, 10 ve 30 frame length'lere tekabül ederler.
#### Lyft 

<p align="center">
  <img src="figures/rmse_lyft.png" width="450">
</p>

#### Argoverse 

<p align="center">
  <img src="figures/rmse_argo.png" width="450">
</p>

#### ApolloScape 
<p align="center">
  <img src="figures/rmse_apol.png" width="450">
</p>

