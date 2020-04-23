## Simülatör Üzerinden Sentetik Veri Eldesi

Simülatörden edinilen direkt konum bilgisi (x,y,z) üzerine sensör okumalarını temsilen eklenen gürültü modeli aşağıda tariflenmiştir.
Her bir nokta için, her eksene ayrı ayrı Gaussian bir gürültü eklenmiştir.
Isotropic olarak ele almak yerine, eksende beklenen olası sapmaları o eksen özelinde ele almak için bu tarz bir modellemeye gidilmiştir. Sensörlerin eksenlere göre sapma payları belirlendiği takdirde, bu yaklaşım sensör çıktılarına yakın sonuçlar üretebilecektir.
Ayrıca, sensör okumalarında meydana gelen ilk defa karşılaşılan bir cisim veya uzaklaşan bir cisim için, modele oturma süreci e^(-x)'le orantılı üstel azalan bir fonksiyonla temsil edilmiştir.

Aşağıda simülasyon ortamından edinilmiş Ground Truth görülmektedir (Zamansallık sağ üstten sol alta doğru ilerlemektedir).
<img src="https://github.com/etarakci-hvl/severalStuff/blob/master/sentetik1.png" width="600">
Burada ise gürültü eklenmiş çıktıyı görmekteyiz.
<img src="https://github.com/etarakci-hvl/severalStuff/blob/master/sentetik2.png" width="600">
Son olarak, gürültünün dağılımını anlamak adına her bir Ground Truth noktası için üretilen 100 adet gürültülü noktanın oluşturduğu çıktıyı görmekteyiz.
<img src="https://github.com/etarakci-hvl/severalStuff/blob/master/sentetik3.png" width="600">








Çıktılar:
Min aşağıdaki konuları açıklayan doküman:
1. Filtrenin zamana bağlı olarak oturması nasıl simüle edilecek (cold-start)
2. Kullanılan Gauss modeli
3. Filtre çıktıları nasıl simüle edilecek (Gauss modeli)
4. Sensör ve algoritma modelleri nasıl simüle edilecek (Kamera, RADAR, LIDAR)
5. Hangi objeler sentetik dünya modeli çıktısı içinde yer alacak? 
6. Sentetik veri data alma sıklığı nasıl ele alınacak? (simulation time, wall clock)
7. Sampling toolu hangi durumda kullanılacak?
