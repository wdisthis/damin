# Tugas Individu Modul 5 – Data Analytics ITERA Airlines

## Ketentuan Pengerjaan

Selamat datang di praktikum Data Analytics ITERA Airlines (nama ini biar keliatan keren hehe)

Pada praktikum ini, Anda akan melakukan analisis terhadap data historis jumlah penumpang penerbangan selama lebih dari satu dekade. Dataset yang digunakan adalah flights yang tersedia pada library Seaborn.

Melalui lima soal berikut, Anda akan mempelajari cara mengenali karakteristik data, menganalisis pola dan distribusi data, melakukan transformasi data, serta membuat visualisasi menggunakan Python dan Seaborn.

---

## Soal 1: Mengenal dan Memahami Data

Sebelum melakukan analisis, pahami terlebih dahulu karakteristik dataset yang akan digunakan. Tuliskan kode Python untuk:

1. Memuat dataset flights dari Seaborn:
(df_flights = sns.load_dataset('flights')
jadi tidak usah download, langsung panggil aja.
2. Menampilkan informasi dataset, meliputi jumlah baris, tipe data setiap kolom, dan jumlah nilai kosong (missing values).
3. Membuat visualisasi diagram batang (bar chart) yang menunjukkan total jumlah penumpang untuk setiap tahun.

### Kode

```python
import seaborn as sns
import matplotlib.pyplot as plt

# 1. Memuat dataset flights langsung dari Seaborn
df_flights = sns.load_dataset('flights')

# 2. Informasi dataset: jumlah baris, tipe data, dan jumlah missing values
print("Jumlah baris dan kolom:", df_flights.shape)
df_flights.info()
print(df_flights.isnull().sum())

# 3. Total penumpang per tahun, lalu ditampilkan dalam bar chart
yearly_total = df_flights.groupby('year')['passengers'].sum().reset_index()

plt.figure(figsize=(10, 5))
sns.barplot(data=yearly_total, x='year', y='passengers', color='steelblue')
plt.title('Total Jumlah Penumpang per Tahun')
plt.xlabel('Tahun')
plt.ylabel('Total Penumpang')
plt.savefig('bar_total_per_tahun.png')
plt.show()
```

### Analisis

Kode pada bagian ini dipakai untuk mengenali data sebelum dianalisis lebih lanjut. `df_flights.shape` dan `info()` menunjukkan ukuran dan tipe data. `isnull().sum()` menghitung nilai kosong di setiap kolom. Bar chart dibuat dengan cara menjumlahkan jumlah penumpang per tahun menggunakan `groupby`.

Berdasarkan hasil yang diharapkan dari dataset ini:

- Dataset berisi **144 baris** dengan **3 kolom**: `year` (int), `month` (category), dan `passengers` (int).
- Tidak ada nilai kosong, sehingga `isnull().sum()` menghasilkan 0 untuk semua kolom.
- Total penumpang per tahun terus meningkat. Tahun 1949 sekitar **1.520** penumpang, sedangkan tahun 1960 sekitar **5.714** penumpang. Jadi jumlah penumpang lebih dari tiga kali lipat dalam 12 tahun.

> Catatan: angka di atas perlu dicocokkan dengan output saat kode dijalankan.

---

## Soal 2: Menganalisis Tren Jumlah Penumpang

Amati bagaimana perubahan jumlah penumpang dari tahun ke tahun dan apakah terdapat pola tertentu berdasarkan bulan. Buatlah visualisasi diagram garis (line plot) menggunakan Seaborn dengan ketentuan:

1. Sumbu X menunjukkan tahun (year).
2. Sumbu Y menunjukkan jumlah penumpang (passengers).
3. Gunakan parameter hue untuk membedakan garis berdasarkan bulan (month).

### Kode

```python
plt.figure(figsize=(12, 6))

# Sumbu X = year, sumbu Y = passengers, garis dibedakan dengan hue = month
sns.lineplot(data=df_flights, x='year', y='passengers', hue='month',
             palette='tab20', linewidth=2)

plt.title('Tren Jumlah Penumpang per Tahun Berdasarkan Bulan')
plt.xlabel('Tahun')
plt.ylabel('Jumlah Penumpang')
plt.legend(title='Bulan', bbox_to_anchor=(1.02, 1), loc='upper left')
plt.tight_layout()
plt.savefig('line_tren_per_bulan.png')
plt.show()
```

### Analisis

`lineplot` dengan `hue='month'` membuat satu garis untuk setiap bulan (12 garis). Dengan cara ini kita bisa melihat dua hal sekaligus: tren dari tahun ke tahun, dan perbedaan jumlah penumpang antarbulan. Palet `tab20` dipakai karena memiliki warna yang cukup berbeda untuk 12 kategori.

Dari pola yang terlihat pada data:

- Semua garis bergerak naik dari tahun 1949 ke 1960, sehingga jumlah penumpang bertumbuh secara konsisten.
- Garis bulan Juli dan Agustus berada paling atas di setiap tahun, yang menunjukkan bahwa musim liburan membuat penumpang paling banyak.
- Garis bulan Februari dan November berada paling bawah, sehingga bulan-bulan tersebut adalah periode sepi.
- Jarak antar garis makin melebar ke kanan. Artinya selisih penumpang antar bulan ikut membesar seiring pertumbuhan jumlah penumpang.

---

## Soal 3: Menganalisis Distribusi dan Pencilan Data

Untuk mengetahui sebaran jumlah penumpang pada setiap bulan, buatlah visualisasi boxplot menggunakan Seaborn dengan ketentuan:

1. Sumbu X menunjukkan bulan (month).
2. Sumbu Y menunjukkan jumlah penumpang (passengers).

Gunakan visualisasi tersebut untuk mengamati distribusi jumlah penumpang serta mengidentifikasi kemungkinan adanya pencilan (outlier).

### Kode

```python
plt.figure(figsize=(12, 6))

sns.boxplot(data=df_flights, x='month', y='passengers', color='lightblue')

plt.title('Distribusi Jumlah Penumpang per Bulan')
plt.xlabel('Bulan')
plt.ylabel('Jumlah Penumpang')
plt.savefig('boxplot_per_bulan.png')
plt.show()
```

### Analisis

Boxplot menampilkan median, kuartil, rentang data, dan titik outlier untuk setiap bulan. Karena data dari beberapa tahun digabung dalam satu bulan, kotak tiap bulan akan mencerminkan pertumbuhan jumlah penumpang sepanjang periode 1949 sampai 1960.

Yang bisa diamati dari grafik:

- Median penumpang pada bulan Juli dan Agustus lebih tinggi dibanding bulan lainnya.
- Kotak setiap bulan cukup lebar, yang menandakan jumlah penumpang sangat bervariasi antar tahun.
- Data bergerak naik secara halus dari tahun ke tahun, sehingga outlier yang sangat ekstrem diperkirakan tidak banyak. Jika ada titik di luar whisker, itu biasanya bulan-bulan puncak di tahun-tahun akhir.

> Catatan: keberadaan outlier perlu dicek langsung di grafik hasil eksekusi.

---

## Soal 4: Mengubah Struktur Data (Reshaping Data)

Sebelum membuat visualisasi heatmap, lakukan transformasi struktur DataFrame flights menggunakan fungsi pivot() dengan ketentuan:

1. Bulan (month) menjadi indeks atau baris.
2. Tahun (year) menjadi kolom.
3. Jumlah penumpang (passengers) menjadi nilai pada setiap sel.

Setelah melakukan transformasi, tampilkan lima baris pertama dari DataFrame hasil transformasi tersebut.

### Kode

```python
# Transformasi: month sebagai baris, year sebagai kolom, passengers sebagai nilai
df_pivot = df_flights.pivot(index='month', columns='year', values='passengers')

# Menampilkan 5 baris pertama
print(df_pivot.head())
```

### Analisis

`pivot()` mengubah format data dari panjang (long) menjadi format matriks. Setiap baris DataFrame asli, yaitu satu kombinasi tahun dan bulan, berubah menjadi satu sel pada tabel baru. Dengan begitu, data siap dipakai untuk heatmap.

Hasil `head()` menampilkan baris pertama yaitu bulan **Jan**, dengan kolom 1949 sampai 1960. Nilai yang diharapkan pada baris Jan adalah sekitar 112 (1949) hingga 417 (1960). Tabel hasil pivot berukuran **12 baris x 12 kolom**, sehingga setiap sel mewakili jumlah penumpang pada satu bulan di satu tahun.

> Catatan: periksa output `head()` untuk memastikan nilainya sesuai.

---

## Soal 5: Membuat Visualisasi Heatmap

Gunakan DataFrame hasil transformasi pada Soal 4 untuk membuat visualisasi heatmap menggunakan Seaborn. Perhatikan ketentuan berikut:

1. Tampilkan nilai numerik pada setiap sel heatmap menggunakan parameter annot.
2. Kunjungi dokumentasi resmi Seaborn pada bagian Choosing color palettes, kemudian pilih dan terapkan salah satu skema warna (colormap) yang tersedia sesuai dengan preferensi visualisasi Anda.
3. Tambahkan judul serta label sumbu agar visualisasi mudah dipahami.
4. Simpan hasil visualisasi ke dalam file bernama heatmap_flights.png.

### Kode

```python
plt.figure(figsize=(14, 7))

# Colormap 'YlGnBu' dipilih dari kategori sequential pada Choosing color palettes
# annot=True menampilkan nilai numerik pada setiap sel, fmt='d' agar berupa bilangan bulat
sns.heatmap(df_pivot, annot=True, fmt='d', cmap='YlGnBu', linewidths=0.5)

plt.title('Heatmap Jumlah Penumpang per Bulan dan Tahun (1949-1960)')
plt.xlabel('Tahun')
plt.ylabel('Bulan')
plt.tight_layout()
plt.savefig('heatmap_flights.png')
plt.show()
```

### Analisis

Heatmap mengubah angka dalam tabel pivot menjadi warna. Sel yang lebih gelap dan lebih kuning-hijau menunjukkan jumlah penumpang yang lebih besar. Parameter `annot=True` menampilkan angka asli di dalam setiap sel, sehingga pembaca bisa melihat nilai pasti tanpa harus menebak dari warna. Colormap `YlGnBu` adalah skema sequential, cocok untuk data angka yang bergerak dari kecil ke besar.

Pola yang terlihat dari heatmap:

- Warna paling gelap terkumpul di **kolom tahun-tahun akhir** (sekitar 1958 sampai 1960), yang menunjukkan pertumbuhan jumlah penumpang dari waktu ke waktu.
- Baris **Juli** dan **Agustus** adalah yang paling gelap di setiap tahun, sehingga itu bulan dengan penumpang terbanyak.
- Nilai tertinggi diperkirakan berada di sel **Juli 1960** (sekitar 622 penumpang), dan nilai terendah berada di awal periode, sekitar 100-an pada bulan-bulan sepi.

> Catatan: pastikan nilai tertinggi dan terendah sesuai dengan output heatmap yang Anda hasilkan.
