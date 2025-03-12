# Predictive-Analytics-Dicoding
Oleh: Noer Hanifah Suganda

# A. Latar Belakang

Perusahaan e-commerce UK yang berbasis di London telah beroperasi sejak tahun 2007 dan menawarkan produk berupa hadiah serta homewares untuk berbagai kalangan, baik individu maupun bisnis kecil. Dataset yang digunakan dalam analisis ini mencakup 500 ribu transaksi selama satu tahun dengan 8 kolom informasi.

Meskipun sebagian besar transaksi berjalan lancar, terdapat persentase kecil transaksi yang dibatalkan. Pembatalan ini umumnya terjadi karena kondisi stok yang tidak memadai, sehingga pelanggan memilih untuk membatalkan transaksi agar dapat menerima semua produk sekaligus. Pembatalan transaksi tidak hanya mengganggu alur operasional, tetapi juga berdampak pada pendapatan dan kepuasan pelanggan.

Melihat kondisi tersebut, penting untuk mengidentifikasi faktor-faktor yang mempengaruhi pembatalan transaksi dan mengembangkan model prediksi yang mampu mendeteksi transaksi berisiko pembatalan. Dengan menerapkan pendekatan machine learning, perusahaan dapat mengambil langkah proaktif untuk mengoptimalkan manajemen inventori, mengurangi kerugian finansial, serta meningkatkan pengalaman pelanggan secara keseluruhan.

# B. Business Understanding

Dalam industri e-commerce, pembatalan transaksi merupakan masalah yang signifikan. Transaksi yang dibatalkan—diidentifikasi melalui penanda "C" pada kolom TransactionNo—dapat mengganggu alur operasional, menyebabkan kerugian finansial, dan menurunkan kepuasan pelanggan. Seringkali, pembatalan terjadi karena kondisi stok yang tidak memadai, di mana pelanggan memilih untuk membatalkan transaksi jika tidak semua produk dapat dikirim sekaligus.

**Problem Statement:**

Bagaimana membangun model prediktif untuk mengidentifikasi transaksi yang berisiko tinggi dibatalkan?

**Goals:**

- Membuat model machine learning yang dapat memprediksi pembatalan transaksi.
- Membandingkan beberapa algoritma model untuk menemukan akurasi terbaik dalam memprediksi pembatalan transaksi.

**Solution Statement**

Pendekatan dengan Algoritma Linear (Logistic Regression):
Diharapkan memberikan interpretasi yang mudah dan insight tentang pengaruh tiap fitur.
Namun, solusi ini mungkin kurang efektif dalam menangkap pola non-linear.
Pendekatan dengan Algoritma Non-Linear (Random Forest dan Decision Tree):
Mampu menangkap hubungan non-linear dan interaksi antar fitur dengan lebih baik.
Diharapkan menghasilkan performa prediksi yang lebih tinggi.

# C. Data Understanding

Sumber: https://www.kaggle.com/datasets/gabrielramos87/an-online-shop-business

Dataset ini memiliki lebih dari 500.000 baris data dan 8 kolom. Berikut adalah deskripsi masing-masing kolom:

1. TransactionNo (kategorikal): Merupakan nomor unik enam digit yang mendefinisikan setiap transaksi. Jika terdapat huruf "C" pada kode, artinya transaksi tersebut merupakan pembatalan
2. Date (numerik): Menunjukkan tanggal saat transaksi terjadi.
3. ProductNo (kategorikal): Karakter unik yang terdiri dari lima atau enam digit, digunakan untuk mengidentifikasi produk tertentu.
4. Product (kategorikal): Nama produk atau barang yang dijual.
5. Price (numerik): Harga per unit dari masing-masing produk, dihitung dalam pound sterling (£).
6. Quantity (numerik): Jumlah produk yang dibeli dalam setiap transaksi. Nilai negatif menunjukkan transaksi yang dibatalkan.
7. CustomerNo (kategorikal): Nomor unik yang terdiri dari lima digit untuk mengidentifikasi setiap pelanggan.
8. Country (kategorikal): Nama negara di mana pelanggan tersebut berada.

**Kondisi Data**
- Duplikasi: Terdapat 5200 baris duplikat yang dihapus.
- Missing Values: 55 baris null pada kolom CustomerNo dihapus.
- Tipe Data: Kolom Date awalnya berupa string dan CustomerNo berupa float; kedua kolom ini dikonversi ke tipe yang sesuai.
- Outliers: Outlier di Price dan Quantity diidentifikasi dengan metode IQR dan dihapus menggunakan kombinasi mask.

**Exploratory Data Analysis (EDA)**

Visualisasi & Analisis Univariate:
- Numerik: Histogram dan boxplot untuk fitur Price dan Quantity (serta Date yang dikonversi ke bentuk numerik) digunakan untuk melihat sebaran data dan mendeteksi outlier.
  
