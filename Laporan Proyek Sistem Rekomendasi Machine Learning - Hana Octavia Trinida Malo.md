# Laporan Proyek Machine Learning - Hana Octavia Trinida Malo

## Domain Proyek

Sistem rekomendasi adalah sistem yang digunakan untuk memberikan rekomendasi kepada customer berdasarkan berbagai informasi  yang bertujuan untuk untuk meningkatkan penjualan produk. Sistem  rekomendasi dapat ditemukan pada situs-situs belanja onlineseperti eBay, Alibaba, OLX, yang menjual pakaian, barang-barang elektronik, peralatan  rumah  tangga, dan yang lainnya. Sistem rekomendasi juga dapat ditemukan pada situs yang murni memberikan rekomendasi kepada penggunanya, seperti situs    MovieLens dan Internet Movie Database (IMDb) yang memberikan rekomendasi film yang akan ditontonkepada penggunanya[1]

Pembaca di perpustakaan memiliki ketertarikan membaca yang berbeda - beda, ada pembaca yang menyukai beberapa penulis buku saja sehingga hanya ingin membaca penulis yang pembaca suka. Ada juga pembaca yang membaca buku, dilihat dari seberapa disukainya buku itu oleh pembaca yang sudah pernah membaca buku sebelumnya. Semakin banyak yang menyukai buku itu, semakin tertarik pembaca tersebut untuk membaca buku itu.

Jumlah buku bacaan yang ada di perpustakaan saat ini, kurang lebih berjumlah 250an ribu buku. Banyaknya jumlah buku ini seringkali membuat pembaca kesulitan untuk memilih buku yang ingin dibaca, dan berakhir mengurungkan niat untuk membaca buku. Untuk mempermudah pembaca dalam membaca buku yang sesuai dengan keinginannya perlu dibangun sebuah sistem rekomendasi, dibangunnya sistem rekomendasi tentunya dapat menjadi solusi terbaik agar jumlah pembaca bertambah.


## Pendefinisian Bisnis
Sebuah perpustakaan memiliki berbagai informasi mengenai pembaca, daftar buku dan beserta rating yang diberikan pembaca untuk buku-buku tersebut. Perpustakaan ini ingin memiliki sebuah sistem rekomendasi yang dapat memberikan rekomendasi kepada pembaca berdasarkan penulis buku yang pernah dia baca sebelumnya dan rating pembaca.

### Masalah
Berdasarkan latar belakang yang telah diuraikan di atas, maka dapat dirumuskan masalah-masalah yang harus diselesaikan antara lain :
- Berdasarkan data mengenai buku dan pembaca, bagaimana membuat sistem rekomendasi yang dipersonalisasi dengan teknik content-based filtering?
- Dengan data rating yang dimiliki, bagaimana perpustakaan dapat merekomendasikan buku lain yang mungkin disukai oleh pengguna? 

### Tujuan
Untuk  menjawab pertanyaan tersebut, buatlah sistem rekomendasi dengan tujuan atau goals sebagai berikut:
- Menghasilkan sejumlah rekomendasi buku yang dipersonalisasi untuk pengguna dengan teknik content-based filtering.
- Menghasilkan sejumlah rekomendasi buku yang sesuai dengan preferensi pengguna dengan teknik collaborative filtering.

### Solusi
Solusi untuk mencapai tujuan diatas adalah :
- menerapkan content based filtering dalam membuat sistem rekomendasi, dimana mennggunakan nama penulis buku untuk merekomendasikan kepada pembaca
- menerapkan collaborative filtering dalam membuat sistem rekomendasi, dimana mennggunakan data rating untuk merekomendasikan kepada pembaca

