# Tugas Individu Modul 6 Data Mining — DBSCAN

> **Cara pakai:** tiap bagian di bawah dibuat berurutan sebagai sel notebook. Blok `python` = sel kode, teks lainnya = sel Markdown.
> Angka pada bagian "Output" berasal dari kode yang sudah dijalankan (scikit-learn 1.9.1). Gambar tidak disertakan, jadi setiap plot saya jelaskan secara tertulis.

---

# Soal 1: Implementasi DBSCAN pada Dataset Sintetis

## 1.1 Import library

```python
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
import matplotlib
from sklearn.datasets import make_moons, make_blobs
from sklearn.cluster import DBSCAN
from sklearn.neighbors import NearestNeighbors
import warnings
warnings.filterwarnings("ignore")
```

## 1.2 Membuat dataset sintetis

Dataset terdiri atas 800 titik `make_moons`, 300 titik `make_blobs` (satu pusat di (2.2, 1.8)), dan 100 titik noise acak seragam.

```python
# Dua bulan sabit
X_moons, _ = make_moons(n_samples=800, noise=0.06, random_state=42)

# Satu blob padat
X_blob, _ = make_blobs(n_samples=300, centers=[(2.2, 1.8)],
                       cluster_std=0.18, random_state=42)

# Noise acak seragam
np.random.seed(42)
X_noise = np.column_stack([
    np.random.uniform(-1.5, 3.0, 100),   # x
    np.random.uniform(-1.0, 2.5, 100)    # y
])

# Gabungkan
X = np.vstack([X_moons, X_blob, X_noise])
df = pd.DataFrame(X, columns=['x', 'y'])

print(df.head())
print("\nJumlah data:", df.shape)
print(df.describe().round(3))
```

**Output:**

```
          x         y
0  1.629639 -0.194725
1  1.474988 -0.409634
2  0.873052  0.413833
3  0.466781 -0.437898
4  0.985871  0.498703

Jumlah data: (1200, 2)
```

Dataset berisi **1.200 titik** dan **2 fitur** (800 + 300 + 100). Rentang x sekitar −1,48 sampai 2,94 dan rentang y sekitar −0,98 sampai 2,49.

## 1.3 Visualisasi sebelum clustering

```python
plt.figure(figsize=(6, 6))
plt.scatter(df['x'], df['y'], s=12, color='grey')
plt.title('Dataset Sintetis Sebelum Clustering')
plt.xlabel('x')
plt.ylabel('y')
plt.show()
```

**Penjelasan pola persebaran:**
Pada plot terlihat tiga struktur padat dan satu sebaran titik jarang:
- **Dua bulan sabit** yang saling mengait dan berbentuk non-konveks (satu melengkung ke atas, satu ke bawah).
- **Satu blob bulat** yang sangat rapat di sekitar (2.2, 1.8), terpisah dari kedua bulan sabit.
- **Titik noise** tersebar acak dan jarang di seluruh bidang, termasuk di sela-sela dan di sekitar kelompok padat.

Bentuk bulan sabit membuat algoritma berbasis centroid seperti K-Means sulit memisahkannya dengan benar. Data seperti ini cocok untuk DBSCAN karena pengelompokannya didasarkan pada kepadatan, bukan jarak ke pusat klaster.

## 1.4 DBSCAN dengan `eps=0.5`, `MinPts=5`

```python
db_05 = DBSCAN(eps=0.5, min_samples=5).fit(X)
df['label_eps05'] = db_05.labels_

print(df['label_eps05'].value_counts().sort_index())

colors = ['black', 'tab:red', 'tab:blue', 'tab:green', 'tab:orange', 'tab:purple']
def plot_clusters(X, labels, title):
    plt.figure(figsize=(6, 6))
    for lab in np.unique(labels):
        m = labels == lab
        if lab == -1:
            plt.scatter(X[m, 0], X[m, 1], s=12, c='black', label='Noise')
        else:
            plt.scatter(X[m, 0], X[m, 1], s=12, c=colors[(lab % 5) + 1], label=f'Cluster {lab}')
    plt.title(title); plt.xlabel('x'); plt.ylabel('y'); plt.legend(); plt.show()

plot_clusters(X, db_05.labels_, 'DBSCAN (eps=0.5, MinPts=5)')
```

