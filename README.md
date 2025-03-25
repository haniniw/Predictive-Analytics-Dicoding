# Predictive-Analytics-Dicoding
Oleh: Noer Hanifah Suganda

# A. Latar Belakang

Perusahaan e-commerce UK yang berbasis di London telah beroperasi sejak tahun 2007 dan menawarkan produk berupa hadiah serta homewares untuk berbagai kalangan, baik individu maupun bisnis kecil. Dataset yang digunakan dalam analisis ini mencakup 500 ribu transaksi selama satu tahun dengan 8 kolom informasi.

Meskipun sebagian besar transaksi berjalan lancar, terdapat persentase kecil transaksi yang dibatalkan. Pembatalan ini umumnya terjadi karena kondisi stok yang tidak memadai, sehingga pelanggan memilih untuk membatalkan transaksi agar dapat menerima semua produk sekaligus. Pembatalan transaksi tidak hanya mengganggu alur operasional, tetapi juga berdampak pada pendapatan dan kepuasan pelanggan.

Melihat kondisi tersebut, penting untuk mengidentifikasi faktor-faktor yang mempengaruhi pembatalan transaksi dan mengembangkan model prediksi yang mampu mendeteksi transaksi berisiko pembatalan. Dengan menerapkan pendekatan machine learning, perusahaan dapat mengambil langkah proaktif untuk mengoptimalkan manajemen inventori, mengurangi kerugian finansial, serta meningkatkan pengalaman pelanggan secara keseluruhan.

# B. Business Understanding

Dalam dunia e-commerce, menjaga pendapatan harian itu sangat penting. Pendapatan harian yang fluktuatif dapat membuat perusahaan kesulitan dalam mengelola stok, mengatur promosi, dan merencanakan operasional. Jika perusahaan dapat mengetahui kapan pendapatan cenderung turun atau naik, mereka bisa lebih cepat merespons—misalnya, dengan menambah stok atau menyesuaikan promosi—sehingga kerugian dapat diminimalkan dan kesempatan penjualan dapat dimaksimalkan.

**Problem Statement:**

- Bagaimana membangun model prediktif berbasis time series untuk meramalkan total pendapatan harian (daily revenue) dari transaksi e-commerce?
- Model yang seperti apa yang memiliki akurasi paling baik?

**Goals:**

- Mengembangkan model forecasting yang dapat memprediksi pendapatan penjualan harian dengan akurasi yang terukur menggunakan metrik Mean Absolute Error (MAE) dan Root Mean Squared Error (RMSE).
- Membandingkan beberapa algoritma model untuk menemukan akurasi terbaik dalam memprediksi pendapatan penjualan harian.

**Solution Statement**

Untuk meramalkan total pendapatan harian (daily revenue) dari transaksi e-commerce, saya menggunakan tiga pendekatan solusi, masing-masing dengan metrik evaluasi terukur (seperti MAE, RMSE, dan MAPE) sehingga solusi yang dihasilkan dapat dinilai secara objektif.

- Menggunakan model ARIMA untuk memodelkan data time series secara univariat. ARIMA memanfaatkan pola historis revenue untuk menangkap tren dan musiman secara langsung melalui parameter (p, d, q). ARIMA memberikan model time series klasik yang mengandalkan pola historis secara langsung.

- Menggunakan Random Forest Regressor untuk forecasting revenue harian dengan memanfaatkan fitur-fitur hasil rekayasa (lag revenue, moving averages, dll.). Pendekatan ini memungkinkan model menangkap hubungan non-linear dan interaksi antar fitur yang mungkin tidak dapat ditangkap oleh model time series tradisional.

- Mengembangkan model deep learning berbasis LSTM yang dirancang untuk menangkap ketergantungan jangka panjang dalam data time series. Pendekatan ini efektif jika pola revenue harian memiliki dinamika non-linear yang kompleks.

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

**1. Membuat Label Pembatalan dan Revenue**
Dibuat kolom IsCancelled untuk menandai apakah transaksi dibatalkan (jika TransactionNo mengandung 'C' atau jika Quantity < 0).

Kolom Revenue dihitung sebagai hasil perkalian Price dan Quantity untuk transaksi yang tidak dibatalkan, sedangkan untuk transaksi yang dibatalkan revenue diset menjadi 0..

