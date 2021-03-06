# Hybrid A* Uygulamaları
Hybrid A* yaklaşımını gerçekleyen, Linux ve Windows üzerinde çalışan iki adet uygulamaya dair notlar bu dokümanda özetlenmiştir. Kurulum ve çalıştırma aşamasında, işletim sistemlerinin yarattığı bir takım farklılıklar mevcuttur. Bu yüzden, dokümanın devamında, kurulum ve değerlendirme süreci iki ayrı alt başlıkta tariflenmiştir. 
## Hybrid A* Algoritması
[Hybrid A* Algoritması](https://ai.stanford.edu/~ddolgov/papers/dolgov_gpp_stair08.pdf), A* algoritmasının bir türevi olup, orijinal algoritmaya benzerlikler göstermektedir. Ana farklılık ise, Hybrid A* algoritmasında, durum geçişleri (state transitions) *continuous space* üzerinde gerçekleşir. Oysa ki bu durum geçişleri, A* algoritmasında *discrete space*'te gerçekleşmektedir. Genelde bu alanda yaşanan sıkıntı, güzergah planlama algoritmalarının *Non-holonomic* robotlarda, takip edilmesi güç rotalar (smooth olmayan) üretmesidir. Hybrid A* Algoritmasının çözümlemeye çalıştığı sorun budur. Ayrıca, Continuous Search Space sonlu olmayacağından, grid tabanlı bir hücreselleştirmeye gidilerek, çizgenin büyümesi limitlenir. Burada mühim olan, oluşturulan çizgenin vertekslerinin grid dahilindeki herhangi bir continuous noktaya ulaşabileceğidir. Her bir state *s = (x, y, θ)*; *x*, *y* pozisyon değerlerinden ve verteksin yöneliminden(*θ*) oluşmaktadır. Yani verilen bir verteks için *s* her biçime girebilir. Fakat, *non-holonomic constraints* hesaba katıldığında, bir verteks üç ayrı aksiyon alabilmektedir: *maximum steering left*, *maximum steering right* ve *no steering*.
Son olarak, Hybrid A* algoritması, aracın hızını hesaba katmaz. Lakin, algoritmanın sağladığı güzergahı takip ederken, uygun bir hız profili oluşturulabilir.
Ayrıca, A* algoritması orta/uzun mesafeyi planlama adına da kullanılabilir. Bu durumda, yakın mesafeyi daha seri çalışan bir algoritma (RRT*) ile hesaplayıp, ana rota olarak Hybrid A*'ın uygun gördüğü rotayı baz rota olarak kullanabiliriz.

## 1. Linux Tarafında Çalışılan Yaklaşım
Bu çalışmanın amacı, nonholonomic araştırma için, real-time bir yol planlama algoritması oluşturmak ve görselleştirmektir.
Algoritma, input olarak araca monte edilen LIDAR tarafından oluşturulan binary engel haritalarını kullanmaktadır.
Algoritma C++ ile geliştirilmiş, ROS platformunda çalıştırılmış ve RViz programı ile görselleştirilmiş/simüle edilmiştir.  
Uygulama path planning algoritması olarak hybrid A* algoritması kullanmıştır.

Kodun alındığı GitHub bağlantısı :  https://github.com/karlkurzer/path_planner <br/>

Temel özellikler:
* Her bir cell başına 72 farklı heading (5° discretization)
* Kısıtlı Sezgisel (Constrained Heuristic) - nonholonomic without obstacles 
* Kısıtsız Sezgisel (Unconstrained Heuristic) - holonomic with obstacles
* Dubin's Shot
* C++ real-time uygulama (~10Hz)

#### Uygulamanın İncelenmesi
RViz platformu açıldıktan sonra 2D Pose Estimate ile başlangıç noktası, 2D Nav Goal ile hedef nokta belirlenir, 
demo haritada algoritma çalışır ve iki nokta arasında path oluşur. <br/><br/>
<img src="https://github.com/etarakci-hvl/severalStuff/blob/master/demomap1.png" width="600"><br/><br/>
Tekrar farklı iki nokta belirleyip değişik senaryolar denenebilir.<br/><br/><br/>
<img src="https://github.com/etarakci-hvl/severalStuff/blob/master/demomap2.png" width="600"><br/><br/>
Nasıl çalıştırılır kısmında belirtildiği gibi farklı haritalar üzerinde algoritma denenebilir.
Örneğin labirent şeklinde bir haritada ve park alanında algoritmanın nasıl davranacağı görülebilir.<br/><br/>

<img src="https://github.com/etarakci-hvl/severalStuff/blob/master/mazemap.png" width="600"><br/><br/>
<img src="https://github.com/etarakci-hvl/severalStuff/blob/master/parkinglot.png" width="600">

#### Linux İçin Geliştirilen Uygulama Nasıl Çalıştırılır?
1. İlk olarak ROS map kullanımı için ros_map_server kurulmalıdırheur : 
ROS Melodic için : 
```
sudo apt-get install ros-melodic-map-server
```
ROS Kinetic için : 
```
sudo apt-get install ros-kinetic-map-server
```
2. Open Motion Planning Library (OMPL) kurulmalıdır :
```
sudo apt install libompl-dev
```
3. Catkin workspace oluşturulmalı ve kodlar workspace'e klonlanmalıdır :
```
mkdir -p ~/catkin_ws/src
cd ~/catkin_ws/src
git clone https://github.com/karlkurzer/path_planner.git
cd ..
catkin_make
source devel/setup.bash
rospack profile
roslaunch hybrid_astar manual.launch
```
4. Bu adımlardan sonra Rviz ekranı açılacaktır.
* Rviz'de map_demo haritası gelecektir. Harita maus ile kontrol edilebilir.
* 2D Pose Estimate ile haritanın herhangi bir yerinde başlangıç noktası belirlenmelidir. 
* 2D Nav Goal ile de haritadaki hedef noktası belirlenmelidir.
* Hedef noktası belirlendikten sonra, algoritma çalışacak ve bulunan yol haritada gösterilecektir.

5. Algoritmayı farklı haritalarda denemek için : 
* home/catkin_ws/src/path_planner/maps klasörü içinden istenilen haritanın ismi, map.yaml dosyası içindeki image:... kısmına yazılarak kaydedilmelidir.
* Tekrar 3. adımdaki roslaunch işlemi yapıldığında, yeni harita görülecektir.

## 2. Windows Tarafında Çalışılan Yaklaşım
Bu kısımdaki çalışmalar, Windows 8.1 ve Unity 2018.4.20f1 simülasyon çevresi üzerinde yürütülmüştür. 
Kodun alındığı GitHub bağlantısı: https://github.com/Habrador/Self-driving-vehicle <br/>
Çalışmanın özetini içeren YouTube bağlantısı: https://www.youtube.com/watch?v=L591fS51F4I <br/>
Çalışmaya dair bazı notların paylaşıldığı Blog paylaşımı:  https://blog.habrador.com/2015/11/explaining-hybrid-star-pathfinding.html <br/>

#### Uygulamanın İncelemesi
Hybrid A* Algoritmasının uygulanmasında kullanılması gereken *Heuristic*'lere dikkat çekilmiştir. Öklidyen Mesafe, Reeds-Shepp Güzergahı ve Flowfield *Heuristic*'leri arasından en büyüğünün kaale alınması gerektiği, geliştirici tarafından not düşülmüştür. Ayrıca, Voronoi Mozaiklerini temel alan Voronoi Alanlarının kullanımının, haritadaki engellerden (obstacle) kaçınma hususunda başarımı yüksek bir çözüm sunduğu görülmüştür. Algoritmanın oluşturduğu güzergahın, ilk hali itibarıyla pürüzsüz bir eğri formatına uzak olduğu belirtilmiş, takip edilebilir bir güzergah eğrisi edinmek için oluşturulan eğriye bir smoothing (yumuşatma) operasyonu uygulanması gerektiğinden bahsedilmiştir. Aracın güzergahı takip etmesi için PID Controller kullanılmıştır.

Çalışılan araç tiplerinden bahsetmek gerekirse; Araba, Kamyon ve Tır için deneysel bir ortam oluşturulmuştur. İlk iki araç için algoritma dahilinde bir problem gözlenmezken, Tır'a dair önerilen çözümde bir takım eksiklikler olduğu gözlenmiştir. Geliştirici de Tır özelinde, bazı eksiklikler olduğunu not düşmüştür. 

Unity arayüzünü ve algoritmanın çalıştırıldığı bir anı gösteren görüntü (beyaz eğri mouse ile seçmek isteyebileceğimiz konuma çizilen güzergahı gösterir.):<br/><br/>
<img src="https://github.com/etarakci-hvl/severalStuff/blob/master/Capture1.PNG" width="600"> <br/><br/>
Hedeflenen konuma gitmekte olan araca dair bir görüntü: <br/><br/>
<img src="https://github.com/etarakci-hvl/severalStuff/blob/master/Capture2.PNG" width="600"> <br/><br/>
Etraftaki engellere dikkat edecek şekilde; takip edilebilecek güzergahların keşfine dair görüntü: <br/><br/>
<img src="https://github.com/etarakci-hvl/severalStuff/blob/master/Capture3.PNG" width="600">

#### Windows İçin Geliştirilen Uygulama Nasıl Çalıştırılır?
1. Uygulamaya dair dosyalar, https://github.com/etarakci-hvl/Self-driving-vehicle üzerinden clone'lanır.
2. **...\Self-driving-vehicle\Self-driving vehicle Unity\Assets** altında bulunan **self-driving.unity** dosyası Unity ile açılır. 

#### 3. Hybrid A* Algoritmasının Çalışması İçin Gereken Ana Girdiler
Kullanılan *Heuristic*'lerin bekledikleri girdiler şu şekildedir:
```
- Euclidean Distance - Initial Position [x, y], Goal Position [x', y'].
- Flow field (8 neighbors: North(N), East(E), West(W), South(S), and SE, SW, NE, NW) with obstacles Initial Position [x, y, cost = 0], Goal Position(x', y', cost' = ?)
- Reeds-Shepp paths - Initial Position with Orientation [x, y, Θ], Goal Position with Orientation [x', y', Θ']
- Dubin's Path - Initial Position with Orientation [x, y, Θ], Goal Position with Orientation [x', y', Θ'], turning radius of velocity (ρ) = forward velocity / maximum angular velocity
```
* Aracın dönüş mekanizmasına tam anlamıyla hakim olmak için, kullanılacak aracın boyut bilgisi, keza *bicycle model* olarak isimlendirilen ve araçların dönüş mekanizmasını sade bir şekilde modelleyen yaklaşımın temellendirildiği arka aksın ve ön aksın konumları ve tekerlerin birbirine olan mesafeleri bilinmelidir.
* Çevrenin, *grid* tabanlı *discretization*'ını yapmak adına, modellenen çevrenin kısımlara ayrılmasını sağlayacak bir yaklaşım gerekmektedir. Bu yaklaşım sağlandığı takdirde, hesaplama maliyeti ziyadesiyle azalacaktır.
* Sensörler (örn: Lidar ve/veya Radar) tarafından oluşturulan *obstacle(engel)* haritaları kullanılmalıdır. Burada engeller yalnızca katı cisimler olarak addedilmemelidir. Bir başka sensörün (örn: Kamera) tespit edeceği *su birikintisi*, *çukur*, *çamurlu yol* veya *toprak yol*; belirtilen *Heuristic*'ler kapsamında maliyetlendirilebilir ve algoritmanın kaçınması sağlanabilir.
* Dünya modeline yansıtılacak çevredeki engellerin konum bilgilerine ek olarak, başlangıç ve hedef noktasının konum bilgisi ve aracın oryantasyonu sağlanmalıdır. Haliyle, *current position*'ı edinmek adına bir lokalizasyon çıktısı gerekmektedir. 

**Ebrar Demirbaş & Emrecan Tarakçı**


