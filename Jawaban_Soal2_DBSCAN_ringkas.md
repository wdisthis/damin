# Soal 2 – Unsupervised Learning: DBSCAN (CPMK111)

## 2a. Karakteristik Algoritma DBSCAN

DBSCAN (*Density-Based Spatial Clustering of Applications with Noise*) adalah algoritma klastering berbasis **kepadatan**: klaster adalah daerah padat yang dipisahkan oleh daerah jarang, dan titik di daerah jarang dianggap noise/outlier.

- **Parameter:** ε (radius lingkungan) dan MinPts (jumlah minimum titik dalam radius ε, termasuk titik itu sendiri).
- **Core point:** punya ≥ MinPts titik dalam radius ε.
- **Border point:** bukan core, tetapi berada dalam radius ε dari suatu core point.
- **Noise (outlier):** bukan core dan bukan border.
- **Klaster:** kumpulan core point yang saling *density-connected* (terhubung lewat rantai tetangga ≤ ε) beserta border point-nya.
- **Kelebihan:** tidak perlu menentukan jumlah klaster, dapat membentuk klaster berbentuk sembarang, dan mampu mendeteksi outlier.
- **Kekurangan:** sensitif terhadap ε dan MinPts, serta kurang baik jika kepadatan antarklaster berbeda atau data berdimensi tinggi.

**Langkah:** (1) hitung ε-neighborhood tiap titik; (2) tandai core point; (3) hubungkan core point yang bertetangga menjadi satu klaster, lalu tambahkan border point; (4) titik sisanya adalah noise.

---

## 2b. Perhitungan (ε = 1,2 ; MinPts = 2)

**Data**

| Titik | A | B | C | D | E | F | G | H | I | J |
|---|---|---|---|---|---|---|---|---|---|---|
| x | 1,5 | 1,0 | 3,0 | 3,5 | 3,0 | 0,5 | 1,5 | 2,0 | 2,5 | 4,0 |
| y | 1,0 | 2,5 | 0,5 | 1,5 | 1,0 | 3,0 | 3,5 | 3,0 | 2,0 | 4,0 |

**Jarak Euclidean** d(p,q) = √[(xₚ−x_q)² + (yₚ−y_q)²]. Contoh: d(B,F) = √(0,5² + 0,5²) = 0,707.

| | B | C | D | E | F | G | H | I | J |
|---|---|---|---|---|---|---|---|---|---|
| **A** | 1,581 | 1,581 | 2,062 | 1,500 | 2,236 | 2,500 | 2,062 | 1,414 | 3,905 |
| **B** | | 2,828 | 2,693 | 2,500 | **0,707** | **1,118** | **1,118** | 1,581 | 3,354 |
| **C** | | | **1,118** | **0,500** | 3,536 | 3,354 | 2,693 | 1,581 | 3,640 |
| **D** | | | | **0,707** | 3,354 | 2,828 | 2,121 | **1,118** | 2,550 |
| **E** | | | | | 3,202 | 2,915 | 2,236 | **1,118** | 3,162 |
| **F** | | | | | | **1,118** | 1,500 | 2,236 | 3,640 |
| **G** | | | | | | | **0,707** | 1,803 | 2,550 |
| **H** | | | | | | | | **1,118** | 2,236 |
| **I** | | | | | | | | | 2,500 |

*(Angka tebal: jarak ≤ 1,2, yaitu bertetangga.)*

**ε-neighborhood dan jenis titik**

| Titik | Tetangga (≤ 1,2) | Jumlah (incl. diri) | Jenis |
|---|---|---|---|
| A | – | 1 | Noise |
| B | F, G, H | 4 | Core |
| C | D, E | 3 | Core |
| D | C, E, I | 4 | Core |
| E | C, D, I | 4 | Core |
| F | B, G | 3 | Core |
| G | B, F, H | 4 | Core |
| H | B, G, I | 4 | Core |
| I | D, E, H | 4 | Core |
| J | – | 1 | Noise |

> 📌 **Letakkan Gambar 2 di sini** (`gambar2_graf_ketetanggaan.png`, opsional).

![Gambar 2 – Graf ketetanggaan](gambar2_graf_ketetanggaan.png)

**Pembentukan klaster:** mulai dari B (core) → Klaster 1, masuk F, G, H. Dari H masuk I. Dari I masuk D dan E. Dari D masuk C. Seluruh core sudah terjangkau, jadi klaster selesai. A dan J tidak punya tetangga dalam radius ε dan tidak terjangkau core point mana pun, sehingga menjadi noise.

**Hasil**
- Jumlah klaster: **1** (anggota: B, C, D, E, F, G, H, I)
- Outlier: **A dan J**

---

## 2c. Plot Hasil Klasterisasi

> 📌 **Letakkan Gambar 3 di sini** (`gambar3_hasil_dbscan.png`, wajib).

![Gambar 3 – Hasil DBSCAN](gambar3_hasil_dbscan.png)

*Gambar 3. Hasil DBSCAN (ε = 1,2 ; MinPts = 2): satu klaster (titik biru) dan dua outlier (X merah: A dan J).*
