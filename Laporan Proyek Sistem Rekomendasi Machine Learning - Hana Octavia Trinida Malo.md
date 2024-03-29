# Laporan Proyek Machine Learning - Hana Octavia Trinida Malo

## Domain Proyek

Sistem rekomendasi adalah sistem yang digunakan untuk memberikan rekomendasi kepada *customer* berdasarkan berbagai informasi  yang bertujuan untuk untuk meningkatkan penjualan produk. Sistem  rekomendasi dapat ditemukan pada situs-situs belanja *online* seperti eBay, Alibaba, OLX, yang menjual pakaian, barang-barang elektronik, peralatan  rumah  tangga, dan yang lainnya. Sistem rekomendasi juga dapat ditemukan pada situs yang murni memberikan rekomendasi kepada penggunanya, seperti situs   MovieLens dan Internet Movie Database (IMDb) yang memberikan rekomendasi film yang akan ditontonkepada penggunanya[1]

Pembaca di perpustakaan memiliki ketertarikan membaca yang berbeda - beda, ada pembaca yang menyukai beberapa penulis buku saja sehingga hanya ingin membaca penulis yang pembaca suka. Ada juga pembaca yang membaca buku, dilihat dari seberapa disukainya buku itu oleh pembaca yang sudah pernah membaca buku sebelumnya. Semakin banyak yang menyukai buku itu, semakin tertarik pembaca tersebut untuk membaca buku itu.

Jumlah buku bacaan yang ada di perpustakaan saat ini, kurang lebih berjumlah 250an ribu buku. Banyaknya jumlah buku ini seringkali membuat pembaca kesulitan untuk memilih buku yang ingin dibaca, dan berakhir mengurungkan niat untuk membaca buku. Untuk mempermudah pembaca dalam membaca buku yang sesuai dengan keinginannya perlu dibangun sebuah sistem rekomendasi, dibangunnya sistem rekomendasi tentunya dapat menjadi solusi terbaik agar jumlah pembaca bertambah.


## Pendefinisian Bisnis

Sebuah perpustakaan memiliki berbagai informasi mengenai pembaca, daftar buku dan beserta rating yang diberikan pembaca untuk buku-buku tersebut. Perpustakaan ini ingin memiliki sebuah sistem rekomendasi yang dapat memberikan rekomendasi kepada pembaca berdasarkan penulis buku yang pernah dia baca sebelumnya dan rating pembaca.

### Masalah

Berdasarkan latar belakang yang telah diuraikan di atas, maka dapat dirumuskan masalah-masalah yang harus diselesaikan antara lain :

- Berdasarkan data mengenai buku dan pembaca, bagaimana membuat sistem rekomendasi yang dipersonalisasi dengan teknik content-based filtering?
- Dengan data rating yang dimiliki, bagaimana perpustakaan dapat merekomendasikan buku lain yang mungkin disukai oleh pengguna? 

### Tujuan

Untuk  menjawab pertanyaan tersebut, buatlah sistem rekomendasi dengan tujuan sebagai berikut:

- Menghasilkan sejumlah rekomendasi buku yang dipersonalisasi untuk pengguna dengan teknik content-based filtering.
- Menghasilkan sejumlah rekomendasi buku yang sesuai dengan preferensi pengguna dengan teknik collaborative filtering.

### Solusi

Solusi untuk mencapai tujuan diatas adalah :

- menerapkan content based filtering dalam membuat sistem rekomendasi, dimana mennggunakan nama penulis buku untuk merekomendasikan kepada pembaca
- menerapkan collaborative filtering dalam membuat sistem rekomendasi, dimana mennggunakan data rating untuk merekomendasikan kepada pembaca

## Content Based Filtering

Content Base Filtering adalah teknik rekomendasi yang memberikan rekomendasi kepada pengguna dengan memahami kebutuhan , preferensi, dan kendala pengguna. Informasi ini dan interaksi pengguna sebelumnya, kemudian dibuat sedemikian rupa untuk membangun profil pengguna. Kelebihan teknik ini adalah dapat memberikan rekomendasi kepada pengguna walaupun hal yang direkomendasi merupakan item baru tanpa harus melihat rating dari item baru tersebut[2]. 

