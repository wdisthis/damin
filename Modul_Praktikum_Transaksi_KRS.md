**MODUL PRAKTIKUM**

**TRANSAKSI BASIS DATA**

*Studi Kasus: Pengambilan KRS (Kartu Rencana Studi)*

**Mata Kuliah** : Teknologi Basis Data

**Dosen Pengampu** : Yoga Aji Sukma, S.Mat., M.Stat.

**Tools Praktikum** : Python (sqlite3) di Google Colab

**A. TUJUAN**

> a. Memahami konsep dan urgensi transaksi basis data dalam kehidupan sehari-hari, khususnya pada proses pengambilan KRS (Kartu Rencana Studi).
>
> b. Mampu mengimplementasikan transaksi basis data (COMMIT, ROLLBACK, SAVEPOINT) menggunakan Python dan SQLite.
>
> c. Mampu menyelesaikan operasi-operasi sensitif (rebutan kuota kelas) dengan memanfaatkan transaksi basis data.

**B. DASAR TEORI**

**1. Transaksi Basis Data**

Transaksi merupakan serangkaian kelompok dari operasi manipulasi database yang dilakukan seolah-olah sebagai satu unit kerja secara individu.

Contoh kegiatan bertransaksi yang perlu diperhatikan adalah proses pengambilan KRS. Misalkan seorang mahasiswa mengambil sebuah mata kuliah yang kuotanya tersisa 1 kursi. Maka dapat diilustrasikan secara sederhana proses yang terjadi adalah sebagai berikut:

> a. Baris baru ditambahkan ke tabel krs yang menandakan mahasiswa tersebut terdaftar pada mata kuliah tersebut.
>
> b. Jumlah terisi pada tabel kelas bertambah 1.

Jika di antara langkah (a) dan (b) terjadi hal-hal buruk yang tidak diinginkan — misalnya koneksi internet terputus, server basis data mengalami gangguan, atau sistem tiba-tiba restart — maka dapat dipastikan bahwa mahasiswa tersebut tercatat “mengambil” mata kuliah, tetapi kuota kelas tidak pernah berkurang. Akibatnya, kelas berisiko mengalami overbooking tanpa disadari oleh sistem.

Dari ilustrasi di atas, dapat disimpulkan bahwa solusi yang tepat adalah memperlakukan perintah-perintah tersebut sebagai satu kesatuan operasi. Sederhananya, lakukan semua operasi atau tidak sama sekali. Konsep inilah yang disebut transaksi basis data (database transaction), yang secara sederhana terdiri dari:

> a. Tandai bagian awal dan akhir himpunan perintah,
>
> b. Putuskan di bagian akhir untuk mengeksekusi (COMMIT) atau membatalkan (ROLLBACK) semua perintah.

**2. Properti Transaksi Basis Data**

Dalam transaksi basis data, terdapat properti-properti yang menjamin bahwa transaksi dilaksanakan dengan baik. Properti-properti ini dikenal sebagai ACID (Atomicity, Consistency, Isolation, Durability).

> a. **Atomicity** — Transaksi dilakukan sekali dan sifatnya atomic, artinya merupakan satu kesatuan tunggal yang tidak dapat dipisah, baik itu pekerjaan yang dilaksanakan secara keseluruhan, ataupun tidak satu pun — biasa dikenal dengan jargon “all or nothing”. Pada kasus KRS: insert baris krs dan update kuota kelas harus berhasil berdua, atau batal berdua.
>
> b. **Consistency** — Jika basis data pada awalnya dalam keadaan konsisten, maka pelaksanaan transaksi dengan sendirinya juga harus meninggalkan basis data tetap dalam status konsisten. Pada kasus KRS, hal ini dijaga melalui batasan (constraint) bahwa jumlah mahasiswa yang terisi tidak boleh melebihi kuota kelas.
>
> c. **Isolation** — Isolasi memastikan bahwa secara bersamaan (konkuren), eksekusi transaksi terisolasi dari transaksi lainnya. Properti ini penting ketika banyak mahasiswa mengakses sistem KRS secara bersamaan pada saat jam pembukaan KRS.
>
> d. **Durability** — Begitu transaksi telah dilaksanakan (di-commit), maka perubahan yang diakibatkan tidak akan hilang atau bersifat tahan lama (durable), sekalipun terjadi kegagalan sistem setelahnya.