**Output:**

| Label | Jumlah anggota |
|:---:|---:|
| 0 | 1199 |
| -1 (noise) | 1 |

**Penjelasan:**
Dengan `eps=0.5` seluruh data hampir menyatu menjadi **satu cluster besar** (1.199 titik) dan hanya 1 titik yang menjadi noise. Radius 0,5 terlalu besar relatif terhadap skala data (jarak antar titik dalam kelompok hanya sekitar 0,05–0,1, dan celah antar bulan sabit maupun ke blob lebih kecil dari 0,5). Akibatnya dua bulan sabit, blob, dan hampir semua titik noise tersambung lewat rantai *density-connected*. Ini contoh `eps` yang **terlalu besar**: struktur data hilang dan noise tidak terdeteksi.

## 1.5 K-distance graph

Karena `MinPts=5` (sudah termasuk titik itu sendiri pada `scikit-learn`), maka dihitung jarak ke tetangga ke-5 dengan `n_neighbors=5`. Kolom terakhir hasil `kneighbors` adalah jarak ke tetangga ke-5 tersebut (kolom pertama adalah jarak ke dirinya sendiri, 0).

```python
nn = NearestNeighbors(n_neighbors=5).fit(X)
distances, _ = nn.kneighbors(X)
k_dist = np.sort(distances[:, -1])

plt.figure(figsize=(8, 4))
plt.plot(k_dist)
plt.axhline(0.12, color='red', linestyle='--', label='eps = 0.12')
plt.title('K-distance Graph (k = 5)')
plt.xlabel('Data points sorted by distance')
plt.ylabel('Jarak ke tetangga ke-5 (eps)')
plt.legend()
plt.show()

print("Persentil 50/75/90/95/99:",
      [round(np.percentile(k_dist, p), 3) for p in (50, 75, 90, 95, 99)])
```

**Output:**

```
Persentil 50/75/90/95/99: [0.046, 0.062, 0.108, 0.279, 0.522]
```

**Pembacaan grafik:** kurva naik sangat landai (0,03–0,10) untuk sekitar 90% titik, lalu melonjak tajam. Titik siku (*elbow*) berada di sekitar **0,10–0,12**. Titik di sebelah kiri siku adalah titik di area padat, sedangkan titik di sebelah kanan adalah kandidat noise. Karena itu `eps = 0.12` dipilih, sesuai instruksi soal.

## 1.6 DBSCAN dengan `eps=0.12`, `MinPts=5`

```python
db = DBSCAN(eps=0.12, min_samples=5).fit(X)
labels = db.labels_

n_clusters = len(set(labels)) - (1 if -1 in labels else 0)
n_noise = (labels == -1).sum()

print("Label unik   :", np.unique(labels))
print("Jumlah cluster:", n_clusters)
print("Jumlah noise  :", n_noise)
print(pd.Series(labels).value_counts().sort_index())

plot_clusters(X, labels, 'DBSCAN (eps=0.12, MinPts=5)')
```

**Output:**

```
Label unik    : [-1  0  1  2]
Jumlah cluster: 3
Jumlah noise  : 83
```

| Label | Jumlah anggota |
|:---:|---:|
| -1 (noise) | 83 |
| 0 | 411 |
| 1 | 408 |
| 2 | 298 |

Visualisasi: tiga cluster berwarna berbeda (dua bulan sabit dan satu blob) dan titik hitam tersebar sebagai noise.

## 1.7 Interpretasi

