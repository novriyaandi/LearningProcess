# Laporan Proyek Machine Learning - Novriandi (M200X0403)

## Domain Proyek

Pada masa sekarang ini film telah menjadi salah satu hiburan favorit utama masyarakat. Jumlah film pertahun terhitung mencapai ribuan. Hal ini membuat suatu keadaan dimana kita kesulitan dalam mencari informasi yang sesuai dengan kriteria penggemar film. Kemungkinan film yang samasekali tidak terpikirkan olehnya namun ternyata menarik untuk dilihat dan sesuai dengan seleranya. Salah satu solusi dari permasalahan ini adalah sistem rekomendasi yang memanfaatkan opini atau *rating* orang lain terhadap suatu film.[1]


## Business Understanding

Sistem rekomendasi adalah sistem yang membantu pengguna dalam mengatasi informasi yang meluap dengan memberikan rekomendasi spesifik bagi pengguna dan diharapkan rekomendasi tersebut bisa memenuhi keinginan dan kebutuhan pengguna. Namun pada perkembangannya, diperlukan suatu model yang dapat memberikan nilai lebih kepada pelanggan yaitu berupa rekomendasi yang dapat memberikan informasi mengenai produk yang dianggap sesuai dengan keinginan pelanggan. Karena itu diperlukan model rekomendasi yang tepat agar rekomendasi yang diberikan sistem sesuai dengan keinginan pelanggan, serta mempermudah pelanggan mengambil keputusan yang tepat dalam menentukan produk yang akan dibelinya.[2]



### Problem Statements

Berdasarkan kondisi yang telah diuraikan sebelumnya, perusahaan akan mengembangkan sebuah sistem rekomendasi film untuk menjawab permasalahan berikut.:
- Bagaimana cara membuat model *collaborative filtering* untuk sistem rekomendasi film ?

### Goals

Menjelaskan tujuan dari pernyataan masalah:
- Menganalisa pembuatan model *collaborative filtering* untuk sistem rekomendasi film.



