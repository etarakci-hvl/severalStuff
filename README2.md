## RRT, RRG ve Fast RRT* Uygulamaları

Bu calismada Rapidly-Exploring Random Tree(RRT), Rapidly-Exploring Random Graph(RRG) ve Rapidly-Exploring Random Tree*(RRT*) algoritmalarina dair uygulamar yer almaktadir. Algoritmayi gercekleyen yazarin bahsettigi uzere bir takim tasarim kararlarindan oturu RRT* algoritmasi standart uygulamaya nazaran ~10 kat hizli calismaktadir. 
Programin calisma prensibinde rastgele yerlestirilen engellere, baslangic ve bitis noktalarina vurgu yapilmistir. 

Kodun alindigi repository: [Fast RRT*](https://github.com/dixantmittal/fast-rrt-star)

Yararlanilan Makale: [Incremental Sampling-based Algorithms
for Optimal Motion Planning](http://roboticsproceedings.org/rss06/p34.pdf)

**RRT ve RRT\* algoritmalarinin ikili kiyasi asagidadir:**

<img src="https://github.com/etarakci-hvl/severalStuff/blob/master/RRT.png" width="580">
<img src="https://github.com/etarakci-hvl/severalStuff/blob/master/RRTStar.png" width="600">


**RRT, RRG ve RRT\* algoritmalarinin uclu kiyasi asagidadir:**
<img src="https://github.com/etarakci-hvl/severalStuff/blob/master/RRT1.png" width="600">
<img src="https://github.com/etarakci-hvl/severalStuff/blob/master/RRG1.png" width="600">
<img src="https://github.com/etarakci-hvl/severalStuff/blob/master/RRTStar1.png" width="600">

Bu ornek sonuclara ek olarak, diger guzergah kiyaslamalari, maliyet ve hesaplama zamani sonuclari *plots* ve *metrics* klasorleri altinda bulunabilir.

**RRT Algoritmasi:**  Kademeli artan ornekleme tabanli algoritmalar tekil-sorgu icin gelistirilmislerdir. Gercek zamanli uygulamalarda en etkin olan algoritmalardan biri de RRT algoritmasidir. Olasiliksal olarak *complete* olduklari gosterilen bu algoritmanin bir diger ozelligi, hata olasiliginin *exponential decay*'e sahip olmasidir. 

**RRG Algoritmasi:** 
Kademeli artan ornekleme tabanli algoritmalara bir diger ornek ise RRG algoritmasidir. Linear Temporal Logic kullanimi ve deterministik µ-calculus  ile verilen spesifikasyonlari yonetir. Kademeli olarak artacak sekilde uretilen rotalar cizge veri yapisi icerisinde tutulur. RRG tarafindan donen en iyi rotanin maliyeti neredeyse her zaman optimuma yakinsar.

**RRT\* Algoritmasi:** 
RRT* algoritmasini, RRG algoritmasinin agac tabanli versiyonu olarak dusunmek mumkundur.
  
#### Uygulamaya Dair Bazi Notlar

Uzay temsili, orjin ve boyut olarak tutulmaktadir((15, 20), (3,3)). Algoritmlarin calismasi icin, cevrede yer alan engellere dair kullanilan bu temsile ek olarak; baslangic durumu, hedef bolge ve engeller haritasi (tum objelere dair Uzay Temsili) gerekmektedir. Asagida ilgili parametrelerin aciklamalari gorulebilir.

##### state_space
Durum Uzayi, dis dunyanin temsili olarak dusunulmelidr. Yukarida bahsedilen formda algoritmaya saglanir. 

##### starting_state
Baslangic durumu, rotasi hesaplanacak ajanin baslangic noktasi olarak kabul edilebilir. Bu parametre ajanin orjini ve alani olarak dusunulmelidir.
##### target_space
Hedef bolge, varilmak istenen bolge olarak dusunulmustur. Varilmak istenen bolgenin koordinatlari ve alani seklinde dusunulmelidir.
##### obstacle_map
Engeller haritasi, durum uzayinda yer alan tum engellerin bilgilerinin barindigi haritadir ve her bir engel icin koordinat-alan ikilisi seklinde tutulmaktadir.
##### n_samples
Iterasyon sayisi, algoritmanin uretecegi ornek noktalariyla iliskilidir. Carpisan ve alanin disinda yer alan noktalar da iterasyonlar arasinda yer alacagindan kabul edilebilir orneklerin sayisi *default = 1000* iterasyon sayisindan az olabilir.
##### granularity
Oge boyu veya taneciklilik olarak nitelendirilen bu kavram algoritmanin sureci ne kadar detayli ele aldigi olarak dusunulebilir. Kademeli artan carpisma denetlemesi kullanildigindan, taneciklilik kavramina ihtiyac duyulmaktadir . Bu parametrenin detayi artirildiginda, programin yavaslayacagi hatirda tutulmalidir.
##### d_threshold
Bu parametre, eldeki dugumden yeni bir nokta ornekleneceginde, orneklenecek noktanin ne kadar uzakta olacagina karar verir.

#### Nasıl Çalıştırılır ? 

* Öncelikle, `conda env create --name FastRRT --file environment.yml` komutuyla gereken kütüphaneler yüklenmelidir ve `conda activate FastRRT` komutuyla çevre aktive edilmedilir.

* Cevre aktive edildikten sonra, terminalde klasörün bulunduğu uzantıya gidilir ve `python Main.py` komutu yurutulur, çıktı olarak RRT* için ve RRT için rastgele olusturulan engellerin ve bu engellere karsi olusturulan rotalara dair 2 adet figür gelecektir.