Kelemahan dari teknik content-based filtering adalah hanya dapat merekomendasika tem-item yang memiliki kemiripan sehingga item yang direkomendasi terbatas[3].

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

#### Unvariate variabel

**Exploratory Variabel books**

Tabel 1 menunjukan daftar variabel-variabel yang ada di dalam books. Tetapi yang kita gunakan hanya variabel ISBN, judul buku, dan penulis buku.
Penjelasan variabel :
- ISBN : nomor ISBN buku
- BookTitle : merupakan data Judul Buku
- BookAuthor : merupakan data penulis buku

| Column            | Non-Null Count        | Dtype      |
| ------------------|----------------------|----------|
|   ISBN             |  271360 non-null | object    |
|   BookTitle         | 271360 non-null | object    |
|  BookAuthor         |271359 non-null | object |
|  YearOfPublication | 271360 non-null  |object|
|   Publisher         | 271358 non-null | object|
|  ImageURLS        |  271360 non-null | object|
|  ImageURLM        |  271360 non-null | object|
| ImageURLL         | 271357 non-null  |object|

Tabel 1. variabel dalam variabel books

Tabel di bawah ini adalah data-data di dalam variabel buku. Dalam tabel dibawah ini ini, yang ditunjukan merupakan 4 data teratas saja

|ISBN	      |BookTitle	            |BookAuthor	       |YearOfPublication |Publisher	               |
| ----------|----------------------|------------------|------------------|-------------------------|
|	195153448	|Classical Mythology	  |Mark P. O. Morford|	2002	            |Oxford University Press	 |
|	2005018	  |Clara Callan	Richard  |Bruce Wright	     |2001	             |HarperFlamingo Canada	   |
|	60973129	 |Decision in Normandy	 |Carlo D'Este	     |1991		            |HarperPerennial	          |
|	374157065	|Flu: The Story of ...	| Gina Bari Kolata	|1999		            |Farrar Straus Giroux      |
|	393045218	|The Mummies of Urumchi|E. J. W. Barber	  |1999		            |W. W. Norton &amp; Company |

Tabel 2. Data variabel books

Untuk total buku yang ada berjumlah, seperti pada gambar dibawah ini