## Data Understanding
File-file ini berisi metadata untuk semua 45.000 film yang terdaftar di Dataset Full MovieLens. Dataset terdiri dari film yang dirilis pada atau sebelum Juli 2017. Poin data termasuk pemeran, kru, kata kunci plot, anggaran, pendapatan, poster, tanggal rilis, bahasa, perusahaan produksi, negara, penghitungan suara TMDB, dan rata-rata suara. Dataset ini juga memiliki file yang berisi 26 juta peringkat dari 270.000 pengguna untuk semua 45.000 film. Peringkat berada pada skala 1-5 dan telah diperoleh dari situs web resmi GroupLens. Source: [The Movies Dataset](https://www.kaggle.com/datasets/rounakbanik/the-movies-dataset).



### Variabel-variabel pada The Movies Dataset adalah sebagai berikut:
- userID: dimulai pada tahun 1750 untuk suhu rata-rata daratan dan tahun 1850 untuk suhu maksimum dan minimum daratan serta suhu lautan dan daratan global
- movieID: suhu daratan rata-rata global dalam celsius
- rating: suhu tanah maksimum rata-rata global dalam celsius
- movies_metadata.csv: File utama metadata movies. Mengandung fitur poster, latar belakang, anggaran, pendapatan, tanggal rilis, bahasa, negara dan perusaan produksi. Namun variabel yang kita gunakan hanya movie_id dan title_id
- ratings_small.csv: The subset yang terdiri dari 100,000 *ratings* dari 700 pengguna pada 9,000 film.



## Data Preparation
Dalam membuat model ini, saya memutuskan untuk menggunakan data dengan label movies_metadata dan ratings_small karena dari file itu saya sudah bisa membuat model collaborative filtering dengan variabel userID, rating, movieID, title_id. 


**Rubrik/Kriteria Tambahan (Opsional)**: 

- Melakukan drop column variabel timestamp karena tidak diperlukan

- Melakukan pengecekan data null

- Mengambil id dan title movies yang mendapatkan votes minimal 25

- Mereset index

- Membuat variabel trainset menggunakan method build_full_trainset()

- Menginisiasi model dan melakukan validasi silang

- Mendeklarasikan opsi similaritas dan menggunakan algoritma KNN untuk menemukan kesamaan user

- Melatih algoritma dalam trainset dan memprediksi rating untuk testset



## Modeling
Data yang saya olah merupakan dataset film yang telah diberi rating oleh user. Dalam metode rekomendasi collaborative filtering, saya akan menghitung kesamaan antara pengguna dan akan mengambil pengguna yang paling mirip menggunakan algoritma (KNN) dan akan merekomendasikan film yang disukai satu pengguna ke pengguna lain dan sebaliknya. Membuat matriks interaksi item pengguna, mengekstrak id produk yang belum berinteraksi dengan user_id, melakukan looping setiap id produk yang belum berinteraksi dengan user_id, memprediksi peringkat untuk id produk yang tidak berinteraksi oleh pengguna, menyortir peringkat yang diprediksi dalam urutan menurun, mengembalikan produk peringkat teratas dan prediksi tertinggi untuk pengguna ini. 

Top-N recommendation

    Tabel.1 Top-N Recommendation
| Movies Name                  | Rating |
|------------------------------|--------|
| ('The Wizard')               | 5      |
| ('Rio Bravo')                | 5      |
| ('The Celebration')          | 5      |
| ('Spiderman 3')              | 5      |
| ('A Streetcar Named Desire') | 5      |
| ('Gentlemen Prefer Blondes') | 5      |
| ('The Evil Dead')            | 5      |
| ('JFK')                      | 5      |
| ('Strangers on a Train')     | 5      |
| ("Singin' in the Rain")      | 5      |

Melakukan Training proses

    Tabel.2 Model Summary
| Model: "model"                                                                                     |               |         |                            |
|----------------------------------------------------------------------------------------------------|---------------|---------|----------------------------|
| __________________________________________________________________________________________________ |               |         |                            |
| Layer (type)                                                                                       | Output Shape  | Param # | Connected to               |
| ================================================================================================== |               |         |                            |
| user_id (InputLayer)                                                                               | [(None, 1)]   | 0       | []                         |
| movie_id (InputLayer)                                                                              | [(None, 1)]   | 0       | []                         |
| User_Embeddings (Embedding)                                                                        | (None, 1, 50) | 33600   | ['user_id[0][0]']          |
| Movie_Embeddings (Embedding)                                                                       | (None, 1, 50) | 7008750 | ['movie_id[0][0]']         |
| reshape (Reshape)                                                                                  | (None, 50)    | 0       | ['User_Embeddings[0][0]']  |
| reshape_1 (Reshape)                                                                                | (None, 50)    | 0       | ['Movie_Embeddings[0][0]'] |
| dot (Dot)                                                                                          | (None, 1)     | 0       | ['reshape[0][0]',          |
| 'reshape_1[0][0]']                                                                                 |               |         |                            |
| dense (Dense)                                                                                      | (None, 1)     | 2       | ['dot[0][0]']              |
| ================================================================================================== |               |         |                            |
| Total params: 7,042,352                                                                            |               |         |                            |
| Trainable params: 7,042,352                                                                        |               |         |                            |
| Non-trainable params: 0                                                                            |               |         |                            |
| __________________________________________________________________________________________________ |               |         |                            |


- Embedding setiap pengguna ke dalam vektor laten
- Embedding setiap film menjadi vektor laten di ruang yang sama
- Menerapkan dot product melalui 2 embeddings proses

Melakukan compile model dengan *loss function Mean Squared Error* yang menghitung rata-rata kuadrat kesalahan antara label dan prediksi, optimizer adam yang melakukan metode penurunan gradien stokastik yang didasarkan pada estimasi adaptif momen orde pertama dan orde kedua, serta menggunakan metriks *Root Mean Squared Error* yang menghitung metrik kesalahan kuadrat rata-rata akar antara y_true dan y_pred.

Memulai training model dengan batch size sebesar 64, epochs sebesar 20, validation split sebesar 20% yang berarti otomatis membagi dataset menjadi train test dan validation test. 


## Evaluation
Metrik yang digunakan untuk mengevaluasi model adalah RMSE. *Root Mean Squared Error* (RMSE) dihitung dengan mengukur perbedaan nilai dari prediksi sebuah model sebagai estimasi atas nilai yang diobservasi. Metrik *Root Mean Squared Error* atau RMSE digunakan untuk menghitung tingkat akurasi atau besar error hasil prediksi rating dari sistem terhadap rating sebenarnya yang user berikan terhadap suatu item.[2]

Berikut hasil dari Training yang didapat

    Tabel.3 Training Model
| Epoch 1/20                                                                                                                                                         |
|--------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 425/425 [==============================] - 41s 90ms/step - loss: 0.3106 - root_mean_squared_error: 0.5573 - val_loss: 0.0872 - val_root_mean_squared_error: 0.2954 |
| Epoch 2/20                                                                                                                                                         |
| 425/425 [==============================] - 36s 85ms/step - loss: 0.0420 - root_mean_squared_error: 0.2050 - val_loss: 0.0360 - val_root_mean_squared_error: 0.1897 |
| Epoch 3/20                                                                                                                                                         |
| 425/425 [==============================] - 36s 84ms/step - loss: 0.0242 - root_mean_squared_error: 0.1556 - val_loss: 0.0344 - val_root_mean_squared_error: 0.1854 |
| Epoch 4/20                                                                                                                                                         |
| 425/425 [==============================] - 34s 80ms/step - loss: 0.0163 - root_mean_squared_error: 0.1276 - val_loss: 0.0357 - val_root_mean_squared_error: 0.1890 |
| Epoch 5/20                                                                                                                                                         |
| 425/425 [==============================] - 38s 89ms/step - loss: 0.0103 - root_mean_squared_error: 0.1015 - val_loss: 0.0382 - val_root_mean_squared_error: 0.1955 |
|                                                                                                                                                                    |
|                                                                                                                                                                    |

Sederhana, dan itulah keuntungan utamanya. Mudah dipahami dan untuk dihitung. Metrik ini direkomendasikan untuk model klasifikasi seperti data yang saya gunakan kali ini. 

Pada model yang saya terapkan saya menggunakan fungsi Callback untuk menghentikan training pada model jika loss mengalami perubahan kurang dari 0.05.


Hasil dari training menunjukan model berhenti melatih data setelah epochs ke-4



Berikut hasil dari evaluasi dengan menghitung loss prediction dan membandingkan prediksi rating dengan aktual rating

Testing Loss: 0.06682105362415314

predicted rating -> 5.276638865470886
Actual Rating -> 5.0

## Kesimpulan 
Semakin kecil loss yang didapat maka semakin baik model yang didapat, lalu sebanding sedikit selisih prediksi rating dengan aktual rating maka semakin baik model. Masih diperlukan beberapa perbaikan data preparation atau hyperparameter tuning untuk membuat model lebih baik lagi 





Referensi :

[1] Halim, Arwin, et al. "Sistem Rekomendasi Film menggunakan Bisecting K-Means dan Collaborative Filtering." (2017): 37-41

[2] Jaja, Visher Laja, Bambang Susanto, and Leopoldus Ricky Sasongko. "Penerapan Metode Item-Based Collaborative Filtering Untuk Sistem Rekomendasi Data MovieLens." d'CARTESIAN: Jurnal Matematika dan Aplikasi 9.2 (2020): 78-83.


**---Ini adalah bagian akhir laporan---**


