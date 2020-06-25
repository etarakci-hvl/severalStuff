# SocialGAN Uygulamasında Kullanılan Veri Setleri

Yayaların davranışlarının tahmininde kullanılan ETH, Hotel , Zara ve Univ veri setlerinin asıl formatlarına dair derleme aşağıdadır. Bahsi geçen format, video formatıdır. Fakat, SocialGAN uygulamasında, time step, x ve y konum bilgileri kısmi bir biçimde ***txt*** formatında paylaşılmıştır.

|   Sample     | Pedestrians | Coordinates | Resolution | Framerate| Density | Avg. Path Length |Description | Source | Reference |
|:----------:  |:----------: |:-----------:|:----------:|:--------:|:-------:|:----------------:|:----------:|:------:|:---------:|
|   BIWI Hotel |    389      |   world     |   720 x 576|    2.5   |     5.6 |    16.82   	|Bird's eye view. Outdoor at building entrance.|[ETHZ Vision Group*](https://vision.ee.ethz.ch/en/datasets/)|[1*](https://ieeexplore.ieee.org/abstract/document/5459260)|
|	BIWI ETH |360|	world|	640 x 480|	2.5|	6.27|	15.26|	Bird's eye view. Outdoor outside a hotel.|* | *|
|Crowds UCY: Zara|    204    |   world| 720 x 576   |      2.5   |     9.24     |   47.66    |Bird's eye view in various outdoor situations, from shopping streets to student pedestrians.*|[UCY Computer Graphics Lab*](https://graphics.cs.ucy.ac.cy/research/downloads/crowd-data)|[3*](https://onlinelibrary.wiley.com/doi/abs/10.1111/j.1467-8659.2007.01089.x)|
|Crowds UCY: University Students|    415  |   world| 720 x 576   |      2.5   |     49.13     |  52.56    | * | *| * |

# SocialLSTM Uygulamasında Kullanılan Veri Setleri

Yayaların davranışlarının tahmininde kullanılan BIWI, MOT, Crowds ve Stanford veri setlerinin asıl formatlarına dair derleme aşağıdadır. Bahsi geçen format, video formatıdır. Fakat, SocialLSTM uygulamasında, time step, x ve y konum bilgileri kısmi bir biçimde ***txt*** formatında paylaşılmıştır.