![Screenshot (463)](https://user-images.githubusercontent.com/86582130/192175319-d3f12775-ef5d-4e26-9753-fbf31ea3e720.png)

Gambar 1. jumlah buku

Gambar 1 menunjukan data buku-buku yang kita miliki berjumlah 271360

**Exploratory Variabel user**

Berdasarkan tabel 3, menunjukan bahwa bahwah variabel user memiliki 3 variabel yaitu 
- UserID : berisi user ID
- location : berisi lokasi user
- age : umur user

|UserID	 |    Location	                      |    Age|
| ------|------------------------------------|-----|
| 1	   |nyc, new york, usa	                 |NaN   |
| 2	   |stockton, california, usa	          |18.0   |    
| 3	   |moscow, yukon territory, russia     |	NaN   |
| 4	   |porto, v.n.gaia, portugal	          |17.0   |
| 5	   |farnborough, hants, united kingdom  	|NaN   |

Tabel 3.Data variabel user


**Exploratory Variabel rating**

|UserID	|ISBN 	       |BookRating|
| -----|-----------|-----------|
|276725	|	034545104X  |	0        |
|276726	|	155061224  |	5        |
|276727	|446520802    |	0        |
|276729	|	052165615X  |	3        |
|276729	|	521795028	  |6        |

Tabel 4. Data rating

Tabel 4 menunjukan jumlah pembaca yang sudah melakukan rating sebanyak 95513 pembaca
Dari fungsi rating.head(), kita dapat mengetahui bahwa data rating terdiri dari 3 kolom  Kolom-kolom tersebut antara lain:

- userID, merupakan identitas pengguna.
- ISBN, merupakan identitas buku, yang berisi nomor ISBN buku.
- Rating, merupakan data rating untuk buku

### Data Preprocessing

**Menggabungkan dataframe rating dengan books berdasarkan ISBN**

Untuk menggabung kan kedua dataframe tersebut, kita gunakan fungsi merge() berdasarkan variabe yang sama dari kedua dataframe tersebut, dalam hal ini ISBN.

Tabel 5, menunjukan data kita berhasil di gabungkan

|UserID |ISBN	      |BookRating	  |BookTitle             |BookAuthor	    |YearOfPublication |Publisher	                  |
| ------|-----------|-------------|-----------------------|--------------|------------------|-----------------------------|
276725	|034545104X	 |0	           |Flesh Tones: A Novel  |	M. J. Rose    |2002	              |Ballantine Books	            |
276726	|155061224	 |5	            |	Rites of Passage	    |Judith Rae	    |2001	              |	Heinle		                    |
276727	|446520802	 |0		           |The Notebook	         |Nicholas Sparks|1996	              |	Warner Books		              |
276729	|052165615X	|3	            |	Help!: Level 1	      |Philip Prowse	  |1999	              |	Cambridge University Press  |
276729	|521795028	 |6		           |The Amsterdam Connection...|Sue Leather|2001		              |Cambridge University Press  |

Tabel 5. Dataframe hasil gabungan

### Data Preparation 

**1. Mengecek missing value**

Untuk mengetahui ada atau tidaknya missing value dalam dataframe yang sudah digabungkan tadi, kita gunakan fungsi isnull() dan gunakan fungsi sum() untuk menghitung jumlahnya, hasilnya seperti pada tabel 6  :

|UserID             |       0|
| ------------------|-----------|
|ISBN               |       0|
|BookRating          |      0|
|BookTitle          |  107463|
|BookAuthor         |  107464|
|YearOfPublication  |  107463|
|Publisher          |  107465|
|ImageURLS          |  107463|
|ImageURLM          |  107463|
|ImageURLL          |  107467|

Tabel 6. Variabel mengandung missing value

**2. Mengatasi missing value**

Variabel terdeteksi null valueBookTitle, BookAuthor, YearOfPublication, Publisher, ImageURLS, ImageURLM, ImageURLL sebanyak 107467. Gunakan fungsi dropna() untuk menghapus row yang mempunyai missing value

Jumlah data yang kitas sekarang punya berjumlah 941105. Selanjutnya kita dapat melihat jumlah buku yang unik berdasarkan ISBN. Dari data ini kita mempunyai 257808 data buku.

**3. Menghitung dan MelihatBerapa kali buku di rating oleh pembaca**

Pada tahap ini, kita akan menghitung berapa kali sebuah buku diberi penilaian oleh pembaca. Fungsi groupby('BookTitle') digunakan untuk mengelompokan data berdasarkan judul buku dan count() digunakan untuk menghitung jumlah rating

|BookTitle	                                        |jumlah_rating|
| --------------------------------------------------|-------------|
|A Light in the Storm: The Civil War Diary of ...	 |4            |
|Always Have Popsicles	                             |1            |
|Apple Magic (The Collector's series)	              |1            |
|Beyond IBM: Leadership Marketing and Finance ...	 |1            |
|Clifford Visita El Hospital (Clifford El Gran...	  |1            |

Tabel 7 jumlah rating per judul buku

Dari tabel 7 dapat kita lihat bahwa buku dengan judul Always Have Popsicles, dinilai sebanyak 1 kali.

**4. Menghitung dan Melihat Berapa kali buku di rating oleh pembaca**

Pada tahap ini, kita akan menghitung rata-rata rating. Fungsi groupby('BookTitle') digunakan untuk mengelompokan data berdasarkan judul buku dan mean() digunakan untuk menghitung rata-rata rating

Dari tabel 8 dapat kita lihat bahwa buku dengan judul Always Have Popsicles memiliki rata-rata rating sebesar 0.

|BookTitle	                                       |BookRating|
| ------------------------------------------------|-----------|
|A Light in the Storm: The Civil War Diary of ...	|2.25      |
|Always Have Popsicles	                           |0.00      |
|Apple Magic (The Collector's series)             |	0.00      |
|Beyond IBM: Leadership Marketing and Finance ...	|0.00      |
|Clifford Visita El Hospital (Clifford El Gran...	|0.00      |

Tabel 8 rata-rata rating per judul buku

**5. Menggabungkan dataframe jml_rating dengan dataframe rt_rating berdasarkan book title**

Setelah kita menghitung berapa kali sebuah buku dinilai pembaca dan menghitung rata-rata rating setiap buku, kita akan menggabungkan kedua dataframe di atas menggunakan fungsi merge(). Penggabungan ini bertujuan untuk membuat dataframe baru yang akan digunakan untuk memfilter buku-buku mana yang populer berdasarkan rating.

**6. Mengambil data buku yang populer**

Dalam proses ini kita akan mengambil data yang sudah dirating lebih dari 500 kali. Kemudian kita urutkan daanya berdasarkan data yang terbesar dengan fungsi sort_values() dan mengatur ascending = False


**7. Merge popular_books dengan dataframe books**

Gabungkan kembali data popular_books diatas dengan dataframe books yang ada awal dengan fungsi merge()kemudian drop booktitle yang duplikat menggunakan fungsi drop_duplicates()


**8. Mengkonversi data series menjadi list**

Pada tahap ini kita akan mengkonversikan variabel ISBN, Book Title dan Book author menjadi list menggunakan fungsi tolist(), kemudian kita hitung jumlahnya dengan fungsi len().

Hasilnya akan menunjukan ketika variabel tersebut mempunyai 28 data

**9. Membuat dictionary pada data**

Pada tahap ini kita akan membuat dictionary untuk menentukan pasangan *key-value* pada data book_id, book_title, dan book_author yang telah kita siapkan sebelumnya.
Gunakan fungsi DataFrame() untuk membuat dataframe baru berdasarkan *key-value* yang sudah ditentukan sebelumnya

### Modeling

**1. Menerapkan TF-IDF Vectorizer**

Pada tahap ini, kita akan membangun sistem rekomendasi sederhana berdasarkan penulis buku. Pada tahap ini kita akan melakukan 3 hal yaitu :
 
- Inisialisasi TfidfVectorizer dengan membuat variabel baru 'tf'
- Melakukan perhitungan idf pada data author dengan fungsi fit()

- Mapping array dari fitur index integer ke fitur nama dengan fungsi get_feature_names()

**2. Melakukan fit dan transformasi ke dalam bentuk matriks**

![Screenshot (471)](https://user-images.githubusercontent.com/86582130/192153035-d4b7208f-a80d-4f96-9951-cc12dcadc52e.png)

gambar 2. hasil transformasi matriks

Perhatikanlah gambar 2, matriks yang kita miliki berukuran (28, 40). Nilai 28 merupakan ukuran data dan 40 merupakan matrik nama author.

**3. Menghasilkan vektor tf-idf dalam bentuk matriks**

Mengubah vektor tf-idf dalam bentuk matriks dengan fungsi todense()

![Screenshot (472)](https://user-images.githubusercontent.com/86582130/192153137-c3c6e914-ca0f-4f7b-9278-3ec01654807f.png)

gambar 3. vektor tf-idf

**4. Melihat matriks tf-idf untuk beberapa judul buku dan nama author**

	
|book_name						|					shapero	|michael|	anita|	diamant|	martel|	melissa|	amy	|rowling|	rebecca	|judy	|sue|	mclaughlin	|crichton	|monk|	
|-------------------------------------------------------|-----------------------------------------------|---------|----------|-----------|------------|---------|-------------------|--------|--------------|---------|---------|--------------|---------|---------|
|The Nanny Diaries: A Novel|	0.0	|0.000000	|0.000000|	0.000000	|0.0	|0.0	|0.000000|	0.0	|0.0|	0.0|	0.000000	|	0.0	|0.0|	0.0|
|Where the Heart Is (Oprah's Book Club (Paperback))|	0.0	|0.000000|	0.000000|	0.000000|	0.0|	0.0	|0.000000|	0.0|	0.0|	0.0|	0.000000|	0.0	|0.0|	0.0|
|Angels &amp; Demons|	0.0|	0.000000	|0.000000|	0.000000	|0.0|	0.0|	0.000000|	0.0|	0.0|	0.0|	0.000000|	0.0	|0.0|	0.0|	
|Bridget Jones's Diary|	0.0	|0.000000|	0.000000|	0.000000	|0.0|	0.0	|0.000000|	0.0|	0.0|	0.0	|	0.000000	|	0.0	|0.0|	0.0|
|Harry Potter and the Sorcerer's Stone (Harry Potter (Paperback))|	0.0|	0.707107	|0.000000|	0.000000|	0.0|	0.0|	0.000000	|1.0|	0.0|	0.0	|0.000000|	0.0	|0.0|	0.0|
|The Pilot's Wife : A Novel|	0.0|	0.000000|	0.664679	|0.000000|	0.0	|0.0	|0.000000	|0.0|	0.0	|0.0|	0.747129|	0.0	|0.0|	0.0|

tabel 9. hasil transformasi matriks

Tabel 9 merupakan output matriks tf-idf yang menunjukkan Buku 'Harry Potter and the Sorcerer's Stone (Harry Potter (Paperback))' ditulis oleh 'rowling. Matriks menunjukan bahwa buku tersebut ditulis oleh rowling. Hal ini terlihat dari nilai matriks 1.0 pada penulis rowling.

**5. Menghitung derajat kesamaan**

Pada tahap ini kita akan menghitung derajat kesamaan antara satu buku dengan buku lainnya untuk menghasilkan kandidat buku yang akan direkomendasikan. Proses ini akan menghasilkan keluaran berupa matriks kesamaan dalam bentuk array. 

**6. Melihat matriks kesamaan setiap buku**

Tabel 10 menunjukan Shape (28, 28) merupakan ukuran matriks similarity dari data yang kita miliki. Artinya, kita mengidentifikasi tingkat kesamaan pada 28 judul. Gambar di atas memilih 10 judul buku pada baris vertikal dan 5 buku pada sumbu horizontal

|book_name	|Summer Sisters	|The Pilot's Wife : A Novel|	Bridget Jones's Diary	|Divine Secrets of the Ya-Ya Sisterhood: A Novel|The Pelican Brief and Fishing|
|------------|--------------|---------------------------|------------------------|-------------------------------------------------|-----------------------------|
|The Brethren|	0.0	|0.000000	|0.0	|0.0	|0.0|
|Harry Potter and the Sorcerer's Stone |	0.0|	0.000000|	0.0	|0.0	|0.0|
|The Da Vinci Code |	0.0|	0.000000|	0.0	|0.0	|0.0|
|The Testament	|0.0	|0.000000|	0.0	|0.0	|1.0|
|A Painted House	|0.0	|0.000000|	0.0	|0.0|	0.0|
|House of Sand and Fog|	0.0|	0.000000|	0.0|	0.0|	0.0|

Tabel 10. matriks kesamaan setiap buku

Angka 1.0 yang diberi kotak merah mengindikasikan bahwa buku pada kolom X (horizontal) memiliki kesamaan dengan buku pada baris Y (vertikal). Sebagai contoh, buku The Testament teridentifikasi sama (similar) dengan buku The Pelican Brief.

**7. Mendapatkan Rekomendasi**

Pada tahap ini kita akan membuat fungsi book_recommendations dengan beberapa parameter sebagai berikut:

- book_name       : Judul buku
- Similarity_data : Dataframe mengenai similarity yang telah kita definisikan sebelumnya.
- Items           : Nama dan fitur yang digunakan untuk mendefinisikan kemiripan, dalam hal ini adalah ‘book_name’ dan ‘author’.
- k               : Banyak rekomendasi yang ingin diberikan.

Gunakan fungsi argpartition() untuk mengambil sejumlah nilai k tertinggi dari similarity data kita

Mengambil data dari tingkat kesamaan yang paling tinggi ke yang paling rendah dengan fungsi columns[index[-1:-(k+2):-1]]

Baris kode di bawah ini digunakan agar judul buku yang tidak direkomendasikan tidak muncul dengan fungsi drop()

|book_name	|author    |
|----------|-----------|
|The Summons	|John Grisham|
|The Testament	|John Grisham|
|A Painted House|	JOHN GRISHAM|
|Life of Pi	|Yann Martel|
|Where the Heart Is (Oprah's Book Club|Billie Letts|

Tabel 10. hasil rekomendasi

Perhatikan tabel 10, 'The Firm' di tulis oleh author John Grisham. Kita berhasil memberikan 5 rekomendasi buku itu 3 di antarnya ditulis oleh John Grisham

### Evaluasi

Untuk Mengevaluasi hasil rekomendasi kita, gunakan metric pression. Precission adalah rasio prediksi benar positif dibandingkan dengan keseluruhan hasil yang diprediksi positif.

Precision = jumlah item rekomendasi yang relevan/jumlah item yang direkomendasi

Dari hasil rekomendasi di atas, diketahui bahwa 'The Firm' di tulis oleh author John Grisham. Dari 5 item yang direkomendasikan, 3 item memiliki di tulis oleh author John Grisham(similar). Maka perhitungan Precission menjadi seperti berikut


Precission = 3/5
Precission = 60%

Sehingga sistem rekomendasi menggunakan content based learning kita menghasilkan nilai precission sebesar 60%

## Collaborative Filtering

Collaborative filtering merupakan salah satu metode rekomendasi yang dapat memberikan rekomendasi kepada pengguna berdasarkan item yang disukai oleh pengguna lain yang memiliki preferensi yang mirip[2].
Kelebihan dari pendekatan metode ini adalah dapat menghasilkan rekomendasi yang berkualitas. Kekurangan metode ini , kompleksitas perhitungan akan semakin bertambah seiring dengan bertambahnya pengguna sistem, semakin banyak pengguna yang menggunakan sistem maka proses perekomendasian akan semakin lama 

### Data Understanding

Pada tahap ini kita mengimport library yang dibutuhkan dan mendefinisikan kemudian membaca dataset. Dataset yang kita gunakan adalah dataset rating, yang kita definisikan di awal sebelumnya.


|UserID	|ISBN 	       |BookRating|
| -----|-----------|-----------|
|276725	|	034545104X  |	0        |
|276726	|	155061224  |	5        |
|276727	|446520802    |	0        |
|276729	|	052165615X  |	3        |
|276729	|	521795028	  |6        |

tabel 11 data rating

Dari tabel 11 kita mengetahui dataset kita berjumlah 1048575 dan memiliki 3 variabel yaitu UserID, ISBN, dan rating


### Data Preparation

**1. Menyandikan (encode) variabel ‘user’ dan ‘ISBN’ ke dalam indeks integer**

- Mengubah 'userID' dan 'ISBN' menjadi list tanpa nilai yang sama menggunakan fungsi tolist()
- Melakukan encoding 'userID' dan 'ISBN' menggunakan fungsi enumerate()
- Melakukan proses encoding angka ke ke 'userID' dan 'ISBN' enumerate()

**2. Memetakan userID dan ISBN ke dataframe yang berkaitan**

- Mapping userID ke dataframe user menggunakan fungsi map()
- Mapping ISBN ke dataframe book menggunakan fungsi map()

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

### Modeling

**Menghitung Skor kecocokan**

Pada tahap ini, model menghitung skor kecocokan antara pengguna dan buku dengan teknik embedding. 
Pertama, kita melakukan proses embedding terhadap data user dan buku. Selain itu, kita juga dapat menambahkan bias untuk setiap user dan buku. 

Barisan kode di bawah ini adalah proses untuk menambahkan bias untuk setiap buku dengan fungsi Embedding(). 

Kemudian kita memangil layer embedding yang sudah dibuat sebelumnya. Terakhir gunakan fungsi aktivasi sigmoid untuk menetapkan skor kecocokan ditetapkan dalam skala [0,1]

**Model compile**

Model ini menggunakan Binary Crossentropy untuk menghitung loss function, Adam (Adaptive Moment Estimation) sebagai optimizer, dan root mean squared error (RMSE) sebagai metrics evaluation. 

**Memulai training**

Melakukan training model dengan fungsi fit(), inisialisasi variabel x dan y untuk mengambil data train, kemudian set epoch = 10, dan batch_size=128 agar proses training berjalan dengan cepat


### Mendapatkan Rekomendasi Buku

Untuk mendapatkan rekomendasi buku, pertama kita ambil sampel user secara acak dan definisikan variabel book_not_read yang merupakan daftar buku yang belum pernah dibaca. sample ini yang akan menjadi buku yang kita rekomendasikan kepada pembaca. 

Sebelumnya, pembaca telah memberi rating pada beberapa buku yang telah dibaca sebelumnya, kita akan menggunakan rating ini untuk membuat rekomendasi buku kepada pembaca.

Untuk memperoleh rekomendasi buku, gunakan fungsi model.predict().

Tabel 12 merupakan rekomendasi buku untuk user dengan ID 224349

Showing recommendations for users: 224349
books with high ratings from user

Top 10 books recommendation

|Book Title                                                     |Book Author          |
|---------------------------------------------------------------|----------------------|
|Harry Potter and the Chamber of Secrets (Book 2)                 | J. K. Rowling       |              
|Harry Potter and the Sorcerer's Stone (Harry Potter (Paperback)) |J. K. Rowling       |       
|Where the Heart Is (Oprah's Book Club (Paperback))                |Billie Letts        |             
|Life of Pi                                                      | Yann Martel        |
|The Notebook                                                      | Nicholas Sparks   |                                 
|Bridget Jones's Diary                                            | Helen Fielding   | 
|The Pilot's Wife : A Novel                                       | Anita Shreve    | 
|The Summons                                                      | John Grisham    | 
|Summer Sisters                                                    | Judy Blume     | 
|The Firm                                                         | John Grisham    | 

tabel 12. rekomendasi 10 buku

### Evaluation

Metrik yang digunakan untuk evaluasi model adalah  Root Mean Squared Error (RMSE). Root Mean Squared Error (RMSE) adalah metrik yang digunakan dalam akurasi prediktif yang perhitungannya konsepnya mirip dengan metrik MAE, namun pengkuadratan kesalahan menghasilkan lebih banyak penekanan pada kesalahan daripada yang menggunakan metrik MAE[4]. Singkatnya, RMSE pada dasarnya mengukur kesalahan kuadrat rata-rata dari prediksi kita. 

Berdasarkan hasil training model, dihasilkan root_mean_squared_error bernilai 0.3949 dan val_root_mean_squared_error bernilai 0.4060. Skor ini sudah cukup bagus untuk sistem rekomendasi kita.

Berikut ini adalah visualisasi metrik dengan plot

![Screenshot (478)](https://user-images.githubusercontent.com/86582130/192162604-78a85e19-a54d-48cd-b65c-11411ca8cae8.png)

Gambar 4. Visualisasi metrik

## Kesimpulan

Berdasarkan hasil *development* sistem rekomendasi menggunakan 2 metode yaitu Content-Based Filtering dan Collaborative Filltering dan evaluasi menggunakanbmetrik RMSE untuk colaborative filtering dan precission untuk content based filtering, menunjukan bahwa kedua metode ini dapat membangun rekomendasi dengan baik, ditunjukan dengan kemampuan merekomendasikan buku berdasarkan penulis dan rating teratas

## Referensi

[1] Ritdrix, A. H. & Wirawan, P. W., 2018. Sistem Rekomendasi Bukumenggunakan Metode Item-Basedcollaborative Filtering. *Jurnal Masyarakat Informatika*, 9(2), pp. 24-32.

[2]Devi, A. A. P. & Tonara, D. B., 2015. Rancang Bangun Recommender System dengan Menggunakan Metode Collaborative Filtering untuk Studi Kasus Tempat Kuliner di Surabaya. *JUISI*, 1(2), pp. 102-112.

[3]Mondi, R. H., Wijayanto, A. & Winarno, 2019. Recommendation System With Content-Based Filtering Method For Culinary Tourism In Mangan Application. *Jurnal Ilmiah Teknologi dan Informasi*, 8(2), pp. 65-72.

[4] M. Nilashi, K. Bagherifard, O. Ibrahim, H. Alizadeh, L. A. Nojeem, and N. Roozegar, 2018. Collaborative filtering recommender systems. *Research Journal of Applied Sciences, Engineering and Technology*, 5(16), pp. 4168–4182.




