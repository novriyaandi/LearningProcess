# Laporan Proyek Machine Learning - Novriandi (M200X0403)

## Domain Proyek

Pemanasan global adalah peningkatan suhu rata-rata bumi yang teramati. Penyebab utama dari fenomena ini adalah pelepasan gas rumah kaca dengan pembakaran bahan bakar fosil, pembersihan lahan, pertanian, antara lain, mengarah pada peningkatan apa yang disebut efek rumah kaca. [1]

Sebuah pendekatan untuk menghadapi masalah penting ini adalah analisis deret waktu. Dalam hal ini, teknik yang berbeda dapat diterapkan untuk mengevaluasi dinamika pemanasan global. Analisis semacam ini memungkinkan seseorang untuk membuat prediksi yang lebih baik meningkatkan pemahaman kita tentang fenomena tersebut.[1]


## Business Understanding

Pemanasan global telah menjadi topik hangat dalam beberapa tahun ini. Perdebatan tentang pemanasan global telah terjadi di seluruh dunia. Visualisasi trend suhu adalah salah satu cara untuk mengetahui apakah pemanasan global benar adanya atau hanya isu isapan jempol belaka.


### Problem Statements

Berdasarkan kondisi yang telah diuraikan sebelumnya, perusahaan akan mengembangkan sebuah sistem prediksi temperatur suhu untuk menjawab permasalahan berikut.:
- Apakah suhu rata - rata permukaan bumi berubah sejak tahun 1750 ?
- Bagaimana cara menentukan membuat model forecasting untuk perubahan rata - rata suhu permukaan bumi ?

### Goals

Menjelaskan tujuan dari pernyataan masalah:
- Mengetahui perubahan suhu rata - rata permukaan bumi ?
- Menentukan membuat model forecasting untuk perubahan rata - rata suhu permukaan bumi