## Data Understanding
Dataset yang saya gunakan merupakan dataset yang memiliki data buku, pembaca, dan rating, yang saya unduh dari link berikut : [Book Recommendation Dataset](https://www.kaggle.com/datasets/arashnic/book-recommendation-dataset) 

Dalam dataset ini terdapat 3 file yaitu books.csv, rating.csv, dan user.csv(pembaca). Berikut ini adalah jumlah data dari 3 file tersebut :

```

```

### Variabel-variabel pada Book Recommendation Dataset adalah sebagai berikut:
Variabel-variabel pada Book Recommendation Dataset adalah sebagai berikut:

books   : merupakan daftar buku-buku yang ada di perpustakaan yang berisi nomor ISBN, judul buku dan penulis buku
user    : merupakan daftar user
rating  : merupakan daftar user dan rating yang diberikan terhadap buku yang sudah di baca

Variabel books dan rating akan digunakan pada model rekomendasi yang akan dibangun.

### Exploratory Data Analysis

contohnya teknik visualisasi data beserta insight atau exploratory data analysis.
#### Unvariate variabel

![Screenshot (439)](https://user-images.githubusercontent.com/86582130/190961226-d56e4502-1df0-4edc-b3dc-47607a547b21.png)




## Data Preparation
Menerapkan dan menyebutkan teknik data preparation yang dilakukan.

Teknik yang digunakan pada notebook dan laporan harus berurutan.
=
Menjelaskan proses data preparation yang dilakukan.

Menjelaskan alasan mengapa diperlukan tahapan data preparation tersebut.

Pada Data Preparation ada beberapa tahap yang harus dilakukan
1. Membagi data train dan data test
Pembagian dataset menjadi data train dan data test ini dilakukan dengan perbandingan 8:2 sehingga pada test_size diset 0.2. Pembagian dataset ini bertujuan agar memudahkan kita dalam proses evaluasi performa model dan agar kita tidak mengotori data uji dengan informasi yang kita dapat dari data latih. 

```
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size = 0.2, random_state = 123) 
```
Berikut ini adalah jumlah keseluruhan data, data train, dan data set
```
Jumlah Seluruh Dataset: 35684
Jumlah data train: 28547
Jumlah data test: 7137
```

2. Melakukan Standarisasi Data numerik
Pada tahap ini kita akan melakukan standarisasi pada data numerik agar data yang kita miliki mempunyai skala yang sama sehingga dapat diolah oleh algoritma
StandardScaler melakukan proses standarisasi fitur dengan mengurangkan mean (nilai rata-rata) kemudian membaginya dengan standar deviasi untuk menggeser distribusi.  StandardScaler menghasilkan distribusi dengan standar deviasi sama dengan 1 dan mean sama dengan 0. 

Setelah itu kita dapat mengecek nilai mean dan standar deviasi, menggunakan kode di bawah ini
```
X_train[numerical_features].describe().round(4)
```

![mean dan deviasi](https://user-images.githubusercontent.com/86582130/190961165-24272516-7a28-4655-9db1-e2b8577a3115.png)

Dari gambar di atas menunjukan nilai mean = 0 dan standar deviasi = 1.

## Modeling
Membuat dan menjelaskan sistem rekomendasi untuk menyelesaikan permasalahan

Menyajikan top-N recommendation sebagai output.
=
Menyajikan dua solusi rekomendasi dengan algoritma yang berbeda.

Menjelaskan kelebihan dan kekurangan pada pendekatan yang dipilih.

Setelah dilakukan pelatihan 1 di antara 3 model ini menghasilkan hasil yang cukup jauh perbedaannya. Model ini adalah Bernoulli Naive Bayes, saat melakukan pelatihan dan melihat nilai akurasi dan f1 score, model ini menghasilkan nilai yang rendah dan perbedaan cukup signifikan dari model yang menggunakan algoritma SVM dan Logistic Regression.
Maka dari itu **model terbaik** untuk solusi dari masalah yang dipaparkan adalah menggunakan model yang dilatih dengan **algoritma SVM dan Logistic Regression**

## Evaluation
Menyebutkan metrik evaluasi yang digunakan.

Menjelaskan hasil proyek berdasarkan metrik evaluasi.

Menjelaskan metrik evaluasi yang digunakan untuk mengukur kinerja model (formula dan cara metrik tersebut bekerja).

Dari gambar di atas, skor akurasi dan f1 untuk model svc dan LR bernilai 1 sesuai dengan perhitungan di atas, tetapi model bnb juga pada skor akurasi dan f1 bernilai 1 di plot tetapi tidak sesuai dengan perhitungan di atas yaitu acc = 0.820 dan f1_score = 0,820.

## Kesimpulan
Berdasarkan hasil pelatihan model menggunakan 3 algoritma berbeda dan evaluasi menggunakan 2  metrik evaluasi yaitu akurasi dan f1 score, kemudian melakukan prediksi, menunjukan model yang tepat untuk melakukan predeksi ada tidaknya asap rokok adalah model yang dibangun dengan Bernaullie Naive Bayes, algoritma Support Vector Machine dan Logistic Regression. Ketiga algortima ini menunjukan hasil prediksi yang baik dan hasil akurasi di atas 80%

## Referensi
[1] Ritdrix, A. H. & Wirawan, P. W., 2018. Sistem Rekomendasi Bukumenggunakan Metode Item-Basedcollaborative Filtering. *Jurnal Masyarakat Informatika*, 9(2), pp. 24-32.

[2] Dewi, I. P., Lhaksmana, K. M. & Jondri, 2021. Prediksi Retweet Menggunakan Metode Bernoulli dan Gaussian Naive Bayes di Media Sosial Twitter Dengan Topik Vaksinasi Covid-19. *e-Proceeding of Engineering*. 2021. 8(5), pp. 11216-11225.

[3]Ichwan, M., Dewi, I. A. & S, Z. M., 2018. Klasifikasi Support Vector Machine (SVM) Untuk Menentukan TingkatKemanisan Mangga Berdasarkan Fitur Warna. *MIND Journal* , 3(2), pp. 16-24.

[4]Satria, F., Zamhariri & Syaripudin, M. A., 2020. Prediksi Ketepatan Waktu Lulus Mahasiswa Menggunakan Algoritma C4.5 Pada Fakultas Dakwah Dan Ilmu Komunikasi UIN Raden Intan Lampung. *Jurnal Ilmiah MATRIK*, 22(1), pp. 28-35.



