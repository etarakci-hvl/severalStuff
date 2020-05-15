# D* Lite Algoritmasi ve Uygulamasi

Kodun alındığı repository: [Birincil Kaynak](https://github.com/mdeyo/d-star-lite), [Ikincil Kaynak](https://github.com/GBJim/d-star-lite), [Ek Kaynak](https://github.com/azampagl/robotics-d-star-lite)
Yararlanılan Makaleler: [D* Lite](http://idm-lab.org/bib/abstracts/papers/aaai02b.pdf)

D* Lite algoritmasi bilinmeyen arazilerde hizli bir sekilde yeniden planlama yapabilen bir robotik navigasyon 
algoritmasi olarak dusunulebilir. Focused Dynamic A* (D*) algoritmasiyla ayni navigasyon stratejilerine sahiptir. Her ikisi algoritma da hedef (goal) vertex'ten robotun bulundugu vertex'e dogru bir search (arama) yapar, aramayi odakli (focused) bir hale sokmak icin heuristic'ler kullanilir, yakin metodlarla priority queue uzerinde yapilan reordering sureci minimize edilmeye calisilir. D* Lite algoritmasi LPA* (Lifelong Planning A*: A*'in incremental versiyonudur ve tum cizgenin hesaplanmasini gerektirmeden degisikliklere adapte olur) uzerine insaa edilmistir. D* ile D* Lite bu noktada ayrilir. Lakin, D* Lite, D* kadar etkin calismaktadir.  

## D* Lite Algoritmasi


## Uygulamaya Dair Bazi Notlar
Kodun alindigi kaynak birinci kaynaktir. Fakat, hesaplamalarin yayiliminin anlasilmasi icin ikinci kaynaktaki gosterimler de goz onunde bulundurulmalidir. 
Fazla basitlestirilmis ornekler uzerinden incelense de buyuk olcekli ve grid tabanli (50-100 santimetre karelik alanlara ayrilmis) haritalarda da basarili bir sekilde sonuc alabilecegimiz bir algoritma oldugunu destekleyen emareler bulunmaktadir.

## Nasıl Çalıştırılır?
1. Yalnizca Python 3 ve Pygame gereklidir. Pygame'in bulundugu kutuphaneyi kaynaklara ekledikten ve guncelleme yaptiktan sonra yuklemi islemi gerceklestirilir.
- `sudo add-apt-repository ppa:thopiekar/pygame`
- `sudo apt-get update`
- `apt-get install python3-pygame` 
2. `python3 main.py` ile kod calistirilir. Kullanici diledigi gibi grid uzerinde ***mouse-click*** ile engel ekleyebilmektedir. 
Agent kirmizi daire, hedef ise yesil hucredir. Engel ekledikten sonra ***space*** tusu ile guzergah'a dair ilerleme ve hucrelerdeki hesaplamalar takip edilebilir. 