- **Pola kelompok:** DBSCAN berhasil memisahkan tepat tiga kelompok yang sesuai dengan struktur sebenarnya: cluster 0 dan 1 adalah dua bulan sabit (masing-masing 400 titik asli), cluster 2 adalah blob (292 titik asli).
- **Pembentukan cluster:** Titik di area padat memiliki ≥ 5 tetangga (termasuk dirinya) dalam radius 0,12, sehingga menjadi *core point* dan saling terhubung membentuk cluster berbentuk bebas. Cluster 0 dan 1 tidak bergabung karena celah di antara keduanya lebih lebar dari `eps`.
- **Pengaruh `eps`:** `eps=0.5` menggabungkan semuanya menjadi 1 cluster dengan hanya 1 noise. Eksperimen tambahan menunjukkan `eps=0.08` menghasilkan 4 cluster dan 115 noise (bulan sabit terpecah karena titik di bagian yang lebih jarang tidak terjangkau), `eps=0.10` menghasilkan 3 cluster dan 94 noise, `eps=0.12` menghasilkan 3 cluster dan 83 noise, dan `eps=0.15` menghasilkan 3 cluster dan 76 noise. Jadi `eps` kecil memecah cluster dan memperbanyak noise, `eps` besar menggabungkan cluster dan menyembunyikan noise.
- **Noise:** Dari 83 titik noise, 75 berasal dari 100 titik noise acak yang disisipkan, 8 berasal dari tepi blob, dan 0 dari bulan sabit. Sebanyak 25 titik noise acak ikut masuk ke cluster (11 ke cluster 0, 8 ke cluster 1, 6 ke cluster 2) karena kebetulan jatuh dekat kelompok padat sehingga menjadi *border point*. Ini wajar karena DBSCAN mengklasifikasikan berdasarkan kepadatan lokal, bukan asal data.

---

# Soal 2: Implementasi DBSCAN Tanpa Library DBSCAN

## 2.1 Import library dan data

Dataset yang dipakai sama dengan Soal 1 (variabel `X`). Jika Soal 2 dijalankan di sel terpisah, pastikan sel pembuatan dataset Soal 1 sudah dijalankan.

```python
import numpy as np
import matplotlib.pyplot as plt
from collections import deque
from scipy.spatial.distance import cdist
from sklearn.cluster import DBSCAN                     # hanya untuk perbandingan
from sklearn.metrics import adjusted_rand_score
```

## 2.2 Fungsi neighbor dan core point

Jumlah tetangga **termasuk titik itu sendiri** (jarak 0 ≤ eps), sehingga core point didefinisikan sebagai titik dengan jumlah tetangga ≥ MinPts.

```python
def get_neighbors(D, i, eps):
    """Indeks semua titik dalam radius eps dari titik i (termasuk i sendiri)."""
    return np.where(D[i] <= eps)[0]

def find_core_points(neighbors, min_pts):
    """True jika jumlah tetangga (termasuk dirinya sendiri) >= min_pts."""
    return np.array([len(nb) >= min_pts for nb in neighbors])
```

## 2.3 Pembentukan dan pengembangan cluster (queue)

```python
def dbscan_manual(X, eps, min_pts):
    n = len(X)
    D = cdist(X, X)                                           # matriks jarak
    neighbors = [get_neighbors(D, i, eps) for i in range(n)]
    is_core = find_core_points(neighbors, min_pts)

    labels = np.full(n, -1)          # -1 = noise (default)
    visited = np.zeros(n, dtype=bool)
    cluster_id = 0

    for i in range(n):
        if visited[i] or not is_core[i]:
            continue

        # core point baru -> mulai cluster baru
        labels[i] = cluster_id
        visited[i] = True
        queue = deque(neighbors[i])

        while queue:
            j = queue.popleft()
            if labels[j] == -1:          # noise/belum berlabel -> masuk cluster
                labels[j] = cluster_id   # (border point jika bukan core)
            if visited[j]:
                continue
            visited[j] = True
            if is_core[j]:               # hanya core point yang memperluas cluster
                queue.extend(neighbors[j])

        cluster_id += 1

    return labels, is_core
```

Cara kerja singkat:
1. Hitung semua tetangga dan tentukan core point.
2. Setiap core point yang belum dikunjungi memulai cluster baru.
3. Tetangganya dimasukkan ke antrean. Setiap titik di antrean diberi label cluster; jika titik itu core point, tetangganya ikut dimasukkan ke antrean (inilah *density connectivity*).
4. Titik non-core yang terjangkau menjadi **border point**, sedangkan titik yang tidak pernah terjangkau tetap berlabel **-1 (noise)**.

