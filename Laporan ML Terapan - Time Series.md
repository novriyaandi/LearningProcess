# Laporan Proyek Machine Learning - Novriandi (M200X0403)

## Domain Proyek

Pemanasan global adalah peningkatan suhu rata-rata bumi yang teramati. Penyebab utama dari fenomena ini adalah pelepasan gas rumah kaca dengan pembakaran bahan bakar fosil, pembersihan lahan, pertanian, antara lain, mengarah pada peningkatan apa yang disebut efek rumah kaca.

Sebuah pendekatan untuk menghadapi masalah penting ini adalah analisis deret waktu. Dalam hal ini, teknik yang berbeda dapat diterapkan untuk mengevaluasi dinamika pemanasan global. Analisis semacam ini memungkinkan seseorang untuk membuat prediksi yang lebih baik meningkatkan pemahaman kita tentang fenomena tersebut.


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

Tiap - tiap variabel terdiri dari sekitar -+3000 value, beberapa memiliki value null/kosong.

## Data Preparation
Dalam membuat model ini, saya memutuskan untuk hanya menggunakan data dengan label LandAverageTemperature karena suhu rata-rata permukaan bumi didarat yang ingin saya coba buat modelnya. Karena setelah melihat data null melalui sintaks .info() hanya label LandAverageTemperature hanya memiliki 12 data null/kosong.

**Rubrik/Kriteria Tambahan (Opsional)**: 
- Melakukan pengecekan data null dengan function .info()
- Mengetahui data dengan label LandAverageTemperature memiliki 12 data null/kosong.
- Dengan metode Interpolasi Spline mengisi data kosong pada label LandAverageTemperature dan memasukkannya ke varibel baru yaitu "Interpolation Spline" 
- Memasukkan value pada label "dt" yang berisi tanggal dan "Interpolation Spline" ke variabel baru yaitu "dates" dan "temp"
- Melakukan visualisasi data menggunakan variabel "dates" dan "temp" menggunakan py.plot


## Modeling
Data yang ingin saya olah merupakan data bertipe time series musiman. Pada time series pola ini terdapat pola yang berulang pada interval tertentu. Untuk arsitektur model menggunakan 2 buah layer LSTM. Lalu pada optimizer, menggunakan parameter learning rate dan momentum. Loss function yang digunakan adalah Huber yang merupakan salah satu loss function yang umum digunakan pada kasus time series. 


## Evaluation
Metrik yang digunakan untuk mengevaluasi model adalah MAE. Mean Absolute Error (MAE) dihitung dengan mengambil mean dari perbedaan absolut antara nilai aktual (juga disebut y) dan nilai prediksi (y_hat).

Sederhana, dan itulah keuntungan utamanya. Mudah dipahami) dan untuk dihitung. Metrik ini direkomendasikan untuk menilai akurasi pada satu seri time series seperti data yang saya gunakan kali ini. 

Pada model yang saya terapkan saya menggunakan fungsi Callback untuk menghentikan training pada model jika MAE sudah mencapai <10% skala data. Semakin kecil MAE dibandingkan skala data semakin baik model yang kita miliki. 


**---Ini adalah bagian akhir laporan---**


