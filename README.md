# Hybrid A* Uygulamaları
Hybrid A* yaklaşımını gerçekleyen, Linux ve Windows üzerinde çalışan iki adet uygulamaya dair notlar bu dokümanda özetlenmiştir. Kurulum ve çalıştırma aşamasında, işletim sistemlerinin yarattığı bir takım farklılıklar mevcuttur. Bu yüzden, dokümanın devamında, kurulum ve değerlendirme süreci iki ayrı alt başlıkta tariflenmiştir. 
## Hybrid A* Algoritması



## 1. Linux Tarafında Çalışılan Yaklaşım

Burada bir giriş yapılmalı.

#### Linux İçin Geliştirilen Uygulama Nasıl Çalıştırılır?
1. İlk olarak ROS map kullanımı için ros_map_server kurulmalıdır : 
ROS Melodic için : 
**sudo apt-get install ros-melodic-map-server**
ROS Kinetic için : 
**sudo apt-get install ros-kinetic-map-server**
2. Open Motion Planning Library (OMPL) kurulmalıdır :
**sudo apt install libompl-dev**
3. Catkin workspace oluşturulmalı ve kodlar workspace'e klonlanmalıdır :
**mkdir -p ~/catkin_ws/src**
**cd ~/catkin_ws/src**
**git clone https://github.com/karlkurzer/path_planner.git**
**cd .\.**
**catkin_make**
**source devel/setup.bash**
**rospack profile**
**roslaunch hybrid_astar manual.launch**

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
Kodun alındığı GitHub bağlantısı: https://github.com/Habrador/Self-driving-vehicle
Çalışmanın özetini içeren YouTube bağlantısı: https://www.youtube.com/watch?v=L591fS51F4I
Çalışmaya dair bazı notların paylaşıldığı Blog paylaşımı:  https://blog.habrador.com/2015/11/explaining-hybrid-star-pathfinding.html

#### Uygulamanın İncelemesi
Hybrid A* Algoritmasının uygulanmasında kullanılması gereken *Heuristic*'lere dikkat çekilmiştir. Öklidyen Mesafe, Reeds-Shepp Güzergahı ve Flowfield *Heuristic*'leri arasından en büyüğünün kaale alınması gerektiği, geliştirici tarafından not düşülmüştür. Ayrıca, Voronoi Mozaiklerini temel alan Voronoi Alanlarının kullanımının, haritadaki engellerden (obstacle) kaçınma hususunda başarımı yüksek bir çözüm sunduğu görülmüştür. Algoritmanın oluşturduğu güzergahın, ilk hali itibarıyla pürüzsüz bir eğri formatına uzak olduğu belirtilmiş, takip edilebilir bir güzergah eğrisi edinmek için oluşturulan eğriye bir smoothing (yumuşatma) operasyonu uygulanması gerektiğinden bahsedilmiştir. Aracın güzergahı takip etmesi için PID Controller kullanılmıştır.

Çalışılan araç tiplerinden bahsetmek gerekirse; Araba, Kamyon ve Tır için deneysel bir ortam oluşturulmuştur. İlk iki araç için algoritma dahilinde bir problem gözlenmezken, Tır'a dair önerilen çözümde bir takım eksiklikler olduğu gözlenmiştir. Geliştirici de Tır özelinde, bazı eksiklikler olduğunu not düşmüştür. 

<img src="https://github.com/etarakci-hvl/severalStuff/blob/master/Capture2.PNG" width="400">
Görsel x: Hedeflenen Konuma Gitmekte Olan Araç.
<img src="https://github.com/etarakci-hvl/severalStuff/blob/master/Capture3.PNG" width="400">
Görsel x+1: Etraftaki engellere dikkat ederek takip edilebilecek güzergahların keşfi.
#### Windows İçin Geliştirilen Uygulama Nasıl Çalıştırılır?
1. Uygulamaya dair dosyalar, https://github.com/etarakci-hvl/Self-driving-vehicle üzerinden clone'lanır.
2. **...\Self-driving-vehicle\Self-driving vehicle Unity\Assets** altında bulunan **self-driving.unity** dosyası Unity ile açılır. 


**Ebrar Demirbaş & Emrecan Tarakçı**