## 2.4 Menjalankan clustering (`eps=0.12`, `MinPts=5`)

```python
labels_manual, is_core = dbscan_manual(X, eps=0.12, min_pts=5)

n_cl_manual = len(set(labels_manual)) - (1 if -1 in labels_manual else 0)
n_noise_manual = (labels_manual == -1).sum()
n_core = is_core.sum()
n_border = ((labels_manual != -1) & (~is_core)).sum()

print("Label unik     :", np.unique(labels_manual))
print("Jumlah cluster :", n_cl_manual)
print("Jumlah noise   :", n_noise_manual)
print("Core point     :", n_core)
print("Border point   :", n_border)
print(pd.Series(labels_manual).value_counts().sort_index())
```

**Output:**

```
Label unik     : [-1  0  1  2]
Jumlah cluster : 3
Jumlah noise   : 83
Core point     : 1096
Border point   : 21
```

| Label | Jumlah anggota |
|:---:|---:|
| -1 (noise) | 83 |
| 0 | 411 |
| 1 | 408 |
| 2 | 298 |

## 2.5 Visualisasi

```python
plot_clusters(X, labels_manual, 'DBSCAN Manual (eps=0.12, MinPts=5)')
```

Plot menampilkan tiga cluster (dua bulan sabit dan satu blob) dengan warna berbeda, dan noise berwarna hitam. Hasilnya sama dengan plot Soal 1 bagian 1.6.

## 2.6 Perbandingan dengan Scikit-learn

```python
labels_sk = DBSCAN(eps=0.12, min_samples=5).fit(X).labels_

n_cl_sk = len(set(labels_sk)) - (1 if -1 in labels_sk else 0)
n_noise_sk = (labels_sk == -1).sum()
ari = adjusted_rand_score(labels_sk, labels_manual)

comparison = pd.DataFrame({
    'Implementasi': ['Manual', 'Scikit-learn'],
    'Jumlah cluster': [n_cl_manual, n_cl_sk],
    'Jumlah noise': [n_noise_manual, n_noise_sk],
})
print(comparison)
print("Adjusted Rand Index :", ari)
print("Label identik       :", np.array_equal(labels_manual, labels_sk))
```

**Output:**

| Implementasi | Jumlah cluster | Jumlah noise |
|---|:---:|:---:|
| Manual | 3 | 83 |
| Scikit-learn | 3 | 83 |

```
Adjusted Rand Index : 1.0
Label identik       : True
```

Jumlah core point juga sama persis dengan `db.core_sample_indices_` milik Scikit-learn (1.096 titik).

## 2.7 Interpretasi

- **Proses pembentukan cluster:** Cluster tumbuh dari sebuah core point lalu merambat lewat tetangga-tetangga yang juga core point (rantai *density-connected*). Dari 1.096 core point, terbentuk tiga komponen yang saling terpisah: dua bulan sabit dan satu blob. Sebanyak 21 titik adalah *border point* yang ikut cluster karena berada dalam jangkauan core point tetapi tidak cukup padat untuk memperluas cluster. Sisanya, 83 titik, tidak terjangkau dari core point mana pun dan menjadi noise.
- **Hasil clustering:** Jumlah cluster, jumlah noise, dan label per titik identik dengan Scikit-learn, sehingga ARI = 1,0 (kesesuaian sempurna). Ini menunjukkan implementasi manual sudah benar.
- **Perbedaan manual vs Scikit-learn:** Dalam hasil ini tidak ada perbedaan. Secara teori, perbedaan hanya mungkin muncul pada *border point* yang berada dalam jangkauan dua cluster berbeda, karena DBSCAN tidak deterministik terhadap urutan data: titik itu masuk ke cluster yang lebih dulu menjangkaunya. Pada dataset ini cluster-nya terpisah jelas sehingga situasi itu tidak terjadi. Dari sisi efisiensi, versi manual membangun matriks jarak penuh `O(n²)` memori, sehingga hanya cocok untuk data kecil seperti ini. Scikit-learn memakai struktur indeks spasial (KD-tree/Ball-tree) yang jauh lebih hemat untuk data besar.