**2. Mengambil Hanya Transaksi yang Berhasil (non-cancelled)**
Untuk forecasting revenue, kita hanya akumulasi transaksi yang tidak dibatalkan. Disimpan dalam df_sucess

**3. Agregasi Revenue Harian**
Data kemudian dikelompokkan berdasarkan tanggal untuk mendapatkan total pendapatan harian.

**4. Pembuatan Fitur Time Series**
Dibuat fitur lag (misalnya, Lag1, Lag7, Lag30) yang menunjukkan pendapatan hari sebelumnya sebagai input untuk model.

Dibuat juga fitur moving average (MA7, MA30) untuk menangkap pola tren jangka pendek dan panjang.

**5. Scaling Fitur Numerik**
Fitur-fitur numerik seperti Revenue, lag, dan moving average di-scale menggunakan StandardScaler agar memiliki rentang nilai yang seragam dan memudahkan proses training model.

**6. Split Data Menjadi Training dan Testing Set**
Data time series dibagi secara kronologis (tanpa pengacakan) ke dalam data training dan testing (misalnya, 80% training dan 20% testing) untuk menjaga urutan waktu.

# E. Model Development 

Tiga algoritma digunakan:

**1. Random Forest Regressor**
Tahapan Pemodelan:
- Fitur tambahan seperti lag dan moving average telah dibuat untuk menangkap pola temporal.
- Splitting Data: Data di-split secara kronologis ke dalam training dan testing set.
- Training Model: Model Random Forest Regressor di-fit pada data training dengan fitur-fitur yang sudah di-scale.
- Prediksi dan Evaluasi: Model digunakan untuk memprediksi revenue pada data training dan testing, kemudian dievaluasi menggunakan metrik seperti MAE, RMSE, dan R².
- Parameter Utama:
n_estimators: Jumlah pohon dalam ensemble (misalnya, 100).
max_depth: Kedalaman maksimum masing-masing pohon (misalnya, 10) untuk mengurangi kompleksitas dan overfitting.
random_state: Untuk memastikan reproducibility hasil.

Kelebihan:
- Mampu menangkap hubungan non-linear dan interaksi antar fitur dengan sangat baik.
- Lebih robust terhadap noise dan overfitting karena menggunakan banyak pohon (ensembling).
- Dapat memanfaatkan fitur rekayasa seperti lag dan moving average sehingga memperkaya informasi.

