# D* Lite Algoritması ve Uygulaması

Kodun alındığı repository: [Birincil Kaynak](https://github.com/mdeyo/d-star-lite), [İkincil Kaynak](https://github.com/GBJim/d-star-lite), [Ek Kaynak](https://github.com/azampagl/robotics-d-star-lite), [Ek Kaynak 2](https://github.com/ArekSredzki/dstar-lite)

Yararlanılan Makaleler: [D* Lite](http://idm-lab.org/bib/abstracts/papers/aaai02b.pdf)

D* Lite algoritması bilinmeyen arazilerde hızlı bir şekilde yeniden planlama yapabilen bir robotik navigasyon
algoritması olarak düşünülebilir. Focused Dynamic A* (D*) algoritmasıyla aynı navigasyon stratejilerine sahiptir. Her iki algoritma da hedef (goal) vertex'ten robotun bulunduğu vertex'e doğru bir search (arama) yapar. Aramayı, odaklı (focused) bir hale sokmak için heuristic'ler kullanılır. Yakın metodlarla priority queue üzerinde yapılan reordering süreci, minimize edilmeye çalışılır. D* Lite algorıtması LPA* (Lifelong Planning A*: A*'ın incremental versiyonudur ve tüm çizgenin hesaplanmasını gerektirmeden değişikliklere adapte olur) üzerine inşa edilmiştir. D* ile D* Lite bu noktada ayrılır. Lakin, D* Lite, D* kadar etkin çalışmaktadır. 

Aşağıda konfigürasyonun değişmesi üzerine replaning ismi verilen yeniden planlama sürecine dair sade bir çıktı görmekteyiz. Bu tarz bir esneklik, ileride global planlama yaparken trafik vs. gibi değişkenliklere uyum sağlayarak çevik sonuçlar almamıza yardımcı olacaktır.

<img src="https://github.com/etarakci-hvl/severalStuff/blob/master/DStarLite.png" width="600">

## Uygulamaya Dair Bazı Notlar
Kodun alındığı kaynak birinci kaynaktır. Fakat, hesaplamaların yayılımının anlaşılması için ikinci kaynaktaki gösterimler de göz önünde bulundurulmalıdır.
Fazla basitleştirilmiş örnekler üzerinden incelense de büyük ölçekli ve grid tabanlı (50-100 santimetre karelik alanlara ayrılmış) haritalarda da başarılı bir şekilde sonuç alabileceğimiz bir algorıtma olduğunu destekleyen emareler bulunmaktadır.

## D* Lite Algoritması
Özetlemek gerekirse, D* Lite'i gerçeklemek için LPA* algorıtması kullanılır. Normal şartlar altında, LPA*, bulunulan vertex ile hedef vertex arasında tekrar tekrar en kısa path'e dair çıktı üretir. Robot hedef vertex'e doğru yol alırken çizge üzerindeki kenarların maliyeti değişecektir. Ama D* Lite algorıtması kapsamında hedef vertex'ten, başlangıç vertex'ine doğru bir arama yapılır. Grid tabanı yaklaşımda, hedef vertex'in etrafındaki 8 hücrenin değeri 1 olarak atanacaktır. Uzaklaştıkça bu değerler artar, ayak basılamaz noktalara ise sonsuz değeri atanmıştır. Maliyet'e, mesafeye ve heuristic'e dayalı bir hesap ile takip edilecek path'e dair bir priority queue oluşturulur. Fakat konfigürasyon değişirse, kenar maliyetleri değişeceğinden, priority queue içerisinde barınan sıralamalar güncellenir. Priority queue'ların uygulanmasında Binary Heap'lerden yararlanılmıştır. Ek olarak, kompleks veri yapıları (Fibonacci Heaps as Priority Queues) kullanılarak, update adımlarının daha etkili bir şekilde gerçekleştirilebileceği önerilmiştir.Algoritmanın detaylı pseudocode'una ve optimize edilmiş haline makale link'inden erişilebilir. 

## Nasıl Çalıştırılır?
1. Yalnızca Python 3 ve Pygame gereklidir. Pygame'in bulunduğu kütüphaneyi kaynaklara ekledikten ve güncelleme yaptıktan sonra yüklemi işlemi gerçekleştirilir.
- `sudo add-apt-repository ppa:thopiekar/pygame`
- `sudo apt-get update`
- `apt-get install python3-pygame`
2. `python3 main.py` ile kod çalıştırılır. Kullanıcı dilediği gibi grid üzerinde ***mouse-click*** ile engel ekleyebilmektedir.
Agent kırmızı daire, hedef ise yeşil hücredir. Engel ekledikten sonra ***space*** tuşu ile güzergah'a dair ilerleme ve hücrelerdeki hesaplamalar takip edilebilir.