---

# Soal 3: Implementasi DBSCAN pada Dataset Lokasi California Housing

## 3.1 Import library dan dataset

```python
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
from sklearn.datasets import fetch_california_housing
from sklearn.cluster import DBSCAN
from sklearn.neighbors import NearestNeighbors
from sklearn.preprocessing import StandardScaler
import warnings
warnings.filterwarnings("ignore")

housing = fetch_california_housing(as_frame=True)
df_ch = housing.frame

print(df_ch.head())
print("\nJumlah baris, kolom:", df_ch.shape)
print("\nTipe data:\n", df_ch.dtypes)
print("\nMissing values:\n", df_ch.isna().sum())
```

> **Alternatif (Google Colab):** jika memakai file bawaan Colab, ganti dengan
> `df_ch = pd.read_csv('/content/sample_data/california_housing_train.csv')`
> lalu gunakan kolom `latitude` dan `longitude` (huruf kecil) pada langkah berikutnya.

**Output (ringkasan):** dataset memiliki **20.640 baris**, kolom lokasi bernama `Latitude` dan `Longitude` (bertipe `float64`). Pada versi `fetch_california_housing` tidak ada *missing values*. Pada versi CSV asli yang saya pakai untuk menguji kode, hanya kolom `total_bedrooms` yang memiliki 207 nilai kosong, sedangkan `latitude` dan `longitude` tidak memiliki *missing values*.

## 3.2 Preprocessing dan sampling

```python
coords = df_ch[['Latitude', 'Longitude']].dropna()      # tangani missing values
sample = coords.sample(n=5000, random_state=42)
X_geo = sample[['Longitude', 'Latitude']].values        # kolom 0 = lon (x), kolom 1 = lat (y)

print(sample.head())
print("Ukuran sampel:", X_geo.shape)
```

Karena tidak ada missing value pada kedua variabel, `dropna()` tidak menghapus baris apa pun, lalu diambil 5.000 sampel acak.

## 3.3 Visualisasi lokasi sebelum clustering

```python
plt.figure(figsize=(6, 7))
plt.scatter(X_geo[:, 0], X_geo[:, 1], s=4, color='grey')
plt.title('Distribusi Lokasi California Housing (n=5000)')
plt.xlabel('Longitude')
plt.ylabel('Latitude')
plt.show()
```

Plot memperlihatkan bentuk garis pantai dan lembah California. Titik sangat rapat di dua wilayah (sekitar 34°N, −118°W dan 37–38°N, −122°W), cukup padat memanjang di Central Valley, dan jarang di wilayah pegunungan, gurun, serta pesisir utara.

## 3.4 K-distance graph (k = 10), data asli (derajat)

```python
def k_distance(X, k=10):
    nn = NearestNeighbors(n_neighbors=k).fit(X)
    dist, _ = nn.kneighbors(X)
    return np.sort(dist[:, -1])

k_raw = k_distance(X_geo, k=10)

plt.figure(figsize=(8, 4))
plt.plot(k_raw)
plt.axhline(0.2, color='red', linestyle='--', label='eps = 0.2')
plt.title('K-distance Graph (k = 10) - Koordinat Asli')
plt.xlabel('Data points sorted by distance')
plt.ylabel('Jarak ke tetangga ke-10 (derajat)')
plt.legend()
plt.show()

print("Persentil 50/75/90/95/98:",
      [round(np.percentile(k_raw, p), 3) for p in (50, 75, 90, 95, 98)])
```

**Output:**

```
Persentil 50/75/90/95/98: [0.03, 0.058, 0.153, 0.24, 0.44]
```

**Pembacaan grafik:** kurva rendah dan landai untuk mayoritas titik (median hanya 0,03°), kemudian menanjak tajam di ujung kanan. Siku berada sekitar **0,2** (titik siku terhitung ≈ 0,206), sehingga dipilih `eps = 0.2`.

## 3.5 DBSCAN pada koordinat asli (`eps=0.2`, `MinPts=10`)

