## Simülatör Üzerinden Sentetik Veri Eldesi

Simülatörden edinilen direkt konum bilgisi (x,y,z) üzerine sensör okumalarını temsilen eklenen gürültü modeli aşağıda tariflenmiştir.

Her bir nokta için, her eksene ayrı ayrı Gaussian bir gürültü eklenmiştir. Gürültünün oluşturulması sırasında [numpy.random.normal](https://numpy.org/doc/stable/reference/random/generated/numpy.random.normal.html?) fonksiyonundan yararlanılmıştır. 
Isotropic olarak ele almak yerine, bir eksende beklenen olası sapmaları o eksen özelinde ele almak için bu tarz bir modellemeye gidilmiştir. Sensörlerin eksenlere göre sapma payları belirlendiği takdirde, bu yaklaşım sensör çıktılarına yakın sonuçlar üretebilecektir.
Ayrıca, sensör okumalarında meydana gelen ilk defa karşılaşılan bir cisim veya uzaklaşan bir cisim için, modele oturma süreci e^(-x)'le orantılı üstel azalan bir fonksiyonla temsil edilmiştir. 

Aşağıda simülasyon ortamından edinilmiş Ground Truth noktaları (kartezyen koordinat sistemi) görülmektedir (Zamansallık sağ üstten sol alta doğru ilerlemektedir).


<img src="https://github.com/etarakci-hvl/severalStuff/blob/master/sentetik1.png" width="600">

Burada ise gürültü eklenmiş çıktıyı görmekteyiz.


<img src="https://github.com/etarakci-hvl/severalStuff/blob/master/sentetik2.png" width="600">

Son olarak, gürültünün dağılımını anlamak adına her bir Ground Truth noktası için üretilen 100 adet gürültülü noktanın oluşturduğu çıktıyı görmekteyiz.


<img src="https://github.com/etarakci-hvl/severalStuff/blob/master/sentetik3.png" width="600">


Araç üzerindeki tüm sensör modelleri (şu an için RADAR, LIDAR ve Kamera) çalıştırılarak ürettikleri veriler cache mekanizması ile saklanacaktır. Sentetik veri oluşturmak için geliştirilen DataGenerator modülü araç çevresinde, tespit mesafesinde olan tüm objeleri görecek haldedir. Bu modül araç üzerine bulunan tüm sensörlerin cache'lenen sonuçlarını da alacak ve her sensör için bir filtreleme yaparak (ilgili obje ilgili sensörde tespit edilip edilmediğine dair) bir liste elde edecektir. Bu filtreleme ile oluşturulan listeleme yöntemi her sensör için ayrı ayrı yapılacak ve bu listelerin birleşimi de bize en az bir sensör tarafından yakalanabilen objelerin listesini dönecektir. Objelere dair ihtiyacımız olan çıktı csv/json formatında tutulacaktır. Alınan noktalara tariflenen gürültü modeli eklenecektir. 

Araç çevresinde olup, verilen an için 3 sensörden (RADAR, LIDAR ve Kamera), en az bir tanesi tarafından tespit edilen tüm objeler sentetik veri setinde yer alacaktır.

Sentetik veri alma sıklığına gelirsek, bu aşamada verileri alabildiğimiz sıklıkta alıp elde edilen çıktıları sublayer'lara bölerek ihtiyaç olan sıklık elde edilecektir.
