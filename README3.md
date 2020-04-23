## Simulator Uzerinden Sentetik Veri Eldesi

Simulatorden edinilen direkt konum bilgisi (x,y,z) uzerine sensor okumalarini temsilen eklenen gurultu modeli asagida tariflenmistir.
Her bir nokta icin, her eksen icin ayri ayri Gaussian bir gurultu eklenmistir. 
Isotropic olarak ele almak yerine, eksende beklenen olasi sapmalari o eksen ozelinde ele almak icin bu tarz bir modellemeye gidilmistir.
Ayrica, sensor okumalarinda meydana gelen ilk defa karsilasilan bir cisim icin modele oturma sureci 
\begin{equation}e^(-x)\end{equation}
'le orantili ussel azalan bir fonksiyonla 
temsil edilmistir.

Çıktılar:
Min aşağıdaki konuları açıklayan doküman:
1. Filtrenin zamana bağlı olarak oturması nasıl simüle edilecek (cold-start)
2. Kullanılan Gauss modeli
3. Filtre çıktıları nasıl simüle edilecek (Gauss modeli)
4. Sensör ve algoritma modelleri nasıl simüle edilecek (Kamera, RADAR, LIDAR)
5. Hangi objeler sentetik dünya modeli çıktısı içinde yer alacak? 
6. Sentetik veri data alma sıklığı nasıl ele alınacak? (simulation time, wall clock)
7. Sampling toolu hangi durumda kullanılacak?
