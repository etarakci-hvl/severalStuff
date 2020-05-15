# D* Lite Algoritması ve Uygulaması

Kodun alındığı repository: [Birincil Kaynak](https://github.com/mdeyo/d-star-lite), [İkincil Kaynak](https://github.com/GBJim/d-star-lite), [Ek Kaynak](https://github.com/azampagl/robotics-d-star-lite), [Ek Kaynak 2](https://github.com/ArekSredzki/dstar-lite)

Yararlanılan Makaleler: [D* Lite](http://idm-lab.org/bib/abstracts/papers/aaai02b.pdf)

D* Lite algoritması bilinmeyen arazilerde hızlı bir şekilde yeniden planlama yapabilen bir robotik navigasyon
algoritması olarak düşünülebilir. Focused Dynamic A* (D*) algoritmasıyla aynı navigasyon stratejilerine sahiptir. Her iki algoritma da hedef (goal) vertex'ten robotun bulunduğu vertex'e doğru bir search (arama) yapar. Aramayı, odaklı (focused) bir hale sokmak için heuristic'ler kullanılır. Yakın metodlarla priority queue üzerinde yapılan reordering süreci, minimize edilmeye çalışılır. D* Lite algorıtması LPA* (Lifelong Planning A*: A*'ın incremental versiyonudur ve tüm çizgenin hesaplanmasını gerektirmeden değişikliklere adapte olur) üzerine inşa edilmiştir. D* ile D* Lite bu noktada ayrılır. Lakin, D* Lite, D* kadar etkin çalışmaktadır.

## D* Lite Algoritmasi


## Uygulamaya Dair Bazı Notlar
Kodun alındığı kaynak birinci kaynaktır. Fakat, hesaplamaların yayılımının anlaşılması için ikinci kaynaktaki gösterimler de göz önünde bulundurulmalıdır.
Fazla basitleştirilmiş örnekler üzerinden incelense de büyük ölçekli ve grid tabanlı (50-100 santimetre karelik alanlara ayrılmış) haritalarda da başarılı bir şekilde sonuç alabileceğimiz bir algorıtma olduğunu destekleyen emareler bulunmaktadır.

## Nasıl Çalıştırılır?
1. Yalnızca Python 3 ve Pygame gereklidir. Pygame'in bulunduğu kütüphaneyi kaynaklara ekledikten ve güncelleme yaptıktan sonra yüklemi işlemi gerçekleştirilir.
- `sudo add-apt-repository ppa:thopiekar/pygame`
- `sudo apt-get update`
- `apt-get install python3-pygame`
2. `python3 main.py` ile kod çalıştırılır. Kullanıcı dilediği gibi grid üzerinde ***mouse-click*** ile engel ekleyebilmektedir.
Agent kırmızı daire, hedef ise yeşil hücredir. Engel ekledikten sonra ***space*** tuşu ile güzergah'a dair ilerleme ve hücrelerdeki hesaplamalar takip edilebilir.
