## RRT, RRG ve Fast RRT* Uygulamaları

Bu çalışmada Rapidly-Exploring Random Tree(RRT), Rapidly-Exploring Random Graph(RRG) ve Rapidly-Exploring Random Tree*(RRT*) algoritmalarına dair uygulamalar yer almaktadır. Algoritmayı gerçekleyen yazarın bahsettiği üzere bir takım tasarım kararlarından ötürü RRT* algoritması standart uygulamaya nazaran ~10 kat hızlı çalışmaktadır. Programın çalışma prensibinde rastgele yerleştirilen engellere, başlangıç ve bitiş noktalarına vurgu yapılmıştır.

Kodun alındığı repository: [Fast RRT*](https://github.com/dixantmittal/fast-rrt-star)

Yararlanılan Makale: [Incremental Sampling-based Algorithms
for Optimal Motion Planning](http://roboticsproceedings.org/rss06/p34.pdf)

**RRT Algoritması:**  Kademeli artan örnekleme tabanlı algoritmalar tekil-sorgu için geliştirilmişlerdir. Gerçek zamanlı uygulamalarda en etkin olan algoritmalardan biri de RRT algoritmasıdır. Olasılıksal olarak *complete* oldukları gösterilen bu algoritmanın bir diğer özelliği, hata olasılığının *exponential decay*'e sahip olmasıdır. 

**RRG Algoritması:** 
Kademeli artan örnekleme tabanlı algoritmalara bir diğer örnek ise RRG algoritmasıdır. Linear Temporal Logic kullanımı ve deterministik µ-calculus ile verilen spesifikasyonları yönetir. Kademeli olarak artacak şekilde üretilen rotalar çizge veri yapısı içerisinde tutulur. RRG tarafından dönen en iyi rotanın maliyeti neredeyse her zaman optimuma yakınsar.

**RRT\* Algoritması:** 
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

**ET&ED**





# RRT Algoritması #
RRT algoritmasında başlangıç ve hedef nokta arasında rastgele noktalar oluşturulur ve mevcut en yakın düğüme bağlanır. Rastgele noktalar belirleme yöntemi bir tasarım kararıdır.
RRT, hem grafik oluşturan hem de yol planlayan bir algoritmadır. Fakat planlanan yol her zaman optimal olmayacaktır.
RRT* ise RRT algoritmasının optimize edilmiş halidir, mesafeye ve diğer metriklere göre en kısa yolu daha kısa sürede bulur.
Her iki algoritma da real continous dimensional space platformlarında kullanılabilir.
Her vertex(tepe noktası) oluşturulduğunda, vertex'in bir engelin(obstacle) dışında kalıp kalmadığına dair bir kontrol yapılır.
Bir engel ile karşılaşıldığında, vertex en yakın komşusuna bağlanır ve engelden uzaklaşılır.
Hedef noktasına veya bir sınıra ulaşıldığında algoritma sona erer.
## RRT Pseudo Code 
Qgoal         // ulaşılması beklenen hedef noktası
Counter = 0   // iterasyonları takip eden sayaç
lim = n       // algoritmanın çalıştırması gereken iterasyon sayısı
G(V,E)        // boş olarak tanımlanan, kenar(edge) ve köşe(vertice) parametrelerini içeren grafik
while counter < lim:               // sayaç, iterasyon sayısından küçük ise : > 
   Xnew = RandomPosition()         // rastgele noktalar oluşturulur
   if IsInObstacle(Xnew) == True:  // nokta, bir engel üzerindeyse 
    continue
   Xnearest = Nearest(G(V,E),Xnew) // en yakın vertexi bulur
   Link = Chain(Xnew,Xnearest)     // nokta ile en yakın nokta bağlanır 
   G.append(Link) 
   if Xnew in Qgoal: 
    Return G
Return G 
Ek olarak, rastgele üretilen vertex leri bağlama yöntemleri de özelleştirilebilir. Örneğin bir yöntem, yeni vertex ile en yakın kenar arasındaki en kısa mesafeyi oluşturan vektörün hesaplanmasını içerir.
Kavşak noktasında, kenara yeni bir düğüm eklenir ve rastgele oluşturulan vertex'e bağlanır.
Alternatif olarak vertex, kendisine en yakın olan ayrık düğümlerin bağlantısını zincirleyerek en yakın düğüme eklenebilir. 
Bu yöntem daha az hesaplama gerektirir ve uygulanması daha kolaydır, ancak daha fazla noktanın saklanmasını gerektirir.
RRT Kübik grafikler üretir. Bu grafiklerin yapısal doğası, optimal bir yol bulma olasılığını engeller.
İki nokta arasındaki hipotenüsü almak yerine, bir üçgenin iki kenarı arasında gezinir.
Bu da daha uzun bir mesafe oluşturması demektir.
Diğer Path Planning algoritmalarına göre RRT hızlıdır, RRT* daha da hızlıdır.
RRT tarafından oluşturulan kübik doğa ve düzensiz yollar, RRT* ile ele alınır.
# RRG Algoritması #
RRG (Rapidly-exploring Random Graphs) yapısı, RRT ile neredeyse aynıdır, farklı olarak RRG algoritası vertexleri genişletir ve bu uzantı başarılı olursa, genel olarak bir grafik oluşturarak yakın prosedür tarafından döndürülen tüm köşeleri de genişletir.
## RRG algoritmasının izlediği adımlar :
* bir düğüm oluşturulur.
* bağlanılacak yeni vertex seçilir.
* yeni vertex'in state space içinde olup olmadığı kontrol edilir.
* Belirli bir yarıçap içindeki en yakın komşular k belirlenir.
* düğüm ile yeni vertex yolu arasında engel olup olmadığı kontrol edilir.
* eğer yol boş ise yeni bir düğüm belirlenir ve ağaca eklenir.
* hedef noktaya ulaşıldığında final state güncellenir.
* eğer yeni final state'in daha küçük cost'u varsa, final state olarak ayarlanır. 
# RRT* Algoritması #
RRT*, RRT algoritmasının optimize edilmiş halidir, düğüm sayısı sonsuza yaklaştığında, RRT* algoritması hedef noktası için mümkün olan en kısa yolu verecektir.
RRT*'ın temel prensibi RRT ile aynıdır, ancak algoritmaya iki temel ekleme ile önemli sonuçlar ortaya koymaktadır.
İlk olarak RRT* her vertex için bir önceki vertex ile kat ettiği mesafeyi kaydeder. Buna cost() denir.
Grafikte en yakın düğüm bulunduktan sonra, yeni düğümün etrafı sabit bir yarıçapla incelenir.
Yeni düğümden daha küçük cost() değeri olan bir düğüm tespit edilirse, bu düğüm yeni düğümün yerini alır.
Bu özelliğin etkisi, ağaç yapısına yelpaze şeklinde dalların eklenmesi olarak görülebilir.
Böylece RRT'nin kübik yapısı elimine edilir.
RRT*'ın eklediği ikinci fark, ağacın yeniden kablolanmasıdır. 
Bir vertex en küçük cost'lu vertex e bağlandıktan sonra, komşular tekrar incelenir. Cost değeri azalacaksa, komşu yeni eklenen vertex'e tekrar bağlanır. 
Bu özellik yolu daha düzgün hale getirir.
Grafikleri, RRT'lerden karakteristik olarak farklıdır.
Özellikle yoğun engel bulunan bir alanda RRT* daha faydalı olacaktır.
## RRT* Pseudo Code

```
Rad=r     // taranacak olan bölgeler için yarıçap belirlenir
G(V,E)    // boş olarak tanımlanan, kenar(edge) ve köşe(vertice) parametrelerini içeren grafik
for itr in range(0...n)
   Xnew = RandomPosition()                             // rastgele noktalar oluşturulur.
   If Obstacle(Xnew) == True, try again                // seçilen noktada bir engel mevcut ise, tekrar dene
   Xnearest = Nearest(G(V,E), Xnew)                    // en yakın vertex tespit edilir
   Cost(Xnew) = Distance(Xnew,Xnearest)                // vertexler arasındaki cost hesaplanır
   Xbest,Xneighbors = findNeighbors(G(V,E),Xnew,Rad)   // belirlenen yarıçaptaki komşular incelenir ve en iyi vertex tespit edilir
   Link = Chain(Xnew,Xbest)                            // bir önceki vertex ile en iyi vertex bağlanır
   for x' in Xneighbors
       if Cost(Xnew) + Distance(Xnew,x') < Cost(x')    // yeni vertex için cost değeri hesaplanır ve kontrol edilir
            Cost(x') = Cost(Xnew) + Distance(Xnew,x')
            Parent(x') = Xnew
            G+= {Xnew,x'}
   G += Link
Return G ```




