```python
db_raw = DBSCAN(eps=0.2, min_samples=10).fit(X_geo)
lab_raw = db_raw.labels_
n_cl_raw = len(set(lab_raw)) - (1 if -1 in lab_raw else 0)
n_noise_raw = (lab_raw == -1).sum()
print("Jumlah cluster:", n_cl_raw)
print("Jumlah noise  :", n_noise_raw)
print(pd.Series(lab_raw).value_counts().head(6))
```

**Output:**

```
Jumlah cluster: 14
Jumlah noise  : 203
```

Lima cluster terbesar:

| Cluster | Anggota | Pusat (lat, lon) |
|:---:|---:|:---:|
| 2 | 2.699 | (33,82; −117,95) |
| 1 | 1.563 | (37,84; −121,91) |
| 0 | 222 | (36,59; −119,63) |
| 6 | 57 | (35,36; −119,02) |
| 9 | 42 | (35,18; −120,59) |

## 3.6 Visualisasi hasil clustering (koordinat asli)

```python
def plot_geo(X, labels, title):
    plt.figure(figsize=(6, 7))
    uniq = [l for l in np.unique(labels) if l != -1]
    cmap = plt.get_cmap('tab20', max(len(uniq), 1))
    for l in uniq:
        m = labels == l
        plt.scatter(X[m, 0], X[m, 1], s=5, color=cmap(l % 20), label=f'C{l}')
    m = labels == -1
    plt.scatter(X[m, 0], X[m, 1], s=8, color='black', label='Noise')
    plt.title(title); plt.xlabel('Longitude'); plt.ylabel('Latitude')
    plt.legend(fontsize=6, ncol=2, markerscale=2)
    plt.show()

plot_geo(X_geo, lab_raw, f'DBSCAN Koordinat Asli (eps=0.2, MinPts=10): {n_cl_raw} cluster, {n_noise_raw} noise')
```

## 3.7 Ulangi dengan koordinat distandarisasi

```python
scaler = StandardScaler()
X_std = scaler.fit_transform(X_geo)
print("Std per fitur (lon, lat):", scaler.scale_.round(3))

# K-distance graph (k=10) data terstandarisasi
k_std = k_distance(X_std, k=10)

plt.figure(figsize=(8, 4))
plt.plot(k_std)
plt.axhline(0.1, color='red', linestyle='--', label='eps = 0.1')
plt.title('K-distance Graph (k = 10) - Koordinat Terstandarisasi')
plt.xlabel('Data points sorted by distance')
plt.ylabel('Jarak ke tetangga ke-10 (satuan std)')
plt.legend()
plt.show()

print("Persentil 50/75/90/95/98:",
      [round(np.percentile(k_std, p), 3) for p in (50, 75, 90, 95, 98)])

# DBSCAN
db_std = DBSCAN(eps=0.1, min_samples=10).fit(X_std)
lab_std = db_std.labels_
n_cl_std = len(set(lab_std)) - (1 if -1 in lab_std else 0)
n_noise_std = (lab_std == -1).sum()
print("Jumlah cluster:", n_cl_std)
print("Jumlah noise  :", n_noise_std)

plot_geo(X_geo, lab_std, f'DBSCAN Terstandarisasi (eps=0.1, MinPts=10): {n_cl_std} cluster, {n_noise_std} noise')
# (diplot pada koordinat asli agar mudah dibandingkan)

# Perbandingan
from sklearn.metrics import adjusted_rand_score
print("ARI antara dua hasil:", adjusted_rand_score(lab_raw, lab_std))
print(pd.DataFrame({
    'Data': ['Asli (derajat)', 'Standarisasi'],
    'eps': [0.2, 0.1],
    'Jumlah cluster': [n_cl_raw, n_cl_std],
    'Jumlah noise': [n_noise_raw, n_noise_std],
}))
```

**Output:**

```
Std per fitur (lon, lat): [2.141 2.006]
Persentil 50/75/90/95/98: [0.015, 0.028, 0.075, 0.116, 0.21]
Jumlah cluster: 14
Jumlah noise  : 185
ARI antara dua hasil: 0.988
```