## Data Understanding
Data berasal dari kompilasi baru yang dikumpulkan oleh Berkeley Earth, yang berafiliasi dengan Lawrence Berkeley National Laboratory. Studi Suhu Permukaan Bumi Berkeley menggabungkan 1,6 miliar laporan suhu dari 16 arsip yang sudah ada sebelumnya. Ini dikemas dengan baik dan memungkinkan untuk diiris menjadi himpunan bagian yang menarik (misalnya berdasarkan negara). Mereka mempublikasikan data sumber dan kode untuk transformasi yang mereka terapkan. Mereka juga menggunakan metode yang memungkinkan pengamatan cuaca dari deret waktu yang lebih pendek untuk dimasukkan, yang berarti lebih sedikit pengamatan yang perlu dibuang. Source: [Climate Change: Earth Surface Temperature Data](https://www.kaggle.com/datasets/berkeleyearth/climate-change-earth-surface-temperature-data?datasetId=29&sortBy=voteCount).



### Variabel-variabel pada Climate Change: Earth Surface Temperature dataset adalah sebagai berikut:
- Date: dimulai pada tahun 1750 untuk suhu rata-rata daratan dan tahun 1850 untuk suhu maksimum dan minimum daratan serta suhu lautan dan daratan global
- LandAverageTemperature: suhu daratan rata-rata global dalam celsius
- LandMaxTemperature: suhu tanah maksimum rata-rata global dalam celsius
- LandMinTemperature: suhu tanah minimum rata-rata global dalam celsius
- LandAndOceanAverageTemperature: suhu daratan dan lautan rata-rata global dalam celsius

![1](https://user-images.githubusercontent.com/110442025/191176116-d8b580d7-36c8-4cab-ab00-cd9d37d5f85a.png)

Tiap - tiap variabel terdiri dari sekitar -+3000 value, beberapa memiliki value null/kosong.

## Data Preparation
Dalam membuat model ini, saya memutuskan untuk hanya menggunakan data dengan label LandAverageTemperature karena suhu rata-rata permukaan bumi didarat yang ingin saya coba buat modelnya. Karena setelah melihat data null melalui sintaks .info() hanya label LandAverageTemperature hanya memiliki 12 data null/kosong.

**Rubrik/Kriteria Tambahan (Opsional)**: 
- Melakukan pengecekan data null dengan function .info() ![2](https://user-images.githubusercontent.com/110442025/191181488-a121796f-12e3-44a2-8c08-650157e96017.png)

- Mengetahui data dengan label LandAverageTemperature memiliki 12 data null/kosong. ![3](https://user-images.githubusercontent.com/110442025/191181707-5dfc0ca2-f4b3-4cc9-83fc-6d1796f88ef6.png)

- Dengan metode Interpolasi Spline mengisi data kosong pada label LandAverageTemperature dan memasukkannya ke varibel baru yaitu "Interpolation Spline" ![4](https://user-images.githubusercontent.com/110442025/191182055-f2a9f968-582f-402d-951f-12b0d36c0ed9.png)

- Memasukkan value pada label "dt" yang berisi tanggal dan "Interpolation Spline" ke variabel baru yaitu "dates" dan "temp" ![5](https://user-images.githubusercontent.com/110442025/191182121-e0de5494-a7d4-4ed5-979b-3c2c54de80a4.png)

- Melakukan visualisasi data menggunakan variabel "dates" dan "temp" menggunakan py.plot ![6](https://user-images.githubusercontent.com/110442025/191182359-fe73a69f-4d5d-482b-bacf-ba4fa33f336d.png)



## Modeling
Data yang ingin saya olah merupakan data bertipe time series musiman. Pada time series pola ini terdapat pola yang berulang pada interval tertentu. Untuk arsitektur model menggunakan 2 buah layer LSTM. Lalu pada optimizer, menggunakan parameter learning rate dan momentum. Loss function yang digunakan adalah Huber yang merupakan salah satu *loss function* yang umum digunakan pada kasus time series. 

![7](https://user-images.githubusercontent.com/110442025/191183711-c60710a2-ebf0-4959-9a66-eabeaf0e7c68.png)

## Evaluation
Metrik yang digunakan untuk mengevaluasi model adalah MAE. *Mean Absolute Error* (MAE) dihitung dengan mengambil mean dari perbedaan absolut antara nilai aktual (juga disebut y) dan nilai prediksi (y_hat). Metrik mean absolute error atau MAE digunakan untuk menghitung tingkat akurasi atau besar error hasil prediksi rating dari sistem terhadap rating sebenarnya yang user berikan terhadap suatu item. MAE diperoleh dengan menghitung error absolut dari N pasang rating asli dan prediksi, kemudian menghitung rata-ratanya. [2]

Sederhana, dan itulah keuntungan utamanya. Mudah dipahami dan untuk dihitung. Metrik ini direkomendasikan untuk menilai akurasi pada satu seri time series seperti data yang saya gunakan kali ini. 

Pada model yang saya terapkan saya menggunakan fungsi Callback untuk menghentikan training pada model jika MAE sudah mencapai <10% skala data. Semakin kecil MAE dibandingkan skala data semakin baik model yang kita miliki. 

![8](https://user-images.githubusercontent.com/110442025/191185242-ac0c0ea6-d1ce-47c8-8d7e-8d72c7c535f1.png)

Hasil dari training menunjukan model dapat mencapai MAE yang ingin dicapai yaitu kurang dari 10% skala data, hal ini menunjukkan training berhasil dan menghasilkan model yang baik.

![9](https://user-images.githubusercontent.com/110442025/191186764-60eecd6f-ef51-440e-b384-d47775074302.png)

![10](https://user-images.githubusercontent.com/110442025/191186794-e9c5647b-6b6c-456a-a23b-620596ecbcbb.png)

Berikut hasil dari visualisasi menggunakan py.plot untuk model dengan metrik "MAE" dan "loss"

![11](https://user-images.githubusercontent.com/110442025/191187028-87cae114-2b31-460f-b506-54fe1832f38e.png)

![12](https://user-images.githubusercontent.com/110442025/191187042-24dac405-0cde-4991-add3-631b068251ec.png)




Referensi :

[1] F. M. Viola, S. L. D. Paiva, and M. A. Savi, “Analysis of the global warming dynamics from temperature time series,” Ecol. Modell., vol. 221, no. 16, pp. 1964–1978, 2010, doi: 10.1016/j.ecolmodel.2010.05.001.

[2] M. Desmala, “Related Papers,” Over Rim, pp. 191–199, 2017, doi: 10.2307/j.ctt46nrzt.12.


**---Ini adalah bagian akhir laporan---**


