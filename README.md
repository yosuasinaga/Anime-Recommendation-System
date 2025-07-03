# Laporan Proyek Machine Learning - Sistem Rekomendasi Film Anime

## Project Overview

Di era digital saat ini, banyaknya pilihan anime yang tersedia secara online sering kali membuat penggemar anime kesulitan untuk menemukan anime yang sesuai dengan preferensi mereka. Sistem rekomendasi anime menjadi solusi penting untuk mengatasi masalah *information overload* ini, membantu pengguna menemukan anime yang relevan dengan minat mereka dari ribuan pilihan yang ada.

Sistem rekomendasi anime juga membantu platform seperti MyAnimeList dan Crunchyroll untuk meningkatkan pengalaman pengguna dan mendorong keterlibatan pengguna yang lebih tinggi. Dengan menggunakan sistem rekomendasi, platform dapat menyarankan anime yang sesuai dengan preferensi pengguna berdasarkan data rating dan interaksi pengguna sebelumnya.

Proyek ini menggunakan dataset rekomendasi anime yang berisi interaksi pengguna dengan anime, termasuk rating dan informasi lainnya. Dengan menganalisis pola perilaku penonton, proyek ini bertujuan untuk mengembangkan sistem rekomendasi yang dapat memprediksi anime yang mungkin disukai oleh pengguna dan memberikan saran yang lebih relevan.

## Business Understanding

### Problem Statements

Berdasarkan latar belakang yang telah diuraikan, berikut beberapa permasalahan yang akan diselesaikan dalam proyek ini:

1. Bagaimana cara mengidentifikasi anime yang mungkin disukai oleh pengguna berdasarkan riwayat penilaian mereka dan penilaian dari pengguna lain dengan preferensi serupa?
2. Bagaimana cara mengatasi masalah cold start untuk pengguna baru yang belum memberikan penilaian pada anime?
3. Bagaimana cara mengukur efektivitas sistem rekomendasi anime yang dikembangkan?

### Goals

Tujuan dari proyek ini adalah:

1. Mengembangkan sistem rekomendasi anime yang dapat memprediksi anime yang mungkin disukai oleh pengguna berdasarkan pola penilaian mereka dan pengguna lain dengan preferensi serupa.
2. Membuat strategi untuk mengatasi masalah cold start dengan mempertimbangkan pendekatan alternatif seperti anime populer atau anime dengan rating tinggi.
3. Mengevaluasi kinerja sistem rekomendasi dengan metrik yang sesuai seperti Root Mean Square Error (RMSE), Mean Absolute Error (MAE), dan NDCG.

### Solution Approach

Untuk mencapai tujuan yang telah ditetapkan, berikut pendekatan solusi yang dapat diterapkan:

**Collaborative Filtering**:
- **User-based**: Mencari pengguna yang memiliki pola penilaian serupa dengan pengguna target, kemudian merekomendasikan anime yang disukai oleh pengguna serupa tersebut.
- **Item-based**: Mencari kesamaan antar anime berdasarkan pola penilaian pengguna, kemudian merekomendasikan anime yang mirip dengan anime yang sudah disukai pengguna.
- **Model-based dengan Matrix Factorization**: Menggunakan teknik seperti Singular Value Decomposition (SVD) atau Neural Collaborative Filtering untuk mempelajari pola tersembunyi dari interaksi pengguna-anime.
- **Keunggulan**: Dapat menemukan rekomendasi yang tidak jelas atau tidak intuitif berdasarkan pola komunitas.
- **Keterbatasan**: Masalah cold start untuk pengguna baru dan anime baru.

Dalam proyek ini, fokus utama akan diberikan pada pendekatan **Collaborative Filtering** karena kemampuannya untuk menangkap pola yang kompleks dari interaksi pengguna-anime dan menghasilkan rekomendasi yang dipersonalisasi.

## Data Understanding