![image](https://github.com/user-attachments/assets/7219bd8b-38ac-44c6-8421-fce05330cee6)
![image](https://github.com/user-attachments/assets/d5782c87-346a-47ea-a9cf-11b13129caaa)
![image](https://github.com/user-attachments/assets/068aefc8-4e5e-425d-b815-ae77a650fd59)
![image](https://github.com/user-attachments/assets/84c053a3-b07e-44ae-9cec-9e3402c7bb84)
![image](https://github.com/user-attachments/assets/b4e8f759-1a0e-4b3c-83ac-4e702f348d11)
![image](https://github.com/user-attachments/assets/d2109128-7a95-4434-9c31-b8a872d697b9)

- Kategorikal: Analisis frekuensi (value_counts) untuk fitur seperti ProductName, CustomerNo, TransactionNo, dan Country.

![image](https://github.com/user-attachments/assets/392826e9-2c4d-4efa-8797-c6c4c126c4dd)
![image](https://github.com/user-attachments/assets/c28240ad-50e2-4584-b93f-77880586725c)
![image](https://github.com/user-attachments/assets/c5e259ee-8123-44c8-8156-b2f124943992)

Analisis Target Variable:
Variabel target dibuat, yaitu IsCancelled, yang ditentukan berdasarkan apakah TransactionNo mengandung huruf "C" atau Quantity negatif. Visualisasi distribusi target (countplot) menunjukkan perbandingan transaksi dibatalkan vs. tidak dibatalkan.

![image](https://github.com/user-attachments/assets/70bb9da8-022c-4f26-add8-d950db4168d0)

Analisis Multivariate:
- Heatmap korelasi antara Price dan Quantity menunjukkan hubungan linear yang lemah.

![image](https://github.com/user-attachments/assets/2a424b36-7720-44ff-aab0-a67a404fa8b4)

- Pairplot mengilustrasikan bahwa sebagian besar transaksi normal memiliki Quantity positif, sementara transaksi yang dibatalkan ditandai dengan Quantity negatif.

![image](https://github.com/user-attachments/assets/a187e8ac-e682-48da-bdf7-d2cc461f9578)

# D. Data Preparation

**1. Pembersihan Data:**
Menghapus duplikasi dan baris dengan missing value.

**2. Konversi Tipe Data:**
Kolom Date dikonversi menjadi datetime, CustomerNo dan TransactionNo diubah menjadi string.

**3. Penanganan Outlier:**
Outlier di Price dan Quantity diidentifikasi menggunakan metode IQR dan kemudian baris yang dianggap outlier dihapus.

**4. Pembuatan Label:**
Membuat kolom IsCancelled dengan menetapkan True jika TransactionNo mengandung "C" atau Quantity < 0.

**5. Ekstraksi Fitur:**
Dari kolom Date, diekstraksi fitur Month, Day, dan Weekday.
Ditambahkan fitur tambahan seperti HighPrice (produk dengan harga di atas median).

**6. Encoding Variabel Kategorik:**
Kolom-kolom dengan nilai unik tinggi (ProductName, CustomerNo, ProductNo, Country) di-encode menggunakan frequency encoding. Setelah itu, kolom asli dihapus untuk menghindari high dimensionality.

**7. Scaling Fitur Numerik:**
Menggunakan StandardScaler untuk menstandarisasi fitur numerik agar memiliki skala seragam.

**8.  Splitting Data:**
Data dibagi menjadi training dan testing set dengan stratifikasi pada variabel target.

**9. Handling Imbalanced Data:**
Menggunakan SMOTE untuk oversampling kelas minoritas dan juga Random Undersampler untuk perbandingan.

# E. Model Development & Evaluation

**- Modeling:**
Tiga algoritma utama digunakan:

- Logistic Regression:
Terlihat gagal mendeteksi transaksi dibatalkan, karena semua prediksi model adalah kelas tidak dibatalkan.
- Random Forest Classifier & Decision Tree Classifier (setelah SMOTE):
Kedua model ini menunjukkan performa sempurna pada data testing (100% akurasi, precision, recall, dan f1-score untuk kedua kelas).
- Random Forest dengan Undersampling:
Juga menunjukkan hasil yang sempurna setelah dilakukan undersampling.

**- Evaluasi Model:**
Evaluasi dilakukan dengan:

- Classification Report dan Confusion Matrix: Menilai metrik seperti precision, recall, f1-score, dan akurasi.
- ROC Curve dan ROC-AUC: Menggunakan fungsi roc_curve dan roc_auc_score untuk mengukur kemampuan model membedakan kelas.
- Cross-Validation dengan StratifiedKFold: Menggunakan cross_val_score dengan metrik f1_macro untuk memastikan performa model konsisten dan tidak overfit.
  
Hasilnya:
Ketiga model yang digunaka efektif dalam menyelesaikan problem statement prediksi pembatalan transaksi pada dataset ini.

![image](https://github.com/user-attachments/assets/b426d168-fdd1-4496-8a5f-2fd1736dd31e)
![image](https://github.com/user-attachments/assets/e54260ac-1efd-41d0-9de1-f06337c7a69b)

# F. Validasi Model
Kode juga melakukan validasi dengan:

- Evaluasi pada Data Testing: Menggunakan hold-out test set untuk mengevaluasi setiap model.
- Stratified K-Fold Cross-Validation: Memberikan rata-rata dan standar deviasi metrik f1_macro untuk memastikan model bekerja konsisten di seluruh subset data training.

Hasil cross-validation menunjukkan performa yang sangat tinggi untuk model tree-based (Random Forest dan Decision Tree) baik pada data oversampled maupun undersampled.