Kekurangan:
- Memerlukan sumber daya komputasi lebih banyak, sehingga training bisa lebih lambat.
- Interpretasi internal model kurang transparan karena merupakan ensemble dari banyak pohon.
- Jika parameter tidak di-tuning dengan baik, bisa tetap overfit pada data training.
  ![image](https://github.com/user-attachments/assets/16fa79c8-511c-45b9-b3d2-e3527ccf8a59)

  ![image](https://github.com/user-attachments/assets/db169969-f242-435e-a478-bd7af08607fd)

**2. ARIMA**
Tahapan Pemodelan:
- Data diurutkan berdasarkan tanggal, dan kolom Date di-set sebagai index time series.
- Data di-split secara kronologis (misalnya 80% training, 20% testing) agar urutan waktu tetap terjaga.
- Penentuan Order (p, d, q): Menggunakan analisis ACF/PACF untuk menentukan nilai p (autoregressive order) dan q (moving average order). d adalah jumlah differencing yang diperlukan untuk membuat data stasioner.

- Fitting Model: Model ARIMA dengan order tertentu (misalnya, ARIMA(1,1,1)) di-fit pada data training.

- Parameter Utama:

p: Jumlah lag pada komponen autoregressive.
d: Jumlah kali differencing yang dilakukan untuk stasioneritas.
q: Jumlah lag pada komponen moving average.
Contoh: ARIMA(1,1,1)

Kelebihan:
- Cocok untuk data time series univariat yang pola tren dan musiman sudah jelas.
- Model sederhana dan mudah diinterpretasikan.
- Banyak referensi dan metode tuning yang telah bagik.

Kekurangan:
- Hanya memodelkan satu variabel (univariat), sehingga tidak dapat - memanfaatkan fitur eksternal atau rekayasa fitur seperti lag dan moving average secara eksplisit.
- Sensitif terhadap penentuan parameter order dan memerlukan data yang stasioner.
- Tidak mampu menangkap hubungan non-linear secara kompleks.
![image](https://github.com/user-attachments/assets/922284cc-a598-4498-a4de-cb1b05616a7a)

**3. LSTM (Long Short-Term Memory)**
Tahapan Pemodelan:
- Data diubah ke bentuk 3D (samples, time_steps, features) sesuai dengan kebutuhan input LSTM. 
- Membangun Model: Model LSTM dibangun menggunakan Sequential API, dengan satu layer LSTM dan satu layer Dense sebagai output.
- Kompilasi Model:
Model dikompilasi menggunakan optimizer Adam dan loss function MSE (Mean Squared Error) karena target adalah nilai kontinu.
- Training Model:
Model dilatih selama sejumlah epoch (50 epoch) dengan batch size (32) dan divalidasi pada data testing.
- Prediksi dan Evaluasi:
Model menghasilkan prediksi revenue pada data testing yang kemudian dievaluasi dengan metrik seperti MAE dan RMSE.

- Parameter Utama:
Jumlah Unit LSTM: 50 unit yang menentukan kompleksitas representasi temporal.
Activation Function:ReLU, untuk menangkap non-linearity.
Epochs: Jumlah iterasi training (50).
Batch Size: Jumlah sampel yang diproses sebelum melakukan update parameter (32).
Learning Rate: Parameter dalam optimizer Adam (misalnya, 0.001).

Kelebihan:
- Sangat efektif untuk data time series dengan ketergantungan jangka panjang.
- Mampu menangkap hubungan non-linear dan pola kompleks dalam data.
- Cocok untuk dataset yang besar dan dinamis.

Kekurangan:
- Memerlukan data dalam jumlah besar dan waktu training yang lebih lama dibandingkan model tradisional.
- Memerlukan tuning hyperparameter yang lebih cermat.
- Interpretasi model deep learning seperti LSTM lebih sulit dibandingkan model tradisional.
  ![image](https://github.com/user-attachments/assets/d09c07b8-4db0-4036-b310-4f6b9da0140f)

# F. Model Evaluation 

Setelah mengembangkan model dengan tiga pendekatan (ARIMA, RandomForestRegressor, dan LSTM. Diperoleh hasilnya:

**- RandomForestRegressor:**
Testing MAE: 0.7630
Testing RMSE: 1.0317
Model ini memberikan error yang relatif rendah pada data testing, menunjukkan prediksi revenue harian yang lebih dekat dengan nilai aktual. Karena Random Forest menggunakan feature engineering (lag, moving average) untuk menangkap pola non-linear dalam data time series, hasilnya mendukung Goal 1 (forecasting revenue harian dengan error rendah).

**- ARIMA:**
MAE: 1.3104
RMSE: 1.6240
Meskipun merupakan pendekatan time series klasik, error ARIMA lebih tinggi, sehingga model ini kurang optimal dibanding Random Forest dalam konteks dataset ini.
Hasil ARIMA menunjukkan bahwa pendekatan univariat tidak cukup menangkap pola kompleks pada data, sehingga Goal 1 belum tercapai dengan optimal jika hanya menggunakan ARIMA.

**- LSTM :**
MAE: 0.9514
RMSE: 1.2049
: Model LSTM, yang merupakan pendekatan deep learning untuk time series, menunjukkan performa yang cukup baik, tetapi error-nya masih sedikit lebih tinggi dibanding Random Forest. Hasil LSTM mendukung Goal 1, karena LSTM dapat menangkap dependensi jangka panjang. Namun, bila dibandingkan dengan Random Forest, performa LSTM belum mencapai tingkat akurasi tertinggi.

![image](https://github.com/user-attachments/assets/178df41c-bdcb-47ec-9b05-38a23ae4525a)

**Kesimpulan**
Dalam konteks forecasting revenue harian, model Random Forest Regressor menunjukkan performa terbaik berdasarkan metrik MAE dan RMSE. LSTM memberikan hasil yang cukup baik dan merupakan alternatif yang menarik, terutama jika data memiliki pola dependensi jangka panjang yang kompleks. Namun, jika tujuan utama adalah akurasi prediksi dan generalisasi, Random Forest lebih direkomendasikan.
