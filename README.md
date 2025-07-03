# Sistem Rekomendasi Anime

## Deskripsi Proyek

Proyek ini mengimplementasikan sistem rekomendasi untuk anime menggunakan teknik **Collaborative Filtering** untuk memprediksi dan menyarankan anime yang mungkin disukai oleh pengguna berdasarkan penilaian dan preferensi mereka. Sistem ini membantu pengguna menavigasi pilihan anime yang sangat banyak, memberikan saran yang relevan dan disesuaikan dengan selera individu.

## Pemahaman Bisnis

### Pernyataan Masalah

1. Bagaimana cara mengidentifikasi anime yang mungkin disukai oleh pengguna berdasarkan penilaian mereka sebelumnya dan penilaian dari pengguna lain yang memiliki preferensi serupa?
2. Bagaimana cara mengatasi masalah *cold start* bagi pengguna baru yang belum memberikan penilaian pada anime?
3. Bagaimana cara mengevaluasi efektivitas sistem rekomendasi anime yang dikembangkan?

### Tujuan

1. Mengembangkan sistem rekomendasi anime yang dapat memprediksi anime yang mungkin disukai oleh pengguna berdasarkan pola penilaian mereka dan preferensi pengguna lain yang serupa.
2. Mengatasi masalah *cold start* dengan memberikan rekomendasi anime populer atau yang memiliki rating tinggi untuk pengguna baru.
3. Mengevaluasi kinerja sistem rekomendasi dengan menggunakan metrik yang sesuai seperti RMSE, MAE, dan NDCG.

## Pendekatan Solusi

Sistem rekomendasi ini didasarkan pada **Collaborative Filtering**, yaitu:

- **User-based Collaborative Filtering**: Mencari pengguna dengan preferensi yang mirip dan memberikan rekomendasi berdasarkan anime yang disukai oleh pengguna yang mirip.
- **Item-based Collaborative Filtering**: Memberikan rekomendasi berdasarkan kesamaan antara anime yang telah dinilai oleh pengguna.
- **Model-based Collaborative Filtering (SVD)**: Menggunakan teknik seperti Singular Value Decomposition (SVD) untuk mengidentifikasi pola tersembunyi dalam interaksi pengguna-anime.

## Pemahaman Data

Dataset yang digunakan untuk proyek ini berasal dari Kaggle dan berisi informasi tentang penilaian anime dan preferensi pengguna. Dataset ini telah dimodifikasi untuk menghapus genre yang tidak relevan dengan fokus penelitian.

**File Dataset Utama:**

1. **anime.csv** - Berisi informasi tentang anime (nama, genre, tipe, rating, episode).
2. **rating.csv** - Berisi rating yang diberikan oleh pengguna untuk anime.

### Eksplorasi Data (EDA)

- Dataset berisi lebih dari 7 juta entri.
- Sebagian besar anime memiliki sedikit rating, dengan beberapa anime yang tidak memiliki rating sama sekali (cold items).
- Sebagian besar rating yang diberikan lebih cenderung pada nilai tinggi.

### Analisis Nilai Hilang

- **anime.csv**: Beberapa kolom memiliki nilai hilang, terutama pada genre, tipe, dan rating.
- **rating.csv**: Tidak ditemukan nilai hilang pada dataset ini.

## Persiapan Data

1. **Penanganan Nilai Hilang**: Menghapus baris dengan data kosong pada kolom yang penting.
2. **Penyaringan dan Pengkodean**: Menggunakan ID pengguna dan anime dalam bentuk numerik untuk efisiensi pemrosesan.
3. **Normalisasi**: Menormalkan rating untuk mengatasi bias pengguna dalam memberikan rating.

## Pemodelan

Tiga pendekatan **Collaborative Filtering** diimplementasikan dan dibandingkan:

1. **User-Based Collaborative Filtering**
2. **Item-Based Collaborative Filtering**
3. **Model-Based Collaborative Filtering menggunakan SVD**

Setiap metode dievaluasi menggunakan metrik seperti RMSE, MAE, dan NDCG.

## Evaluasi

- **RMSE** dan **MAE** digunakan untuk mengukur akurasi prediksi rating.
- **NDCG** digunakan untuk mengevaluasi kualitas peringkat dari daftar rekomendasi.

### Hasil:

| Model           | RMSE   | MAE    | NDCG   | 
|-----------------|--------|--------|--------|
| SVD             | 1.2485 | 0.9521 | 0.4869 |
| User-based CF   | 1.7076 | 1.3457 | 0.4861 |
| Item-based CF   | 1.6805 | 1.3163 | 0.4854 |

## Kesimpulan

Model **SVD** memberikan hasil terbaik dalam hal akurasi dan kualitas peringkat. Ini adalah pendekatan yang paling efisien dan efektif untuk sistem rekomendasi anime ini. Model **User-based CF** dan **Item-based CF** juga memberikan rekomendasi yang relevan, namun memiliki keterbatasan dalam menangani masalah sparsity dan cold-start.
