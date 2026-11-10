# Analisis dan Perbandingan Kinerja Metode SARIMA dan Exponential Smoothing (ETS) dalam Peramalan Jumlah Pendonor Darah Harian

Repositori ini merupakan dokumentasi proyek analisis data untuk pemenuhan tugas mata kuliah **Analisis Deret Waktu (Kelas A)** pada Program Studi Sains Data, Fakultas Ilmu Komputer, UPN "Veteran" Jawa Timur. Penelitian ini mengkaji dinamika fluktuasi harian calon pendonor darah yang mengalami penolakan pada tahap skrining kesehatan akibat ketidaksesuaian kadar hemoglobin (Tolak HB) di Unit Donor Darah (UDD) Palang Merah Indonesia (PMI) Kota Surabaya.

---

## 📖 Latar Belakang

Darah merupakan komponen esensial dalam sistem pelayanan kesehatan modern yang ketersediaannya sangat bergantung pada partisipasi sukarela masyarakat. Unit Donor Darah (UDD) Palang Merah Indonesia (PMI) Kota Surabaya memegang peranan krusial sebagai penyedia dan pengelola stok darah utama bagi puluhan rumah sakit di wilayah Surabaya dan sekitarnya. Dalam menjalankan fungsinya, UDD PMI Kota Surabaya menerapkan standar kualitas dan keamanan yang ketat sesuai regulasi Kementerian Kesehatan untuk memastikan keselamatan baik bagi pendonor maupun resipien (penerima darah).

Salah satu tahapan paling kritis dalam proses seleksi pendonor darah adalah pemeriksaan kadar hemoglobin (HB) sebelum pengambilan darah dilakukan. Calon pendonor diwajibkan memiliki kadar HB dalam rentang normal (minimal 12,5 g/dL hingga 17,0 g/dL). Apabila kadar HB berada di bawah batas minimum (indikasi anemia) atau melebihi batas maksimum, maka calon pendonor dinyatakan tidak lolos skrining dan harus ditunda proses donornya—kondisi yang dalam operasional UDD PMI dikenal dengan istilah **Tolak HB**. 

Fenomena tingginya angka penolakan pendonor akibat faktor HB memberikan dampak signifikan terhadap efisiensi operasional dan stabilitas rantai pasok darah. Pertama, terdapat selisih (*gap*) antara jumlah kedatangan calon pendonor dengan jumlah kantong darah aktual yang berhasil dihimpun, yang berisiko mengganggu pemenuhan permintaan darah dari rumah sakit apabila tidak diantisipasi. Kedua, setiap proses skrining tetap menyerap sumber daya operasional, seperti penggunaan bahan habis pakai medis (*lancet*, reagen/kuvet pemeriksaan HB) dan beban kerja tenaga teknis di lapangan.

Mengingat jumlah kunjungan dan tingkat kelolosan pendonor bersifat dinamis dari waktu ke waktu serta dipengaruhi oleh faktor musiman, jadwal kegiatan *Mobile Unit* (MU), hingga kondisi kesehatan masyarakat pada periode tertentu, maka pendekatan **Analisis Deret Waktu (*Time Series Analysis*)** menjadi sangat relevan. Melalui analisis deret waktu terhadap data historis harian kasus Tolak HB, pola pergerakan, tren jangka panjang, serta ketidakstabilan varians (non-stasioneritas) dari fenomena ini dapat diidentifikasi secara terukur. Hasil pemodelan dan peramalan (*forecasting*) dari studi ini diharapkan mampu memberikan landasan berbasis data (*data-driven insight*) bagi manajemen UDD PMI Kota Surabaya dalam merencanakan kuota cadangan calon pendonor, mengoptimalkan alokasi logistik skrining, serta merumuskan strategi edukasi gizi masyarakat guna menekan angka kegagalan donor di masa mendatang.

---

## 📊 Deskripsi & Statistik Dataset

Dataset yang dianalisis merupakan data sekunder yang diperoleh dari pencatatan operasional harian UDD PMI Kota Surabaya sepanjang periode **1 Januari 2026 hingga 31 Agustus 2026**.

* **Nama File**: `data_tolak_hb.csv`
* **Dimensi Data**: 243 baris observasi (harian) dan 2 kolom atribut
* **Variabel Data**:
  1. `TglPeriksa`: Tanggal dilakukannya pemeriksaan skrining hemoglobin calon pendonor (tipe data: *Datetime*, rentang `2026-01-01` s.d. `2026-08-31`).
  2. `Jumlah_Pendonor`: Jumlah agregat calon pendonor yang mengalami gagal donor karena faktor HB pada tanggal terkait (tipe data: *Integer*).