|   Sample     | Pedestrians | Coordinates | Resolution | Framerate| Density | Avg. Path Length |Description | Source | Reference |
|:----------:  |:----------: |:-----------:|:----------:|:--------:|:-------:|:----------------:|:----------:|:------:|:---------:|
|   BIWI Hotel |    389      |   world     |   720 x 576|    2.5   |     5.6 |    16.82   	|Bird's eye view. Outdoor at building entrance.|[ETHZ Vision Group*](https://vision.ee.ethz.ch/en/datasets/)|[1*](https://ieeexplore.ieee.org/abstract/document/5459260)|
|	BIWI ETH |360|	world|	640 x 480|	2.5|	6.27|	15.26|	Bird's eye view. Outdoor outside a hotel.|* | *|
|  MOT PETS  |   19 	     |    world    |   768 x 576|    2.5   |   4.15  |   123.16         | Multisensor sequences containing different crowd activities. Outdoors on a path.|[Reading University Vision Group](http://www.cvg.reading.ac.uk/PETS2009/data.html)|[2](https://ieeexplore.ieee.org/abstract/document/5399556)|
|Crowds UCY: Zara|    204    |   world| 720 x 576   |      2.5   |     9.24     |   47.66    |Bird's eye view in various outdoor situations, from shopping streets to student pedestrians.*|[UCY Computer Graphics Lab*](https://graphics.cs.ucy.ac.cy/research/downloads/crowd-data)|[3*](https://onlinelibrary.wiley.com/doi/abs/10.1111/j.1467-8659.2007.01089.x)|
|Crowds UCY: University Students|    415  |   world| 720 x 576   |      2.5   |     49.13     |  52.56    | * | *| * |
|Crowds UCY: Arxiepiskopi|    24    |   world| 720 x 576   |      2.5   |     17.83     |   57.96    | * | *| * |
|   Stanford  Drone Dataset |    3297   |  world     |  595 x 326*   |    2.5      |     *     |     *     | Bird's eye's view. Videos of various types of agents (pedestrians, bicyclists, skateboarders, cars, buses, and golf carts) that navigate in 8 different outdoor scenes. Agents are labeled with boxes.* |[Stanford Computational Vision and Geometry Lab](https://cvgl.stanford.edu/projects/uav_data/)|[4*](https://link.springer.com/chapter/10.1007/978-3-319-46484-8_33)|
| \|--->  Stanford Drone Bookstore |    587   |  world     |  *   |    2.5      |    10.41     |     83.66     | * | *| * |
| \|--->  Stanford Drone Coupa |    226   |  world     |  *   |    2.5      |    8.75     |     115.35     | *  | *| * |
| \|--->  Stanford Drone Death Circle |    879   |  world     |  *   |    2.5      |    13.72     |     51.72     | * | *| * |
| \|--->  Stanford Drone Gates |    459   |  world     |  *   |    2.5      |    6.85    |     53.25     | * | *| * |
| \|--->  Stanford Drone Hyang |    370   |  world     |  *   |    2.5      |    7.95    |    76.86     | * | *| * |
| \|--->  Stanford Drone Nexus |    663   |  world     |  *   |    2.5      |    8.8    |    68.84     | * | *| * |


# TrafficPredict Uygulamasında Kullanılan Veri Seti

Heterojen trafik varlıklarının hareketlerinin tahmininde kullanılan Apolloscape veri setinin asıl formatına dair derleme aşağıdadır. TrafficPredict uygulamasında, veri ***txt***  formatında paylaşılmıştır.

|   Dataset     |  Duration | Frames | Number of Objects | Source | Reference |
|:----------:   |:----------:      |:-----------:             |:----------:   |:------:|:--------:|
|Apolloscape:Trajectory	| 155		|	93000	| <p>Pedestrian: 16200<br> Bicycle: 5500 <br> Vehicle: 60100 </p>   |[Peking University \| Baidu](http://apolloscape.auto/trajectory.html) and [Dataset API](https://github.com/sibozhang/dataset-api)	|[6](https://arxiv.org/pdf/1803.06184.pdf) and [7](https://arxiv.org/pdf/1811.02146.pdf)|

# SC-GLSTM Uygulamasında Kullanılan Veri Setleri

Heterojen trafik varlıklarının hareketlerinin tahmininde kullanılan Argoverse, Apolloscape ve Lyft veri setlerinin asıl formatlarına dair derleme aşağıdadır. SC-GLSTM uygulamasında, veri ***npy (numpy array dosyası)*** ve ***pkl (pickle dosyası)***  formatlarında paylaşılmıştır. 


|   Dataset     | Segment Duration | Total Number of Segments | Total Tracked Objects | Total Time | Source | Reference |
|:----------:   |:----------:      |:-----------:             |:----------:           |:--------:  |:------:|:--------:|
|Argoverse: 3D Tracking| 15-30 Sec.|          113     |           11052 |    -        |[Argo AI](https://www.argoverse.org/data.html)|[5](https://arxiv.org/pdf/1911.02620.pdf)|
|Argoverse Motion Forecasting      |    5 Sec.       |       324557	 |     -       | 320 Hours           |* | *|


|   Dataset     |  Duration | Frames | Number of Objects | Source | Reference |
|:----------:   |:----------:      |:-----------:             |:----------:   |:------:|:--------:|
|Apolloscape: Trajectory	| 155		|	93000	| <p>Pedestrian: 16200<br> Bicycle: 5500 <br> Vehicle: 60100 </p>   |[Peking University \| Baidu](http://apolloscape.auto/trajectory.html) and [Dataset API](https://github.com/sibozhang/dataset-api)	|[6](https://arxiv.org/pdf/1803.06184.pdf) and [7](https://arxiv.org/pdf/1811.02146.pdf)|


|   Dataset     | Segment Duration | Number of Scenes | Number of Samples | Source | Reference |
|:----------:   |:----------:      |:-----------:             |:----------:    |:------:|:--------:|
|Lyft	|	25-45 Sec.	   | 180		      |22680 		      |[Lyft Level 5](https://level5.lyft.com/dataset/) and [DevKit](https://github.com/lyft/nuscenes-devkit) | [8](https://www.nuscenes.org/data-format)|


Lyft veri seti, NuScenes[8](https://www.nuscenes.org/data-format) veri setinin formatına uygun bir şekilde oluşturulmuştur.
Ek olarak, [SC-GLSTM makalesinde](https://obj.umiacs.umd.edu/gamma-umd-website-imgs/pdfs/autonomousdriving/spectralcows_full.pdf) paylaşılan yoğunluk değerleri aşağıdadır. 

|   Dataset  | Batch       | Avg. Density |
|:----------:|:-----------:|:------------:|
| Argoverse  |	<p>Train<br> Val. <br> Test</p>| <p>0.75<br>0.67<br>1.67</p>|
| Apolloscape|	<p>Train<br> Val. <br> Test</p>| <p>3.49<br>3.50<br>2.56</p>|
| Lyft 	     |	<p>Train<br> Val. <br> Test</p>| <p>0.80<br>0.83<br>0.84</p>|
	
Her bir veri seti için SC-GLSTM uygulamasındaki input/output boyutları aşağıdaki gibidir.

|   Dataset  | I/O Lengths|
|:----------:|:-----------:|
| Argoverse  |	<p>Input Length: 20 <br> Output Length: 30</p>|
| Apolloscape|	<p>Input Length: 6 <br> Output Length: 10</p>|
| Lyft 	     |	<p>Input Length: 20 <br> Output Length: 30</p>|


**ET**

