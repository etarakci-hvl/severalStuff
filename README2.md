## RRT, RRG ve Fast RRT* Uygulamaları

Bu çalışmada Rapidly-Exploring Random Tree(RRT), Rapidly-Exploring Random Graph(RRG) ve Rapidly-Exploring Random Tree*(RRT*) algoritmalarına dair uygulamalar yer almaktadır. Algoritmayı gerçekleyen yazarın bahsettiği üzere bir takım tasarım kararlarından ötürü RRT* algoritması standart uygulamaya nazaran ~10 kat hızlı çalışmaktadır. Programın çalışma prensibinde rastgele yerleştirilen engellere, başlangıç ve bitiş noktalarına vurgu yapılmıştır. Unutmamak gerekir ki, çizge teorisinde, ağaç veri yapıları yönsüz çizgeler olarak nitelendirilebilir(Her iki düğümün yalnızca birer kenar ile bağlandığı, çevrimsiz (acyclic), yönsüz (undirected) çizgeler olarak açıklamak mümkündür).

Kodun alındığı repository: [Fast RRT*](https://github.com/dixantmittal/fast-rrt-star)

Yararlanılan Makaleler: [Incremental Sampling-based Algorithms
for Optimal Motion Planning](http://roboticsproceedings.org/rss06/p34.pdf),
[Sampling-based Algorithms for Optimal Motion Planning](https://arxiv.org/pdf/1105.1186.pdf)

Yararlanılan Blog Yazısı: [Robotic Path Planning:RRT and RRT*](https://medium.com/@theclassytim/robotic-path-planning-rrt-and-rrt-212319121378)

**RRT Algoritması:**  Kademeli artan örnekleme tabanlı algoritmalar tekil-sorgu için geliştirilmişlerdir. Gerçek zamanlı uygulamalarda en etkin olan algoritmalardan biri de RRT algoritmasıdır. Olasılıksal olarak *complete* oldukları gösterilen bu algoritmanın bir diğer özelliği, hata olasılığının *exponential decay*'e sahip olmasıdır. 
RRT algoritmasında başlangıç ve hedef nokta arasında rastgele noktalar oluşturulur ve mevcut en yakın vertex'e bağlanır. Fakat planlanan yol her zaman optimal olmayacaktır. Her vertex(köşe noktası) oluşturulduğunda, vertex'in bir engelin(obstacle) dışında kalıp kalmadığına dair bir kontrol yapılır. Bir engel ile karşılaşıldığında, vertex en yakın komşusuna bağlanır ve engelden uzaklaşılır.
Hedef noktasına veya bir sınıra ulaşıldığında algoritma sona erer.
## RRT Pseudo Code 
```
Qgoal         // ulaşılması beklenen hedef noktası
Counter = 0   // iterasyonları takip eden sayaç
lim = n       // algoritmanın çalıştırması gereken iterasyon sayısı
G(V,E)        // boş olarak tanımlanan, kenar(edge) ve köşe(vertice) parametrelerini içeren Çizge
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
```
RRT köşeli çıktılar üretir. Bu noktada elde edilecek rotaların yapısal doğası, optimal bir yol bulma olasılığını engeller.
İki nokta arasındaki hipotenüsü almak yerine, bir üçgenin iki kenarı arasında gezinir.
Bu da daha uzun bir mesafe oluşturması demektir. 

**RRG Algoritması:** 
Kademeli artan örnekleme tabanlı algoritmalara bir diğer örnek ise RRG algoritmasıdır. Linear Temporal Logic kullanımı ve deterministik µ-calculus ile verilen spesifikasyonları yönetir. Kademeli olarak artacak şekilde üretilen rotalar çizge veri yapısı içerisinde tutulur. RRG tarafından dönen en iyi rotanın maliyeti neredeyse her zaman optimuma yakınsar.


y. Informally speaking, the RRT algorithm extends
the nearest vertex towards the sample. The RRG algorithm first
extends the nearest vertex, and if such extension is successful,
it also extends all the vertices returned by the Near procedure,
producing a graph in general. In both cases, all the extensions
resulting in collision-free trajectories are added to the graph
as edges, and their terminal points as new vertices.


RRG ve RRT Algoritmalarının Ortak Kısımları:
```
V <- {x_init}; E <- ∅; i <- 0;                           // Köşe, Kenar, ve İterasyon değişkenlerine öndeğer atamaları yapılır.
while i < N do                                           // Üst limite varılana kadar:
   G <- (V,E);                                           // Çizge oluşturulur.
   x_rand <- Sample(i); i <- i+1;                        // Rastgele nokta belirlenir.
   (V,E)<-Extend(G,x_rand);                              // Extend_RRG veya Extend_RRT algoritması çağrılabilir.
```
Extend_RRG(G,x) Algoritması:
```
V' <- V; E' <- E;                                        // Köşe ve Kenar değişkenlerine öndeğer atamaları yapılır.
x_nearest <- Nearest(G,x);                               // Çizge ve bir x noktası alınır. Bir mesafe fonksiyonu(Örn.: Euclidean D.) uyarınca, x değerine en yakın vertex döner.
x_new <- Steer(x_nearest, x);                            // Steer fonksiyonunun çıktısı için, birinci parametresinden çok uzaklaşmayacak biçimde, ikinci parametresine birincisine nazaran daha yakın bir değer türetilir.
if ObstacleFree(x_nearest, x_new) then                   // İki nokta arasında line segment çizilmesine bir engel yoksa: 
   V' <- V' U {x_new};                                   // Yeni nokta vertex'lere eklenir.
   E' <- E' U {(x_nearest, x_new), (x_new, x_nearest)};  // Yeni nokta ve ona yakın bir konumda bulunan vertex'e dair çift yönlü kenarlar hali hazırda bulunan edge'lere eklenir. 
   X_near <- Near(G, x_new, |V|);                        // Near; çizge, nokta ve bir sayı alır ve verilen noktaya en yakın vertex'ler çıktı olarak verilir. Nearest fonksiyonunun genellemesidir. Bir değer dönmesi yerine değerler kümesi döner.    
   for all x_near ∈ X_near do                            // Near fonksiyonunun döndüğü değerler kümesinde yer alan her bir değer için:
      if ObstacleFree(x_new, x_near) then                // Yeni nokta ile vertex arasında bir engel yoksa:
         E' <- E' U {(x_near, x_new),(x_new, x_near)};   // Hali hazırda vertex kümesine dahil edilmiş olan x_new noktasına diğer vertexlerden kenar çekilebildiği müddetçe, çift yönlü kenarlar bu satırlar kenar listesine eklenir. 
return G' = (V', E')                                     // Güncellenmiş Çizge döner.
```

## RRG algoritmasının izlediği adımlar :
* bir düğüm oluşturulur.
* bağlanılacak yeni vertex seçilir.
* yeni vertex'in state space içinde olup olmadığı kontrol edilir.
* Belirli bir yarıçap içindeki en yakın komşular k belirlenir.
* düğüm ile yeni vertex yolu arasında engel olup olmadığı kontrol edilir.
* eğer yol boş ise yeni bir düğüm belirlenir ve ağaca eklenir.
* hedef noktaya ulaşıldığında final state güncellenir.
* eğer yeni final state'in daha küçük cost'u varsa, final state olarak ayarlanır. 

**RRT\* Algoritması:** 
RRT*, RRT algoritmasının optimize edilmiş halidir, düğüm sayısı sonsuza yaklaştığında, RRT* algoritması hedef noktası için mümkün olan en kısa yolu verecektir. Ayrıca, RRT* algorıtmasını, RRG algoritmasının ağaç tabanlı versiyonu olarak düşünmek mümkündür. RRG'nin *asymptotic optimality* özelliğini, ağaç veri yapısını kullanarak gerçekleyen bu metodoloji, hali hazırda ağaçta bulunan düğümlere uzanan en düşük maliyetli güzergahları keşfeder. Bunu yaparken kendini yeniden yapılandırır, ***rewire*** eder.

RRT*'ın temel prensibi RRT ile aynıdır, ancak algoritmaya iki temel ekleme ile önemli sonuçlar ortaya koymaktadır.
İlk olarak RRT* her vertex için bir önceki vertex ile kat ettiği mesafeyi kaydeder. Buna cost() denir.
Grafikte en yakın düğüm bulunduktan sonra, yeni düğümün etrafı sabit bir yarıçapla incelenir.
Yeni düğümden daha küçük cost() değeri olan bir düğüm tespit edilirse, bu düğüm yeni düğümün yerini alır.
Bu özelliğin etkisi, ağaç yapısına yelpaze şeklinde dalların eklenmesi olarak görülebilir.
Böylece RRT'nin köşeli rota yapısı elimine edilir. RRT*'ın eklediği ikinci fark, ağacın yeniden yapılandırılmasıdır (rewiring). 
Bir vertex en küçük cost'lu vertex e bağlandıktan sonra, komşular tekrar incelenir. Cost değeri azalacaksa, komşu yeni eklenen vertex'e tekrar bağlanır. Bu özellik üretilen yolu daha düzgün (smooth) hale getirir.
Görsel olarak da, RRT'lerden karakteristik olarak farklıdır. Özellikle yoğun engel bulunan bir alanda RRT* daha faydalı olacaktır.

## RRT* Pseudo Code
```
Rad=r     // taranacak olan bölgeler için yarıçap belirlenir
G(V,E)    // boş olarak tanımlanan, kenar(edge) ve köşe(vertice) parametrelerini içeren Çizge
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
Return G 
```

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

