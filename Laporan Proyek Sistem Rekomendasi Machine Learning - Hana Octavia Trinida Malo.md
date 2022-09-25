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

## Content Based Filtering
### Data Understanding

Dataset yang saya gunakan merupakan dataset yang memiliki data buku, pembaca, dan rating, yang saya unduh dari link berikut : [Book Recommendation Dataset](https://www.kaggle.com/datasets/arashnic/book-recommendation-dataset) 

Dalam dataset ini terdapat 3 file yaitu books.csv, rating.csv, dan user.csv(pembaca). Berikut ini adalah jumlah data dari 3 file tersebut :

```
Jumlah data buku:  271360
Jumlah data user:  278858
Jumlah data rating:  95513
```

#### Variabel-variabel pada Book Recommendation Dataset adalah sebagai berikut:

Variabel-variabel pada Book Recommendation Dataset adalah sebagai berikut:

- books   : merupakan daftar buku-buku yang ada di perpustakaan yang berisi nomor ISBN, judul buku dan penulis buku
- user    : merupakan daftar user
- rating  : merupakan daftar user dan rating yang diberikan terhadap buku yang sudah di baca

Variabel books dan rating akan digunakan pada model rekomendasi yang akan dibangun.

#### Exploratory Data Analysis

##### Unvariate variabel

**Exploratory Variabel books**

Berikut ini adalah variabel-variabel yang ada di dalam books. Tetapi yang kita gunakan hanya variabel ISBN, judul buku, dan penulis buku.

![Screenshot (459)](https://user-images.githubusercontent.com/86582130/192146231-b68fa486-351f-423c-a521-7fee0872cd2c.png)

Gambar di bawah ini adalah data-data di dalam variabel buku. Dalam gambar ini, yang ditunjukan merupakan 4 data teratas saja

![Screenshot (458)](https://user-images.githubusercontent.com/86582130/192146153-d57f17fe-c867-44a4-b1b4-03e3500c939f.png)

Untuk total buku yang ada berjumlah, seperti pada gambar dibawah ini

![Screenshot (461)](https://user-images.githubusercontent.com/86582130/192146507-d88897c4-405c-40b4-8da5-4038b9505c32.png)

Gambar di atas menunjukan data buku-buku yang kita miliki berjumlah 271360

**Exploratory Variabel user**

Data user yang merupakan pembaca buku ini, berjumlah 278858 dan memiliki 3 variabel yaitu UserID, lokasi tempat tinggal, dan umur.

![Screenshot (462)](https://user-images.githubusercontent.com/86582130/192146637-44bc18a5-2cd9-4f9c-8b84-c6cb47eeabe6.png)


**Exploratory Variabel rating**

![Screenshot (463)](https://user-images.githubusercontent.com/86582130/192146821-77fac7d8-019e-418c-bf76-20a10cc47e90.png)
Dari gambar di atas menunjukan jumlah pembaca yang sudah melakukan rating sebanyak 95513 pembaca
Dari fungsi rating.head(), kita dapat mengetahui bahwa data rating terdiri dari 3 kolom  Kolom-kolom tersebut antara lain:

- userID, merupakan identitas pengguna.
- ISBN, merupakan identitas buku, yang berisi nomor ISBN buku.
- Rating, merupakan data rating untuk buku

### Data Preprocessing

**Menggabungkan dataframe rating dengan books berdasarkan ISBN**

Untuk menggabung kan kedua dataframe tersebut, kita gunakan fungsi merge berdasarkan variabe yang sama dari kedua dataframe tersebut, dalam hal ini ISBN
```
books = pd.merge(rating, books , on='ISBN', how='left')
books
```
Gambar di bawah ini, menunjukan data kita berhasil di gabungkan
![Screenshot (464)](https://user-images.githubusercontent.com/86582130/192147158-45ca0d59-e632-4df4-bfa0-425d4873e7b9.png)

### Data Preparation 

**1. Mengecek missing value**

Untuk mengetahui ada atau tidaknya missing value dalam dataframe yang sudah digabungkan tadi, kita gunakan fungsi isnull() dan gunakan fungsi sum() untuk menghitung jumlahnya, hasilnya seperti pada gambar di bawahh ini :

![Screenshot (465)](https://user-images.githubusercontent.com/86582130/192150687-22f62420-350a-4d35-81be-696a0efb7839.png)

**2. Mengatasi missing value**

Gunakan fungsi di atas untuk menghapus row yang mempunyai missing value.
```
books_clean = books.dropna()
```
Sekarang data yang kita punya berjumlah 941105. Selanjutnya kita dapat melihat jumlah buku yang unik berdasarkan ISBN. Dari data ini kita mempunyai 257808 data buku.

**3. Menghitung dan MelihatBerapa kali buku di rating oleh pembaca**

Pada tahap ini, kita akan menghitung berapa kali sebuah buku diberi penilaian oleh pembaca. Fungsi groupby('BookTitle') digunakan untuk mengelompokan data berdasarkan judul buku dan count() digunakan untuk menghitung jumlah rating

```
jml_rating=books_clean.groupby('BookTitle').count()['BookRating'].reset_index()
jml_rating.rename(columns={'BookRating':'jumlah_rating'},inplace=True)
jml_rating
```
Hasil dari potongan di atas, dapat dilihat dari gambar di bawah ini :

![Screenshot (466)](https://user-images.githubusercontent.com/86582130/192151454-de7c912f-3e77-492e-b415-a3acedf66ae2.png)

Dari gambar di atas dapat kita lihat bahwa buku dengan judul Always Have Popsicles, dinilai sebanyak 1 kali.

**4. Menghitung dan Melihat Berapa kali buku di rating oleh pembaca**

Pada tahap ini, kita akan menghitung rata-rata rating. Fungsi groupby('BookTitle') digunakan untuk mengelompokan data berdasarkan judul buku dan mean() digunakan untuk menghitung rata-rata rating
```
rt_rating=books_clean.groupby('BookTitle').mean()['BookRating'].reset_index()
rt_rating
```
Hasil dari potongan di atas, dapat dilihat dari gambar di bawah ini :

![Screenshot (467)](https://user-images.githubusercontent.com/86582130/192151654-fc9673db-7297-4010-9718-0371acb933df.png)

Dari gambar di atas dapat kita lihat bahwa buku dengan judul Always Have Popsicles memiliki rata-rata rating sebesar 0.

**5. Menggabungkan dataframe jml_rating dengan dataframe rt_rating berdasarkan book title**

Setelah kita menghitung berapa kali sebuah buku dinilai pembaca dan menghitung rata-rata rating setiap buku, kita akan menggabungkan kedua dataframe di atas. Penggabungan ini bertujuan untuk membuat dataframe baru yang akan digunakan untuk memfilter buku-buku mana yang populer berdasarkan rating.
```
popular_books= jml_rating.merge(rt_rating,on = 'BookTitle')
popular_books
```
**6. Mengambil data buku yang populer**

Dalam proses ini kita akan mengambil data yang sudah dirating lebih dari 500 kali. 
```
popular_books=popular_books[popular_books['jumlah_rating']>=500].sort_values('BookRating',ascending=False).head(50)
```

**7. Merge popular_books dengan dataframe books**

Gabungkan kembali data popular_books diatas dengan dataframe books yang ada awal kemudian drop booktitle yang duplikat.
![Screenshot (468)](https://user-images.githubusercontent.com/86582130/192152281-2717e9ba-0cc7-468c-a7eb-df5acb737d4a.png)

**8. Mengkonversi data series menjadi list**

Pada tahap ini kita akan mengkonversikan variabel ISBN, Book Title dan Book author menjadi list, kemudian kita hitung jumlahnya 
```
print(len(book_id))
print(len(book_title))
print(len(book_author))
```
Hasilnya akan menunjukan ketika variabel tersebut mempunyai 28 data

**9. Membuat dictionary pada data**

Pada tahap ini kita akan membuat dictionary untuk menentukan pasangan *key-value* pada data book_id, book_title, dan book_author yang telah kita siapkan sebelumnya.

![Screenshot (469)](https://user-images.githubusercontent.com/86582130/192152703-05a8888b-db6e-452f-993b-7947e7254c96.png)

Gambar di atas ini merupakan dataframe yang akan kita gunakan di tahap selanjutnya yaitu model development dengan content based filtering

### Modeling

**1. Menerapkan TF-IDF Vectorizer**

Pada tahap ini, kita akan membangun sistem rekomendasi sederhana berdasarkan penulis buku. Pada tahap ini kita akan melakukan 3 hal yaitu :
 
- Inisialisasi TfidfVectorizer
```
tf = TfidfVectorizer()
```
- Melakukan perhitungan idf pada data author
```
tf.fit(book_new['author']) 
```
- Mapping array dari fitur index integer ke fitur nama
```
tf.get_feature_names() 
```
**2. Melakukan fit dan transformasi ke dalam bentuk matriks**

![Screenshot (471)](https://user-images.githubusercontent.com/86582130/192153035-d4b7208f-a80d-4f96-9951-cc12dcadc52e.png)

Perhatikanlah, matriks yang kita miliki berukuran (28, 40). Nilai 28 merupakan ukuran data dan 40 merupakan matrik nama author.

**3. Menghasilkan vektor tf-idf dalam bentuk matriks**

![Screenshot (472)](https://user-images.githubusercontent.com/86582130/192153137-c3c6e914-ca0f-4f7b-9278-3ec01654807f.png)

**4. Melihat matriks tf-idf untuk beberapa judul buku dan nama author**

![Screenshot (473)](https://user-images.githubusercontent.com/86582130/192153289-ddb146f3-fff1-4edc-a693-677608d3817b.png)

Output matriks tf-idf di atas menunjukkan Buku 'Harry Potter and the Sorcerer's Stone (Harry Potter (Paperback))' ditulis oleh 'rowling. Matriks menunjukan bahwa buku tersebut ditulis oleh rowling. Hal ini terlihat dari nilai matriks 1.0 pada penulis rowling.

**5. Menghitung derajat kesamaan**

Pada tahap ini kita akan menghitung derajat kesamaan antara satu buku dengan buku lainnya untuk menghasilkan kandidat buku yang akan direkomendasikan. Proses ini akan menghasilkan keluaran berupa matriks kesamaan dalam bentuk array. 

**6. Melihat matriks kesamaan setiap resto**

Gambar di bawah menunjukan Shape (28, 28) merupakan ukuran matriks similarity dari data yang kita miliki. Artinya, kita mengidentifikasi tingkat kesamaan pada 28 judul. Gambar di atas memilih 10 judul buku pada baris vertikal dan 5 buku pada sumbu horizontal

![Screenshot (474)](https://user-images.githubusercontent.com/86582130/192153873-769e15fe-2a93-40e3-a185-1afc6cb6aa87.png)

Angka 1.0 yang diberi kotak merah mengindikasikan bahwa buku pada kolom X (horizontal) memiliki kesamaan dengan buku pada baris Y (vertikal). Sebagai contoh, buku The Testament teridentifikasi sama (similar) dengan buku The Pelican Brief.

**7. Mendapatkan Rekomendasi**

Pada tahap ini kita akan membuat fungsi book_recommendations dengan beberapa parameter sebagai berikut:

- book_name       : Judul buku
- Similarity_data : Dataframe mengenai similarity yang telah kita definisikan sebelumnya.
- Items           : Nama dan fitur yang digunakan untuk mendefinisikan kemiripan, dalam hal ini adalah ‘book_name’ dan ‘author’.
- k               : Banyak rekomendasi yang ingin diberikan.

Gunakan argpartition untuk mengambil sejumlah nilai k tertinggi dari similarity data kita
```
index = similarity_data.loc[:,book_name].to_numpy().argpartition(
        range(-1, -k, -1))
```
Mengambil data dari tingkat kesamaan yang paling tinggi ke yang paling rendah
```
closest = similarity_data.columns[index[-1:-(k+2):-1]]
```
Baris kode di bawah ini digunakan agar judul buku yang tidak direkomendasikan tidak muncul
```
closest = closest.drop(book_name, errors='ignore')
```

![Screenshot (475)](https://user-images.githubusercontent.com/86582130/192154419-3c3a67d5-ce43-481f-af4d-3d2aa32a733f.png)

Perhatikan gambar di atas , 'The Firm' di tulis oleh author John Grisham. Kita berhasil memberikan 5 rekomendasi buku itu 3 di antarnya ditulis oleh John Grisham

## Collaborative Filtering

### Data Understanding

Pada tahap ini kita mengimport library yang dibutuhkan dan mendefinisikan kemudian membaca dataset. Dataset yang kita gunakan adalah dataset rating, yang kita definisikan di awal sebelumnya.

![Screenshot (476)](https://user-images.githubusercontent.com/86582130/192155048-a1b8e720-d689-4bb2-80b6-07cc673d7541.png)

Dari gambar di atas kita mengetahui dataset kita berjumlah 1048575 dan memiliki 3 variabel yaitu UserID, ISBN, dan rating


### Data Preparation

**1. Menyandikan (encode) variabel ‘user’ dan ‘ISBN’ ke dalam indeks integer**

- Mengubah userID menjadi list tanpa nilai yang sama
```
user_ids = df['UserID'].unique().tolist()
```
 
 - Melakukan encoding userID

```
user_to_user_encoded = {x: i for i, x in enumerate(user_ids)}
```
 
- Melakukan proses encoding angka ke ke userID
```
user_encoded_to_user = {i: x for i, x in enumerate(user_ids)}
```
Lakukan hal yang sama pada variabel 'ISBN'
**2. Memetakan userID dan placeID ke dataframe yang berkaitan**

- Mapping userID ke dataframe user
```
df['user'] = df['UserID'].map(user_to_user_encoded)
```
- Mapping ISBN ke dataframe book
```
df['book'] = df['ISBN'].map(book_to_book_encoded)
```

**3. Mengecek jumlah data dan mengubah nilai rating**

Mengecek jumlah data seperti jumlah user, jumlah buku, kemudian mengubah nilai rating menjadi float. 
```
95513
322473
Jumlah User: 95513, Jumlah buku: 322473, Min Rating: 0.0, Max Rating: 10.0
```
Dari keluaran di atas kita dapat mengetahui jumlah User aalah 95513 dan jumlah buku adalah 322473

**4. Mengacak dataset**

Proses ini dilakukan agar distribusi data menjadi random

**5. Membagi dataset**

Dalam tahap ini, kita akan memetakan (mapping) data user dan buku menjadi satu value terlebih dahulu, kemudian membuat data  rating dalam skala 0 sampai 1 agar mudah dalam melakukan proses training. Terakhir kita dapat membagi dataset kita menjadi  data train dan validasi dengan komposisi 80:20. Pembagian dataset ini bertujuan agar memudahkan kita dalam proses evaluasi performa model
```
train_indices = int(0.8 * df.shape[0])
x_train, x_val, y_train, y_val = (
    x[:train_indices],
    x[train_indices:],
    y[:train_indices],
    y[train_indices:]
)
```

### Modeling

**Menghitung Skor kecocokan**

Pada tahap ini, model menghitung skor kecocokan antara pengguna dan buku dengan teknik embedding. 
Pertama, kita melakukan proses embedding terhadap data user dan buku. 
```
    self.user_embedding = layers.Embedding( # layer embedding user
        num_users,
        embedding_size,
        embeddings_initializer = 'he_normal',
        embeddings_regularizer = keras.regularizers.l2(1e-6)
    )
```
Barisan kode di atas kita melakukan  embedding terhadap data user.

Barisan kode di bawah ini adalah proses untuk menambahkan bias untuk setiap buku. 
```
self.book_bias = layers.Embedding(num_book, 1) 
```

**Model compile**

Model ini menggunakan Binary Crossentropy untuk menghitung loss function, Adam (Adaptive Moment Estimation) sebagai optimizer, dan root mean squared error (RMSE) sebagai metrics evaluation. 

**Memulai training**
Melakukan training model dengan fungsi fit(), kemudian set epoch = 10, dan batch_size=128 agar proses training berjalan dengan cepat

```
history = model.fit(
    x = x_train,
    y = y_train,
    batch_size = 128,
    epochs = 10,
    validation_data = (x_val, y_val)
)
```

Membuat dan menjelaskan sistem rekomendasi untuk menyelesaikan permasalahan

Menyajikan top-N recommendation sebagai output.

Menyajikan dua solusi rekomendasi dengan algoritma yang berbeda.

Menjelaskan kelebihan dan kekurangan pada pendekatan yang dipilih.


### Evaluation

Metrik yang digunakan untuk evaluasi model adalah  Root Mean Squared Error (RMSE)
Menyebutkan metrik evaluasi yang digunakan.

Menjelaskan hasil proyek berdasarkan metrik evaluasi.

Menjelaskan metrik evaluasi yang digunakan untuk mengukur kinerja model (formula dan cara metrik tersebut bekerja).

Perhatikanlah, proses training model cukup smooth dan model konvergen pada epochs sekitar 100. Dari proses ini, kita memperoleh nilai error akhir sebesar sekitar 0.23 dan error pada data validasi sebesar 0.34. Nilai tersebut cukup bagus untuk sistem rekomendasi. Mari kita cek, apakah model ini bisa membuat rekomendasi dengan baik?



## Kesimpulan
Berdasarkan hasil pelatihan model menggunakan 3 algoritma berbeda dan evaluasi menggunakan 2  metrik evaluasi yaitu akurasi dan f1 score, kemudian melakukan prediksi, menunjukan model yang tepat untuk melakukan predeksi ada tidaknya asap rokok adalah model yang dibangun dengan Bernaullie Naive Bayes, algoritma Support Vector Machine dan Logistic Regression. Ketiga algortima ini menunjukan hasil prediksi yang baik dan hasil akurasi di atas 80%

## Referensi
[1] Ritdrix, A. H. & Wirawan, P. W., 2018. Sistem Rekomendasi Bukumenggunakan Metode Item-Basedcollaborative Filtering. *Jurnal Masyarakat Informatika*, 9(2), pp. 24-32.

[2] Dewi, I. P., Lhaksmana, K. M. & Jondri, 2021. Prediksi Retweet Menggunakan Metode Bernoulli dan Gaussian Naive Bayes di Media Sosial Twitter Dengan Topik Vaksinasi Covid-19. *e-Proceeding of Engineering*. 2021. 8(5), pp. 11216-11225.

[3]Ichwan, M., Dewi, I. A. & S, Z. M., 2018. Klasifikasi Support Vector Machine (SVM) Untuk Menentukan TingkatKemanisan Mangga Berdasarkan Fitur Warna. *MIND Journal* , 3(2), pp. 16-24.

[4]Satria, F., Zamhariri & Syaripudin, M. A., 2020. Prediksi Ketepatan Waktu Lulus Mahasiswa Menggunakan Algoritma C4.5 Pada Fakultas Dakwah Dan Ilmu Komunikasi UIN Raden Intan Lampung. *Jurnal Ilmiah MATRIK*, 22(1), pp. 28-35.



