# 📘 Bahan Belajar Ujian Essay — Data Mining (SD25-31001)

Program Studi Sains Data — Institut Teknologi Sumatera

> **Cakupan (Pertemuan 1–7, materi UTS):**
> 1. Pengantar Data Mining
> 2. Objek Data & Tipe Atribut (Getting to Know Your Data)
> 3. Preprocessing Data
> 4. Data Cleaning & Data Integration
> 5. Data Reduction & Data Transformation
> 6. Information Extraction (Feature Selection)
> 7. Proximity Measurement
> 8. Data Splitting & Resampling
>
> **Catatan membaca:**
> - Rumus yang di modul `.txt` kacau/terpotong sudah **dilengkapi dan dibetulkan** dari slide asli (ditulis dalam blok kode agar terbaca di semua aplikasi).
> - Bagian bertanda **➕ Tambahan** adalah penjelasan pelengkap di luar slide agar jawaban essay lebih utuh. Bagian lain murni dari materi.
> - Di akhir ada **bank soal essay + jawaban model**, **ringkasan perbandingan**, dan **lembar contekan rumus**.

---

## 📑 DAFTAR ISI

1. [Pengantar Data Mining](#bab-1--pengantar-data-mining)
2. [Objek Data & Tipe Atribut](#bab-2--objek-data--tipe-atribut)
3. [Preprocessing Data](#bab-3--preprocessing-data)
4. [Data Cleaning](#bab-4--data-cleaning)
5. [Data Integration](#bab-5--data-integration)
6. [Data Reduction](#bab-6--data-reduction)
7. [Data Transformation](#bab-7--data-transformation)
8. [Information Extraction & Feature Selection](#bab-8--information-extraction--feature-selection)
9. [Proximity Measurement](#bab-9--proximity-measurement)
10. [Data Splitting & Resampling](#bab-10--data-splitting--resampling)
11. [Bank Soal Essay + Jawaban Model](#bab-11--bank-soal-essay--jawaban-model)
12. [Lembar Contekan Rumus](#bab-12--lembar-contekan-rumus)

---

# BAB 1 — PENGANTAR DATA MINING

## 1.1 Mengapa Data Mining?

- **Pertumbuhan data yang eksplosif**: dari terabyte hingga petabyte.
- Sumber data melimpah:
  - **Bisnis**: web, e-commerce, transaksi, saham.
  - **Sains**: remote sensing, bioinformatika, simulasi ilmiah.
  - **Masyarakat**: berita, kamera digital, YouTube.
- Ungkapan kunci: ***"We are drowning in data, but starving for knowledge!"*** (tenggelam dalam data, tetapi kelaparan pengetahuan).
- *"Necessity is the mother of invention"* → data mining = **analisis otomatis atas kumpulan data masif**.

### Evolusi ilmu pengetahuan
| Periode | Fase |
|---|---|
| Sebelum 1600 | Ilmu empiris |
| 1600–1950an | Ilmu teoretis |
| 1950an–1990an | Ilmu komputasional (simulasi) |
| 1990–sekarang | **Data science** (banjir data dari instrumen ilmiah & simulasi, penyimpanan petabyte murah, internet) |

### Evolusi teknologi basis data
| Dekade | Perkembangan |
|---|---|
| 1960an | Pengumpulan data, pembuatan database, IMS & network DBMS |
| 1970an | Model data relasional, implementasi RDBMS |
| 1980an | RDBMS, model data lanjut (extended-relational, OO, deduktif), DBMS berorientasi aplikasi (spasial, ilmiah, rekayasa) |
| 1990an | **Data mining**, data warehousing, database multimedia, web database |
| 2000an | Stream data mining, aplikasi data mining, teknologi web (XML, integrasi data) |

## 1.2 Apa itu Data Mining?

**Definisi (dari modul):** Proses menemukan **pola, tren, dan informasi berguna** dari kumpulan data besar menggunakan **teknik statistik, algoritma, dan metode analisis**. Tujuannya mengidentifikasi informasi yang **tidak terlihat langsung/tersembunyi** dalam data, untuk membantu keputusan bisnis, memahami perilaku pelanggan, atau menemukan peluang baru.

**Definisi (Han & Kamber):** *Data mining (knowledge discovery from data)* = ekstraksi pola atau pengetahuan yang **menarik** dari sejumlah besar data. Pola "menarik" berarti:
- **non-trivial** (tidak remeh),
- **implicit** (tersirat),
- **previously unknown** (sebelumnya tidak diketahui),
- **potentially useful** (berpotensi berguna).

**Nama lain:** Knowledge Discovery in Databases (KDD), knowledge extraction, data/pattern analysis, data archeology, data dredging, information harvesting, business intelligence.

**Apa yang BUKAN data mining?**
- Pencarian sederhana dan pemrosesan query.
- Sistem pakar deduktif.

## 1.3 Proses KDD (Knowledge Discovery in Databases)

### Pandangan komunitas database & data warehousing
Urutan:
1. **Data cleaning**
2. **Data integration**
3. **Data selection**
4. **Data transformation**
5. **Data mining** ← inti (peran esensial)
6. **Pattern/model evaluation**
7. **Knowledge presentation**

(Langkah 1–4 disebut **data preparation**.)

### Pandangan komunitas Machine Learning & Statistik
```
Input Data → Data Pre-Processing → Data Mining → Post-Processing
```
| Tahap | Isi |
|---|---|
| Data Pre-Processing | Data integration, normalization, feature selection, dimension reduction |
| Data Mining | Pattern discovery, association & correlation, classification, clustering, outlier analysis |
| Post-Processing | Pattern evaluation, pattern selection, pattern interpretation, pattern visualization |

**Contoh – medical data mining:** preprocessing (termasuk feature extraction & dimension reduction) → klasifikasi dan/atau clustering → post-processing untuk presentasi.

**Contoh – Web mining framework:** data cleaning → data integration dari banyak sumber → warehousing → data cube construction → data selection → data mining → presentasi hasil → pola/pengetahuan disimpan ke knowledge base.

## 1.4 Data Mining dalam Business Intelligence (piramida)

Dari bawah ke atas (potensi mendukung keputusan bisnis makin tinggi):

| Lapisan | Pengguna |
|---|---|
| Data Sources (kertas, file, dokumen web, eksperimen ilmiah, sistem database) | — |
| Data Preprocessing/Integration, Data Warehouses | **DBA** |
| Data Exploration (ringkasan statistik, query, reporting) | |
| Data Mining (information discovery) | **Data Analyst** |
| Data Presentation (teknik visualisasi) | **Business Analyst** |
| Decision Making | **End User** |

## 1.5 Pandangan Multi-Dimensi Data Mining

| Dimensi | Isi |
|---|---|
| **Data yang ditambang** | Database (relasional, OO, heterogen, legacy), data warehouse, transaksional, stream, spatiotemporal, time-series, sequence, teks & web, multimedia, graf & jaringan sosial |
| **Pengetahuan yang ditambang (fungsi DM)** | Karakterisasi, diskriminasi, asosiasi, klasifikasi, clustering, trend/deviasi, outlier analysis |
| **Teknik yang dipakai** | Data-intensive, data warehouse (OLAP), machine learning, statistik, pattern recognition, visualisasi, high-performance computing |
| **Aplikasi** | Retail, telekomunikasi, perbankan, deteksi fraud, bio-data mining, analisis pasar saham, text mining, web mining |

Juga: **Descriptive vs Predictive** data mining → *"apa yang terjadi?"* vs *"apa yang akan terjadi?"*.

## 1.6 Jenis Data yang Dapat Ditambang

### Keragaman tipe data (I)
- **Structured data**: seragam, bentuk record/tabel, didefinisikan *data dictionary*, atribut tetap dengan rentang nilai & makna semantik tetap. Contoh: database relasional, data cube, data matrix, data warehouse.
- **Semi-structured data**: objek boleh mengandung nilai himpunan, beberapa nilai bertipe heterogen, atau struktur bersarang; struktur dapat didefinisikan fleksibel/dinamis. Contoh: data transaksional, data sequence (time-series, gen/protein, weblog).
- **Graph/network data**: tipe semi-terstruktur yang lebih canggih.
- **Unstructured data**: teks dan multimedia (audio, gambar, video).
- Data dunia nyata sering **campuran** terstruktur + semi-terstruktur + tidak terstruktur.

### Keragaman tipe data (II) — per aplikasi
- Aplikasi berbeda → dataset & metode analisis berbeda.
- Sequence: sekuens biologis vs sekuens transaksi belanja.
- **Time-series** = himpunan nilai numerik berurutan dengan **interval waktu sama**.
- Spatial, temporal, spatiotemporal.
- Graf & jaringan: sosial, komunikasi komputer, biologis, informasi — semantik bisa berbeda.
- Pada dataset yang sama, pola berbeda memerlukan metode berbeda (mis. program software: mencari modul plagiat vs mencari bug copy-paste).

### Keragaman tipe data (III)
- **Stored data**: terbatas (finite), disimpan di repositori besar.
- **Streaming data** (mis. video surveillance, remote sensing): dinamis, terus datang, tak terbatas, butuh respons real-time → tantangan.

### Daftar data yang dapat ditambang
- *Database-oriented*: relasional, data warehouse, transaksional.
- *Advanced*: data stream & sensor, time-series/temporal/sequence (termasuk bio-sequence), data struktur/graf/jaringan sosial/multi-linked, object-relational, heterogen & legacy, spasial & spatiotemporal, multimedia, teks, World Wide Web.

## 1.7 Fungsi (Pola) Data Mining

### (1) Generalisasi
- Integrasi informasi & konstruksi data warehouse (cleaning, transformasi, integrasi, model data multidimensi).
- **Data cube** & **OLAP** (online analytical processing): komputasi agregat multidimensi yang scalable.
- **Deskripsi konsep multidimensi**: *karakterisasi* dan *diskriminasi* → generalisasi, ringkasan, dan kontras karakteristik data (mis. wilayah kering vs basah).

### (2) Analisis Asosiasi & Korelasi
- **Frequent pattern / frequent itemset**: item apa yang sering dibeli bersamaan?
- Contoh aturan asosiasi: **`Diaper → Beer [0.5%, 75%]`** → (**support**, **confidence**).
- Pertanyaan: apakah item yang berasosiasi kuat juga berkorelasi kuat? Asosiasi/korelasi ≠ kausalitas.

### (3) Klasifikasi
- **Klasifikasi & prediksi label**: membangun model (fungsi) berdasarkan contoh latih; mendeskripsikan & membedakan kelas untuk prediksi masa depan; memprediksi label kelas yang belum diketahui.
- Metode: decision tree, naïve Bayes, SVM, neural network, rule-based, pattern-based, logistic regression.
- Aplikasi: deteksi fraud kartu kredit, direct marketing, klasifikasi bintang/penyakit/halaman web.

### (4) Analisis Cluster
- **Unsupervised learning** (label kelas tidak diketahui).
- Mengelompokkan data membentuk kategori baru (cluster), mis. cluster rumah untuk melihat pola distribusi.
- **Prinsip: maksimalkan kemiripan intra-kelas & minimalkan kemiripan antar-kelas.**

### (5) Analisis Outlier
- **Outlier** = objek data yang **tidak sesuai dengan perilaku umum** data.
- *Noise atau exception?* — "sampah seseorang bisa jadi harta orang lain".
- Metode: produk sampingan clustering/regresi.
- Berguna untuk deteksi fraud & analisis kejadian langka.

### (6) Waktu & Urutan: Sequential Pattern, Trend, Evolution
- Analisis tren, time-series, deviasi (mis. regresi & prediksi nilai).
- **Sequential pattern mining** (mis. beli kamera digital dulu, lalu kartu memori SD besar).
- Analisis periodisitas, motif & analisis sekuens biologis, analisis berbasis kemiripan.
- Mining data stream (berurutan, berubah-ubah menurut waktu, berpotensi tak hingga).

### (7) Analisis Struktur & Jaringan
- **Graph mining**: frequent subgraph (mis. senyawa kimia), pohon (XML), substruktur (fragmen web).
- **Information network analysis**: jaringan sosial (aktor = node, relasi = edge), jaringan heterogen ganda; *link mining*.
- **Web mining**: web = jaringan informasi besar (dari PageRank sampai Google); penemuan komunitas web, opinion mining, usage mining.

### (8) Deep Learning
- Frontier machine learning yang berkembang cepat; arsitektur: feed-forward, **CNN**, **RNN**, **GNN**, **Transformer**.
- Aplikasi: computer vision, NLP, machine translation, analisis jaringan sosial; mengubah banyak tugas DM (klasifikasi, clustering, deteksi outlier, reinforcement learning).

## 1.8 Evaluasi Pengetahuan (Interestingness)

Apakah semua pengetahuan hasil mining menarik? Tidak. Sebagian hanya cocok pada ruang dimensi tertentu (waktu, lokasi), tidak representatif, atau sementara. Kriteria evaluasi: **descriptive vs predictive, coverage, typicality vs novelty, accuracy, timeliness**.

## 1.9 Data Mining: Pertemuan Banyak Disiplin (Confluence)

Machine Learning, Pattern Recognition, Statistik, Visualisasi, High-Performance Computing, Teknologi Database, Algoritma, Aplikasi.

**Mengapa banyak disiplin?**
- Jumlah data sangat besar → algoritma harus **scalable** (terabyte).
- **Dimensionalitas tinggi** (mis. micro-array puluhan ribu dimensi).
- **Kompleksitas data tinggi**: stream & sensor, time-series, graf, data heterogen/legacy, spasial/multimedia/teks/web, program software, simulasi ilmiah.
- Aplikasi baru yang canggih.

## 1.10 Aplikasi Data Mining

- Analisis halaman web (klasifikasi, clustering, ranking; PageRank & HITS).
- Collaborative analysis & **recommender systems**.
- Analisis keranjang belanja → targeted marketing.
- Analisis data biologis & medis (klasifikasi, cluster microarray, sekuens & jaringan biologis).
- Data mining & rekayasa perangkat lunak; text analysis; analisis jaringan sosial & informasi.
- **Invisible data mining** (fungsi bawaan di Google, Microsoft, LinkedIn, Meta, dll).
- Tools khusus: SAS, MS SQL Server Analysis Manager, Oracle Data Mining.

## 1.11 Isu Utama dalam Data Mining

| Kategori | Isu |
|---|---|
| **Metodologi mining** | Menambang jenis pengetahuan baru, pengetahuan di ruang multidimensi, upaya interdisipliner, memperkuat kemampuan penemuan di lingkungan jaringan, **menangani noise, ketidakpastian & ketidaklengkapan data**, evaluasi pola & mining berbasis pola/kendala |
| **Interaksi pengguna** | Interactive mining, penggabungan background knowledge, presentasi & visualisasi hasil |
| **Efisiensi & skalabilitas** | Efisiensi algoritma; metode paralel, terdistribusi, stream, dan incremental |
| **Keragaman tipe data** | Menangani tipe data kompleks; repositori data dinamis, berjejaring, global |
| **Data mining & masyarakat** | Dampak sosial, **privacy-preserving data mining**, invisible data mining |

**Data Mining & Masyarakat:** bermanfaat (penemuan ilmiah, manajemen bisnis, pemulihan ekonomi, perlindungan keamanan — mis. deteksi penyusup/serangan siber real-time), tetapi perlu dijaga dari **penyalahgunaan** (risiko membocorkan informasi rahasia bisnis/pemerintah dan data pribadi). Filosofinya: tetap menjaga **sensitivitas data, keamanan data, dan privasi** sambil melakukan mining yang sukses.

## 1.12 Sejarah Singkat

- 1989: IJCAI Workshop on KDD.
- 1991–1994: workshop KDD; 1995–1998: konferensi internasional KDD; 1997: jurnal *Data Mining and Knowledge Discovery*.
- ACM SIGKDD sejak 1998; konferensi lain: PAKDD (1997), PKDD (1997), SIAM-DM (2001), IEEE ICDM (2001); ACM TKDD sejak 2007.

## 1.13 Ringkasan Bab 1

- Data mining = menemukan pola & pengetahuan menarik dari data masif; evolusi alami teknologi database; banyak aplikasi.
- Proses KDD: cleaning, integration, selection, transformation, mining, pattern evaluation, knowledge presentation.
- Fungsionalitas: karakterisasi, diskriminasi, asosiasi, klasifikasi, clustering, analisis outlier & tren.

## 1.14 Dua kata kunci penting dari modul `.txt` (Pengantar)

Proses data mining umumnya melibatkan **beberapa langkah yang dirancang untuk mengambil data mentah dan mengubahnya menjadi informasi berguna** (yaitu: preprocessing → mining → evaluasi/presentasi).

---

# BAB 2 — OBJEK DATA & TIPE ATRIBUT

## 2.1 Apa itu Data?

**Data = kumpulan objek data beserta atribut-atributnya.**

Contoh tabel (baris = objek, kolom = atribut):

| Tid | Refund | Marital Status | Taxable Income | Cheat |
|---|---|---|---|---|
| 1 | Yes | Single | 125K | No |
| 2 | No | Married | 100K | No |
| 3 | No | Single | 70K | No |
| 4 | Yes | Married | 120K | No |
| 5 | No | Divorced | 95K | Yes |
| … | … | … | … | … |

## 2.2 Objek Data

- Himpunan data dibangun dari **objek-objek data**.
- **Objek data menyatakan sebuah entitas.**
  - Dataset universitas → objek: mahasiswa, dosen, mata kuliah.
  - Dataset operator seluler → objek: pelanggan, layanan, fitur, dst.
- Objek digambarkan dengan **atribut**.
- Objek disebut juga **sampel, contoh (example/instance), atau titik data**.
- Dalam basis data, objek disimpan sebagai **tuple**:
  - **Baris** → objek data.
  - **Kolom** → atribut.

## 2.3 Atribut

- **Atribut** = simbol yang menggambarkan identitas atau karakteristik suatu kelompok. Dalam data mining: **data field yang merepresentasikan karakteristik/fitur dari objek data**.
- Sebutan di bidang lain:
  - **Dimensi** → data warehousing
  - **Fitur** → machine learning
  - **Variabel** → statistik
- Contoh: Objek = mahasiswa → atribut: NIM, Prodi, Alamat, dst.

## 2.4 Tipe Atribut

Empat tipe utama: **Nominal, Binary, Ordinal, Numeric.**

Pengelompokan alternatif:
```
Binary / Dichotomous variables
Categorical (kualitatif)
 ├─ Binary variables
 ├─ Nominal variables
 └─ Ordinal variables
Numeric variables (kuantitatif)
 ├─ Interval-scale variables
 └─ Ratio-scaled variables
```

### a) Atribut Nominal
- Disebut juga **kategorial**: nilai menggambarkan **kategori, kode, atau status yang TIDAK memiliki urutan**.
- Contoh: warna (merah, putih, biru); kategori pelanggan (silver, gold, platinum — *dalam slide dicontohkan sebagai nominal*); kode wilayah (A, B, C, D); pekerjaan (archaeologist, engineer, doctor, teacher, driver).
- **Operasi yang bisa diterapkan: `=`, `≠`.**
- **Bisakah nominal berupa angka?** Ya. Misalnya warna: merah=0, putih=1, biru=3; kode wilayah A=0, B=1, C=2, D=3. Angkanya hanya **label**, bukan besaran (tidak bermakna dijumlah/dirata-rata).

### b) Atribut Biner (Boolean)
- Atribut nominal yang **hanya punya dua nilai: 0 atau 1** (true=1, false=0). Contoh: `isSmoker` (0/1).
- Dipandang khusus dalam data mining karena karakternya unik. Ada dua jenis:

| Jenis | Kriteria | Contoh |
|---|---|---|
| **Biner simetris** | Kedua nilai memberi **dampak/bobot setara** | Jenis kelamin (Pria/Wanita) |
| **Biner asimetris** | Kedua nilai memberi **dampak berbeda** (satu keadaan lebih penting) | Tes buta warna (Buta warna / Tidak buta warna) |

### c) Atribut Ordinal
- Mirip nominal, tetapi **M state-nya berurutan/ter-ranking secara bermakna**.
- **Ada urutan** antar nilai, **namun selisih (difference) dan rasio antar nilai TIDAK bermakna.**
- Contoh: nilai sekolah {A, B, C, D, F}; rating film {hate, dislike, indifferent, like, love}; rating bintang {*, **, ***, ****, *****}; medali {bronze, silver, gold}.
- **Operasi: `=`, `≠`, `<`, `>`.**

### d) Atribut Numerik

**Interval-scale**
- **Selisih antar nilai bermakna** (selisih 90°–100° sama dengan 40°–50°).
- Contoh: **tanggal kalender, suhu dalam Celsius/Fahrenheit**.
- **Rasio TIDAK bermakna**: 2°C tidak "dua kali lebih panas" dari 1°C. Alasannya titik 0° pada skala Celsius **secara fisik arbitrer**.
- **Operasi: `=`, `≠`, `<`, `>`, `+`, `−`.**

**Ratio-scale**
- **Selisih dan rasio keduanya bermakna.** Orang 100 kg = dua kali berat orang 50 kg; usia 50 tahun = dua kali usia 25 tahun.
- Punya **nilai nol yang bermakna (unik, tidak arbitrer)**.
- Contoh: umur, berat, panjang, jumlah penjualan.
- **Operasi: `=`, `≠`, `<`, `>`, `+`, `−`, `×`, `÷`.**

### Tabel ringkasan tipe atribut

| Tipe | Urutan? | Selisih bermakna? | Rasio bermakna? | Nol bermakna? | Operasi | Contoh |
|---|---|---|---|---|---|---|
| Nominal | ✗ | ✗ | ✗ | ✗ | =, ≠ | warna, pekerjaan, kode wilayah |
| Biner | ✗ | ✗ | ✗ | ✗ | =, ≠ | isSmoker, jenis kelamin |
| Ordinal | ✓ | ✗ | ✗ | ✗ | =, ≠, <, > | rating bintang, nilai A–F, medali |
| Interval | ✓ | ✓ | ✗ | ✗ (arbitrer) | =, ≠, <, >, +, − | suhu °C, tanggal |
| Rasio | ✓ | ✓ | ✓ | ✓ | + semua operasi aritmetika | umur, berat, penjualan |

### Contoh klasifikasi atribut (dari slide)

| ID | Nama | Jenis Kelamin | Status Beasiswa | Tingkat Kepuasan | Umur | IPK |
|---|---|---|---|---|---|---|
| 01 | Rina | Perempuan | Ya | Tinggi | 21 | 3.60 |
| 02 | Budi | Laki-laki | Tidak | Sedang | 23 | 3.20 |
| 03 | Sinta | Perempuan | Ya | Rendah | 20 | 3.85 |
| 04 | Andi | Laki-laki | Tidak | Tinggi | 22 | 3.00 |

Klasifikasi yang wajar:
- **ID** → nominal (hanya pengenal; tidak punya urutan/besaran).
- **Nama** → nominal.
- **Jenis Kelamin** → biner simetris.
- **Status Beasiswa** (Ya/Tidak) → biner.
- **Tingkat Kepuasan** (Rendah/Sedang/Tinggi) → ordinal.
- **Umur** → numerik rasio.
- **IPK** → numerik (diperlakukan sebagai numerik kontinu; ➕ umumnya dianggap interval/rasio karena skalanya terhingga 0–4).

## 2.5 Tipe Dataset

### (1) Record Data
- **Relational records** — tabel relasional, sangat terstruktur (contoh: tabel Person dan Car berelasi lewat `Pers_ID`).
- **Data matrix** — mis. matriks numerik, crosstab (contoh: penjualan produk per negara).
- **Transaction data** — contoh: TID 1 = {Bread, Coke, Milk}, TID 2 = {Beer, Bread}, dst.
- **Document data** — **term-frequency vector** (matriks): baris = dokumen, kolom = kata (team, coach, play, ball, score, game, win, lost, timeout, season), isi = frekuensi kata.

### (2) Graph & Network
- Jaringan transportasi, World Wide Web, struktur molekul, jaringan sosial/informasi.

### (3) Ordered Data
- **Video data**: urutan gambar (sequence of images).
- **Temporal data**: time-series.
- **Sequential data**: urutan transaksi.
- **Genetic sequence data**: sekuens DNA.

### (4) Spatial, Image & Multimedia
- **Spatial data**: peta (lapisan vektor dan raster: batas administratif, jalan, persil, penggunaan lahan, elevasi).
- **Image data**, **Video data**.

---

# BAB 3 — PREPROCESSING DATA

## 3.1 Definisi & Tujuan

- **Preprocessing data** = proses mengubah **data mentah (raw data)** menjadi data yang **siap menjadi bahan training model machine learning**.
- Juga berarti analisis untuk menentukan **bagaimana data dapat diolah terlebih dahulu** (Han & Kamber, 2011).
- **Tujuan:**
  1. Meningkatkan **kualitas data** → meningkatkan kualitas hasil penambangan.
  2. Meningkatkan **efisiensi dan kemudahan** proses penambangan.

## 3.2 Mengapa Data Perlu Dipreprocess?

- Pada kasus nyata, data mentah biasanya **kotor dan berantakan**.
- Kualitas data penting dalam ilmu data: ***garbage in, garbage out*** → data berkualitas rendah menghasilkan hasil penambangan berkualitas rendah.
- Analisis data bertujuan meningkatkan kualitas data.

## 3.3 Masalah Umum Kualitas Data

| Masalah | Penjelasan | Contoh |
|---|---|---|
| **Noise** | Class noise: kontradiktif/salah label. Attribute noise: nilai salah (salah entri, melanggar batasan data yang diketahui) | — |
| **Tak-konsisten** | Perbedaan kode atau nama | Ulang tahun = "03/07/2004" tapi Usia = "32" → tgl lahir = "03/07/2000"; rating "1,2,3" vs "A,B,C" |
| **Data hilang (missing)** | Nilai atribut kosong karena responden tidak mau mengisi survei, human error, dsb. | Pekerjaan = " " (seharusnya "staff administrasi"/"staff keuangan") |
| **Data duplikat** | Baris yang sama persis (atau hampir sama) muncul lebih dari sekali | — |
| **Dataset tidak seimbang (imbalanced)** | Jumlah sampel satu kelas jauh lebih banyak dari kelas lain | 950 normal vs 50 fraud |
| **Pencilan (outlier)** | Data yang menyimpang jauh dari mayoritas; sangat besar atau sangat kecil dari nilai normal | — |

### Identifikasi Outlier dengan Aturan IQR
```
IQR          = Q3 − Q1
Batas bawah  = Q1 − 1,5 × IQR     → data < batas bawah  = low outlier
Batas atas   = Q3 + 1,5 × IQR     → data > batas atas   = high outlier
```

**Contoh dari slide (19 data):**
`5, 7, 10, 15, 19, 21, 21, 22, 22, 23, 23, 23, 23, 23, 24, 24, 24, 24, 25`

- Median = 23; Q1 = 19; Q3 = 24
- IQR = 24 − 19 = **5**
- 1,5 × IQR = 7,5
- Batas bawah = 19 − 7,5 = **11,5**; batas atas = 24 + 7,5 = **31,5**
- Data di bawah 11,5 → **5, 7, 10 adalah outlier (low outlier)**. Tidak ada high outlier (maks = 25 < 31,5).

## 3.4 Tugas Utama Preprocessing

| Tugas | Isi |
|---|---|
| **Data cleaning** | Mengisi nilai hilang, menghaluskan data noisy, mengidentifikasi/menghapus outlier, menyelesaikan inkonsistensi |
| **Data integration** | Integrasi beberapa database, data cube, atau file |
| **Data reduction** | Dimensionality reduction, numerosity reduction, data compression |
| **Data transformation & discretization** | Normalisasi, pembentukan concept hierarchy |

## 3.5 Tantangan dalam Preprocessing

- Memakan waktu lama.
- Porsi teknis yang dominan.
- Data yang tersedia tidak akurat / tidak jelas / tidak langsung pakai.
- Data tidak seimbang saat pengambilan sampel.
- Rentan terhadap error.

## 3.6 Preprocessing untuk Data Citra (Image Processing)

**Tentu berbeda** dengan data tabular: langkah awal membersihkan & menyiapkan gambar sebelum analisis atau dimasukkan ke model (klasifikasi, deteksi objek, segmentasi).

**Tujuan utama:**
- Menstandarkan input agar model memprosesnya efisien.
- Meningkatkan kualitas data/fitur agar model belajar lebih baik.
- Mengurangi noise atau informasi tidak relevan.

| Teknik | Fungsi |
|---|---|
| **Resize** | Menyeragamkan ukuran gambar; penting untuk deep learning (input CNN harus fixed size, mis. 224×224 px) |
| **Grayscale** | Mengurangi dimensi warna dari 3 channel (RGB) menjadi 1; mempercepat proses, cocok untuk tugas sederhana (mis. deteksi tepi) |
| **Denoising** | Menghilangkan bintik/gangguan visual |
| **Normalization (nilai pixel)** | Menyesuaikan nilai pixel ke rentang tertentu (0–1 atau −1–1); membantu konvergensi lebih cepat |
| **Histogram equalization** | Meningkatkan kontras/visibilitas objek; cocok untuk gambar dengan pencahayaan buruk |
| **Data augmentation** (opsional tapi penting) | Membuat variasi gambar asli (rotasi, flipping, zoom, crop, brightness) untuk meningkatkan performa dan **menghindari overfitting** |
| **Edge detection** | Menemukan bentuk/batas objek; metode: Canny, Sobel, Laplacian |

## 3.7 Preprocessing untuk Data Spasial

| Langkah | Isi |
|---|---|
| **Visualisasi awal (ESDA – Exploratory Spatial Data Analysis)** | Peta distribusi data, heatmap, peta choropleth |
| **Pembersihan data** | Hapus duplikat entri lokasi; tangani nilai kosong/null (mis. koordinat hilang); perbaiki format (mis. lat/lon terbalik); validasi geometri (shapefile/GeoJSON) |
| **Konversi koordinat/proyeksi** | Pastikan semua data satu sistem koordinat; contoh: EPSG:4326 (WGS 84) → EPSG:3857 (Web Mercator) |
| **Normalisasi & standarisasi atribut** | Skala atribut non-spasial (suhu, curah hujan, populasi) agar bisa dipakai di ML |
| **Pengolahan geometri** | Cek validitas bentuk (polygon harus tertutup); hitung centroid, luas, panjang garis; **buffer** (area sekitar titik) |
| **Ekstraksi fitur spasial** | Jarak antar titik; **point-in-polygon**; kedekatan ke fasilitas umum (RS, sekolah) |
| **Penggabungan (spatial join/merge)** | Gabung data spasial dengan data atribut berdasarkan lokasi (mis. data COVID dengan wilayah administratif) |

---

# BAB 4 — DATA CLEANING

**Definisi:** *Fill in missing values, smooth noisy data, identify or remove outliers, and resolve inconsistencies* (mengisi nilai hilang, menghaluskan data noisy, mengidentifikasi/menghapus outlier, dan menyelesaikan inkonsistensi).

**Tujuan Data Cleaning (dari modul):**
1. Memastikan kualitas data.
2. Menghapus atau memperbaiki nilai tidak valid, hilang, atau tidak konsisten.
3. Meningkatkan akurasi model machine learning.

**Contoh masalah:** (a) nilai hilang, (b) data duplikat, (c) kesalahan format (mis. penulisan tanggal tidak konsisten), (d) outlier.

## 4.1 Penyebab Missing Values

- Perangkat/peralatan tidak berfungsi.
- Inkonsistensi data.
- Data tidak disimpan karena kesalahpahaman (kesalahan manusia).
- Data dianggap tidak penting (tidak wajib diisi).
- Perubahan/penghapusan data.
- Kerusakan data akibat kegagalan perangkat lunak database.

## 4.2 Jenis Data Hilang (Mekanisme Missing)

| Jenis | Penjelasan (versi slide) | Contoh |
|---|---|---|
| **MCAR** — *Missing Completely At Random* | Hilang murni acak; nilai hilang mengikuti **distribusi yang sama** dengan nilai yang diketahui | Tabel tercetak lalu tinta tumpah sehingga beberapa sel tak terbaca |
| **MAR** — *Missing At Random* | Data kosong karena **kesengajaan responden tidak mengisi** | Isian pendapatan/gaji: tidak semua responden mau mengisi |
| **MNAR** — *Missing Not At Random* | Mekanisme hilang **tidak terdistribusi random** | Suhu tidak terekam sensor saat suhu < 5 °C |

> ➕ **Tambahan (definisi baku statistik):** MAR berarti kemungkinan hilang **bergantung pada variabel lain yang teramati**, bukan pada nilai yang hilang itu sendiri. MNAR berarti kemungkinan hilang **bergantung pada nilai yang hilang itu sendiri**. Untuk ujian, tulis definisi dari slide lalu (opsional) tambahkan definisi baku ini.

## 4.3 Cara Menangani Missing Values

1. **Abaikan** (hapus record yang mengandung nilai kosong) — *bukan pilihan jika nilai kosong banyak*, karena menghapus akan mengurangi jumlah data secara signifikan.
2. **Isi manual** — tidak efektif jika data banyak.
3. **Imputasi otomatis** — mengganti nilai hilang (missing value, NaN, blank, dll.) dengan **nilai pengganti**.

## 4.4 Teknik Imputasi

| Tipe data | Teknik |
|---|---|
| **Numerik** | Imputasi mean/median; imputasi nilai arbitrer (suka-suka); imputasi nilai ujung (end of tail) |
| **Nominal (kategorikal)** | Imputasi kategori yang sering muncul (modus); menambah kategori "hilang" |
| **Lainnya** | Nol/konstanta; K-NN; regresi deterministik; regresi stokastik; random sample |

### a) Imputasi Mean atau Median
Contoh slide: Age = 29, 43, NA, 25, 34, NA, 50 → mean = (29+43+25+34+50)/5 = **36,2** → kedua NA diisi 36,2.

| Kelebihan | Kekurangan |
|---|---|
| Mudah & cepat | **Tidak memperhitungkan korelasi antar fitur** (bekerja di level kolom) |
| Efektif untuk dataset numerik berukuran kecil | Kurang akurat |
| Cocok untuk variabel numerik | Tidak memperhitungkan probabilitas/ketidakpastian |
| Cocok untuk data **MCAR** | **Tidak cocok bila missing > 5%** |
| Dapat dipakai di produksi (model deployment) | **Mendistorsi variansi & distribusi** variabel asli serta kovarians dengan variabel lain |

### b) Imputasi Nilai Arbitrer (Suka-suka)
Contoh: NA diganti **99** (mis. Age).

| Kelebihan | Kekurangan |
|---|---|
| Asumsinya data **tidak** missing at random | Mendistorsi variansi, distribusi, kovarians |
| Mudah & cepat melengkapi dataset | Dapat **membentuk outlier** (bila nilai di ujung distribusi) |
| | Semakin banyak NA → semakin besar distorsi |
| | **Hindari memilih nilai dekat mean/median** |

### c) Imputasi Nilai Akhir (End of Tail)
Mirip imputasi arbitrer; cocok untuk variabel numerik. Contoh slide: NA → 66,89.

**Ketentuan memilih nilai:**
```
Jika distribusi normal :  nilai = mean + 3 × standar deviasi
Jika distribusi skew   :  aturan IQR proximity
                          IQR         = Q3 − Q1
                          Upper limit = Q3 + 3 × IQR
                          Lower limit = Q1 − 3 × IQR
```
**Hanya digunakan pada data latih (training set).**

> ⚠️ Bedakan: deteksi outlier memakai **1,5 × IQR**, sedangkan imputasi end-of-tail memakai **3 × IQR**.

### d) Imputasi Frequent Category (Modus)
Contoh: kolom Make (Ford, Ford, Fiat, BMW, Ford, Kia, **NA**, Fiat, Ford, **NA**, Kia) → modus = **Ford** → NA diganti Ford.

| Kelebihan | Kelemahan |
|---|---|
| Cocok untuk data missing at random | **Mendistorsi relasi** label berfrekuensi tertinggi dengan variabel lain |
| Mudah & cepat | Menghasilkan **over-representation** bila banyak data hilang |
| Cocok untuk data yang skew | |
| Dapat dipakai di produksi | |

### e) Imputasi Sampel Acak (Random Sample)
Mengisi NA dengan nilai yang **diambil acak dari nilai teramati** variabel yang sama.

| Kelebihan | Kekurangan |
|---|---|
| Cocok untuk data missing at random | **Randomness** (hasil tidak deterministik) |
| Mengganti dengan nilai dalam **distribusi yang sama** dengan variabel asli | Butuh **memori besar** saat deployment karena harus menyimpan data latih asli untuk mengekstrak nilai |
| Mudah & cepat | |
| **Mempertahankan varians** variabel | |

### f) Teknik Imputasi Lainnya

| Teknik | Catatan |
|---|---|
| **Nol / konstanta** | Mudah, tetapi dapat memunculkan **bias** |
| **K-NN** | **Lebih akurat** dibanding teknik lain, tetapi **komputasi besar** |
| **Regresi deterministik** | Mengganti NA dengan **prediksi tepat** dari model regresi |
| **Regresi stokastik** | Mengatasi masalah regresi deterministik dengan menambah **"random error"** sehingga reproduksi korelasi X–Y lebih tepat |

## 4.5 Penanganan Data Kotor (Noisy Data)

**Data kotor** = memuat **error** atau **outlier** (data yang secara nyata berbeda dari data lain).

**Bentuk data kotor:** data tidak konsisten, pencilan (outlier), data tidak utuh (incomplete), data ganda (duplicate).

### Teknik penanganan

| Teknik | Penjelasan |
|---|---|
| **Binning** | Data **diurutkan & dikelompokkan**, lalu dilakukan **penghalusan (smoothing)**. Bisa diterapkan pada data kategorik dan numerik. Model lebih **robust** dan mencegah **overfitting**. |
| **Regression** | Penghalusan berdasarkan **fungsi regresi** |
| **Clustering** | **Mendeteksi & menghilangkan outlier** |
| **Kombinasi komputer + manual** | Memerlukan campur tangan manusia |

> ➕ **Tambahan — cara kerja binning (standar Han & Kamber):** data diurutkan → dibagi ke dalam *bin* (**equal-width**: rentang sama; **equal-frequency/depth**: jumlah data sama) → dihaluskan dengan **bin means** (semua nilai diganti rata-rata bin), **bin median**, atau **bin boundaries** (tiap nilai diganti batas bin terdekat).
> Contoh: data terurut 4, 8, 15, 21, 21, 24, 25, 28, 34 → 3 bin equal-frequency: (4,8,15), (21,21,24), (25,28,34). Smoothing by means: (9,9,9), (22,22,22), (29,29,29).

---

# BAB 5 — DATA INTEGRATION

**Definisi:** menggabungkan data dari **beberapa sumber** (database, data cube, atau file) ke dalam **satu penyimpanan/tampilan yang koheren dan konsisten**.

**Mengapa penting (modul):**
1. Data sering tersebar di berbagai file, database, atau sistem.
2. Integrasi yang baik meningkatkan kualitas analisis dan hasil prediksi.

**Contoh kasus:** menggabungkan data pelanggan dari sistem penjualan & sistem keuangan; menggabungkan data karyawan dari HR & sistem kehadiran.

## 5.1 Tiga Masalah Utama Integrasi

| Masalah | Penjelasan | Contoh |
|---|---|---|
| **Schema Integration** | Mengintegrasikan **metadata** dari berbagai sumber | `A.cust-id ≡ B.cust-#` |
| **Entity Identification Problem** | Mengidentifikasi **entitas dunia nyata** yang sama dari banyak sumber | Bill Clinton = William Clinton |
| **Detecting & Resolving Data Value Conflicts** | Untuk entitas yang sama, **nilai atribut berbeda** antar sumber. Penyebab: representasi berbeda, skala berbeda | Satuan metrik vs British units |

## 5.2 Menangani Redundansi saat Integrasi

Data redundan sering muncul saat integrasi banyak database:
- **Object identification:** atribut/objek yang sama punya **nama berbeda** di database berbeda.
- **Derivable data:** satu atribut merupakan atribut **turunan** di tabel lain (mis. *annual revenue*).
- Integrasi yang hati-hati membantu mengurangi/menghindari **redundansi & inkonsistensi** serta meningkatkan **kecepatan dan kualitas** mining.
- **Atribut redundan dapat dideteksi dengan analisis korelasi dan analisis kovarians.**

## 5.3 Analisis Korelasi — Data Nominal: Uji Chi-Square (χ²)

**Rumus:**
```
χ² = Σ  (Observed − Expected)² / Expected
```
- **Observed** = frekuensi hasil pengamatan tiap sel tabel kontingensi.
- **Expected** = frekuensi harapan bila kedua atribut **independen**:
```
Expected(sel) = (jumlah baris × jumlah kolom) / total keseluruhan
```
- Derajat bebas: **df = (jumlah baris − 1) × (jumlah kolom − 1)**.

**Interpretasi:**
- **Makin besar nilai χ², makin besar kemungkinan kedua variabel berhubungan.**
- Sel yang paling besar kontribusinya adalah sel yang **frekuensi aktualnya sangat berbeda dari frekuensi harapan**.
- **Korelasi ≠ kausalitas.** Contoh: jumlah rumah sakit dan jumlah pencurian mobil di sebuah kota berkorelasi, keduanya dipengaruhi variabel ketiga: **populasi**.

### Contoh dari slide (like science fiction × play chess)

| | Play chess | Not play chess | Sum (row) |
|---|---|---|---|
| Like science fiction | 250 (90) | 200 (360) | 450 |
| Not like science fiction | 50 (210) | 1000 (840) | 1050 |
| Sum (col) | 300 | 1200 | 1500 |

(Angka dalam kurung = expected. Contoh: E = 450 × 300 / 1500 = 90.)

```
χ² = (250−90)²/90 + (50−210)²/210 + (200−360)²/360 + (1000−840)²/840
   = 284,44 + 121,90 + 71,11 + 30,48
   = 507,93
```
**Kesimpulan:** `like_science_fiction` dan `play_chess` **berkorelasi** pada kelompok tersebut. (df = 1, χ² jauh di atas nilai kritis 3,841 pada α = 0,05.)

### ✏️ LATIHAN SLIDE: Dosen vs Nilai Ujian

| Nilai Ujian | Dosen A | Dosen B | Total |
|---|---|---|---|
| Lulus | 20 | 10 | 30 |
| Tidak Lulus | 10 | 20 | 30 |
| Total | 30 | 30 | 60 |

**Penyelesaian:**
- Expected tiap sel = (30 × 30) / 60 = **15** (keempat sel sama).
- χ² = (20−15)²/15 + (10−15)²/15 + (10−15)²/15 + (20−15)²/15
  = 25/15 × 4 = **6,67**
- df = (2−1)(2−1) = 1; nilai kritis χ² (α = 0,05, df = 1) = **3,841** → 6,67 > 3,841 (p ≈ 0,0098).
- **Kesimpulan:** ada hubungan signifikan antara dosen pengampu dan hasil ujian (dosen A cenderung menghasilkan lebih banyak mahasiswa lulus: 20 vs 10 lulus). Ingat: ini hubungan/asosiasi, **bukan bukti sebab-akibat**.

## 5.4 Analisis Korelasi — Data Numerik: Koefisien Korelasi Pearson

*(juga disebut Pearson's product moment coefficient)*

```
         Σ (aᵢ − Ā)(bᵢ − B̄)         Σ (aᵢbᵢ) − n·Ā·B̄
r_A,B = ───────────────────── = ─────────────────────
            (n − 1)·σA·σB             (n − 1)·σA·σB
```
Keterangan: `n` = jumlah tuple; `Ā`, `B̄` = rata-rata A dan B; `σA`, `σB` = simpangan baku A dan B; `Σ(aᵢbᵢ)` = jumlah *cross-product* AB.

**Interpretasi:**
- `r > 0` → **berkorelasi positif** (nilai A naik seiring B); makin tinggi, makin kuat.
- `r = 0` → **independen** (tidak ada hubungan linear).
- `r < 0` → **berkorelasi negatif**.
- Rentang −1 sampai +1. Scatter plot: titik membentuk garis menurun tegas (−1,00) → menyebar acak (0,00) → garis menaik tegas (+1,00).
- Korelasi mengukur **hubungan linear** antar objek.

**Korelasi dipandang sebagai dot product (slide 22):** standardisasi A dan B, lalu hitung dot product:
```
a'ₖ = (aₖ − mean(A)) / std(A)
b'ₖ = (bₖ − mean(B)) / std(B)
correlation(A, B) = A' • B'
```

## 5.5 Kovarians (Data Numerik)

```
Cov(A,B) = E[(A − Ā)(B − B̄)] = Σ (aᵢ − Ā)(bᵢ − B̄) / n
```
**Bentuk sederhana untuk komputasi:**
```
Cov(A,B) = E(A·B) − Ā·B̄
```
**Hubungan dengan korelasi:**
```
r_A,B = Cov(A,B) / (σA · σB)
σA = √[ (1/n) Σ (Aᵢ − Ā)² ]
```

**Interpretasi:**
- `Cov > 0` → A dan B **cenderung sama-sama lebih besar dari nilai harapannya** (naik bersama).
- `Cov < 0` → bila A di atas nilai harapannya, B cenderung di bawah nilai harapannya.
- **Independensi:** jika A dan B independen maka `Cov = 0`, **tetapi kebalikannya tidak selalu benar**. Beberapa pasangan variabel acak punya kovarians 0 namun tidak independen. Hanya dengan asumsi tambahan (mis. data mengikuti distribusi normal multivariat) kovarians 0 berarti independen.

### Contoh slide: dua saham A dan B
Data (A, B): (2,5), (3,8), (5,10), (4,11), (6,14).
- E(A) = (2+3+5+4+6)/5 = 20/5 = **4**
- E(B) = (5+8+10+11+14)/5 = 48/5 = **9,6**
- Cov(A,B) = (2·5 + 3·8 + 5·10 + 4·11 + 6·14)/5 − 4 × 9,6 = 212/5 − 38,4 = 42,4 − 38,4 = **4**
- Cov > 0 → **harga A dan B naik bersama**.

### ✏️ LATIHAN SLIDE: Saham AllElectronics vs HighTech (Tabel 3.2 Han & Kamber)

| Time point | AllElectronics (A) | HighTech (B) |
|---|---|---|
| t1 | 6 | 20 |
| t2 | 5 | 10 |
| t3 | 4 | 14 |
| t4 | 3 | 5 |
| t5 | 2 | 5 |

**Penyelesaian:**
- E(A) = (6+5+4+3+2)/5 = 20/5 = **4**
- E(B) = (20+10+14+5+5)/5 = 54/5 = **10,8**
- E(A·B) = (6·20 + 5·10 + 4·14 + 3·5 + 2·5)/5 = (120+50+56+15+10)/5 = 251/5 = **50,2**
- **Cov(A,B) = 50,2 − 4 × 10,8 = 50,2 − 43,2 = 7**
- Cov = 7 > 0 → **harga kedua saham cenderung naik bersama** (jika dipengaruhi tren industri yang sama, naik/turun bersamaan).

---

# BAB 6 — DATA REDUCTION

## 6.1 Definisi & Alasan

- **Reduksi data** = memperoleh **representasi dan volume data yang lebih kecil** tetapi tetap memberikan **hasil analisis yang sama (atau hampir sama)**.
- **Mengapa?** Database dapat menyimpan data hingga **terabyte**; analisis kompleks pada data sebesar itu bisa **sangat lama**.
- Berguna meminimalkan ukuran dataset sambil **mempertahankan informasi terpenting**; meningkatkan efisiensi & kinerja algoritma pembelajaran mesin.
- **Pertimbangan (trade-off):** ukuran data vs akurasi → nilai risiko dan manfaat dengan cermat sebelum menerapkan.

**Strategi reduksi data** (Preprocessing): *dimensionality reduction, numerosity reduction, data compression*. Modul membahas **dua jenis**:

```
DATA REDUCTION
 ├─ 1. Dimensionality Reduction  (kurangi jumlah fitur/atribut)
 │     ├─ Transformasi Wavelet
 │     ├─ PCA (Principal Component Analysis)
 │     └─ Feature/Attribute Subset Selection (supervised/nonlinear)
 └─ 2. Numerosity Reduction  (kurangi volume data / jumlah objek)
       ├─ Parametrik     : Regresi, Log-linear model
       └─ Non-parametrik : Histogram, Clustering, Sampling
```

## 6.2 Dimensionality Reduction

### Curse of Dimensionality
> Ketika jumlah dimensi meningkat, data menjadi **semakin jarang (sparse)** dan **sulit dianalisis**. (Ilustrasi slide: 1 dimensi = 10 posisi; 2 dimensi = 100 posisi; 3 dimensi = 1000 posisi → ruang tumbuh eksponensial sementara jumlah data tetap.)

### Manfaat Dimensionality Reduction
1. **Menghindari curse of dimensionality.**
2. Membantu **menghilangkan fitur tidak penting & mengurangi kebisingan (noise)**.
3. **Mengurangi waktu dan ruang** (memori).
4. **Memudahkan visualisasi.**

Teknik: **Transformasi Wavelet**, **PCA**, **teknik supervised & nonlinear (mis. feature selection)**.

(Modul `.txt`: teknik ini *mengurangi jumlah fitur dengan menghapus fitur tidak relevan ataupun menggabungkan beberapa fitur menjadi satu*. Contoh metode: Wavelet transformation, PCA, feature selection, attribute subset selection.)

### a) Transformasi Wavelet (Wavelet Transform)
- **Merepresentasikan data dalam bentuk komponen frekuensi dan waktu secara simultan.**
- **Tujuan:**
  - Mengubah data asli menjadi **representasi yang lebih sederhana**.
  - **Mengabaikan komponen frekuensi tinggi yang sering berupa noise.**
  - **Mempertahankan karakteristik utama** data dengan ukuran lebih kecil.
- **Jenis data & penggunaannya:**
  - **Gambar:** kompresi gambar, deteksi tepi, penghapusan noise
  - **Sinyal audio:** kompresi, denoising
  - **Data time-series:** penyederhanaan pola data, deteksi anomali

### b) Principal Component Analysis (PCA)
- Mencari **proyeksi yang menangkap variansi terbesar** dalam data.
- Data asli diproyeksikan ke **ruang yang jauh lebih kecil** → dimensi berkurang.
- Caranya: mencari **vektor eigen (eigenvector) dari matriks kovarians**; vektor-vektor eigen ini **mendefinisikan ruang baru** (komponen utama: PC1, PC2, …).
- Ilustrasi slide: data 3D (x,y,z) → diproyeksikan ke PC1 & PC2 (2D) → lalu PC1 saja (1D).

> ➕ **Tambahan — langkah PCA:** (1) standardisasi data; (2) hitung matriks kovarians; (3) hitung eigenvalue & eigenvector; (4) urutkan eigenvector menurut eigenvalue terbesar (PC1 = arah dengan variansi terbesar); (5) pilih k komponen teratas; (6) proyeksikan data ke k komponen. Eigenvalue = besar variansi yang ditangkap tiap komponen.

### c) Attribute Subset Selection
Cara lain mengurangi dimensi data dengan membuang atribut:
- **Atribut redundan** — mengandung banyak atau seluruh informasi yang sudah ada di satu atau lebih atribut lain. *Contoh: harga produk dan pajaknya.*
- **Atribut tidak relevan** — tidak mengandung informasi berguna untuk tugas mining. *Contoh: NIM mahasiswa tidak berpengaruh pada IPK.*

## 6.3 Numerosity Reduction

**Mengurangi volume data dengan memilih bentuk representasi alternatif yang lebih kecil.**

| Kelompok | Konsep | Contoh |
|---|---|---|
| **Parametrik** | **Mengasumsikan data sesuai suatu model**, estimasi parameter model, **simpan hanya parameternya**, buang data (kecuali kemungkinan outlier) | Regresi; **Log-linear models** (nilai pada suatu titik di ruang m-dimensi diperoleh sebagai hasil kali subruang marginal yang sesuai) |
| **Non-parametrik** | **Tidak mengasumsikan model** | Histogram, clustering, sampling |

### Analisis Regresi
- Nama kolektif untuk teknik **pemodelan & analisis data numerik** yang terdiri atas nilai **variabel dependen** (response/measurement) dan satu atau lebih **variabel independen** (explanatory/predictor).
- Parameter diestimasi agar memberi **"best fit"**; paling umum dinilai dengan **metode kuadrat terkecil (least squares)**, meski kriteria lain juga dipakai.
- Digunakan untuk **prediksi** (termasuk forecasting time-series), **inferensi**, **uji hipotesis**, dan **pemodelan hubungan kausal**.
- Contoh ilustrasi slide: garis `y = x + 1`; data (X1, Y1) diganti nilai prediksi Y1'.
- Dalam reduksi: cukup simpan **koefisien garis (parameter)**, bukan seluruh data.

### Analisis Histogram
- **Merepresentasikan distribusi data** menggunakan **rentang nilai (bins) dan frekuensi kemunculannya.**
- **Tujuan:** mengganti data asli dengan **representasi statistik berupa jumlah frekuensi dalam interval tertentu**, sehingga ukuran data lebih kecil **tanpa kehilangan pola utama**.

### Sampling
- **Sampling** = mendapatkan **sampel kecil S** untuk merepresentasikan seluruh dataset N.
- Memungkinkan algoritma mining berjalan dengan kompleksitas yang berpotensi **sub-linear** terhadap ukuran data.
- **Prinsip kunci:** pilih **subset yang representatif**.
- **Simple random sampling** bisa berkinerja sangat buruk pada data yang **skew** → dikembangkan metode adaptif, mis. **stratified sampling**.
- Catatan: sampling **belum tentu mengurangi I/O database** (karena halaman dibaca per page).

**Jenis sampling:**
| Jenis | Penjelasan |
|---|---|
| **Simple random sampling** | Setiap item punya **probabilitas sama** untuk terpilih |
| **Sampling without replacement** | Objek yang sudah terpilih **dikeluarkan** dari populasi |
| **Sampling with replacement** | Objek terpilih **tidak dikeluarkan** (bisa terpilih lagi) |
| **Stratified sampling** | Data dibagi ke dalam strata/kelompok, lalu sampel diambil dari tiap strata (menjaga proporsi) |

---

# BAB 7 — DATA TRANSFORMATION

## 7.1 Definisi

> Fungsi yang memetakan **seluruh himpunan nilai suatu atribut ke himpunan nilai pengganti yang baru**, sehingga setiap nilai lama dapat diidentifikasi dengan salah satu nilai baru.

Dalam konteks penambangan data: tindakan mengubah data mentah ke **format yang sesuai untuk analisis dan pemodelan**, memastikan data **bebas kesalahan & inkonsistensi**, dan membantu kinerja algoritma dengan **menskalakan nilai ke rentang umum**.

## 7.2 Metode Transformasi

| Metode | Penjelasan |
|---|---|
| **Smoothing** | Menghilangkan noise dari data |
| **Attribute/Feature construction** | Membentuk atribut baru dari atribut yang ada |
| **Aggregation** | Peringkasan, pembangunan data cube |
| **Normalization** | Menskalakan nilai ke rentang yang lebih kecil dan ditentukan |
| **Discretization** | Menaikkan level pada **concept hierarchy** |

Normalisasi: **min-max**, **z-score**, dan **decimal scaling**. (Modul: normalisasi mengubah semua variabel ke rentang tertentu, biasanya **[0,1]**.)

## 7.3 Min-Max Normalization

Memetakan ke rentang `[new_minA, new_maxA]`:
```
v' = ( (v − minA) / (maxA − minA) ) × (new_maxA − new_minA) + new_minA
```
**Contoh slide:** pendapatan rentang \$12.000–\$98.000 dinormalisasi ke [0,1]. Maka \$73.600:
```
v' = (73.600 − 12.000) / (98.000 − 12.000) × (1,0 − 0) + 0 = 61.600 / 86.000 = 0,716
```
- Menjaga hubungan antar data asli; **rentan terhadap outlier** (min/max ikut bergeser). ➕

## 7.4 Z-Score Normalization

(μ = rata-rata, σ = simpangan baku)
```
v' = (v − μA) / σA
```
**Contoh slide:** μ = 54.000, σ = 16.000 → \$73.600:
```
v' = (73.600 − 54.000) / 16.000 = 1,225
```
- Hasil: rata-rata 0 dan simpangan baku 1. Nilai positif = di atas rata-rata; negatif = di bawah. ➕

## 7.5 Normalization by Decimal Scaling

```
v' = v / 10^j      dengan j = bilangan bulat terkecil sehingga Max(|v'|) < 1
```
**Contoh slide:** nilai terbesar 76.300 → j = 5 → v' = 76.300 / 10⁵ = **0,763**.

### ✏️ LATIHAN SLIDE: Normalisasi Tinggi Badan dengan Ketiga Metode

| No | Mahasiswa | Tinggi (cm) |
|---|---|---|
| 1 | Kumiko | 155 |
| 2 | April | 160 |
| 3 | Rossy | 170 |
| 4 | Kepin | 180 |
| 5 | Yasmin | 162 |
| 6 | Syahrul | 175 |
| 7 | Tania | 168 |
| 8 | Zafa | 164 |

**Statistik dasar:** n = 8; Σ = 1334; min = 155; max = 180; **mean μ = 166,75**.
Kuadrat selisih: 138,0625; 45,5625; 10,5625; 175,5625; 22,5625; 68,0625; 1,5625; 7,5625 → jumlah = 469,5.
- Variansi populasi = 469,5/8 = 58,6875 → **σ = 7,6608** (populasi).
- Variansi sampel = 469,5/7 = 67,0714 → s = 8,1897 (sampel).

**1) Min-Max ke [0,1]:** `v' = (v − 155)/(180 − 155) = (v − 155)/25`

| Mahasiswa | v | Min-Max |
|---|---|---|
| Kumiko | 155 | **0,00** |
| April | 160 | **0,20** |
| Rossy | 170 | **0,60** |
| Kepin | 180 | **1,00** |
| Yasmin | 162 | **0,28** |
| Syahrul | 175 | **0,80** |
| Tania | 168 | **0,52** |
| Zafa | 164 | **0,36** |

**2) Z-Score:** `z = (v − 166,75)/σ`

| Mahasiswa | v | Z (σ populasi = 7,6608) | Z (σ sampel = 8,1897) |
|---|---|---|---|
| Kumiko | 155 | −1,534 | −1,435 |
| April | 160 | −0,881 | −0,824 |
| Rossy | 170 | 0,424 | 0,397 |
| Kepin | 180 | 1,730 | 1,618 |
| Yasmin | 162 | −0,620 | −0,580 |
| Syahrul | 175 | 1,077 | 1,007 |
| Tania | 168 | 0,163 | 0,153 |
| Zafa | 164 | −0,359 | −0,336 |

*(Tulis di ujian mana yang kamu pakai: populasi atau sampel. Rumus slide memakai σ tanpa keterangan "n−1", jadi σ populasi aman sebagai jawaban utama.)*

**3) Decimal Scaling:** nilai terbesar 180 → agar |v'| < 1 diperlukan **j = 3** (180/10² = 1,8 ≥ 1 tidak memenuhi; 180/10³ = 0,18 < 1 memenuhi).

| Mahasiswa | Decimal scaling (v/1000) |
|---|---|
| Kumiko | 0,155 |
| April | 0,160 |
| Rossy | 0,170 |
| Kepin | 0,180 |
| Yasmin | 0,162 |
| Syahrul | 0,175 |
| Tania | 0,168 |
| Zafa | 0,164 |

### Perbandingan tiga metode normalisasi
| Aspek | Min-Max | Z-Score | Decimal Scaling |
|---|---|---|---|
| Rentang hasil | [new_min, new_max] (umumnya 0–1) | Tidak terbatas (rata-rata 0, σ 1) | (−1, 1) |
| Butuh | min & max | mean & std | nilai absolut maksimum |
| Sensitif outlier | Sangat | Lebih tahan (tapi mean & std terpengaruh) | Tergantung nilai maks |
| Cocok untuk | Rentang tetap diperlukan | Data kira-kira normal; algoritma berbasis jarak/gradien | Skala sederhana |

---

# BAB 8 — INFORMATION EXTRACTION & FEATURE SELECTION

## 8.1 Information Extraction (IE)

- **Definisi:** proses **mengidentifikasi, mengekstrak, dan menyusun informasi terstruktur dari data tidak/semi-terstruktur** (teks, dokumen, web, media sosial). **Tujuan: mengubah data mentah menjadi fakta yang bisa dianalisis.**
- Dalam data mining: IE dipakai menemukan **pola/pengetahuan baru dari data besar** sehingga lebih terstruktur. IE dilatih dengan **algoritma machine learning** untuk mengenali pola otomatis.
- **Fungsi IE dalam ML (modul):**
  1. **Menyediakan data relevan** bagi model (informasi penting diekstrak dari sumber mentah: teks berita, laporan, komentar).
  2. **Meningkatkan kualitas fitur (feature engineering)** — hasil IE menjadi *input features* untuk klasifikasi, prediksi, rekomendasi.
  3. **Mendukung otomatisasi analisis big data** — mengenali entitas, hubungan, peristiwa tanpa campur tangan manusia.
- **Catatan penting:** IE sering menghasilkan **banyak informasi yang tidak relevan**. **Semakin banyak fitur ≠ semakin baik.** Kualitas IE yang baik menghasilkan fitur yang **relevan dan diskriminatif** (mampu membedakan kelas, mis. kanker vs non-kanker). Kuantitas fitur belum tentu berbanding lurus dengan kualitas model.

**Fitur** = atribut, variabel, atau karakteristik yang menggambarkan suatu objek/entitas dan dipakai sebagai input model ML.

## 8.2 Apa yang terjadi bila fitur terlalu banyak?

1. **Curse of dimensionality**
2. **Overfitting**
3. **Redundansi**
4. **Interpretabilitas turun**
5. **Biaya (cost) tinggi** — kompleksitas model, waktu komputasi lama.

**Contoh modul (BMI):** klasifikasi Indeks Massa Tubuh hanya dipengaruhi **berat badan** dan **tinggi badan**; warna rambut, pekerjaan, alamat tidak relevan → perlu **feature selection**.

## 8.3 Feature Selection

**Definisi:** proses memilih **subset fitur paling relevan** dari sekumpulan fitur yang ada.

**Tujuan:**
- Mengurangi **dimensionality**.
- Mengurangi **noise / data tidak relevan**.
- Meningkatkan **akurasi** model.
- **Mempercepat** pelatihan dan inferensi.
- **Menghindari overfitting**.

**Kapan feature selection penting?**
- Data mengandung banyak **noise**.
- Banyak fitur yang **jarang muncul**.
- Kolom fitur memiliki banyak **missing value**.
- Data mengandung fitur dengan **banyak tipe data**.
- **Jumlah fitur > jumlah sampel** data.
- Model memiliki banyak parameter (**model kompleks**).

**Alur klasifikasi ML:** data → (preprocessing, **seleksi fitur**) → training → testing → evaluasi.

## 8.4 Tiga Kelompok Metode Seleksi Fitur

| Metode | Prinsip | Melibatkan model ML? |
|---|---|---|
| **Filter** | Memilih fitur berdasarkan **perhitungan statistik / hubungan matematis** fitur–target | ✗ Tidak |
| **Wrapper** | Memakai **model ML sebagai "penguji"** untuk menilai subset fitur | ✓ Ya (berulang kali) |
| **Embedded** | Seleksi dilakukan **bersamaan dengan pelatihan model**; fitur tidak relevan diberi **penalti/bobot kecil** | ✓ Ya (di dalam training) |

### A. FILTER METHODS

Teknik umum: **Korelasi (Correlation Coefficient), Chi-Square, Algoritma Relief, ANOVA, Information Gain / Mutual Information, Variance Threshold.**

#### A1. Korelasi
- Ukuran statistik hubungan antara dua variabel; di feature selection dipakai untuk melihat apakah **fitur berhubungan kuat dengan target (label)**.
- Jenis: **Pearson**, **Spearman Rank**, **Kendall's Tau**.

**Pearson Correlation Coefficient (PCC)** — mengukur **kekuatan (strength)** dan **arah (direction)** hubungan **linear** antar dua variabel.
- *Kekuatan:* seberapa erat hubungan.
- *Arah:* positif (X naik, Y naik) atau negatif (X naik, Y turun).
- Nilai −1 s.d. +1.
```
          Σ (xᵢ − x̄)(yᵢ − ȳ)
r = ─────────────────────────────────────
     √[Σ (xᵢ − x̄)²] · √[Σ (yᵢ − ȳ)²]
```
dengan `n` = jumlah titik data; `x`, `y` = dua variabel; `Σ` = penjumlahan.
- Diagram slide: strong positive, medium positive, weak/no correlation, medium negative, strong negative.
- Seleksi: pilih fitur dengan **|r| lebih besar dari ambang batas**.

#### A2. Chi-Square
- Rumus sama seperti Bab 5: `χ² = Σ (O − E)² / E`.
- Dalam seleksi fitur: **pilih fitur dengan nilai χ² besar** (makin besar makin relevan terhadap target). Cocok untuk fitur **kategorikal** vs target kategorikal.

#### A3. Information Gain (IG)
- Ukuran **berapa banyak informasi tentang target (label)** yang diperoleh dari suatu fitur. Sering dipakai pada **Decision Tree** untuk memilih atribut terbaik di tiap *split*.
```
Entropy (H):            H(Y) = − Σᵢ₌₁ⁿ P(yᵢ) · log₂ P(yᵢ)
Information Gain (IG):  IG(Y, X) = H(Y) − H(Y | X)
```
- `H(Y)` = entropi awal target; `H(Y|X)` = entropi target setelah mengetahui fitur X.
- **IG besar → fitur makin informatif.**

> ➕ **Contoh hitung IG (tambahan):** 14 sampel, target Y: 9 "Ya", 5 "Tidak". H(Y) = −(9/14)log₂(9/14) − (5/14)log₂(5/14) = **0,940**. Fitur Outlook: Sunny (2 Ya, 3 Tidak) → H = 0,971; Overcast (4 Ya, 0 Tidak) → H = 0; Rain (3 Ya, 2 Tidak) → H = 0,971. H(Y|Outlook) = (5/14)(0,971) + (4/14)(0) + (5/14)(0,971) = 0,694. **IG = 0,940 − 0,694 = 0,246.**

#### A4. Algoritma Relief
- **Memperkirakan kualitas atribut** berdasarkan **seberapa baik atribut membedakan antara contoh yang berdekatan**.

**Cara kerja:**
1. **Inisialisasi bobot fitur** (semua fitur, biasanya 0).
2. **Sampling instance**: pilih acak satu instance dari dataset.
3. **Cari tetangga terdekat:**
   - **Nearest Hit (H)**: instance terdekat dengan **kelas sama**.
   - **Nearest Miss (M)**: instance terdekat dari **kelas berbeda**.
4. **Update bobot** tiap fitur:
   - Nilai fitur **berbeda dengan nearest hit** → bobot **diturunkan** (fitur tidak konsisten dalam kelas yang sama).
   - Nilai fitur **berbeda dengan nearest miss** → bobot **dinaikkan** (fitur membantu membedakan kelas).
5. **Ulangi** langkah 2–4 untuk sejumlah iterasi / sampai semua instance diproses.
6. **Hasil:** fitur dengan **bobot positif dan tinggi** = relevan; **negatif/rendah** = kurang penting.

**Rumus (disederhanakan):**
```
W[A] = W[A] − diff(A, X, H) + diff(A, X, M)
```
- `W[A]` = bobot fitur A
- `diff(A, X, H)` = perbedaan nilai fitur A antara instance X dan nearest hit H
- `diff(A, X, M)` = perbedaan nilai fitur A antara instance X dan nearest miss M

#### A5. ANOVA & Variance Threshold (disebut di slide)
> ➕ **Tambahan:** **ANOVA (F-test)** menguji apakah rata-rata fitur numerik berbeda signifikan antar kelas target (F besar → fitur relevan). **Variance Threshold** membuang fitur yang variansinya di bawah ambang (fitur hampir konstan tidak informatif).

### B. WRAPPER METHODS

Menggunakan **model ML untuk menguji berbagai subset fitur**, lalu memilih kombinasi dengan **performa terbaik**.

**Alur umum:** (1) pilih subset fitur → (2) latih model dengan subset itu → (3) ukur performa (mis. akurasi) → (4) ulangi dengan subset lain → (5) pilih subset terbaik.

Jenis: **Forward Selection, Backward Elimination, Recursive Feature Elimination (RFE), Heuristic/Metaheuristic Search.**

#### B1. Forward Selection
Membangun subset **dari nol**, menambah fitur satu per satu berdasarkan kontribusinya.
1. Mulai dengan **subset kosong**.
2. Evaluasi **tiap fitur tunggal** dengan model (regresi/logistic regression/decision tree).
3. Pilih fitur yang memberi **peningkatan performa terbesar**.
4. Tambahkan ke subset.
5. Ulangi, selalu menambah fitur yang meningkatkan performa paling signifikan.
6. **Berhenti** bila **tidak ada peningkatan signifikan** atau **semua fitur sudah dipilih**.

Diagram slide: `Null → {x1}{x2}{x3} → {x2x1}{x2x3} → {x2x3x1}`.

#### B2. Backward Elimination
**Kebalikan** forward: mulai dari **semua fitur**, lalu **menghapus satu per satu** hingga kinerja paling optimum.
Diagram slide: `{x1x2x3} → {x1x2}{x1x3}{x2x3} → {x2}{x3} → Null`.

#### B3. Recursive Feature Elimination (RFE)
Menghapus **fitur dengan kontribusi terkecil secara bertahap** sampai tersisa jumlah fitur yang diinginkan.
1. Pilih **model dasar** (Logistic Regression, SVM, Random Forest).
2. Latih model dengan **semua fitur**.
3. Hitung **importance** tiap fitur (koefisien atau feature importance).
4. **Buang** fitur berkontribusi terkecil.
5. Ulangi langkah 2–4 sampai jumlah fitur sesuai target.

### C. EMBEDDED METHODS
- Seleksi fitur dilakukan **sekaligus saat model dilatih**; proses belajar bobot dan penentuan fitur penting terjadi **bersamaan**. Fitur tidak relevan diberi penalti/bobot kecil.
- Algoritma: **Regularisasi (L1, L2)** dan **Random Forest Importance**.

> ➕ **Tambahan:** **L1 (Lasso)** dapat mengecilkan koefisien fitur tidak penting hingga **tepat nol** (otomatis menyeleksi fitur). **L2 (Ridge)** mengecilkan koefisien tetapi umumnya tidak sampai nol. **Random Forest Importance** menilai fitur dari seberapa besar fitur itu menurunkan impuritas di pohon-pohon.

### Perbandingan Filter vs Wrapper vs Embedded

| Aspek | Filter | Wrapper | Embedded |
|---|---|---|---|
| Dasar penilaian | Statistik (korelasi, χ², IG, …) | Performa model pada subset | Bobot/penalti saat training |
| Pakai model ML? | Tidak | Ya, berulang | Ya, bagian dari training |
| Kecepatan | **Paling cepat** | **Paling lambat/mahal** | Menengah |
| Memperhatikan interaksi antar fitur | Umumnya tidak | Ya | Ya |
| Risiko overfitting | Rendah | Lebih tinggi | Sedang |
| Contoh | Pearson, χ², Relief, ANOVA, IG, Variance Threshold | Forward, Backward, RFE, metaheuristik | L1/L2, RF importance |

## 8.5 Bagaimana Menerapkan Feature Selection? (Lifecycle ML)

1. **Persiapan data**
2. **Exploratory Data Analysis (EDA)**
3. **Pilih metode feature selection** (Filter / Wrapper / Embedded)
4. **Terapkan feature selection** → beri skor tiap fitur → pilih fitur dengan skor terbaik
5. **Training model** (pakai fitur terpilih)
6. **Evaluasi model**
7. **Iterasi & optimasi**

## 8.6 Berapa Batasan dalam Pemilihan Fitur? (Metode Filter)

| Kriteria | Penjelasan |
|---|---|
| **Nilai statistik / skor** | Chi-Square: pilih χ² besar (makin besar makin relevan). Pearson: pilih |r| di atas ambang tertentu |
| **p-value** (signifikansi statistik) | Chi-Square dan Pearson dapat dihitung p-value-nya |
| **Jumlah fitur (Top-k)** | Ambil k fitur dengan skor tertinggi |
| **Proporsi/persentase** | Ambil sekian % fitur teratas |
| **Evaluasi model** | Pilih jumlah fitur yang memberi performa terbaik |

---

# BAB 9 — PROXIMITY MEASUREMENT

## 9.1 Konsep

- **Proximity measurement** = cara mengukur **seberapa mirip atau seberapa jauh** hubungan antar data/objek dalam dataset, berdasarkan karakteristik/atribut yang dimiliki.
- Sangat penting pada algoritma **clustering** (K-Means, Hierarchical Clustering), karena harus mengukur kedekatan objek.
- Proximity mencakup dua konsep:

| Konsep | Arti | Nilai |
|---|---|---|
| **Similarity (kemiripan)** | Seberapa mirip dua objek | **Makin tinggi → makin mirip.** Umumnya rentang **0 sampai 1** |
| **Dissimilarity (ketidakmiripan)** | Seberapa berbeda dua objek | **Makin kecil → makin mirip.** Minimum **0**, batas atas bisa **tak terhingga** |

- **Studi kasus COVID-19:** saat pandemi, rumah sakit & tenaga medis keterbatasan sumber daya. ML dapat membantu **diagnosis awal otomatis** dari data gejala pasien. Proximity measurement dipakai untuk mengukur kemiripan/perbedaan antar pasien berdasarkan gejala (klaster penderita COVID-19).

## 9.2 Similarity Measurement

**Kegunaan:** dipakai pada **supervised (neighbor-based)** dan **unsupervised (clustering)**, serta **sistem rekomendasi**.

**Tujuan utama:**
1. Mengidentifikasi **pola/hubungan** antar data.
2. **Mengelompokkan** data yang mirip (mis. K-Means).
3. **Memprediksi kategori** data baru berdasarkan kemiripan dengan data yang ada (**K-Nearest Neighbor**).
4. Menemukan **item serupa** dalam sistem rekomendasi (film, produk, lagu).

Jenis: **Cosine Similarity, Jaccard Similarity, Correlation Coefficient.**

### a) Cosine Similarity
- Mengukur kemiripan dua **vektor** berdasarkan **arah (sudut)** di antara keduanya; **tidak memperhatikan besar (magnitudo)**. Dua vektor berarah sama → similarity tinggi **meskipun panjangnya berbeda**.

```
                    Σᵢ₌₁ⁿ  Aᵢ·Bᵢ                 A • B
Cosine Similarity = ──────────────────────── = ───────────
                    √(Σ Aᵢ²) × √(Σ Bᵢ²)         ‖A‖ × ‖B‖
```
(n = jumlah fitur/atribut.)

- Nilai 1 = arah sama persis; 0 = tegak lurus (tidak ada kemiripan); −1 = berlawanan arah (untuk vektor yang boleh bernilai negatif). ➕
- Cocok untuk **teks/dokumen (term-frequency)** dan **sistem rekomendasi** (rating).

#### ✏️ LATIHAN SLIDE: Cosine Similarity Pelanggan

| Pelanggan | Novel | Komik | Majalah |
|---|---|---|---|
| Ann (A) | 1 | 2 | 2 |
| Bob (B) | 2 | 2 | 1 |
| Charles (C) | 4 | 4 | 2 |

Norma: ‖A‖ = √(1+4+4) = 3; ‖B‖ = √(4+4+1) = 3; ‖C‖ = √(16+16+4) = 6.

- **A–B:** A•B = 1·2 + 2·2 + 2·1 = 8 → cos = 8 / (3×3) = **0,889**
- **A–C:** A•C = 1·4 + 2·4 + 2·2 = 16 → cos = 16 / (3×6) = **0,889**
- **B–C:** B•C = 2·4 + 2·4 + 1·2 = 18 → cos = 18 / (3×6) = **1,000**

**Pasangan paling mirip: B–C (Bob & Charles)**, similarity = 1. Alasan: Charles = 2 × Bob (vektor C = (4,4,2) = 2·(2,2,1)) → **arah identik walau magnitudo berbeda**. Ini mendemonstrasikan sifat cosine: mengabaikan besar nilai.

### b) Jaccard Similarity (Jaccard Index)
- Mengukur kemiripan **dua himpunan (set)**: seberapa besar elemen yang sama.

```
Jaccard Similarity(A,B) = |A ∩ B| / |A ∪ B|
```
- **A, B** = dua himpunan data yang dibandingkan
- **A ∩ B** (irisan) = elemen yang sama di kedua set
- **A ∪ B** (gabungan) = semua elemen unik dari kedua set
- Nilai 0 (tidak ada elemen sama) sampai 1 (himpunan identik). Cocok untuk data **biner/set/keranjang belanja**.

#### ✏️ LATIHAN SLIDE: Jaccard Hobi Siswa

| Siswa | Hobi |
|---|---|
| Ani | {membaca, menulis, menggambar, musik} |
| Beni | {membaca, musik, futsal, renang} |
| Cici | {futsal, renang, catur} |

- **Ani–Beni:** ∩ = {membaca, musik} → 2; ∪ = {membaca, menulis, menggambar, musik, futsal, renang} → 6 → J = 2/6 = **0,333**
- **Ani–Cici:** ∩ = ∅ → 0; ∪ → 4 + 3 = 7 → J = **0**
- **Beni–Cici:** ∩ = {futsal, renang} → 2; ∪ = {membaca, musik, futsal, renang, catur} → 5 → J = 2/5 = **0,4**

**Paling mirip: Beni–Cici (0,4)**; paling tidak mirip: Ani–Cici (0).

### c) Correlation Coefficient (Pearson)
- Ukuran statistik **tingkat hubungan (asosiasi) dua variabel numerik**. Menunjukkan **arah** (positif/negatif) dan **kekuatan** (lemah/sedang/kuat).

```
          Σ (Xᵢ − X̄)(Yᵢ − Ȳ)
r = ─────────────────────────────────
     √[Σ (Xᵢ − X̄)²] · √[Σ (Yᵢ − Ȳ)²]
```
- Sebagai similarity: r mendekati +1 → pola naik-turun sama (mirip); mendekati −1 → berlawanan.

## 9.3 Dissimilarity Measurement

Ukuran **seberapa berbeda** dua objek. **Makin kecil nilai → makin mirip** (min 0).

Metode umum (tergantung tipe data): **Euclidean, Manhattan, Minkowski, Hamming, Jaccard Distance.**

### a) Euclidean Distance
Jarak **lurus (garis terpendek)** antara dua titik di ruang n-dimensi. Untuk A = (A₁,…,Aₙ) dan B = (B₁,…,Bₙ):
```
d(A,B) = √( Σᵢ₌₁ⁿ (Aᵢ − Bᵢ)² )
```
Bentuk umum dengan p fitur: `d(i,j) = √( Σₖ₌₁ᵖ (xᵢₖ − xⱼₖ)² )`.

### b) Manhattan Distance (City Block Distance)
Jumlah **selisih absolut** tiap atribut; seperti menyusuri jalan kota berbentuk **grid**.
```
d(A,B) = Σᵢ₌₁ⁿ |Aᵢ − Bᵢ|
```
**Contoh slide:** A(1,1), B(4,5) → d = |1−4| + |1−5| = 3 + 4 = **7**.
*(Bandingkan: Euclidean = √(9+16) = 5.)*

### c) Minkowski Distance
**Generalisasi** Euclidean & Manhattan dengan parameter fleksibel **p**:
```
d(A,B) = ( Σᵢ₌₁ⁿ |Aᵢ − Bᵢ|^p )^(1/p)
```
| Nilai p | Hasil |
|---|---|
| p = 1 | **Manhattan** |
| p = 2 | **Euclidean** |
| p > 2 | Jarak makin **sensitif terhadap perbedaan besar** antar nilai (menekankan perbedaan ekstrem) |

Contoh: A(1,1), B(4,5), p = 3 → (3³ + 4³)^(1/3) = 91^(1/3) ≈ 4,50.

### d) Hamming Distance
Menghitung **jumlah posisi yang berbeda** antara dua **vektor biner (0/1) atau string berpanjang sama**; yaitu berapa banyak atribut yang nilainya tidak sama.
```
D(x,y) = Σᵢ₌₁ⁿ 𝟙(xᵢ ≠ yᵢ)
```
(𝟙 = fungsi indikator: bernilai 1 jika xᵢ ≠ yᵢ, 0 jika sama.)

Contoh ➕: x = 1 0 1 1 0, y = 1 1 1 0 0 → berbeda di posisi 2 dan 4 → **D = 2**.
Contoh string: "karolin" vs "kathrin" → berbeda di 3 posisi.

### e) Jaccard Distance
Disebut di slide sebagai ukuran dissimilarity untuk data himpunan/biner.
```
Jaccard Distance(A,B) = 1 − Jaccard Similarity(A,B) = 1 − |A∩B| / |A∪B|
```
(Contoh Ani–Beni: 1 − 0,333 = 0,667.)

### Perbandingan Similarity vs Dissimilarity

| Aspek | Similarity | Dissimilarity |
|---|---|---|
| Arti | Seberapa mirip | Seberapa berbeda |
| Nilai besar | Makin mirip | Makin berbeda |
| Rentang | Umumnya 0–1 | Min 0, bisa tak terhingga |
| Contoh | Cosine, Jaccard, Korelasi | Euclidean, Manhattan, Minkowski, Hamming, Jaccard Distance |

### Memilih ukuran berdasarkan tipe data / kasus

| Ukuran | Cocok untuk |
|---|---|
| Euclidean | Data numerik kontinu, jarak garis lurus (K-Means, KNN, anomaly detection) |
| Manhattan | Data numerik; atribut berjalan pada grid/ tidak ingin kuadrat selisih; lebih tahan outlier dibanding Euclidean ➕ |
| Minkowski | Generalisasi; pilih p sesuai kebutuhan |
| Hamming | Data biner/string/kategorikal sama panjang |
| Cosine | Data teks/vektor berdimensi tinggi, arah lebih penting daripada besar |
| Jaccard | Himpunan / data biner asimetris (keranjang belanja, hobi) |
| Korelasi | Kemiripan pola/tren antar variabel numerik |

> ➕ **Catatan praktis:** ukuran berbasis jarak (Euclidean/Manhattan/Minkowski) **sensitif terhadap skala atribut** → lakukan **normalisasi** (Bab 7) terlebih dahulu agar atribut berskala besar tidak mendominasi.

## 9.4 Proximity Measurement pada Machine Learning

| Tugas ML | Fungsi proximity | Ukuran yang dipakai |
|---|---|---|
| **Clustering** | Menentukan kelompok berdasarkan jarak antar data | Euclidean, Manhattan, Cosine Similarity |
| **K-Nearest Neighbor (KNN)** | Menentukan kelas data baru dari kedekatan dengan data latih | Euclidean, Minkowski, Hamming |
| **Anomaly Detection** | Menentukan data yang jauh berbeda dari mayoritas | Euclidean |
| **Recommendation System** | Kemiripan antar pengguna/produk | Cosine Similarity, Jaccard Similarity |
| **Dimensionality Reduction (PCA, MDS, t-SNE)** | Mempertahankan hubungan kedekatan antar data di dimensi lebih rendah | Euclidean, Correlation Coefficient |
| **Clustering Hierarki (Agglomerative)** | Menggabungkan data terdekat berdasarkan jarak minimum antar klaster | Euclidean, Manhattan |

## 9.5 Penerapan Praktis

1. **Diagnosis COVID-19 (clustering & classification)**
   - Data pasien: suhu tubuh, batuk, sesak napas, saturasi oksigen.
   - **Euclidean/Manhattan** untuk mengelompokkan pasien bergejala serupa.
   - Dengan **KNN**, pasien baru diklasifikasikan positif/negatif berdasarkan jarak ke pasien yang sudah terdiagnosis.
2. **Sistem rekomendasi film**
   - Kemiripan antar pengguna dihitung dengan **Cosine Similarity** dari rating film.
   - Jika pengguna A mirip B → film yang disukai B direkomendasikan ke A.

---

# BAB 10 — DATA SPLITTING & RESAMPLING

## 10.1 Mengapa Data Perlu Dibagi?

Jika seluruh data dipakai melatih model, **tidak ada cara objektif menguji performa pada data baru**. Model bisa **"menghafal"** data latih, bukan mempelajari polanya.

| Masalah | Penjelasan |
|---|---|
| **Overfitting** | Model terlalu "hafal" pola data latih sehingga **gagal generalisasi** ke data baru (bagus di train, buruk di test) |
| **Underfitting** | Model terlalu sederhana → **gagal menangkap pola, bahkan pada data latih** |

### Istilah penting
| Set | Fungsi |
|---|---|
| **Training set** | Data untuk **melatih (fitting)** model |
| **Validation set** | Untuk **tuning hyperparameter** & memilih model terbaik selama pengembangan |
| **Test set** | Untuk **evaluasi akhir** secara objektif — **hanya dipakai sekali** |

## 10.2 Teknik Data Splitting

### a) Hold-Out Method (Train-Test Split)
- Data dibagi **dua bagian secara acak**. Rasio umum **70:30, 80:20, 90:10** (train:test).
- **Kelebihan:** cepat & mudah diimplementasikan.
- **Kekurangan:** hasil evaluasi **bervariasi tergantung pembagian acak** — sensitif pada dataset kecil.

### b) Train-Validation-Test Split
Untuk kasus yang perlu **tuning parameter**: data dibagi **tiga**.

| Bagian | Proporsi | Fungsi |
|---|---|---|
| Training | 60–70% | melatih model |
| Validation | 15–20% | tuning & seleksi model |
| Test | 15–20% | evaluasi akhir |

**Catatan penting:** **test set tidak boleh "dilihat"** atau dipakai dalam proses apa pun sebelum evaluasi akhir, agar hasil objektif dan tidak bias.

### c) K-Fold Cross Validation
- Data dibagi menjadi **K bagian (fold)**. Model dilatih **K kali**; tiap iterasi **satu fold jadi data uji**, sisanya data latih. Hasil evaluasi **dirata-ratakan** dari K iterasi.
- Contoh 5-fold: Iterasi 1 → fold 1 sebagai test; Iterasi 2 → fold 2; … ; Iterasi 5 → fold 5. **Akurasi akhir = rata-rata 5 hasil.**
- **Kelebihan:** menggunakan **seluruh data** untuk latih & uji; hasil **lebih stabil** (tak bergantung 1 split); cocok **dataset kecil–sedang**.
- **Kekurangan:** **waktu komputasi lebih lama** (melatih model K kali).

### d) Cross Validation Lainnya
| Jenis | Penjelasan |
|---|---|
| **Stratified K-Fold** | Menjaga **proporsi kelas** tiap fold sama seperti data asli — penting untuk **data tidak seimbang** |
| **Leave-One-Out (LOOCV)** | Kasus ekstrem K-Fold dengan **K = jumlah data**; tiap data diuji satu per satu; cocok dataset **sangat kecil** |
| **Leave-P-Out CV** | Mirip LOOCV, tetapi menyisakan **P data** sebagai data uji tiap iterasi |
| **Time Series Split** | Untuk **data deret waktu**; **urutan waktu dipertahankan** — data uji harus **setelah** data latih |

## 10.3 Resampling

**Resampling** = pengambilan sampel **berulang** dari data yang ada, untuk:
1. **Mengatasi data tidak seimbang (imbalanced)** antar kelas.
2. **Mengestimasi ketepatan/kestabilan** suatu statistik atau model (**Bootstrap**).
3. **Memvalidasi model** (cross-validation juga termasuk resampling).

**Contoh data tidak seimbang:** 1000 transaksi: 950 normal, 50 fraud. Model yang **selalu menebak "normal"** punya akurasi **95%** — padahal gagal total mendeteksi fraud.

### a) Oversampling — menambah data kelas minoritas

| Teknik | Cara | Kelebihan | Kekurangan |
|---|---|---|---|
| **Random Oversampling** | **Menduplikasi** data minoritas secara acak | Sederhana | Risiko **overfitting** (data duplikat identik) |
| **SMOTE** (*Synthetic Minority Oversampling Technique*) | Membuat **data sintetis baru (bukan duplikat)** dengan **interpolasi antar data minoritas berdekatan (k-nearest neighbors)** | Lebih variatif, kurangi overfitting | Dapat menimbulkan **noise** bila data minoritas tersebar/overlap |

### b) Undersampling — mengurangi data kelas mayoritas

| Teknik | Cara | Kelebihan | Kekurangan |
|---|---|---|---|
| **Random Undersampling** | **Membuang** sebagian data mayoritas secara acak | Mengurangi waktu komputasi | Risiko **kehilangan informasi penting** |
| **Tomek Links** | Menghapus **pasangan data dari kelas berbeda yang saling berdekatan (ambigu)** → batas kelas lebih jelas | Memperbaiki **decision boundary** | **Tidak menyeimbangkan** jumlah data secara signifikan bila dipakai sendirian |

### c) Hybrid: SMOTE + Tomek Links
Menggabungkan oversampling & undersampling: data minoritas ditambah sintetis, lalu data **tumpang tindih/ambigu dibersihkan** → hasil lebih optimal & bersih. Kekurangan: **lebih kompleks & lambat**.

### Ringkasan perbandingan teknik resampling

| Teknik | Kelebihan | Kekurangan |
|---|---|---|
| Random Oversampling | Sederhana, tidak kehilangan data | Risiko overfitting |
| SMOTE | Lebih variatif, kurangi overfitting | Bisa timbulkan noise jika data overlap |
| Random Undersampling | Cepat, kurangi waktu latih | Risiko hilang informasi penting |
| Tomek Links | Perbaiki batas keputusan | Tidak signifikan menyeimbangkan sendirian |
| SMOTE + Tomek | Hasil lebih optimal & bersih | Lebih kompleks & lambat |

### d) Bootstrap
**Resampling dengan pengembalian (sampling with replacement)** dari data yang ada, untuk **mengestimasi distribusi suatu statistik tanpa asumsi distribusi yang ketat**.

**Langkah:**
1. Ambil sampel acak **berukuran n** dari data asli (ukuran n) **dengan pengembalian**.
2. Hitung statistik yang diinginkan dari sampel itu.
3. **Ulangi langkah 1–2 sebanyak B kali** (mis. 1000 kali).
4. Gunakan distribusi B hasil untuk estimasi **standard error / interval kepercayaan**.

**Aplikasi: Bagging (Bootstrap Aggregating)** — dasar algoritma **Random Forest**: banyak pohon keputusan dilatih pada **sampel bootstrap berbeda**, hasil digabung lewat **voting (klasifikasi) atau rata-rata (regresi)**.

> ➕ Fakta: dalam satu sampel bootstrap, sekitar **63,2%** data asli terpilih; sisanya (~36,8%) disebut *out-of-bag* dan dapat dipakai sebagai data uji.

## 10.4 Metrik Evaluasi (penting untuk data imbalanced)

Berdasarkan **confusion matrix** (kelas positif = fraud):

| | Prediksi Positif | Prediksi Negatif |
|---|---|---|
| **Aktual Positif** | TP (fraud tertangkap) | FN (fraud lolos) |
| **Aktual Negatif** | FP (normal salah diblokir) | TN (normal benar) |

```
Accuracy  = (TP + TN) / (TP + TN + FP + FN)
Precision = TP / (TP + FP)        → dari yang diprediksi fraud, berapa yang benar-benar fraud
Recall    = TP / (TP + FN)        → dari semua fraud sebenarnya, berapa yang tertangkap
F1-score  = 2 × Precision × Recall / (Precision + Recall)
AUC-ROC   = luas di bawah kurva ROC (kemampuan membedakan kelas; 1 = sempurna, 0,5 = acak)
```
**Pelajaran:** pada data tidak seimbang **jangan percaya akurasi** — lihat **recall, precision, F1, AUC**.

## 10.5 Studi Kasus: Deteksi Fraud Kartu Kredit

**Skenario:** data scientist di bank; **10.000 transaksi** → **9.744 normal (97,4%)**, **256 fraud (2,6%)**. Tiga pertanyaan: (1) bagaimana membagi data → *data splitting*; (2) bagaimana menangani fraud yang sedikit → *resampling (SMOTE)*; (3) apakah hasilnya bisa dipercaya → *cross validation*.

### Langkah 1 — Splitting (stratified 80:20)
```python
X_train, X_test, y_train, y_test = train_test_split(
    X, y, test_size=0.2, stratify=y, random_state=42)
```
| Set | Normal | Fraud |
|---|---|---|
| Seluruh data | 9.744 | 256 |
| Training (80%) | 7.795 | 205 |
| Test (20%) | 1.949 | 51 |

- Model belajar dari 8.000 transaksi, diuji pada 2.000 yang belum pernah dilihat.
- **`stratify=y`** menjaga proporsi fraud ≈ 2,6% di training & test. Tanpa ini, pada data kecil test set bisa nyaris tanpa fraud.
- Mulai sekarang **test set "dikunci"**: tidak dipakai untuk melatih atau resampling.

### Langkah 2 — Model tanpa resampling (Random Forest)
Confusion matrix: `[[1949, 0], [35, 16]]` → normal semua benar; **35 fraud lolos, 16 tertangkap**.

| Metrik | Nilai |
|---|---|
| Accuracy | 98,3% |
| Precision | 100% |
| **Recall** | **31,4%** (16/51) |

- **Akurasi tinggi menipu**; penyebabnya 97% data latih adalah normal sehingga model **kurang belajar pola fraud**.

### Langkah 3 — Resampling dengan SMOTE (hanya pada TRAINING set)
```python
smote = SMOTE(random_state=42)
X_res, y_res = smote.fit_resample(X_train, y_train)   # hanya X_train!
```
- SMOTE membuat **7.590 data fraud sintetis** (205 → 7.795) hingga seimbang: training sesudah = normal 7.795 | fraud 7.795.
- **Test set tidak disentuh**: tetap 1.949 normal & 51 fraud (kondisi nyata).
- **Urutan benar: split dulu, resampling kemudian.**

### Analisis: sebelum vs sesudah SMOTE

| Metrik | Tanpa resampling | Dengan SMOTE |
|---|---|---|
| Accuracy | 98,3% | 97,4% |
| Precision | 100% | 47,8% |
| **Recall** | 31,4% | **43,1%** |
| F1-Score | 47,8% | 45,4% |
| AUC-ROC | 0,805 | 0,834 |
| Fraud tertangkap | 16 dari 51 | 22 dari 51 |
| Normal diblokir (FP) | 0 | 24 |

- Recall naik 31,4% → 43,1% (**6 fraud tambahan tertangkap**).
- **Harganya:** precision turun 100% → 47,8%, dan **24 transaksi normal ikut diblokir**.
- F1 hampir sama, AUC sedikit naik → **SMOTE menggeser keseimbangan precision–recall, bukan menyulap model jadi sempurna.**
- **Pilih sesuai biaya kesalahan:** bila fraud lolos lebih mahal → utamakan **recall**.

### Langkah 4 — Memastikan hasil stabil (Stratified 5-Fold CV + Pipeline)
```python
skf = StratifiedKFold(n_splits=5, shuffle=True, random_state=42)
pipe = Pipeline([("smote", SMOTE(random_state=42)),
                 ("rf", RandomForestClassifier(random_state=42))])
hasil = cross_validate(pipe, X, y, cv=skf, scoring="recall")
```
| Skema | Recall rata-rata (std) |
|---|---|
| Tanpa resampling | 0,281 (0,066) |
| SMOTE (Pipeline) | **0,414 (0,054)** |

- **Pipeline** memastikan SMOTE hanya dipakai pada **data latih di tiap fold**; data uji tiap fold tetap asli.
- SMOTE unggul di **kelima fold**. Satu split tadi (31,4%) hanya **satu potret**; CV menunjukkan rentang (tanpa resampling) 15,7%–35,3%.

### ⚠️ Jebakan: DATA LEAKAGE

| ❌ SALAH: SMOTE dulu, baru split | ✅ BENAR: split dulu, baru SMOTE |
|---|---|
| `SMOTE().fit_resample(X, y)` pada semua data lalu `train_test_split` | `train_test_split(X, y, stratify=y)` lalu `SMOTE().fit_resample(X_train, y_train)` |
| Recall 98,3%, Precision 98,4%, AUC 0,999 (**palsu**) | Recall 43,1%, Precision 47,8%, AUC 0,834 (jujur) |

**Mengapa palsu?** Titik sintetis SMOTE mirip data aslinya, sehingga sebagian masuk ke test set sementara "tetangganya" ada di training. Test set juga menjadi 50:50, bukan 97:3 seperti dunia nyata. **Hasil terlalu bagus → curigai data leakage.**

### Kesimpulan kasus
1. **Stratified split** → test set tetap jujur (97,4% : 2,6%) & tidak dipakai melatih.
2. **SMOTE hanya di training** → recall fraud naik 31,4% → 43,1%, precision turun 100% → 47,8%.
3. **Cross validation + Pipeline** → satu split bisa menyesatkan; di 5 fold SMOTE konsisten lebih baik (41,4% vs 28,1%).
4. **Metrik yang tepat** → akurasi 98% menipu; gunakan recall, precision, F1, AUC, dan baca confusion matrix.

## 10.6 Data Splitting vs Resampling

| Aspek | Data Splitting | Resampling |
|---|---|---|
| Tujuan utama | Memisahkan data untuk **pelatihan & evaluasi objektif** | **Menyeimbangkan kelas** / estimasi kestabilan statistik |
| Fokus | **Validitas evaluasi** model | **Kualitas & distribusi data latih** |
| Contoh teknik | Train-test split, k-fold CV | Oversampling, undersampling, SMOTE, bootstrap |
| Kapan dipakai | **Selalu**, di setiap proyek data mining | Saat data tidak seimbang / perlu estimasi statistik |

## 10.7 Kesalahan yang Harus Dihindari

1. ✗ **Resampling sebelum data splitting** → **data leakage**.
2. ✗ **Menggunakan test set untuk tuning hyperparameter** → evaluasi bias.
3. ✗ **Hanya mengandalkan akurasi pada data imbalanced** → gunakan precision, recall, F1, AUC-ROC.
4. ✗ **Tidak memakai stratified split pada data tidak seimbang** → proporsi kelas bisa berbeda jauh.
5. ✗ **Resampling pada data time series tanpa memperhatikan urutan waktu** → merusak struktur temporal.

## 10.8 Kapan Memakai Apa?

| Situasi | Teknik |
|---|---|
| Data besar, butuh evaluasi cepat | **Hold-out / Train-Validation-Test** |
| Data kecil–sedang, hasil harus stabil | **K-Fold Cross Validation** |
| Kelas tidak seimbang | **Stratified split + SMOTE/undersampling (hanya pada training)** |
| Data deret waktu | **Time Series Split** (urutan waktu dijaga) |
| Estimasi ketidakpastian / ensemble | **Bootstrap & Bagging (Random Forest)** |

---

# BAB 11 — BANK SOAL ESSAY + JAWABAN MODEL

> Cara pakai: tutup jawaban, coba jawab sendiri, lalu cocokkan. Jawaban dibuat ringkas tapi lengkap seperti yang diharapkan di ujian essay.

## A. Pengantar Data Mining

**Soal 1. Jelaskan apa itu data mining dan mengapa disebut sebagai bagian dari proses KDD!**
> Data mining adalah proses menemukan pola, tren, dan informasi berguna (non-trivial, implisit, sebelumnya tidak diketahui, dan berpotensi berguna) dari kumpulan data besar menggunakan teknik statistik, algoritma, dan metode analisis. Dalam proses KDD (Knowledge Discovery in Databases), data mining adalah langkah inti setelah data cleaning, integration, selection, dan transformation, sebelum pattern evaluation dan knowledge presentation.

**Soal 2. Sebutkan tahapan proses KDD!**
> (1) Data cleaning, (2) data integration, (3) data selection, (4) data transformation, (5) data mining, (6) pattern/model evaluation, (7) knowledge presentation. Tahap 1–4 = data preparation.

**Soal 3. Sebutkan dan jelaskan fungsi-fungsi data mining!**
> Karakterisasi & diskriminasi (generalisasi/ringkasan & kontras data), asosiasi & korelasi (frequent itemset, mis. Diaper → Beer [support, confidence]), klasifikasi (model dari data latih untuk memprediksi label kelas), clustering (unsupervised; maksimalkan kemiripan intra-cluster & minimalkan antar-cluster), analisis outlier (objek yang menyimpang dari perilaku umum; deteksi fraud), analisis sequence/tren/evolusi, analisis graf/jaringan & web mining.

**Soal 4. Apa beda data mining deskriptif dan prediktif?**
> Deskriptif menjawab "apa yang telah terjadi" (karakterisasi, asosiasi, clustering). Prediktif menjawab "apa yang akan terjadi" (klasifikasi, prediksi/regresi).

**Soal 5. Jelaskan perbedaan data terstruktur, semi-terstruktur, dan tidak terstruktur beserta contohnya!**
> Terstruktur: format tabel/record seragam, atribut dan rentang nilai tetap (database relasional, data matrix). Semi-terstruktur: punya struktur dengan semantik jelas tetapi fleksibel/bersarang (data transaksional, time-series, sequence gen, weblog, graf). Tidak terstruktur: teks dan multimedia (audio, gambar, video).

**Soal 6. Sebutkan isu-isu utama dalam data mining!**
> Metodologi mining (menangani noise, ketidakpastian, ketidaklengkapan; evaluasi pola), interaksi pengguna (interactive mining, visualisasi), efisiensi & skalabilitas (algoritma paralel/terdistribusi/stream/incremental), keragaman tipe data, serta dampak sosial (privasi, privacy-preserving data mining, invisible data mining).

**Soal 7. Mengapa data mining berkaitan dengan banyak disiplin ilmu?**
> Karena data sangat besar (butuh algoritma scalable), berdimensi tinggi (micro-array puluhan ribu dimensi), dan kompleks (stream, time-series, graf, heterogen, multimedia), sehingga membutuhkan machine learning, statistik, pattern recognition, teknologi database, visualisasi, dan high-performance computing.

## B. Objek Data & Tipe Atribut

**Soal 8. Jelaskan perbedaan objek data dan atribut! Apa istilah lain atribut di bidang lain?**
> Objek data = entitas yang digambarkan (baris/tuple; juga disebut sampel, contoh, titik data). Atribut = karakteristik/fitur objek (kolom). Atribut disebut *dimensi* di data warehousing, *fitur* di machine learning, dan *variabel* di statistik.

**Soal 9. Jelaskan keempat tipe atribut beserta operasi yang dapat diterapkan dan contohnya!**
> Nominal: kategori tanpa urutan (warna, pekerjaan), operasi =, ≠. Biner: nominal dengan dua nilai 0/1 (isSmoker), dibedakan simetris (jenis kelamin) dan asimetris (tes buta warna). Ordinal: kategori berurutan, selisih & rasio tidak bermakna (rating bintang, nilai A–F), operasi =, ≠, <, >. Numerik: interval (selisih bermakna, tanpa nol mutlak; suhu °C, tanggal; operasi + −) dan rasio (selisih & rasio bermakna, nol bermakna; umur, berat; semua operasi aritmetika).

**Soal 10. Apa beda atribut biner simetris dan asimetris? Beri contoh!**
> Simetris: kedua nilai berdampak setara (jenis kelamin Pria/Wanita). Asimetris: kedua nilai berdampak berbeda/tidak setara; biasanya salah satu keadaan lebih penting (hasil tes buta warna: buta warna vs tidak).

**Soal 11. Mengapa suhu dalam Celsius adalah interval, bukan rasio?**
> Karena nol pada skala Celsius bersifat arbitrer (bukan ketiadaan suhu secara fisik), sehingga rasio dua suhu tidak bermakna: 2 °C bukan "dua kali lebih panas" dari 1 °C. Selisih tetap bermakna.

**Soal 12. Bisakah atribut nominal berupa angka? Jelaskan!**
> Bisa. Angka hanya label/kode (merah=0, putih=1, biru=3). Operasi aritmetika tidak bermakna, hanya =, ≠.

**Soal 13. Klasifikasikan tipe atribut: ID, Jenis Kelamin, Status Beasiswa, Tingkat Kepuasan (Rendah/Sedang/Tinggi), Umur, IPK.**
> ID: nominal; Jenis Kelamin: biner simetris; Status Beasiswa: biner; Tingkat Kepuasan: ordinal; Umur: numerik rasio; IPK: numerik.

**Soal 14. Sebutkan jenis-jenis dataset dan contohnya!**
> Record data (relational records, data matrix/crosstab, transaction data, document term-frequency), graph & network (transportasi, WWW, molekul, jaringan sosial), ordered data (video, time-series, sequential transaksi, sekuens genetik), spatial/image/multimedia (peta, citra, video).

## C. Preprocessing, Cleaning, Integration

**Soal 15. Mengapa data perlu dipreprocess? Sebutkan masalah umum kualitas data!**
> Data nyata biasanya kotor; data berkualitas rendah menghasilkan hasil mining berkualitas rendah. Tujuan preprocessing: meningkatkan kualitas data dan efisiensi mining. Masalah umum: noise (class & attribute noise), inkonsistensi, data hilang, data duplikat, dataset tidak seimbang, outlier.

**Soal 16. Sebutkan empat tugas utama preprocessing!**
> Data cleaning, data integration, data reduction (dimensionality, numerosity, compression), data transformation & discretization (normalisasi, concept hierarchy).

**Soal 17. Jelaskan MCAR, MAR, dan MNAR beserta contohnya!**
> MCAR: hilang murni acak, distribusi nilai hilang sama dengan yang teramati (tinta tumpah menutupi sebagian sel). MAR: data kosong karena responden sengaja tidak mengisi (pendapatan/gaji). MNAR: mekanisme hilang tidak acak (sensor tidak merekam suhu < 5 °C).

**Soal 18. Bagaimana cara menangani missing value? Kapan menghapus data tidak disarankan?**
> (1) Hapus record, (2) isi manual, (3) imputasi otomatis (mean/median, arbitrer, end-of-tail, modus, random sample, konstanta, K-NN, regresi). Penghapusan tidak disarankan bila nilai kosong banyak karena mengurangi jumlah data secara signifikan; isi manual tidak efektif untuk data besar.

**Soal 19. Jelaskan imputasi mean/median beserta kelebihan dan kekurangannya! Hitung imputasi mean untuk Age: 29, 43, NA, 25, 34, NA, 50.**
> Mengganti NA dengan rata-rata/median kolom. Kelebihan: mudah, cepat, cocok data numerik kecil dan MCAR, bisa dipakai di produksi. Kekurangan: tidak memperhitungkan korelasi antar fitur, kurang akurat, mendistorsi variansi/distribusi/kovarians, tidak cocok bila missing > 5%. Hitung: (29+43+25+34+50)/5 = 181/5 = **36,2**; kedua NA diisi 36,2.

**Soal 20. Jelaskan imputasi end-of-tail dan cara menentukan nilainya!**
> Mengganti NA dengan nilai di ujung distribusi. Jika normal: mean + 3 × standar deviasi. Jika skew: aturan IQR proximity (Upper = Q3 + 3×IQR; Lower = Q1 − 3×IQR). Hanya digunakan pada data latih.

**Soal 21. Jelaskan imputasi modus (frequent category) dan random sample!**
> Modus: NA kategorikal diganti kategori paling sering (mudah, cocok data skew; kelemahan: distorsi relasi & over-representation). Random sample: NA diganti nilai acak dari nilai teramati (mempertahankan varians & distribusi; kekurangan: randomness dan butuh menyimpan data latih saat deployment).

**Soal 22. Apa beda regresi deterministik dan stokastik dalam imputasi?**
> Deterministik mengganti NA dengan prediksi tepat model regresi (mengurangi variasi). Stokastik menambahkan komponen random error sehingga korelasi X–Y dan variasi data lebih terjaga.

**Soal 23. Apa itu noisy data dan bagaimana penanganannya?**
> Data yang memuat error atau outlier (inkonsisten, pencilan, tidak utuh, ganda). Penanganan: binning (urut-kelompokkan lalu haluskan), regresi (penghalusan fungsi regresi), clustering (deteksi & buang outlier), kombinasi komputer dan manusia.

**Soal 24. Tentukan outlier dari data: 5, 7, 10, 15, 19, 21, 21, 22, 22, 23, 23, 23, 23, 23, 24, 24, 24, 24, 25 dengan aturan IQR.**
> Q1 = 19, Q3 = 24, IQR = 5. Batas bawah = 19 − 1,5×5 = 11,5; batas atas = 24 + 7,5 = 31,5. Data < 11,5 → outlier: **5, 7, 10**. Tidak ada outlier atas.

**Soal 25. Jelaskan masalah-masalah dalam data integration!**
> Schema integration (menyatukan metadata, mis. cust-id ≡ cust-#), entity identification problem (Bill Clinton = William Clinton), serta deteksi & penyelesaian konflik nilai data (beda representasi/skala, mis. metrik vs British units). Redundansi: object identification dan derivable data; dideteksi dengan analisis korelasi & kovarians.

**Soal 26. Jelaskan uji chi-square untuk analisis korelasi data nominal! Hitung untuk tabel Dosen A/B × Lulus/Tidak Lulus (20,10 / 10,20).**
> χ² = Σ (Observed − Expected)²/Expected; makin besar χ² makin besar kemungkinan variabel berhubungan; sel berkontribusi terbesar adalah yang selisih observed–expected-nya besar; korelasi ≠ kausalitas. Hitung: expected tiap sel = 30×30/60 = 15; χ² = 4 × (5²/15) = **6,67**; df = 1; nilai kritis 3,841 (α=0,05) → signifikan → ada hubungan dosen dan hasil ujian.

**Soal 27. Hitung kovarians dua saham AllElectronics (6,5,4,3,2) dan HighTech (20,10,14,5,5) dan interpretasikan.**
> E(A)=4; E(B)=10,8; E(AB)=251/5=50,2; Cov = 50,2 − 4×10,8 = **7** > 0 → kedua saham cenderung naik/turun bersama.

**Soal 28. Jelaskan koefisien korelasi Pearson dan hubungannya dengan kovarians! Apa arti kovarians 0?**
> r = Cov(A,B)/(σA σB); rentang −1…+1; r>0 positif, r=0 tidak ada hubungan linear, r<0 negatif. Cov = 0 belum tentu independen; hanya jika ada asumsi tambahan (mis. normal multivariat).

## D. Data Reduction & Transformation

**Soal 29. Apa itu reduksi data dan mengapa penting? Sebutkan jenisnya!**
> Mendapatkan representasi/volume lebih kecil dengan hasil analisis sama atau hampir sama; penting karena database bisa terabyte dan analisis kompleks lama. Jenis: dimensionality reduction (wavelet, PCA, feature/attribute subset selection) dan numerosity reduction (parametrik: regresi, log-linear; non-parametrik: histogram, clustering, sampling).

**Soal 30. Jelaskan curse of dimensionality dan manfaat dimensionality reduction!**
> Saat dimensi bertambah, data makin jarang (sparse) dan sulit dianalisis. Manfaat reduksi dimensi: menghindari curse of dimensionality, membuang fitur tak penting & mengurangi noise, menghemat waktu dan ruang, memudahkan visualisasi.

**Soal 31. Jelaskan cara kerja PCA!**
> Mencari proyeksi yang menangkap variansi terbesar; data diproyeksikan ke ruang lebih kecil. Dicari vektor eigen matriks kovarians; vektor eigen mendefinisikan ruang baru (komponen utama). Mengurangi dimensi dengan mempertahankan komponen berkontribusi variansi terbesar.

**Soal 32. Apa itu transformasi wavelet, tujuan, dan penggunaannya?**
> Merepresentasikan data dalam komponen frekuensi dan waktu sekaligus. Tujuan: representasi lebih sederhana, mengabaikan komponen frekuensi tinggi (noise), mempertahankan karakteristik utama dalam ukuran lebih kecil. Dipakai pada gambar (kompresi, deteksi tepi, denoising), audio (kompresi, denoising), time-series (penyederhanaan pola, deteksi anomali).

**Soal 33. Jelaskan atribut redundan dan tidak relevan dengan contoh!**
> Redundan: informasinya sudah ada di atribut lain (harga produk dan pajaknya). Tidak relevan: tidak berguna bagi tugas mining (NIM tidak memengaruhi IPK).

**Soal 34. Bedakan metode parametrik dan non-parametrik pada numerosity reduction!**
> Parametrik mengasumsikan data cocok dengan suatu model (regresi, log-linear): estimasi parameter, simpan hanya parameter, buang data (kecuali outlier). Non-parametrik tanpa asumsi model: histogram, clustering, sampling.

**Soal 35. Jelaskan jenis-jenis sampling dan masalah simple random sampling pada data skew!**
> Simple random sampling (peluang sama), without replacement (objek terpilih dikeluarkan), with replacement (tidak dikeluarkan), stratified (adaptif). Simple random bisa berkinerja buruk pada data skew karena kelompok kecil tidak terwakili → gunakan stratified sampling.

**Soal 36. Sebutkan metode transformasi data!**
> Smoothing, attribute/feature construction, aggregation, normalization (min-max, z-score, decimal scaling), discretization (concept hierarchy climbing).

**Soal 37. Normalisasi pendapatan \$73.600 jika rentang \$12.000–\$98.000 (min-max ke [0,1]), μ=54.000, σ=16.000 (z-score), dan nilai terbesar 76.300 (decimal scaling).**
> Min-max: (73.600−12.000)/(98.000−12.000) = **0,716**. Z-score: (73.600−54.000)/16.000 = **1,225**. Decimal scaling: 76.300/10⁵ = **0,763** (j = 5).

**Soal 38. Normalisasikan tinggi badan 155, 160, 170, 180, 162, 175, 168, 164 dengan ketiga metode.**
> Min-max (min 155, max 180): 0; 0,20; 0,60; 1,00; 0,28; 0,80; 0,52; 0,36. Z-score (μ=166,75, σ populasi=7,66): −1,53; −0,88; 0,42; 1,73; −0,62; 1,08; 0,16; −0,36. Decimal scaling (j=3): 0,155; 0,160; 0,170; 0,180; 0,162; 0,175; 0,168; 0,164.

## E. Feature Selection

**Soal 39. Apa itu information extraction dan peranannya dalam ML?**
> Proses mengidentifikasi, mengekstrak, dan menyusun informasi terstruktur dari data tidak/semi-terstruktur sehingga menjadi fakta yang dapat dianalisis. Perannya: menyediakan data relevan bagi model, meningkatkan kualitas fitur (feature engineering), dan mengotomatisasi analisis big data.

**Soal 40. Mengapa fitur yang terlalu banyak bermasalah? Jelaskan "semakin banyak fitur ≠ semakin baik"!**
> Menimbulkan curse of dimensionality, overfitting, redundansi, turunnya interpretabilitas, dan biaya komputasi tinggi. IE sering menghasilkan informasi tidak relevan; kuantitas fitur tidak berbanding lurus dengan kualitas model. Contoh: klasifikasi BMI hanya perlu berat dan tinggi badan.

**Soal 41. Apa tujuan feature selection dan kapan penting dilakukan?**
> Tujuan: mengurangi dimensionality dan noise, meningkatkan akurasi, mempercepat training/inferensi, menghindari overfitting. Penting saat data banyak noise, banyak fitur jarang muncul, banyak missing value, banyak tipe data, fitur > sampel, atau model kompleks.

**Soal 42. Bedakan metode filter, wrapper, dan embedded!**
> Filter: seleksi dengan statistik/hubungan matematis fitur–target tanpa model ML (korelasi, χ², Relief, ANOVA, IG, variance threshold); cepat. Wrapper: memakai model ML untuk menguji subset fitur secara berulang (forward, backward, RFE); akurat tapi mahal. Embedded: seleksi terjadi saat training, fitur tak relevan dipenalti/diberi bobot kecil (L1/L2, random forest importance).

**Soal 43. Jelaskan cara kerja algoritma Relief!**
> Inisialisasi bobot nol → pilih instance acak → cari nearest hit (kelas sama) dan nearest miss (kelas beda) → update bobot: W[A] = W[A] − diff(A,X,H) + diff(A,X,M) (turun bila beda dengan hit, naik bila beda dengan miss) → ulangi → fitur berbobot positif & tinggi dianggap relevan.

**Soal 44. Jelaskan Information Gain dan rumus entropi!**
> IG(Y,X) = H(Y) − H(Y|X) dengan H(Y) = −Σ P(yᵢ) log₂ P(yᵢ). Mengukur berapa banyak informasi tentang target yang diperoleh dari fitur X; dipakai pada decision tree untuk memilih atribut terbaik tiap split; IG besar → fitur informatif.

**Soal 45. Jelaskan langkah forward selection, backward elimination, dan RFE!**
> Forward: mulai kosong → tambah fitur yang paling meningkatkan performa satu per satu → berhenti jika tak ada peningkatan signifikan/semua terpilih. Backward: mulai semua fitur → hapus satu per satu sampai kinerja optimum. RFE: latih model dengan semua fitur → hitung importance → buang yang terkecil → ulangi sampai jumlah target tercapai.

**Soal 46. Apa kriteria batasan pemilihan fitur pada metode filter?**
> Berdasarkan nilai statistik/skor (χ² besar, |r| di atas ambang), p-value, jumlah fitur (top-k), proporsi/persentase, atau evaluasi model.

**Soal 47. Jelaskan koefisien korelasi Pearson dalam seleksi fitur dan interpretasi nilainya.**
> Mengukur kekuatan dan arah hubungan linear fitur–target (−1…+1). |r| mendekati 1 = hubungan kuat (fitur relevan); mendekati 0 = lemah. Pilih fitur dengan |r| di atas ambang.

## F. Proximity Measurement

**Soal 48. Apa beda similarity dan dissimilarity?**
> Similarity mengukur kemiripan (makin tinggi makin mirip; umumnya 0–1). Dissimilarity mengukur perbedaan (makin kecil makin mirip; minimum 0, bisa tak terhingga).

**Soal 49. Hitung cosine similarity A=(1,2,2), B=(2,2,1), C=(4,4,2). Pasangan mana paling mirip dan mengapa?**
> A–B = 8/9 = 0,889; A–C = 16/18 = 0,889; B–C = 18/18 = 1. Paling mirip B–C karena C = 2B (arah sama persis); cosine mengabaikan magnitudo.

**Soal 50. Hitung Jaccard similarity: Ani {membaca, menulis, menggambar, musik}, Beni {membaca, musik, futsal, renang}, Cici {futsal, renang, catur}.**
> Ani–Beni = 2/6 = 0,333; Ani–Cici = 0/7 = 0; Beni–Cici = 2/5 = 0,4.

**Soal 51. Bedakan Euclidean, Manhattan, dan Minkowski. Hitung jarak A(1,1) ke B(4,5).**
> Euclidean = akar jumlah kuadrat selisih (garis lurus) = √(9+16) = 5. Manhattan = jumlah selisih absolut (seperti jalan grid) = 3+4 = 7. Minkowski = (Σ|Aᵢ−Bᵢ|^p)^(1/p): p=1 Manhattan, p=2 Euclidean, p>2 lebih sensitif terhadap perbedaan besar.

**Soal 52. Apa itu Hamming distance dan kapan dipakai?**
> Jumlah posisi yang berbeda antara dua vektor biner/string sama panjang; dipakai untuk data biner/kategorikal (mis. KNN dengan atribut biner). Contoh 10110 vs 11100 → 2.

**Soal 53. Kapan cosine lebih cocok daripada Euclidean?**
> Saat arah/pola lebih penting daripada besar nilai, mis. dokumen teks (term-frequency) atau rating pengguna di sistem rekomendasi, di mana dua dokumen bisa berarah sama tetapi berbeda panjang.

**Soal 54. Sebutkan penerapan proximity measurement pada machine learning!**
> Clustering (Euclidean, Manhattan, Cosine), KNN (Euclidean, Minkowski, Hamming), anomaly detection (Euclidean), sistem rekomendasi (Cosine, Jaccard), reduksi dimensi PCA/MDS/t-SNE (Euclidean, korelasi), clustering hierarki (Euclidean, Manhattan). Contoh: pengelompokan/diagnosis pasien COVID-19 dan rekomendasi film.

## G. Data Splitting & Resampling

**Soal 55. Mengapa data harus dibagi? Jelaskan overfitting dan underfitting!**
> Agar performa dapat diuji objektif pada data baru; jika semua data dipakai melatih, model hanya menghafal. Overfitting: terlalu hafal data latih, gagal generalisasi. Underfitting: terlalu sederhana, gagal menangkap pola bahkan pada data latih.

**Soal 56. Jelaskan fungsi training, validation, dan test set!**
> Training: melatih model. Validation: tuning hyperparameter dan memilih model terbaik. Test: evaluasi akhir objektif, hanya dipakai sekali dan tidak boleh dilihat sebelum evaluasi akhir.

**Soal 57. Jelaskan metode hold-out beserta kelebihan dan kekurangannya!**
> Data dibagi acak menjadi train/test (70:30, 80:20, 90:10). Kelebihan: cepat dan mudah. Kekurangan: hasil bervariasi tergantung pembagian acak, sensitif pada dataset kecil.

**Soal 58. Jelaskan K-fold cross validation dan variannya!**
> Data dibagi K fold; model dilatih K kali dengan tiap fold bergantian jadi data uji; hasil dirata-ratakan. Kelebihan: seluruh data dipakai, hasil stabil, cocok data kecil–sedang; kekurangan: komputasi lama. Varian: Stratified K-Fold (proporsi kelas dijaga), LOOCV (K = jumlah data), Leave-P-Out, Time Series Split (urutan waktu dijaga).

**Soal 59. Apa itu data imbalanced dan mengapa akurasi menyesatkan?**
> Jumlah sampel satu kelas jauh lebih banyak. Contoh 950 normal : 50 fraud; model yang selalu menebak normal berakurasi 95% tetapi tidak mendeteksi fraud sama sekali. Gunakan precision, recall, F1, AUC dan confusion matrix.

**Soal 60. Bandingkan random oversampling, SMOTE, random undersampling, Tomek Links, dan SMOTE+Tomek!**
> Lihat tabel di Bab 10.3: random oversampling (duplikasi minoritas; sederhana tapi overfitting); SMOTE (sintetis hasil interpolasi k-NN; variatif tapi bisa noise bila overlap); random undersampling (buang mayoritas; cepat tapi kehilangan informasi); Tomek Links (hapus pasangan ambigu lintas kelas; perbaiki batas tapi tidak menyeimbangkan sendirian); SMOTE+Tomek (oversample lalu bersihkan; optimal tapi kompleks/lambat).

**Soal 61. Jelaskan cara kerja SMOTE!**
> Untuk tiap data minoritas, cari k tetangga terdekat sesama minoritas, lalu buat titik sintetis baru di sepanjang garis antara data tersebut dan tetangganya (interpolasi), sehingga data minoritas bertambah tanpa duplikat identik.

**Soal 62. Jelaskan bootstrap dan bagging!**
> Bootstrap: resampling dengan pengembalian berukuran n, hitung statistik, ulangi B kali, gunakan distribusinya untuk estimasi standard error/interval kepercayaan tanpa asumsi distribusi ketat. Bagging: melatih banyak model (pohon keputusan) pada sampel bootstrap berbeda dan menggabungkan hasil dengan voting/rata-rata; dasar Random Forest.

**Soal 63. Apa itu data leakage pada SMOTE dan bagaimana mencegahnya?**
> Melakukan SMOTE sebelum split membuat titik sintetis mirip data asli masuk ke test set sementara tetangganya di training, dan test set menjadi 50:50 sehingga metrik terlihat sempurna (recall 98,3%, AUC 0,999) tetapi palsu. Cegah dengan split dulu (stratified), SMOTE hanya pada training, dan gunakan Pipeline saat cross validation.

**Soal 64. Pada kasus fraud kartu kredit (10.000 transaksi, 256 fraud), jelaskan hasil sebelum dan sesudah SMOTE!**
> Tanpa resampling: accuracy 98,3%, precision 100%, recall 31,4% (16 dari 51 fraud tertangkap). Dengan SMOTE: accuracy 97,4%, precision 47,8%, recall 43,1% (22 dari 51), 24 transaksi normal terblokir, AUC 0,805 → 0,834. SMOTE menaikkan recall dengan harga turunnya precision. 5-fold CV: recall rata-rata 41,4% vs 28,1%, SMOTE unggul di semua fold.

**Soal 65. Sebutkan 5 kesalahan yang harus dihindari dalam splitting dan resampling!**
> Resampling sebelum splitting (leakage); memakai test set untuk tuning; hanya mengandalkan akurasi pada data imbalanced; tidak memakai stratified split pada data imbalanced; resampling time series tanpa memperhatikan urutan waktu.

**Soal 66. Tentukan teknik yang tepat: (a) data besar & evaluasi cepat, (b) data kecil & hasil stabil, (c) kelas tidak seimbang, (d) data deret waktu, (e) estimasi ketidakpastian.**
> (a) Hold-out / train-validation-test; (b) K-fold CV; (c) stratified split + SMOTE/undersampling hanya di training; (d) Time Series Split; (e) Bootstrap & bagging.

## H. Soal Analisis / Gabungan

**Soal 67. Urutkan dan jelaskan alur preprocessing hingga evaluasi model untuk dataset pelanggan bank yang berisi missing value, atribut berbeda skala, banyak fitur, dan kelas imbalanced.**
> (1) Integrasi data dari sumber berbeda (resolusi konflik skema & entitas). (2) Cleaning: tangani missing (imputasi), duplikat, outlier. (3) Split data (stratified) — test set dikunci. Catatan: statistik imputasi/normalisasi dihitung dari training set saja agar tidak leakage. (4) Transformasi: normalisasi (min-max/z-score). (5) Reduksi: feature selection (filter→wrapper/embedded) atau PCA. (6) Resampling (SMOTE) hanya pada training. (7) Training, tuning dengan validation/CV. (8) Evaluasi pada test set memakai recall, precision, F1, AUC.

**Soal 68. Mengapa normalisasi penting sebelum menghitung jarak Euclidean pada data umur (20–60) dan pendapatan (1–50 juta)?**
> Jarak Euclidean sensitif terhadap skala; pendapatan yang bernilai jutaan akan mendominasi perbedaan umur, sehingga kemiripan hanya ditentukan pendapatan. Normalisasi menyamakan skala atribut.

**Soal 69. Seorang peneliti menemukan korelasi tinggi antara penjualan es krim dan kasus tenggelam. Apa kesimpulan yang tepat?**
> Korelasi bukan kausalitas; keduanya kemungkinan dipengaruhi variabel ketiga (musim panas/suhu), sama seperti jumlah rumah sakit dan pencurian mobil yang dipengaruhi populasi.

---

# BAB 12 — LEMBAR CONTEKAN RUMUS

## Outlier (IQR)
```
IQR = Q3 − Q1
Low outlier  : x < Q1 − 1,5·IQR
High outlier : x > Q3 + 1,5·IQR
End-of-tail (skew): Upper = Q3 + 3·IQR ; Lower = Q1 − 3·IQR
End-of-tail (normal): mean + 3·σ
```

## Normalisasi
```
Min-Max       : v' = (v − min)/(max − min) × (new_max − new_min) + new_min
Z-score       : v' = (v − μ)/σ
Decimal scale : v' = v / 10^j   (j terkecil sehingga max|v'| < 1)
```

## Korelasi & Kovarians
```
Chi-square : χ² = Σ (O − E)² / E ;  E = (total baris × total kolom)/N ;  df = (r−1)(c−1)
Pearson    : r = Σ(aᵢ − Ā)(bᵢ − B̄) / [(n−1)·σA·σB]  =  [Σ aᵢbᵢ − n·Ā·B̄] / [(n−1)·σA·σB]
             (bentuk fitur-selection: r = Σ(x−x̄)(y−ȳ) / [√Σ(x−x̄)² · √Σ(y−ȳ)²])
Kovarians  : Cov(A,B) = E[(A−Ā)(B−B̄)] = Σ(aᵢ − Ā)(bᵢ − B̄)/n = E(A·B) − Ā·B̄
Hubungan   : r = Cov(A,B) / (σA·σB)
```

## Feature Selection
```
Entropy          : H(Y) = − Σ P(yᵢ) log₂ P(yᵢ)
Information Gain : IG(Y,X) = H(Y) − H(Y|X)
Relief           : W[A] = W[A] − diff(A,X,H) + diff(A,X,M)
```

## Proximity
```
Cosine similarity : cos = Σ AᵢBᵢ / (√ΣAᵢ² × √ΣBᵢ²)
Jaccard similarity: J = |A∩B| / |A∪B|          Jaccard distance = 1 − J
Euclidean         : d = √Σ (Aᵢ − Bᵢ)²
Manhattan         : d = Σ |Aᵢ − Bᵢ|
Minkowski         : d = (Σ |Aᵢ − Bᵢ|^p)^(1/p)    (p=1 Manhattan, p=2 Euclidean)
Hamming           : D = Σ 𝟙(xᵢ ≠ yᵢ)
```

## Evaluasi Model
```
Accuracy  = (TP+TN)/(TP+TN+FP+FN)
Precision = TP/(TP+FP)
Recall    = TP/(TP+FN)
F1        = 2·P·R/(P+R)
```

## Angka-Angka Penting untuk Dihafal
| Item | Nilai |
|---|---|
| Contoh min-max slide | \$73.600 → **0,716** |
| Contoh z-score slide | → **1,225** |
| Contoh decimal scaling slide | 76.300 → **0,763** (j=5) |
| Contoh χ² slide | **507,93** |
| Cov saham slide | **4** (E(A)=4, E(B)=9,6) |
| Mean imputation contoh | **36,2** |
| Missing > ? tidak cocok mean/median | **> 5%** |
| Rasio hold-out | 70:30, 80:20, 90:10 |
| Train/Val/Test | 60–70 / 15–20 / 15–20 % |
| Fraud case | 10.000 total; 9.744 normal; 256 fraud |
| Recall fraud | 31,4% → 43,1% (SMOTE); CV 28,1% → 41,4% |
| Manhattan A(1,1)-B(4,5) | **7** |
| Outlier contoh IQR | batas 11,5 & 31,5; outlier 5, 7, 10 |

## Mind-Map Ringkas

```
DATA MINING
│
├─ Pengantar ── KDD, fungsi DM, tipe data, isu
├─ Data & Atribut ── objek/atribut; nominal·biner·ordinal·interval·rasio; tipe dataset
├─ PREPROCESSING
│    ├─ Cleaning ── missing (MCAR/MAR/MNAR → imputasi), noisy (binning/regresi/clustering), outlier (IQR)
│    ├─ Integration ── skema, entitas, konflik nilai, redundansi (χ², Pearson, kovarians)
│    ├─ Reduction ── dimensionality (wavelet, PCA, subset selection) | numerosity (regresi, histogram, clustering, sampling)
│    └─ Transformation ── smoothing, konstruksi, agregasi, normalisasi, diskretisasi
├─ Feature Selection ── Filter | Wrapper | Embedded
├─ Proximity ── similarity (cosine, Jaccard, korelasi) | dissimilarity (Euclid, Manhattan, Minkowski, Hamming)
└─ Splitting & Resampling ── hold-out, train-val-test, K-fold | over/under-sampling, SMOTE, Tomek, bootstrap | leakage
```

---
**Selesai. Selamat belajar dan semoga sukses ujian! 🎓**