**3. Penanganan Kesalahan**

Fasilitas penanganan kesalahan (error handling) diperlukan untuk mengantisipasi terjadinya kesalahan pada suatu proses transaksi, sehingga programmer bisa mengatur skenario jika suatu operasi gagal sebagian atau seluruhnya.

Pada modul sqlite3 di Python, secara default setiap transaksi berjalan dalam mode manual (bukan autocommit) untuk perintah DML (INSERT/UPDATE/DELETE): perubahan baru tersimpan secara permanen setelah conn.commit() dipanggil. Sebaliknya, mode AUTOCOMMIT (isolation_level=None) membuat setiap perintah langsung tersimpan permanen begitu dieksekusi, sehingga rollback() tidak lagi memiliki efek apa pun.

Pemilihan mode yang tepat sangat penting: untuk operasi sensitif seperti pengambilan KRS, mode manual lebih disarankan karena memberi kesempatan untuk membatalkan (ROLLBACK) seluruh transaksi jika salah satu langkah gagal.

**C. LATIHAN PRAKTIKUM**

Sebelum masuk ke skenario, pahami definisi berikut:

> a. **COMMIT** — menyelesaikan transaksi dan menyimpan semua perubahan yang dilakukan selama transaksi tersebut secara permanen ke dalam database.
>
> b. **ROLLBACK** — membatalkan semua perubahan yang dilakukan selama sebuah transaksi; database kembali ke keadaan semula seperti sebelum transaksi dimulai.
>
> c. **SAVEPOINT** — titik sementara yang dapat dibuat di dalam sebuah transaksi (mirip “pos pemeriksaan”), sehingga rollback dapat dilakukan sebagian hingga ke titik tertentu, tanpa membatalkan seluruh transaksi.

*Catatan Tools: seluruh praktikum ini dikerjakan di Google Colab menggunakan modul bawaan Python, sqlite3, tanpa perlu instalasi server database maupun aplikasi tambahan seperti XAMPP/PhpMyAdmin.*

**Skenario: Pengambilan KRS (Kartu Rencana Studi)**

Skenario ini menggunakan proses pengambilan KRS untuk menunjukkan bagaimana transaksi memastikan bahwa semua operasi berhasil atau tidak sama sekali (properti Atomicity dalam ACID).

1.  Siapkan basis data. Buat koneksi SQLite in-memory, lalu buat dua tabel: kelas (daftar mata kuliah beserta kuota dan jumlah terisi) dan krs (catatan pengambilan KRS tiap mahasiswa). Constraint CHECK (terisi <= kuota) memaksa basis data tetap Consistent.

```python
import sqlite3
import pandas as pd
from IPython.display import display

conn = sqlite3.connect(':memory:')
cur = conn.cursor()

def tampilkan(sql, params=(), koneksi=None):
    return pd.read_sql_query(sql, koneksi or conn, params=params)

cur.execute('''
CREATE TABLE kelas (
    id_kelas INTEGER PRIMARY KEY,
    nama_matkul TEXT,
    kuota INTEGER,
    terisi INTEGER DEFAULT 0,
    CHECK (terisi <= kuota)
)
''')

cur.execute('''
CREATE TABLE krs (
    id_krs INTEGER PRIMARY KEY AUTOINCREMENT,
    nim TEXT,
    id_kelas INTEGER,
    status TEXT
)
''')

conn.commit()
print('Tabel kelas dan krs berhasil dibuat.')
```

***Hasil:***

|                                      |
|--------------------------------------|
| Tabel kelas dan krs berhasil dibuat. |

2.  Masukkan data awal kelas. Kuota dibuat kecil agar skenario “kuota penuh” mudah didemonstrasikan.

```python
cur.execute("INSERT INTO kelas (id_kelas, nama_matkul, kuota, terisi) VALUES (1, 'Teknologi Basis Data', 2, 1)")
cur.execute("INSERT INTO kelas (id_kelas, nama_matkul, kuota, terisi) VALUES (2, 'Struktur Data', 3, 1)")
conn.commit()

print('Data kelas awal berhasil ditambahkan.')
```

3.  Periksa data awal.

```python
tampilkan("SELECT * FROM kelas")
```

***Hasil:***