Siku K-distance graph terstandarisasi berada di sekitar **0,10** (titik siku terhitung ≈ 0,100).

**Perbandingan:**

| Data | eps (hasil K-distance) | Jumlah cluster | Jumlah noise | Cluster terbesar |
|---|:---:|:---:|:---:|---|
| Koordinat asli (derajat) | 0,2 | 14 | 203 | 2.699 / 1.563 / 222 |
| Terstandarisasi | 0,1 | 14 | 185 | 2.702 / 1.603 / 223 |

## 3.8 Interpretasi

- **Distribusi lokasi:** Data tidak tersebar merata. Titik menumpuk di kawasan metropolitan dan sangat jarang di daerah pedalaman, sehingga kepadatan sangat bervariasi antar wilayah. Inilah kasus yang cocok untuk DBSCAN karena klaster berbentuk tidak beraturan mengikuti garis pantai dan lembah.
- **Karakteristik cluster:** Dua cluster mendominasi: cluster dengan pusat sekitar (33,8; −117,9) berisi sekitar 2.700 titik, kira-kira kawasan Los Angeles–Orange County, dan cluster di sekitar (37,8; −121,9) berisi sekitar 1.560–1.600 titik, kira-kira kawasan Bay Area. Keduanya wilayah padat penduduk dan menyerap sekitar 85% titik non-noise. Cluster ketiga (≈ 222 titik) berpusat di sekitar (36,6; −119,6), kira-kira Central Valley. Sisanya cluster kecil (puluhan titik) yang mewakili kota atau kawasan permukiman lain yang terpisah. Penamaan wilayah ini adalah interpretasi dari koordinat pusat cluster, bukan hasil algoritma.
- **Noise:** Sebanyak 203 titik (≈ 4%) pada koordinat asli dan 185 titik pada data terstandarisasi berada di area jarang seperti pegunungan, gurun, dan pesisir utara, di mana tetangga dalam radius `eps` kurang dari 10. Ini lokasi permukiman terpencil, bukan kesalahan data.
- **Pengaruh standarisasi:** Standar deviasi longitude (≈ 2,14) dan latitude (≈ 2,01) hampir sama, sehingga standarisasi hampir menskalakan kedua sumbu secara seragam (±1/2). Karena itu nilai `eps` dari K-distance graph ikut mengecil dari 0,2 menjadi 0,1 (setara kira-kira 0,2° jika dikonversi kembali). Jumlah cluster sama (14), jumlah noise sedikit berbeda (203 vs 185), dan ARI 0,988, artinya 97,9% label sama. Perbedaan kecil muncul dari skala longitude dan latitude yang tidak persis sama sehingga jarak antar titik sedikit berubah. Pada kasus ini standarisasi tidak terlalu diperlukan karena kedua fitur sudah berupa derajat dengan skala sejenis. Namun nilai `eps` tidak dapat dibandingkan langsung antar kedua versi karena satuannya berbeda (derajat vs satuan standar deviasi). Standarisasi menjadi penting jika fitur memiliki skala yang sangat berbeda. Perlu dicatat juga bahwa jarak Euclidean pada derajat bukan jarak sebenarnya di permukaan bumi (1° longitude lebih pendek dari 1° latitude di California). Untuk analisis yang lebih akurat dapat dipakai metrik `haversine`.

---

## Catatan Reproduksibilitas

- Soal 1 dan 2 dijalankan persis dengan kode di atas, sehingga hasilnya akan sama jika `random_state` dan `seed` dipertahankan.
- Soal 3 diuji memakai file CSV California Housing yang berisi data wilayah yang sama (20.640 baris). Jika notebook Anda memakai `fetch_california_housing`, nama kolomnya `Latitude`/`Longitude` dan urutan baris bisa berbeda, sehingga **sampel 5.000 titik bisa tidak sama persis** dan angka (jumlah cluster/noise, titik siku) mungkin sedikit bergeser. Jika berbeda, sesuaikan nilai `eps` dengan siku pada K-distance graph milik Anda dan perbarui angka pada interpretasi.