Dataset yang digunakan dalam proyek ini adalah dataset rekomendasi anime yang dapat diakses dari Kaggle, dan dapat ditemukan di [tautan berikut](https://www.kaggle.com/datasets/CooperUnion/anime-recommendations-database). Dataset ini berisi informasi tentang preferensi pengguna terhadap anime, termasuk rating yang diberikan. Namun, perlu dicatat bahwa dataset ini telah dimodifikasi untuk menghapus beberapa data dengan genre tertentu yang tidak sesuai dengan fokus penelitian. Hal ini dilakukan untuk memastikan bahwa sistem rekomendasi yang dibangun hanya menyarankan anime dengan genre yang lebih sesuai dan relevan dengan audiens yang lebih luas. Modifikasi ini membantu meningkatkan kualitas dan relevansi rekomendasi yang diberikan kepada pengguna. 

Dataset terdiri dari dua file utama:

1. **anime.csv** - Informasi tentang anime dengan total 11.153 entri:
   - `anime_id`: ID unik untuk setiap anime.
   - `name`: Nama lengkap anime.
   - `genre`: Daftar genre yang terkait dengan anime.
   - `type`: Tipe anime (misalnya Movie, TV, OVA).
   - `episodes`: Jumlah episode (1 jika anime berupa film).
   - `rating`: Rating rata-rata dari anime (skala 1-10).
   - `members`: Jumlah anggota komunitas yang tergabung dalam anime tersebut.

2. **rating.csv** - Informasi rating anime dari pengguna dengan total 7.712.252 entri:
   - `user_id`: ID pengguna yang dihasilkan secara acak dan tidak dapat diidentifikasi.
   - `anime_id`: ID anime yang dinilai oleh pengguna.
   - `rating`: Rating yang diberikan oleh pengguna terhadap anime (skala 1-10, dengan `-1` berarti pengguna telah menonton tetapi tidak memberikan rating).

Dataset ini memberikan wawasan penting tentang preferensi pengguna dan dapat digunakan untuk membangun sistem rekomendasi anime berbasis Collaborative Filtering.

Dengan dataset ini, kita bisa mengembangkan sistem rekomendasi yang dapat menyarankan anime yang sesuai dengan selera pengguna berdasarkan data rating dan interaksi antar pengguna.

### Exploratory Data Analysis (EDA)

#### Analisis Dataset Anime

Dataset anime terdiri dari dua file utama, yaitu dataset `anime.csv` yang berisi informasi tentang anime, dan dataset `rating.csv` yang berisi rating yang diberikan oleh pengguna terhadap anime. Berdasarkan hasil eksplorasi:

- Mayoritas anime dalam dataset hanya memiliki sedikit rating, dengan sebagian besar anime hanya mendapatkan 1 hingga 5 rating.
- Terdapat sejumlah anime yang tidak memiliki rating sama sekali, yang dikenal sebagai "cold items".
- Sebagian kecil anime sangat populer dan mengumpulkan sebagian besar rating, sementara banyak anime lainnya memiliki jumlah rating yang sangat rendah.

#### Analisis Dataset Rating

Dataset rating berisi lebih dari 100.000 entri dengan informasi tentang pengguna dan rating yang diberikan pada anime. Berdasarkan distribusi rating, ditemukan bahwa:

- Mayoritas pengguna hanya memberikan sedikit rating, dengan distribusi rating per pengguna menunjukkan sebagian besar pengguna memberikan kurang dari 100 rating.
- Sebagian besar rating yang diberikan berada di kisaran nilai tinggi (misalnya, nilai 8 dan 9), namun terdapat juga distribusi yang lebih luas yang mencakup nilai rendah.

#### Analisis Interaksi Pengguna dan Anime

Dari hasil analisis interaksi, ditemukan bahwa:

1. Sebagian besar pengguna hanya memberikan rating pada sedikit anime. Ini mengindikasikan sparsity yang tinggi dalam dataset, di mana tidak semua pengguna memberikan feedback untuk banyak anime.
2. Banyak anime yang hanya menerima sedikit rating, dengan sedikit anime yang mendapatkan puluhan hingga ratusan rating. Hal ini menunjukkan bahwa sebagian besar anime adalah "cold items" yang tidak mendapat perhatian cukup dari pengguna.

![Distribusi Jumlah Rating per Pengguna](https://i.imgur.com/Y95WSRK.png)

![Distribusi Rating per Anime](https://i.imgur.com/EyIvHqw.png)

### Kesimpulan Eksplorasi

Dari eksplorasi data yang dilakukan, beberapa insight yang dapat diambil adalah:

1. Dataset anime cukup besar dengan berbagai macam anime, namun dengan sparsity tinggi, karena banyak anime yang memiliki sedikit rating.
2. Mayoritas anime diterbitkan dan dirilis dengan popularitas yang terbatas, dengan hanya sedikit anime yang mengumpulkan banyak rating.
3. Terdapat ketidakseimbangan yang jelas dalam distribusi rating, dengan banyak rating yang diberikan pada anime populer, sementara banyak anime lainnya tidak mendapat banyak perhatian.
4. Sebagian besar pengguna hanya memberikan rating pada sedikit anime, yang menciptakan sparsity tinggi dalam dataset.
5. Beberapa anime berfungsi sebagai "cold items" karena hampir tidak mendapatkan feedback dari pengguna.


### Missing Values Analysis

Berdasarkan analisis missing values:

1. **Dataset Anime**: Kolom anime_id, name, episodes, dan members tidak memiliki nilai yang hilang, sementara genre memiliki 62 nilai yang hilang, type memiliki 25 nilai yang hilang, dan rating memiliki 222 nilai yang hilang.

2. **Dataset Ratings**: Tidak ditemukan missing values pada kolom user_id, anime_id, dan rating.


## Data Preparation

Beberapa tahapan data preparation yang dilakukan dalam proyek ini:

### 1. Penanganan Missing Values
#### 1.1 Penanganan Missing Values pada Dataset Anime
```python
anime_df = anime_df.dropna(subset=['genre', 'type', 'rating'])
```
Menghapus baris anime yang memiliki nilai kosong pada atribut penting seperti genre, type, atau rating. Hal ini dilakukan karena:
- Informasi genre dan type sangat penting untuk content-based filtering dan model rekomendasi.
- Jumlah missing values pada kolom genre (62 nilai hilang), type (25 nilai hilang), dan rating (222 nilai hilang) relatif kecil dibandingkan dengan total data.
- Penghapusan tidak akan berdampak signifikan terhadap kualitas dataset atau model rekomendasi yang dibangun.

### 2. Memproses Dataset Rating
Dataset asli memiliki lebih dari **6 juta** entri setelah menghapus data dengan **rating -1**. Namun, karena dataset yang sangat besar, proses komputasi bisa menjadi sangat lambat. Oleh karena itu, untuk efisiensi pemrosesan, dataset dipangkas menjadi **1.3 juta** entri dengan mengambil **sampel acak**. Detailnya adalah sebagai berikut:
- **Penghapusan Rating -1** mengurangi ukuran dataset dari 7 juta entri menjadi sekitar 6.2 juta entri.
- **Sampling acak** menghasilkan dataset berukuran 1.3 juta entri, yang lebih efisien untuk pemrosesan lebih lanjut.
- **Pencocokan `anime_id`** memastikan bahwa tidak ada **`anime_id`** yang hilang di antara dua dataset (rating dan anime).

#### 2.1 Reset Index
```python
# Reset index sehingga index dimulai dari 0
rating_df = rating_df.reset_index(drop=True)

# Cek hasil setelah reset index
rating_df.head()
```
```python
# Reset index sehingga index dimulai dari 0
anime_df = anime_df.reset_index(drop=True)

# Cek hasil setelah reset index
anime_df.head()
```
Melakukan **reset index** pada kedua dataframe, yaitu `ratings_df` dan `anime_df`, untuk memastikan bahwa indeks dimulai dari 0 dan berlanjut secara berturut-turut tanpa ada kesenjangan. Ini penting untuk memastikan bahwa data lebih mudah dikelola selama pemrosesan lebih lanjut, terutama saat melakukan operasi seperti pemilahan data atau penggabungan dataset.


### 3. Filter Data

Mengkonversi **episodes** ke tipe numerik dan menangani nilai yang tidak valid dengan coercion. Memfilter anime berdasarkan jumlah episode yang masuk akal (misalnya, lebih dari 0 episode). Kemudian, memfilter dataset **Rating** untuk hanya menyertakan rating eksplisit (nilai > 0). Rating eksplisit ini mencerminkan preferensi pengguna secara lebih akurat, sedangkan rating 0 menunjukkan bahwa pengguna belum memberikan rating atau tidak memiliki preferensi terkait anime tersebut.

Selanjutnya, untuk memperoleh data yang relevan, kita menghitung jumlah rating yang diberikan oleh setiap pengguna dan untuk setiap anime. Dataset kemudian difilter untuk hanya menyertakan pengguna yang memberikan minimal 5 rating dan anime yang menerima minimal 5 rating. Hal ini bertujuan untuk memastikan bahwa hanya data yang cukup representatif yang digunakan dalam analisis sistem rekomendasi.

### 4. Encoding User-ID dan Anime-ID

Membuat mapping bidirectional antara **user_id** dan **anime_id** dengan indeks numerik (0, 1, 2, ...) untuk mempermudah pembuatan matriks dan komputasi dalam model. Mapping ini memungkinkan kita untuk mengonversi ID asli ke indeks numerik yang lebih efisien untuk operasi matriks dan juga memungkinkan konversi kembali dari indeks ke ID asli untuk interpretasi hasil.

#### Membuat Mapping Dictionary
```python
user_to_idx = {user: idx for idx, user in enumerate(filtered_ratings['user_id'].unique())}
anime_to_idx = {anime: idx for idx, anime in enumerate(filtered_ratings['anime_id'].unique())}

idx_to_user = {idx: user for user, idx in user_to_idx.items()}
idx_to_anime = {idx: anime for anime, idx in anime_to_idx.items()}
```

#### Konversi ID ke Indeks
```python
filtered_ratings['user_idx'] = filtered_ratings['user_id'].map(user_to_idx)
filtered_ratings['anime_idx'] = filtered_ratings['anime_id'].map(anime_to_idx)
```

Menambahkan kolom baru dengan indeks numerik untuk **user_id** dan **anime_id** untuk mempermudah komputasi. Langkah ini dilakukan karena model machine learning membutuhkan input numerik, dan indeks berurutan (0, 1, 2, ...) lebih efisien untuk operasi matriks.


### 5. Pembagian Dataset
```python
train_data, test_data = train_test_split(
    filtered_ratings,
    test_size=0.2,
    random_state=42,
    stratify=filtered_ratings['rating']  # Stratify berdasarkan nilai rating
)
```
Memisahkan data menjadi set pelatihan (80%) dan pengujian (20%) untuk menghindari overfitting dan mengukur performa model secara adil.

### 6. Normalisasi Rating
#### 6.1 Menghitung Rata-rata Rating per Pengguna
```python
user_mean = train_data.groupby('user_idx')['rating'].mean()
```
#### 6.2 Fungsi Normalisasi
```python
def normalize_ratings(df):
    df_copy = df.copy()
    for user in df_copy['user_idx'].unique():
        if user in user_mean:
            user_mask = df_copy['user_idx'] == user
            df_copy.loc[user_mask, 'normalized_rating'] = df_copy.loc[user_mask, 'rating'] - user_mean[user]
    return df_copy

train_data = normalize_ratings(train_data)
test_data = normalize_ratings(test_data)
```
Menormalisasi rating dengan mengurangkan rata-rata rating pengguna dari setiap rating yang diberikan. Untuk mengatasi bias pengguna yang cenderung memberikan rating tinggi atau rendah secara konsisten, membuat skala rating menjadi relatif terhadap preferensi masing-masing pengguna, dan meningkatkan akurasi prediksi dalam collaborative filtering.


## Modeling

Dalam proyek ini, tiga pendekatan collaborative filtering diimplementasikan dan dibandingkan:

### 1. Pre-komputasi Matriks untuk Efisiensi
Untuk meningkatkan efisiensi dalam perhitungan dan prediksi, dilakukan **pre-komputasi** pada beberapa matriks penting, yaitu matriks **user-item**, matriks kesamaan antar pengguna, dan matriks kesamaan antar anime. Tujuan utama dari pre-komputasi ini adalah untuk menghindari perhitungan matriks yang berulang setiap kali fungsi rekomendasi dipanggil, yang dapat mengurangi waktu komputasi secara signifikan.

Matriks **user-item** digunakan untuk menyusun data interaksi pengguna dengan anime, di mana baris mewakili pengguna, kolom mewakili anime, dan nilai di dalamnya menunjukkan rating yang diberikan oleh pengguna terhadap anime tersebut. Matriks ini sangat penting dalam pengembangan model rekomendasi berbasis kolaboratif, baik untuk menghitung kesamaan antar pengguna maupun item.
```python
def build_user_item_matrix():
    # Create user-item matrix from training data
    user_item_matrix = pd.pivot_table(
        train_data,
        values='rating',
        index='user_idx',
        columns='anime_idx',
        fill_value=0
    )
    return user_item_matrix
```

### 2. User-Based Collaborative Filtering
User-based Collaborative Filtering (CF) mencari pengguna dengan preferensi yang mirip dengan pengguna target, kemudian memberikan rekomendasi berdasarkan item yang disukai oleh pengguna serupa.

#### Prinsip Kerja:
- **Mengukur kesamaan antar pengguna** menggunakan **cosine similarity**.
- **Prediksi rating** dihitung berdasarkan rating dari pengguna dengan preferensi serupa.
- Merekomendasikan item dengan **prediksi rating tertinggi**.

#### Proses Rekomendasi:
1. **Validasi `user_id`**: Memastikan apakah `user_id` ada dalam dataset.
2. **Menampilkan anime dengan rating tertinggi** dari pengguna sebagai gambaran preferensi.
3. **Menghitung similarity** antara pengguna target dan pengguna lainnya menggunakan matriks **user-item**.
4. **Prediksi rating** untuk anime yang belum dirating berdasarkan **weighted average** dari rating pengguna serupa.
5. **Mengurutkan dan memilih N rekomendasi teratas** berdasarkan prediksi rating tertinggi.

### 3. Item-Based Collaborative Filtering
Item-based Collaborative Filtering (CF) mengukur kesamaan antar item (dalam hal ini anime) berdasarkan pola rating yang diberikan oleh pengguna.

#### Prinsip Kerja:
- Menghitung kesamaan antar item menggunakan **cosine similarity**.
- Untuk pengguna target, memprediksi rating pada anime yang belum dirating berdasarkan kesamaan dengan anime yang sudah dirating.
- Merekomendasikan anime dengan **prediksi rating tertinggi**.

#### Proses Rekomendasi:
1. **Validasi `user_id`**: Memastikan bahwa `user_id` tersedia dalam dataset.
2. **Menampilkan anime dengan rating tertinggi** dari pengguna untuk memahami preferensi.
3. **Mengidentifikasi anime yang sudah dirating** oleh pengguna target.
4. **Prediksi rating** pada anime yang belum dirating menggunakan **weighted average** dari rating terhadap anime serupa.
5. **Mengurutkan hasil prediksi** dan menampilkan **N rekomendasi teratas**.


### 4. Model-Based Collaborative Filtering dengan SVD
**Singular Value Decomposition (SVD)** adalah metode pemfaktoran matriks yang digunakan untuk mengidentifikasi pola tersembunyi dalam data rating. Dengan menggunakan **SVD**, kita dapat mendekomposisi matriks **user-item** menjadi tiga matriks terpisah: faktor pengguna (**U**), nilai singular (**Σ**), dan faktor item (**V**). Ini membantu dalam memprediksi rating pengguna terhadap item yang belum mereka beri rating.

#### Prinsip Kerja:
- **Matriks User-Item** dipecah menjadi tiga matriks: **U** (faktor pengguna), **Σ** (nilai singular), dan **V** (faktor item).
- **Faktor pengguna (U)** menggambarkan karakteristik pengguna dalam dimensi laten, dan **faktor item (V)** menggambarkan karakteristik item.
- Prediksi rating dihitung berdasarkan dekomposisi matriks yang dihasilkan.

#### Proses Penggunaan SVD:
1. **Data Preparation**: Data rating dikonversi ke format yang dapat diproses oleh library **Surprise**.
2. **Training Model**: Model **SVD** dilatih menggunakan data training dengan parameter: `n_factors=100`, `n_epochs=20`, `lr_all=0.005`, dan `reg_all=0.02`.
3. **Prediksi Rating**: Untuk setiap anime yang belum dirating, prediksi rating dihitung menggunakan model **SVD**.
4. **Rekomendasi**: Rekomendasi diberikan berdasarkan prediksi rating tertinggi.

### 5. Fungsi Prediksi untuk Model User-Based dan Item-Based
Untuk meningkatkan efisiensi dalam prediksi rating, kami mengimplementasikan dua fungsi prediksi yang lebih efisien untuk model **User-Based** dan **Item-Based** Collaborative Filtering. Kedua fungsi ini menggunakan matriks yang telah dikomputasi sebelumnya untuk menghindari perhitungan ulang, yang mempercepat proses rekomendasi dan meminimalkan penggunaan sumber daya komputasi.

#### 5.1. User-Based Prediction Function
Fungsi **User-Based Prediction** memprediksi rating untuk pasangan **user-item** berdasarkan kesamaan antar pengguna. Proses prediksi dilakukan dengan menghitung skor kemiripan antara pengguna target dan pengguna lain dalam dataset, kemudian menggunakan **weighted average** dari rating yang diberikan oleh pengguna yang mirip.

##### Langkah-langkah:
1. **Cek Validitas Indeks**: Memeriksa apakah indeks pengguna dan item ada dalam matriks.
2. **Menghitung Similarity**: Menghitung skor kesamaan antara pengguna target dan semua pengguna lain menggunakan matriks **user-item**.
3. **Prediksi Rating**: Untuk item yang belum dirating oleh pengguna target, fungsi memprediksi rating berdasarkan **weighted average** rating dari pengguna yang memiliki kesamaan preferensi.

#### 5.2. Item-Based Prediction Function
Fungsi **Item-Based Prediction** memprediksi rating untuk pasangan **user-item** berdasarkan kesamaan antar item. Fungsi ini menggunakan matriks **item-user** untuk menghitung kesamaan antar anime berdasarkan rating yang telah diberikan oleh pengguna.

##### Langkah-langkah:
1. **Cek Validitas Indeks**: Memeriksa apakah indeks pengguna dan item ada dalam matriks.
2. **Prediksi Rating**: Untuk setiap item yang belum dirating, fungsi menghitung weighted average dari rating yang diberikan pengguna pada item-item yang serupa.

### 6. Rekomendasi Berdasarkan Popularitas
Membuat sistem rekomendasi berdasarkan popularitas anime dengan menghitung jumlah rating dan rata-rata rating untuk setiap anime. Filter diterapkan untuk hanya memasukkan anime yang memiliki minimal 10 rating agar hasil rekomendasi lebih dapat diandalkan. Anime kemudian diurutkan berdasarkan rating rata-rata tertinggi untuk memberikan rekomendasi anime terpopuler.

### 7. Testing dan Perbandingan Ketiga Model
Untuk memberikan gambaran yang komprehensif tentang performa ketiga model dalam menghasilkan rekomendasi yang beragam, dilakukan testing terhadap semua model menggunakan pengguna yang sama. Hal ini memungkinkan perbandingan langsung antara hasil rekomendasi dari **SVD**, **User-based Collaborative Filtering**, dan **Item-based Collaborative Filtering**.

### 8. Implementasi Fungsi Perbandingan
Untuk membandingkan hasil rekomendasi dari ketiga model, kami mengembangkan fungsi **`compare_all_recommendations()`**. Fungsi ini menjalankan ketiga algoritma rekomendasi secara berurutan untuk pengguna yang sama, sehingga memungkinkan analisis perbedaan hasil rekomendasi yang diberikan oleh masing-masing metode. Fungsi ini akan menampilkan output dari ketiga model dalam format yang terstruktur dan mudah dibandingkan.

### 8.1. Fungsi `compare_all_recommendations()`
Fungsi ini pertama-tama akan memanggil masing-masing model rekomendasi (SVD, User-based CF, dan Item-based CF) untuk menghasilkan rekomendasi anime berdasarkan **user_id** yang sama. Setelah itu, hasil dari ketiga model akan ditampilkan dan dibandingkan. Fungsi ini juga mengembalikan hasil rekomendasi dalam bentuk DataFrame, yang memudahkan analisis lebih lanjut atau evaluasi performa relatif antar model dalam memberikan rekomendasi yang relevan.


### 8.2. Testing Semua Model untuk Pengguna yang Sama
Fungsi ini dijalankan untuk **user\_id** yang dipilih secara acak, dan hasil rekomendasi dari ketiga model akan dibandingkan. Hasil rekomendasi dari masing-masing model ditampilkan dengan struktur yang terorganisir untuk memudahkan analisis.

### 9. Contoh Hasil Perbandingan untuk User `43074`
#### Profil Pengguna (Anime dengan Rating Tinggi):
Pengguna dengan ID `43074` memiliki preferensi tinggi terhadap anime dengan rating tinggi sebagai berikut:
- **Natsume Yuujinchou: Itsuka Yuki no Hi ni**: Rating 9
- **Evangelion: 1.0 You Are (Not) Alone**: Rating 9
- **Free!: Eternal Summer**: Rating 9
- **Love Stage!! OVA**: Rating 8
- **Love Stage!!**: Rating 8

#### Hasil Rekomendasi dari Ketiga Model:
##### 1. **SVD Model**:
SVD memberikan rekomendasi berdasarkan pemfaktoran matriks dengan mengidentifikasi pola tersembunyi dalam data rating. Berikut adalah hasil rekomendasi untuk **User 43074**:
- Kimi no Na wa.
- Fullmetal Alchemist: Brotherhood
- Gintama°
- Steins;Gate
- Hunter x Hunter (2011)
- Ginga Eiyuu Densetsu
- Shigatsu wa Kimi no Uso
- Hajime no Ippo
- Mushishi Zoku Shou
- Neon Genesis Evangelion

###### Kelebihan SVD:
- Efisiensi komputasi tinggi, waktu training dan prediksi cepat.
- Mengatasi masalah sparsity dengan baik dan mengisi missing values.
- Menemukan pola tersembunyi dalam data.
- Dapat menangani dataset besar dengan efisien.
- Generalisasi yang baik pada pola kompleks dalam data.

###### Kekurangan SVD:
- Sulit untuk diinterpretasi (bersifat "black box").
- Risiko overfitting jika tidak diatur dengan baik.
- Membutuhkan tuning hyperparameter yang cermat.
- Masalah cold start, kesulitan merekomendasikan item atau pengguna baru.


##### 2. **User-Based Collaborative Filtering**:
Model ini memberikan rekomendasi berdasarkan kesamaan antar pengguna. Berikut adalah hasil rekomendasi untuk **User 43074**:
- One Piece Film: Gold
- Taiyou no Ko Esteban
- Meitantei Holmes
- Saint Seiya: Soushuuhen
- Sayonara Ginga Tetsudou 999: Andromeda Shuuchakueki
- El Hazard: The Magnificent World
- Kyoro-chan
- Pokemon the Movie XY&Z: Volcanion to Karakuri no Magiana
- Candy Candy (Movie)
- Time Bokan Series: Yattodetaman

###### Kelebihan User-based CF:
- Mudah dipahami dan diinterpretasi.
- Personalisasi tinggi, berdasarkan pengguna dengan preferensi serupa.
- Dapat menemukan item niche yang tidak populer.
- Tidak memerlukan informasi konten, hanya menggunakan data interaksi pengguna.

###### Kekurangan User-based CF:
- Kesulitan menangani sparsity, terutama data rating yang sangat jarang.
- Skalabilitas menurun seiring bertambahnya jumlah pengguna.
- Masalah cold start untuk pengguna baru.
- Perhitungan similarity yang intensif dalam waktu nyata.


##### 3. **Item-Based Collaborative Filtering**:
Model ini memberikan rekomendasi berdasarkan kesamaan antar item. Berikut adalah hasil rekomendasi untuk **User 43074**:
- Unico
- Nils no Fushigi na Tabi (Movie)
- Hikari no Megami
- Saishuu Shiken Kujira Progressive
- Cutey Honey Flash: The Movie
- Gun-dou Musashi
- Shin Calimero
- Iczer-Girl Iczelion
- Hyper-Psychic Geo Garaga
- The Bathroom

###### Kelebihan Item-based CF:
- Stabilitas tinggi, item similarity lebih stabil dibandingkan user similarity.
- Mudah diinterpretasi: "Karena Anda menyukai X, maka Anda mungkin suka Y".
- Similarity dihitung secara offline (pre-computation), meningkatkan efisiensi.
- Skalabilitas lebih baik karena jumlah item lebih sedikit dibandingkan jumlah pengguna.

###### Kekurangan Item-based CF:
- Kurang keberagaman dalam rekomendasi (filter bubble).
- Popularitas bias, item populer cenderung lebih sering direkomendasikan.
- Masalah cold start untuk item baru dalam sistem.
- Meskipun lebih sedikit, masih ada masalah sparsity.


### Analisis Perbedaan Rekomendasi
Dari hasil pengujian terhadap ketiga model, terdapat beberapa pola yang dapat diamati:

1. **Keberagaman Rekomendasi**:
Setiap model menghasilkan daftar rekomendasi yang berbeda, di mana masing-masing algoritma menangkap preferensi pengguna dari perspektif yang unik.

2. **Konsistensi Genre**:
SVD cenderung memberikan rekomendasi anime dengan genre yang lebih beragam, seperti drama, aksi, dan fiksi ilmiah. User-based CF lebih terfokus pada genre yang disukai oleh pengguna serupa, dengan kecenderungan pada genre seperti sport dan petualangan. Item-based CF menyarankan kombinasi anime populer dan klasik.

3. **Minimal Overlap**:
Terdapat sedikit tumpang tindih antara rekomendasi dari ketiga model, yang menunjukkan bahwa masing-masing pendekatan memiliki keunggulan dalam menangkap preferensi pengguna yang berbeda.

4. **Kualitas Rekomendasi**:
Walaupun SVD secara kuantitatif menunjukkan performa terbaik, ketiga model tetap memberikan rekomendasi yang relevan dan masuk akal berdasarkan profil pengguna yang diuji.


## Evaluation

Untuk mengevaluasi performa model sistem rekomendasi, digunakan tiga model utama yaitu User-based CF, Item-based CF, dan SVD. Evaluasi dilakukan dengan berbagai metrik seperti RMSE, MAE, NDCG.

### 1. Root Mean Square Error (RMSE) dan Mean Absolute Error (MAE)

RMSE dan MAE digunakan untuk mengukur akurasi prediksi rating. Semakin kecil nilai RMSE dan MAE, semakin akurat model dalam memprediksi rating.

Hasil evaluasi:
- **SVD**           : RMSE: 1.2485, MAE: 0.9521
- **User-based CF** : RMSE: 1.7076, MAE: 1.3457
- **Item-based CF** : RMSE: 1.6805, MAE: 1.3163

### 2. Normalized Discounted Cumulative Gain (NDCG)

NDCG mengukur kualitas ranking dari sistem rekomendasi dengan mempertimbangkan posisi item yang relevan dalam daftar rekomendasi.

Hasil evaluasi:
- **SVD**           : NDCG: 0.4869
- **User-based CF** : NDCG: 0.4861
- **Item-based CF** : NDCG: 0.4854

### 3. Perbandingan Model

Hasil perbandingan ketiga model:

| Model           | RMSE   | MAE    |   NDCG   | 
|-----------------|--------|--------|----------|
| SVD             | 1.2485 | 0.9521 |  0.4869  |
| User-based CF   | 1.7076 | 1.3457 |  0.4861  |
| Item-based CF   | 1.6805 | 1.3163 |  0.4854  |


### Analisis Hasil Evaluasi

Berdasarkan hasil evaluasi menyeluruh terhadap ketiga model, berikut adalah kesimpulannya:

1. **Akurasi (RMSE dan MAE)**: Model SVD menunjukkan akurasi terbaik dengan nilai RMSE sebesar 1.2485 dan MAE 0.9521, lebih baik dibandingkan dengan User-based CF dan Item-based CF yang memiliki nilai yang lebih tinggi pada keduanya.

2. **Kualitas Ranking (NDCG)**: Model SVD juga unggul dalam hal kualitas ranking, dengan NDCG sebesar 0.4869, mengungguli kedua model memory-based CF yang memiliki NDCG sedikit lebih rendah.

Secara keseluruhan, model SVD menunjukkan performa superior dalam sebagian besar aspek evaluasi, baik dari segi akurasi prediksi, kualitas ranking, maupun efisiensi komputasi.

### Kelebihan dan Kekurangan Model
#### Kelebihan
1. SVD mampu menangkap pola tersembunyi dalam data yang mungkin tidak terdeteksi oleh pendekatan memory-based CF.
2. Sistem rekomendasi berbasis kolaboratif dapat menyarankan anime yang mungkin tidak langsung berhubungan dengan anime yang sudah disukai pengguna.
3. Penggunaan fallback berbasis popularitas membantu mengatasi masalah cold-start dengan merekomendasikan item populer untuk pengguna atau item baru.

#### Kekurangan
1. Masalah sparsity menjadi tantangan besar, terutama pada memory-based CF yang bergantung pada kesamaan rating antar pengguna.
2. Collaborative filtering murni tidak memanfaatkan informasi konten seperti genre atau metadata lainnya, yang bisa meningkatkan relevansi rekomendasi.
3. Sistem ini tidak mempertimbangkan dinamika perubahan preferensi pengguna seiring waktu, yang bisa mempengaruhi akurasi rekomendasi jangka panjang.

## Evaluasi terhadap Business Understanding
### 1. Problem Statement
#### 1.1 Identifikasi Anime yang Mungkin Disukai Pengguna
Model SVD berhasil mengidentifikasi anime yang sesuai dengan preferensi pengguna dengan nilai RMSE 1.2485 dan MAE 0.9521, yang menunjukkan akurasi prediksi yang sangat baik. Sistem dapat memprediksi rating dengan error rata-rata hanya 0.95 poin dari skala 1-10, yang cukup akurat untuk memberikan rekomendasi yang relevan kepada pengguna.

#### 1.2 Mengatasi Masalah Cold Start untuk Pengguna Baru
Penerapan rekomendasi berbasis popularitas efektif dalam mengatasi masalah cold start bagi pengguna baru yang belum memberikan rating. Meskipun pendekatan ini tidak sepenuhnya dipersonalisasi, ia memberikan rekomendasi yang masuk akal dengan mempertimbangkan preferensi komunitas secara keseluruhan, sehingga tetap fungsional meskipun tidak ada data historis pengguna.

#### 1.3 Mengukur Efektivitas Sistem Rekomendasi
Evaluasi menggunakan metrik RMSE, MAE, dan NDCG berhasil memberikan pemahaman yang komprehensif mengenai efektivitas model. Hal ini memungkinkan pemilihan model terbaik berdasarkan data objektif yang memperhitungkan berbagai aspek kinerja sistem rekomendasi.

### 2. Goals
#### 2.1 Mengembangkan Sistem Rekomendasi yang Akurat
Model SVD terbukti paling efektif dengan performa unggul dalam hal akurasi prediksi (RMSE: 1.2485) dan kualitas ranking (NDCG: 0.4869). Sistem berhasil mempersonalisasi rekomendasi berdasarkan pola preferensi pengguna serta kesamaan dengan pengguna lain yang memiliki minat serupa.

#### 2.2 Membuat Strategi Mengatasi Cold Start
Pendekatan berbasis popularitas berhasil diimplementasikan untuk mengatasi cold start dengan memberikan rekomendasi anime populer kepada pengguna baru. Ini memastikan sistem tetap memberikan rekomendasi yang bermanfaat meskipun tidak ada data historis pengguna.

#### 2.3 Mengevaluasi Kinerja dengan Metrik yang Sesuai
Evaluasi multi-metrik yang meliputi RMSE, MAE, dan NDCG memberikan gambaran menyeluruh mengenai kinerja model, dari segi akurasi, relevansi, dan kualitas ranking, yang memungkinkan penilaian model secara lebih komprehensif.

### 3. Solution Approach
#### 3.1 Collaborative Filtering
* **User-based CF**: Memberikan baseline dengan nilai RMSE 1.7076, tetapi tidak cukup efektif dalam menemukan preferensi pengguna.
* **Item-based CF**: Menunjukkan kemampuan yang baik untuk mengidentifikasi item yang relevan berdasarkan kesamaan antar item.
* **SVD**: Memberikan kombinasi terbaik dari akurasi prediksi (RMSE: 1.2485) dan kualitas ranking (NDCG: 0.4869), menjadikannya pilihan terbaik untuk sistem rekomendasi.

### 4. Dampak Bisnis
* **Mengurangi Information Overload**: Dengan akurasi prediksi yang sangat baik (RMSE 1.2485), sistem ini efektif membantu pengguna menemukan anime yang sesuai dari ribuan pilihan yang ada.
* **Meningkatkan User Experience**: NDCG yang tinggi (0.4869) menunjukkan kualitas ranking yang baik, memastikan bahwa anime yang paling relevan muncul di posisi teratas dalam rekomendasi.
* **Skalabilitas dan Efisiensi**: Model SVD memungkinkan implementasi real-time pada platform dengan jutaan pengguna, tanpa mengorbankan performa.
* **Adaptabilitas untuk Pengguna Baru**: Pendekatan berbasis popularitas membantu memastikan bahwa sistem tetap memberikan rekomendasi yang relevan bagi pengguna baru yang belum memberikan rating.

## Kesimpulan
Dari proyek sistem rekomendasi anime ini, dapat disimpulkan:
1. Sistem rekomendasi berbasis **Collaborative Filtering** terbukti efektif dalam memberikan rekomendasi anime yang sesuai dengan preferensi pengguna, dengan model **SVD** memberikan performa terbaik.
2. **Matrix Factorization dengan SVD** lebih unggul dibandingkan dengan **memory-based CF** dalam hal akurasi dan efisiensi komputasi, terutama untuk dataset besar.
3. Pendekatan **Item-based CF** lebih baik dibandingkan dengan **User-based CF**, menunjukkan bahwa pola preferensi terhadap anime lebih konsisten dibandingkan pola preferensi antar pengguna.
4. Masalah **cold-start** dapat diatasi dengan menggunakan rekomendasi berbasis popularitas, meskipun pendekatan ini tidak sepenuhnya memberikan rekomendasi yang dipersonalisasi.
5. Tantangan utama dalam sistem rekomendasi anime adalah masalah **sparsity data**, di mana mayoritas pengguna hanya memberikan rating untuk sebagian kecil dari keseluruhan koleksi anime yang ada.