| **id_kelas** | **nama_matkul**      | **kuota** | **terisi** |
|--------------|----------------------|-----------|------------|
| 1            | Teknologi Basis Data | 2         | 1          |
| 2            | Struktur Data        | 3         | 1          |

*Kelas Teknologi Basis Data: kuota 2, terisi 1 — tersisa 1 kursi.*

4.  Memulai transaksi manual. Berbeda dengan MySQL yang membutuhkan perintah START TRANSACTION, modul sqlite3 di Python secara default sudah menonaktifkan autocommit untuk perintah DML. Transaksi otomatis dimulai begitu perintah pertama dijalankan, dan baru tersimpan permanen setelah conn.commit() dipanggil.

5.  Ambil KRS yang sukses (COMMIT). Mahasiswa dengan NIM 121140001 mengambil kelas Teknologi Basis Data (id_kelas=1, masih tersisa 1 kursi). Dua operasi harus berhasil bersamaan (properti Atomicity): (1) insert baris baru ke tabel krs, dan (2) update terisi = terisi + 1 di tabel kelas.

```python
try:
    cur.execute("INSERT INTO krs (nim, id_kelas, status) VALUES ('121140001', 1, 'diambil')")
    cur.execute("UPDATE kelas SET terisi = terisi + 1 WHERE id_kelas = 1")
    conn.commit()
    print('Transaksi berhasil, KRS tersimpan permanen.')
except sqlite3.Error as e:
    conn.rollback()
    print('Transaksi dibatalkan:', e)

tampilkan("SELECT * FROM kelas WHERE id_kelas = 1")
```

***Hasil:***

| **id_kelas** | **nama_matkul**      | **kuota** | **terisi** |
|--------------|----------------------|-----------|------------|
| 1            | Teknologi Basis Data | 2         | 2          |

*Kuota sekarang penuh (terisi == kuota).*

6.  Ambil KRS yang gagal — kuota penuh (ROLLBACK). Mahasiswa 121140002 mencoba mengambil kelas yang sama. Karena kuota sudah penuh, perintah UPDATE akan melanggar constraint CHECK (terisi <= kuota) dan SQLite akan menolaknya. Baris krs yang sudah ter-insert tidak boleh ikut tersimpan kalau update kuota gagal — ROLLBACK membatalkan keduanya sekaligus.

```python
try:
    cur.execute("INSERT INTO krs (nim, id_kelas, status) VALUES ('121140002', 1, 'diambil')")
    cur.execute("UPDATE kelas SET terisi = terisi + 1 WHERE id_kelas = 1")
    conn.commit()
    print('Transaksi berhasil.')
except sqlite3.Error as e:
    conn.rollback()
    print('Transaksi dibatalkan (ROLLBACK):', e)

display(tampilkan("SELECT * FROM kelas WHERE id_kelas = 1"))
display(tampilkan("SELECT * FROM krs WHERE nim = '121140002'"))
```

***Hasil — tabel kelas (tetap, tidak berubah):***

| **id_kelas** | **nama_matkul**      | **kuota** | **terisi** |
|--------------|----------------------|-----------|------------|
| 1            | Teknologi Basis Data | 2         | 2          |

***Hasil — tabel krs untuk NIM 121140002 (kosong):***

| **id_krs** | **nim** | **id_kelas** | **status** |
|------------|---------|--------------|------------|

*Baris krs yang sempat ter-insert ikut dibatalkan bersama update kuota, meskipun insert itu sendiri “berhasil” sebelum error terjadi. Ini adalah inti dari Atomicity: all-or-nothing.*

7.  SAVEPOINT — rollback sebagian. Mahasiswa 121140003 mengambil 2 kelas dalam satu kali transaksi KRS: Struktur Data (id_kelas=2, masih ada kuota — harus tetap tersimpan) dan Teknologi Basis Data (id_kelas=1, kuota sudah penuh — harus dibatalkan). Tanpa SAVEPOINT, satu kegagalan akan membatalkan seluruh transaksi. Dengan SAVEPOINT, hanya bagian yang gagal yang dibatalkan.

