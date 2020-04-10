## RRT, RRG ve Fast RRT* Uygulamaları

Bu çalışmada Rapidly-Exploring Random Tree(RRT), Rapidly-Exploring Random Graph(RRG) ve Rapidly-Exploring Random Tree*(RRT*) algoritmalarına dair uygulamalar yer almaktadır. Algoritmayı gerçekleyen yazarın bahsettiği üzere bir takım tasarım kararlarından ötürü RRT* algoritması standart uygulamaya nazaran ~10 kat hızlı çalışmaktadır. Programın çalışma prensibinde rastgele yerleştirilen engellere, başlangıç ve bitiş noktalarına vurgu yapılmıştır.

Kodun alındığı repository: [Fast RRT*](https://github.com/dixantmittal/fast-rrt-star)

Yararlanılan Makale: [Incremental Sampling-based Algorithms
for Optimal Motion Planning](http://roboticsproceedings.org/rss06/p34.pdf)

**RRT Algoritmasi:**  Kademeli artan örnekleme tabanlı algoritmalar tekil-sorgu için geliştirilmişlerdir. Gerçek zamanlı uygulamalarda en etkin olan algoritmalardan biri de RRT algoritmasıdır. Olasılıksal olarak *complete* oldukları gösterilen bu algoritmanın bir diğer özelliği, hata olasılığının *exponential decay*'e sahip olmasıdır. 

**RRG Algoritmasi:** 
Kademeli artan örnekleme tabanlı algoritmalara bir diğer örnek ise RRG algoritmasıdır. Linear Temporal Logic kullanımı ve deterministik µ-calculus ile verilen spesifikasyonları yönetir. Kademeli olarak artacak şekilde üretilen rotalar çizge veri yapısı içerisinde tutulur. RRG tarafından dönen en iyi rotanın maliyeti neredeyse her zaman optimuma yakınsar.

**RRT\* Algoritmasi:** 
RRT* algorıtmasını, RRG algoritmasının ağaç tabanlı versiyonu olarak düşünmek mümkündür. RRG'nin *asymptotic optimality* özelliğini, ağaç veri yapısını kullanarak gerçekleyen bu metodoloji, hali hazırda ağaçta bulunan düğümlere uzanan en düşük maliyetli güzergahları keşfeder. Bunu yaparken kendini yeniden yapılandırır, ***rewire*** eder.

#### Uygulamaya Dair Bazi Notlar

Uzay temsili, orjin ve boyut olarak tutulmaktadır((15, 20), (3,3)). Algoritmlaarın çalışması için, çevrede yer alan engellere dair kullanılan bu temsile ek olarak; başlangıç durumu, hedef bölge ve engeller haritası (tüm objelere dair Uzay Temsili) gerekmektedir. Aşağıda ilgili parametrelerin açıklamaları görülebilir.

##### state_space
Durum uzayı, dış dünyanın temsili olarak düşünülmelidr. Yukarıda bahsedilen formda algoritmaya sağlanır.
##### starting_state
Başlangıç durumu, rotası hesaplanacak ajanın başlangıç noktası olarak kabul edilebilir. Bu parametre ajanın orjini ve alanı olarak düşünülmelidir.
##### target_space
Hedef bölge, varılmak istenen bölge olarak tanımlanmıştır. Varılmak istenen bölgenin koordinatları ve alanı şeklinde düşünülmelidir.
##### obstacle_map
Engeller haritası, durum uzayında yer alan tüm engellerin bilgilerinin barındığı haritadir ve her bir engel için koordinat-alan ikilisi şeklinde tutulmaktadır.
##### n_samples
Iterasyon sayısı, algoritmanın üreteceği örnek noktalarıyla ilişkilidir. Çarpışan ve alanın dışında yer alan noktalar da iterasyonlar arasında yer alacağından kabul edilebilir örneklerin sayısı *default = 1000* iterasyon sayısından az olabilir.
##### granularity
Öge boyu veya taneciklilik olarak nitelendirilen bu kavram algoritmanın süreci ne kadar detaylı ele aldığı şeklinde düşünülebilir. Kademeli artan çarpışma denetlemesi kullanıldığından, taneciklilik kavramına ihtiyaç duyulmaktadır. Bu parametrenin detayı artırıldığında, programın yavaşlayacağı hatırda tutulmalıdır.
##### d_threshold
Bu parametre, eldeki düğümden yeni bir nokta örnekleneceğinde, örneklenecek noktanın ne kadar uzakta olacağına karar verir.

**RRT ve RRT\* algoritmalarının ikili kıyası (n_samples=5000) aşağıdadır:**

<img src="https://github.com/etarakci-hvl/severalStuff/blob/master/RRT.png" width="580">
<img src="https://github.com/etarakci-hvl/severalStuff/blob/master/RRTStar.png" width="600">


**RRT, RRG ve RRT\* algoritmalarının üçlü kıyası (n_samples=1000) aşağıdadır:**
<img src="https://github.com/etarakci-hvl/severalStuff/blob/master/RRT1.png" width="600">
<img src="https://github.com/etarakci-hvl/severalStuff/blob/master/RRG1.png" width="595">
<img src="https://github.com/etarakci-hvl/severalStuff/blob/master/RRTStar1.png" width="600">

Bu örnek sonuçlara ek olarak, diğer güzergah kıyaslamaları, maliyet ve hesaplama zamanı sonuçları *plots* ve *metrics* klasörleri altında bulunabilir.

#### Nasıl Çalıştırılır ? 

* Öncelikle, `conda env create --name FastRRT --file environment.yml` komutuyla gereken kütüphaneler yüklenmelidir ve `conda activate FastRRT` komutuyla çevre aktive edilmedilir.

* Çevre aktive edildikten sonra, terminalde klasörün bulunduğu konuma gidilir ve `python Main.py` komutu yürütülür. Çıktı olarak RRT, RRG ve RRT* algoritmalarını test etmek için rastgele oluşturulan engelli bir harita ve bu engelli haritaya çözüm sunan algoritmalara dair çıktılar barındıran 3 adet figür gelecektir.