### Ringkasan Statistik Deskriptif (`Jumlah_Pendonor`)
| Metrik Statistik | Nilai | Interpretasi Singkat |
| :--- | :---: | :--- |
| **Jumlah Observasi (N)** | 243 hari | Data harian lengkap tanpa *missing values* (Januari – Agustus 2026) |
| **Rata-rata (*Mean*)** | 53,58 | Rata-rata terdapat sekitar 53 hingga 54 kasus tolak HB per hari |
| **Standar Deviasi (*Std*)** | 24,07 | Tingkat penyebaran/fluktuasi data harian tergolong cukup tinggi |
| **Nilai Minimum (*Min*)** | 7 orang | Jumlah kasus penolakan harian terendah |
| **Kuartil 1 (Q1 - 25%)** | 37 orang | 25% hari memiliki kasus penolakan di bawah 37 orang |
| **Median (Q2 - 50%)** | 50 orang | Nilai tengah jumlah penolakan harian |
| **Kuartil 3 (Q3 - 75%)** | 66 orang | 75% hari memiliki kasus penolakan di bawah 66 orang |
| **Nilai Maksimum (*Max*)** | 131 orang | Lonjakan kasus penolakan harian tertinggi |

---

## 📈 Status Pengerjaan & Tahapan Analisis

Proyek ini dikerjakan secara bertahap mengikuti metodologi standar analisis deret waktu:

- [x] **Tahap 1: Akuisisi & Prapemrosesan Data (*Data Preprocessing*)**
  - Pemuatan dataset `data_tolak_hb.csv` dan inspeksi struktur data.
  - Konversi tipe data atribut `TglPeriksa` dari *object/string* menjadi `datetime64[ns]`.
  - Pengurutan data secara kronologis dan pengaturan ulang indeks (*reset index*).
- [x] **Tahap 2: Eksplorasi & Visualisasi Deret Waktu (*Time Series Visualization*)**
  - Pembuatan grafik *Time Series Plot* untuk mengamati pola pergerakan aktual jumlah pendonor tolak HB harian.
- [x] **Tahap 3: Analisis Kestabilan Rata-rata & Varians (*Rolling Statistics*)**
  - Perhitungan dan visualisasi **Rolling Mean** dengan jendela waktu (*window*) 7 hari (mingguan) untuk menghaluskan fluktuasi jangka pendek dan mengobservasi pergerakan tren lokal.
  - Perhitungan dan visualisasi **Rolling Standard Deviation** (7 hari) untuk mengevaluasi volatilitas dan indikasi awal ketidakstasioneran dalam varians maupun rata-rata.
- [ ] **Tahap 4: Uji Stasioneritas Formal & Identifikasi Model** *(Dalam Pengerjaan)*
  - Pengujian hipotesis menggunakan *Augmented Dickey-Fuller (ADF) Test* dan analisis korelogram (plot ACF/PACF).
- [ ] **Tahap 5: Estimasi Parameter, Uji Diagnostik, & Peramalan** *(Rencana Lanjutan)*[cite: 1]

---

## Rencana Pemodelan Selanjutnya

Berdasarkan eksplorasi visual dari *Rolling Mean* dan *Rolling Standard Deviation* yang menunjukkan adanya fluktuasi dinamis dari minggu ke minggu, tahapan pemodelan statistik yang direncanakan meliputi:
1. **Pengujian Stasioneritas Formal**: Membuktikan secara statistik apakah deret waktu bersifat non-stasioner menggunakan uji akar unit (*Unit Root Test* / ADF) serta melihat pola penurunan *lag* pada grafik *Autocorrelation Function* (ACF).
2. **Transformasi & *Differencing***: Melakukan transformasi Box-Cox (jika tidak stasioner dalam varians) dan proses pembedaan (*differencing* orde $d$ atau musiman $D$) untuk menstasionerkan data.
3. **Pemodelan Deret Waktu (SARIMA / RegARIMA / Alternatif)**: Mengingat data harian sering kali memuat efek siklus mingguan ($s=7$), pemodelan akan diarahkan pada kandidat model terbaik yang mampu mengakomodasi pola fluktuasi dan musiman data operasional PMI.
4. **Evaluasi & *Forecasting***: Melakukan uji diagnostik residual (*white noise* dan normalitas) serta menghitung metrik akurasi peramalan (seperti MAE, RMSE, dan MAPE).

---

## 📁 Struktur Repositori

```text
├── Analisi_Deret_Waktu_kelas_A.ipynb   # Notebook utama berisi kode analisis dan visualisasi data
├── data_tolak_hb.csv                   # Dataset harian jumlah pendonor tolak HB (Jan - Agu 2026)
└── README.md                           # Dokumentasi lengkap laporan proyek[cite: 1]
