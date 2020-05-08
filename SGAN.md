# Social GAN

Social GAN algoritmasinin ([Social GAN: Socially Acceptable Trajectories with Generative Adversarial Networks](https://arxiv.org/abs/1803.10892) (CVPR 2018) gerçeklenmesine dair çalışmalar bu repo'da toplanmistir.
Kodun alındığı kaynak: https://github.com/agrimgupta92/sgan olarak belirtilebilir.

Normal sartlar altinda, insan hareketi bireyler arasinda gerceklesen, bircok farkli davranisa evrilebilme potansiyeli olan ve sosyal adetlere uygun icra edilen bir surectir. Social GAN makalesinde, bahsi gecen bu karmasik konuya dair bir yaklasim sunulmustur. Bu yaklasimda, Sequence Prediction ve Generative Adversarial Networks konseptlerini birlikte kullanarak, bir recurrent sequence-to-sequence model elde edilmistir. Bu model hareket gecmislerini gozlemleyerek ve yayalara dair bilgileri toplama adina kullandigi pooling mekanizmasi sayesinde gelecege dair davranislari kestirmeyi amaclar. 
Asagida, kompleks senaryolarda, model tarafindan uretilmis ve sosyal olarak kabul edilebilir olarak nitelendirilen tahminleri gormekteyiz. Her bir insan farkli bir renkle belirtilir. Gozlem verisi noktalarla, tahmin verisi yildizlarla gosterilmektedir.
![](images/2.png)
![](images/3.png)

#### 1. Model
Sunulan mimari 3 kisimdan meydana gelmektedir. Generator (G), Pooling Module (PM) ve Discriminator (D). Generator yapisi, Encoder ve Decoder'a dair Hidden State'lerin Pooling Module araciligiyla baglandigi Encoder-Decoder cercevesi uzerine oturur. Generator yapisi, bir mahalde bulunan yayalara dair trajectory'leri girdi olarak alir ve ilgili tahmin trajectory'lerini olusturur. Discriminator yapisi ise, tum sekansi, yani hem input trajectory'lerini hem de bu trajectory'lere dair tahminleri bir arada edinir ve gercek/sahte seklinde siniflandirir.
![Mimari](images/model.png)
![Pooling Module](images/PM.png)
Makalede onerilen Pooling mekanizmasi (kirmizi noktali oklari) ile Social Pooling (kirmizi cizgili grid) yaklasiminin kirmizi insan icin kiyaslanmasini gormekteyiz. Onerilen metod kirmizi insan ile diger insanlar arasindaki relatif pozisyonlari hesaplar. Bu pozisyonlar her bir insanin hidden state'ine pespese eklenir. Sonrasinda bir MLP tarafindan bagimsiz bir sekilde islenir ve sonrasinda elementwise pool'lanarak kirmizi insanin pooling vector'u (P1) hesaplanir. Social Pooling sadece grid icersindeki insanlari hesaba katar ve her bir ikili arasindaki etkilesimi modelleyemez.

#### 2. Setup
Bu calisma Ubuntu 16.04 uzerinde, Python 3.5 ve PyTorch 0.4 ile gerceklenmistir. Kodun alindigi repo'da onerilen kurulum adimlari su sekildedir.
```bash
python3 -m venv env               # Create a virtual environment
source env/bin/activate           # Activate virtual environment
pip install -r requirements.txt   # Install dependencies
echo $PWD > env/lib/python3.5/site-packages/sgan.pth  # Add current directory to python path
# Work for a while ...
deactivate  # Exit virtual environment
```

#### 3. Pretrained Modeller
`bash download_models.sh` script'i ile pretrained modeller indirilebilir. Bu script asagidaki modelleri indirecektir.
- `sgan-models/<dataset_name>_<pred_len>.pt`: 5 veri seti icin toplam 10 adet pretrained model barindirmaktadir. Bu modeller asagidaki tablolarin SGAN-20V-20 kismina tekabul eder.
- `sgan-p-models/<dataset_name>_<pred_len>.pt`: 5 veri seti icin toplam 10 adet pretrained model barindirmaktadir. Bu modeller asagidaki tablolarin SGAN-20VP-20 kismina tekabul eder.

![Karsilastirma](images/comparison.png)

Kendi yontemlerini SGAN-kVP-N diye anmaktalar. kV ile vurgulanan modelin variety loss kullanilarak egitilip egitilmedigidir (k=1 variety loss yoktur demektir). P ise one surulen Pooling modulunun kullanilip kullanilmadigini vurgular. Test surecinde, nicel degerlendirme kapsaminda, coklu sample'lama yapilarak ve L2 normundaki en dogru tahmini kaale alarak ilerlenmistir. Son olarak N, test surecinde modelden kac defa sample alindigini belirtmektedir. t\_pred = 8 ve 12 icin, iki adet metre cinsinden error metrigi raporlanmistir: Average Displacement Error (ADE) and Final Displacement Error (FDE). Repo'daki kod, makaledeki sonuclardan daha olumlu sonuclar vermistir. print\_args kullanarak egitme surecinde kullanilan hyperparameter'lar elde edilebilir. Makalede onerildigi uzere, SGAN-20VP-20 icin, local yerine global kullanilmistir.

**SGAN-20V-20**

| Model | ADE-8  |  ADE-12 | FDE-8  | FDE-12  |
|-----|-----|---    |---    |---   |
| `ETH`| 0.58 |0.71 |1.13 |1.29 |
| `Hotel`| 0.36 |0.48 |0.71 |1.02|
| `Univ`| 0.33 |0.56 |0.70 |1.18 |
| `Zara1`| 0.21 |0.34 |0.42 |0.69|
| `Zara2`| 0.21 |0.31|0.42 |0.64|

**SGAN-20VP-20**

| Model | ADE-8  |  ADE-12 | FDE-8  | FDE-12  |
|-----|-----|---    |---    |---   |
| `ETH`| 0.57 |0.77|1.14 |1.39|
| `Hotel`| 0.38 |0.43|0.73 |0.88|
| `Univ`| 0.42 |0.75|0.79 |1.50|
| `Zara1`| 0.22 |0.34|0.43 |0.68|
| `Zara2`| 0.24 |0.36|0.48 |0.73|


#### 4. Modelleri Calistirmak Icin
`evaluate_model.py` script'i ile herhangi bir veri seti uzerinde pretrained modelleri calistirmak mumkundur. Tum veri setleri icin, karsilastirma tablosundaki SGAN-20V-20 konfigurasyonundaki sonuclari elde etme adina asagidaki script calistirilir. 

```bash
python evaluate_model.py --model_path models/sgan-models
```
#### 5. Yeni Modellerin Egitilmesi
Asagidaki adimlar takip edilerek egitme sureci gerceklestirilebilir. 
##### 5.1. Veri Setlerinin Edinilmesi 
***bash scripts/download_data.sh*** calistirilarak veri setleri indirilir.
Script calistirildiginda `datasets/<dataset_name>` klasoru altinda veri setleri `train/`, `val/`, `test/` ayrimi yapilmis halde bulunacaktir. Kullanilacak veri setleri, Dunya koordinatlarina -metrik sisteme- uygun olacak sekilde pre-process islemi gecirmistir. Calisilan algoritma kapsaminda 5 adet veri seti desteklenmektedir: ETH, ZARA1, ZARA2, HOTEL ve UNIV. 
Egitme surecinde kullanilan leave-one-out yaklasimi uyarinca, her bir veri seti, 4 set training ve 1 set test icin kullanilmaktadir. Trajectory 8 zaman adimi boyunca gozlenir ve 8 ila 12 zaman adimi icin tahmin uretilir. 1 zaman adiminin 0.4 sn oldugu kabul edilmis; 3.2 sn gozlemlenmis ve 3.2 sn - 4.8 sn'lik tahminler olusturulmustur. 

##### 5.2. Egitim Sureci
Veri seti indirildikten ve hazirlandiktan sonra ***python train*.*py*** scripti ile yeni model egitilmesi saglanir.
Bu script uzerinde yapilacak degisiklikler ile calisilacak veri seti degistirilebilir. Script calistirildiktan sonra periyodik bir bicimde checkpoint dosyalari olusturulur (`checkpoint_with_model.pt` altinda model agirliklari ve optimizer'in state'i barinir,  `checkpoint_no_model.pt` altinda ise model agirliklarindan vb arinmis gorece sig bir checkpoint tutulur).
Egitme surecine dair model mimarisini yapilandiracak, hyperparameter'larin ve I/O islemlerinin ayarlanmasinda kullanilacak bircok command-line flag mevcuttur.
###### 5.2.1. Optimizasyon
- `--batch_size`: How many sequences to use in each minibatch during training. Varsayilan deger, 64.
- `--num_iterations`: Egitim surecinde kullanilacak iterasyon sayisi. Varsayilan deger, 10000.
- `--num_epochs`: Egitim surecinde kullanilacak iterasyon sayisi. Lakin bu defa epoch'lara bolunur. Her bir epoch icerisinde belirli sayida iterasyon gerceklestirilir. Varsayilan deger, 200.

###### 5.2.2. Veri Seti Ayarlari

- `--dataset_name`: Egitme surecinde kullanilacak veri seti. Varsayilan set, `zara1`.
- `--delim`: Veri setinin dosyalarinda kullanilacak ayrac. Varsayilan deger, `' '`.
- `--obs_len`: Girdi trajectory'lerde bulunacak zaman adimi miktari. Varsayilan deger, 8. 
- `--pred_len`: Tahmin ciktilarinda bulunacak zaman adimi miktari. Varsayilan deger, 8.
- `--loader_num_workers`: Veri yuklemede kullanilan arkaplan thread'lerinin sayisi. Varsayilan deger, 4.
- `--skip`: Veri setini olustururken atlanacak frame miktari. Varsayilan deger, 1.

###### 5.2.3. Model Ayarlari
Bahsi gecen model 3 ayri bilesenden meydana gelmektedir.
-   Generator
-   Pooling Module
-   Discriminator

Asagidaki hyperparameter'lar hem generator hem de discriminator kismi icin gecerlilerdir.
- `--embedding_dim`: input (x, y) koordinatlari icin embedding layer'in boyutu. Varsayilan deger, 64.
- `--num_layers`: LSTM hucresindeki katmanlarin sayisi. Yalnizca num_layers = 1 destekleniyor. Multi-layer RNN yaklasimi saglanmamis durumda.
- `--dropout`: Dropout'u belirleyen Float deger. Varsayilan deger, 0 (no dropout).
- `--batch_norm`: Batch Normalization yapilip yapilmadigina dair bir boolean flag. Varsayilan deger, False.
- `--mlp_dim`: MLP boyutlarini belirleyen deger. Varsayilan deger, 1024.

**Generator Ayarlari**: Generator yapisi verilen sekans icerisindeki tum trajectory'leri edinir ve simultane bir sekilde sosyal olarak kabuledilebilir trajectory'leri tahmin eder. Asagidaki flag'ler generator mimarisine ozgu hyperparameter'lari kontrol etmektedir:
- `--encoder_h_dim_g`: Encoder'da bulunan hidden layer'in boyutunu belirler. Varsayilan deger, 64.
- `--decoder_h_dim_g`: Decoder'da bulunan hidden layer'in boyutunu belirler. Varsayilan deger, 64.
- `--noise_dim`: Decoder'in input'una eklenen gurultunun boyutlarini tutan tuple. Varsayilan deger, None.
- `--noise_type`: Eklenecek gurultunun tipi. Iki opsiyon destekleniyor: *uniform* ve *gaussian*. Varsayilan deger, *gaussian*.
- `--noise_mix_type`: Eklenen gurultu tum yayalar icin ayni olabilecegi gibi her sahsa ayri gurultu yaklasimi da mumkun. Desteklenen opsiyonlar: *global* ve *ped*. Varsayilan deger, *ped*.
- `--clipping_threshold_g`: Gradient'lerin kirpilacagi esigi belirten float deger. Varsayilan deger, 0.
- `--g_learning_rate`: Generator icin ogrenme katsayisi. Varsilan deger, 5e-4. Kullanilan optimizer `Adam`.
- `--g_steps`: Generator uzerinde her bir iterasyonda gerceklesecek g_steps miktarinda forward backward pass gerceklesir. Varsayilan deger, 1.

**Pooling Ayarlari**: Jenerik tasarimlari sayesinde herhangi bir pooling turu desteklenebilir. Iki adet pooling modulu hali hazirda desteklenmekte: 
    
- Social Pooling 
- Pool Net. 

Asagidaki flag'ler pooling modulune ozgu hyperparameter'lari kontrol etmektedir:
- `--pooling_type`: Kullanilacak pooling modulunun tipi. "pool_net" ve "spool" desteklenmekte. Varsayilan ayar, "pool_net".
- `--pool_every_timestep`: Hidden state'ler her bir zaman adiminda veya her bir obs_len adim atildiktan sonra toplanabilir. Varsayilan deger, False.
- `--bottleneck_dim`: Pool Net adina Pool'lanan vektorun cikti boyutu. Varsayilan deger, 1024.
- `--neighborhood_size`: Social Pooling icin komsuluk bolgesi boyutu. S-LSTM makalesi bu konuyu acikliyor. Varsayilan deger, 2.
- `--grid_size`: Komsuluk bolgesi grid_size x grid_size grid'lere boluntmustur. Varsayilan deger, 8.

**Discriminator Ayarlari**: Asagidaki flag'ler discriminator mimarisine ozgu hyperparameter'lari kontrol etmektedir:
- `--d_type`: Discriminator ya her bir trajectory'yi bagimsiz bir sekilde ele alir ya da generator mantigina yakin bir sekilde tum trajectory'lere dair bilgi pool'lanir ve ilgili konfigurasyonun gercek mi sahte mi olduguna dair karar verilir. Birincisi *local*, ikincisi *global* ayarlardir. Varsayilan ayar, "local".
- `--encoder_h_dim_d`:  Encoder'da bulunan hidden layer'in boyutunu belirler. Varsayilan deger, 64.
- `--d_learning_rate`: Discriminator icin ogrenme katsayisi. Varsayilan deger, 5e-4. Kullanilan optimizer `Adam`.
- `--d_steps`: Discriminator uzerinde her bir iterasyonda gerceklesecek d_steps miktarinda forward backward pass gerceklesir. Varsayilan deger, 2.
- `--clipping_threshold_d`: Gradient’lerin kirpilacagi esigi belirten float deger. Varsayilan deger, 0.

###### 5.2.4. Output Ayarlari
Asagidaki flag'ler training script'inin ciktilarini kontrol etmektedir: 
- `--output_dir`: Checkpoint'lerin kaydedilecegi dizin. Varsayilan ayar, mevcut dizin.
- `--print_every`: Egitme surecindeki loss'larin yazdirilma ve kayit edilme araligi. Varsayilan deger, 10 (her 10 iterasyonun ardindan).
- `--timing`: Eger bu flag 1'e esitlenirse zaman olculur ve print edilir.
- `--checkpoint_every`: Her `N` iterasyonda bir kaydetme islemi gerceklestirilir. Varsayilan deger, 100. Her bir checkpoint, training loss'larini, ADE ve FDE errorlerini, generator/discriminator ve optimizer state'lerini, ve eger egitme sureci sekteye ugrarsa kaldigi yerden devam edebilmesi icin gereken diger bilgiler tutulur.
- `--checkpoint_name`: Kaydedilen checkpoint'ler icin base dosya ismi; varsayilan deger 'checkpoint'.
- `--restore_from_checkpoint`: Rutin davranis sifirdan egitmek ve eger mevcutsa output checkpoint path'inin uzerine yazmaktir. Eger bu flag 1'e esitse, bulunan checkpoint dosyasi ile egitme surecine kalinan yerden devam edilir. 
- `--checkpoint_start_from`: Eger bu flag aktifse verilen checkpoint noktasinda egitme surecine devam edilir. Bu flag `--restore_from_checkpoint` flag'ine agir basar, eger her ikisi de aktifse. 
- `--num_samples_check`: Training veri seti uzerinde  metrikler hesaplanirken, degerlendirmek icin istenilen ornek sayisini limitleyebiliriz. 