```python
# Operasi 1: ambil Struktur Data (akan tetap disimpan)
cur.execute("INSERT INTO krs (nim, id_kelas, status) VALUES ('121140003', 2, 'diambil')")
cur.execute("UPDATE kelas SET terisi = terisi + 1 WHERE id_kelas = 2")

cur.execute('SAVEPOINT setelah_ambil_struktur_data')

# Operasi 2: coba ambil Teknologi Basis Data (akan dibatalkan, kuota penuh)
try:
    cur.execute("INSERT INTO krs (nim, id_kelas, status) VALUES ('121140003', 1, 'diambil')")
    cur.execute("UPDATE kelas SET terisi = terisi + 1 WHERE id_kelas = 1")
except sqlite3.Error as e:
    cur.execute('ROLLBACK TO SAVEPOINT setelah_ambil_struktur_data')
    print('Operasi 2 dibatalkan (rollback ke savepoint):', e)

conn.commit()

display(tampilkan("SELECT * FROM krs WHERE nim = '121140003'"))
display(tampilkan("SELECT * FROM kelas"))
```

***Hasil — tabel krs untuk NIM 121140003:***

| **id_krs** | **nim**   | **id_kelas** | **status** |
|------------|-----------|--------------|------------|
| 2          | 121140003 | 2            | diambil    |

***Hasil — tabel kelas setelah SAVEPOINT:***

| **id_kelas** | **nama_matkul**      | **kuota** | **terisi** |
|--------------|----------------------|-----------|------------|
| 1            | Teknologi Basis Data | 2         | 2          |
| 2            | Struktur Data        | 3         | 2          |

*Hanya 1 baris tersimpan untuk NIM 121140003 (Struktur Data). Kelas Teknologi Basis Data tidak pernah tercatat karena dibatalkan ke SAVEPOINT.*

8.  AUTOCOMMIT vs Manual. Parameter isolation_level saat membuka koneksi menentukan perilaku ini: isolation_level=None berarti mode AUTOCOMMIT (tiap statement langsung permanen, rollback() tidak berefek), sedangkan isolation_level='' (default) berarti mode manual (perubahan baru permanen setelah commit() dipanggil).

***Mode AUTOCOMMIT:***

```python
conn_auto = sqlite3.connect(':memory:', isolation_level=None)
cur_auto = conn_auto.cursor()
cur_auto.execute('CREATE TABLE contoh (nilai INTEGER)')
cur_auto.execute('INSERT INTO contoh VALUES (100)')

conn_auto.rollback()  # tidak akan berefek karena sudah autocommit

tampilkan('SELECT * FROM contoh', koneksi=conn_auto)
```

| **nilai** |
|-----------|
| 100       |

***Mode Manual (default):***

```python
conn_manual = sqlite3.connect(':memory:')
cur_manual = conn_manual.cursor()
cur_manual.execute('CREATE TABLE contoh (nilai INTEGER)')
conn_manual.commit()

cur_manual.execute('INSERT INTO contoh VALUES (100)')
conn_manual.rollback()  # akan berefek karena belum di-commit

tampilkan('SELECT * FROM contoh', koneksi=conn_manual)
```

| **nilai** |
|-----------|

*Pada mode AUTOCOMMIT, data tetap ada meskipun rollback() dipanggil. Pada mode manual, data hilang setelah rollback() karena belum sempat di-commit.*

Tutup koneksi setelah selesai:

```python
conn.close()
conn_auto.close()
conn_manual.close()
print('Semua koneksi ditutup.')
```

**D. SOAL LATIHAN MANDIRI**

Kerjakan soal-soal berikut pada notebook praktikum, lanjutan dari kondisi akhir skenario di atas.

1.  Tambahkan kelas baru 'Pemrograman Web' dengan kuota 1, terisi 0. Buat transaksi yang menambahkan 1 mahasiswa ke kelas tersebut, lalu buktikan dengan SELECT bahwa kuota sekarang penuh.

2.  Buat skenario dua mahasiswa mencoba mengambil kelas 'Pemrograman Web' yang kuotanya sudah penuh dari soal nomor 1. Tunjukkan bahwa mahasiswa kedua gagal dan tidak ada baris krs yang tersisa untuknya.

3.  Buat transaksi yang mencoba mengambil 3 kelas sekaligus untuk satu NIM, di mana kelas ke-2 pasti gagal (kuota penuh). Gunakan SAVEPOINT supaya kelas ke-1 dan ke-3 tetap tersimpan, hanya kelas ke-2 yang dibatalkan.

4.  Jelaskan dengan kata-kata sendiri: pada soal nomor 2, properti ACID mana yang paling berperan, dan mengapa?
