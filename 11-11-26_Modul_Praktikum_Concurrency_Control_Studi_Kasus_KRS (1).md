# MODUL PRAKTIKUM

## CONCURRENCY CONTROL (KONTROL KONKURENSI)

*Lanjutan Studi Kasus: Pengambilan KRS (Kartu Rencana Studi)*

**Mata Kuliah** : Teknologi Basis Data

**Dosen Pengampu** : Yoga Aji Sukma, S.Mat., M.Stat.

**Tools Praktikum** : Python (sqlite3) di Google Colab

---

## A. TUJUAN PRAKTIKUM

a. Mahasiswa mampu menjelaskan konsep dasar kontrol konkurensi pada sistem basis data multiuser.

b. Mahasiswa mampu mengidentifikasi permasalahan yang dapat muncul akibat transaksi bersamaan, seperti Lost Update, Dirty Read, Non-Repeatable Read, dan Phantom Read.

c. Mahasiswa mampu menerapkan simulasi dua transaksi konkuren menggunakan Python dan SQLite, serta memahami perbedaan arsitektur konkurensi SQLite dengan MySQL.

---

## B. DASAR TEORI

### 1. Kontrol Konkurensi

Kontrol konkurensi adalah koordinasi pelaksanaan transaksi yang berjalan secara simultan dalam sistem basis data multiuser. Tujuannya adalah memastikan serializability: hasil akhir dari transaksi yang dijalankan bersamaan (konkuren) harus setara dengan hasil jika transaksi-transaksi tersebut dijalankan satu per satu (serial), tanpa konflik atau inkonsistensi data.

Concurrency control penting karena eksekusi simultan dalam sistem multiuser — misalnya banyak mahasiswa atau staf mengakses tabel/baris yang sama pada sistem KRS — dapat menciptakan masalah integritas dan konsistensi data. Tiga masalah utama yang mungkin timbul adalah:

a. Update hilang (Lost Update)

b. Data belum final terbaca (Dirty Read / Uncommitted Data)

c. Pembacaan tidak konsisten (Inconsistent Retrievals)

### 2. Lost Update

Terjadi ketika dua transaksi atau lebih membaca data yang sama, lalu masing-masing memperbarui data tersebut berdasarkan nilai lama yang sama, sehingga pembaruan transaksi yang pertama commit hilang karena ditimpa oleh transaksi berikutnya.

Skenario KRS: Dua staf TU (Akademik dan Keuangan) sama-sama mengoreksi kolom terisi pada kelas yang sama. Keduanya membaca nilai lama yang sama, menghitung koreksi masing-masing di aplikasi mereka, lalu menyimpan hasilnya secara terpisah — bukan dengan pola UPDATE atomic (terisi = terisi + n) seperti yang diajarkan pada Pertemuan 9. Akibatnya, koreksi salah satu staf hilang total, tertimpa oleh commit staf yang datang belakangan.

### 3. Dirty Read (Uncommitted Data)

Terjadi ketika suatu transaksi membaca data yang belum di-commit oleh transaksi lain. Jika transaksi tersebut kemudian melakukan rollback, data yang sudah sempat terbaca menjadi tidak pernah valid.

Skenario KRS: Staf TU sedang menguji perubahan nama mata kuliah (belum commit), sementara dashboard akademik sempat membaca nama yang sedang diuji tersebut. Staf TU kemudian membatalkan perubahan (rollback) karena kurikulum belum disetujui — dashboard sudah terlanjur menampilkan data yang tidak pernah benar-benar berlaku.

### 4. Non-Repeatable Read & Phantom Read

Non-Repeatable Read terjadi ketika satu transaksi membaca baris data yang sama dua kali, tetapi mendapat hasil berbeda karena transaksi lain mengubah dan meng-commit baris tersebut di antara kedua pembacaan.

Phantom Read terjadi ketika satu transaksi membaca sekumpulan baris berdasarkan kondisi tertentu, lalu transaksi lain menambah/menghapus baris yang memenuhi kondisi itu, sehingga pembacaan ulang menghasilkan jumlah baris yang berbeda.

Tabel berikut merangkum anomali apa saja yang diizinkan pada tiap isolation level standar SQL:

| **Isolation Level** | **Dirty Read** | **Non-Repeatable Read** | **Phantom Read** |
| --- | --- | --- | --- |
| Read Uncommitted | Mungkin terjadi | Mungkin terjadi | Mungkin terjadi |
| Read Committed | Dicegah | Mungkin terjadi | Mungkin terjadi |
| Repeatable Read | Dicegah | Dicegah | Mungkin terjadi |
| Serializable | Dicegah | Dicegah | Dicegah |

**Catatan: SQLite vs MySQL**

MySQL (InnoDB) mendukung perintah SET TRANSACTION ISOLATION LEVEL yang dapat diatur dari Read Uncommitted hingga Serializable, menggunakan row-level locking, sehingga keempat anomali di atas BISA direproduksi tergantung level yang dipilih.

*SQLite TIDAK mendukung SET TRANSACTION ISOLATION LEVEL. SQLite menggunakan locking di level file (mode rollback-journal) atau snapshot isolation (mode WAL). Akibatnya, Non-Repeatable Read dan Phantom Read otomatis DICEGAH oleh SQLite sendiri — tidak dapat direproduksi walau dicoba. Hanya Lost Update dan Dirty Read (dengan trik shared-cache tertentu) yang dapat dipraktikkan dan dibuktikan langsung pada modul ini.*

---

## C. LATIHAN PRAKTIKUM

*Catatan Tools: seluruh praktikum ini dikerjakan di Google Colab menggunakan modul bawaan Python, sqlite3. Dua staf yang bekerja bersamaan disimulasikan dengan DUA KONEKSI sqlite3 berbeda yang menunjuk ke file database fisik yang sama (bukan database in-memory seperti pada Pertemuan 9) — ini wajib, agar kedua koneksi bisa saling melihat perubahan satu sama lain.*

### Skenario 1: Lost Update

Setelah migrasi dari sistem akademik lama, data terisi pada kelas Teknologi Basis Data perlu dikoreksi manual oleh staf TU.

- Siapkan database file dan data awal.

```python
import sqlite3
import pandas as pd
import os

DB_FILE = 'krs_concurrency.db'
if os.path.exists(DB_FILE):
    os.remove(DB_FILE)

conn_setup = sqlite3.connect(DB_FILE)
conn_setup.execute('''
CREATE TABLE kelas (
    id_kelas INTEGER PRIMARY KEY,
    nama_matkul TEXT,
    kuota INTEGER,
    terisi INTEGER
)
''')
conn_setup.execute("INSERT INTO kelas VALUES (1, 'Teknologi Basis Data', 40, 25)")
conn_setup.commit()
conn_setup.close()

def tampilkan(sql, koneksi, params=()):
    return pd.read_sql_query(sql, koneksi, params=params)
```

- Buka dua koneksi terpisah ke file yang sama, masing-masing mewakili satu staf TU.

```python
akademik = sqlite3.connect(DB_FILE)
keuangan = sqlite3.connect(DB_FILE)
```

- Kedua staf membaca nilai lama yang sama, SEBELUM siapa pun menulis.

```python
terisi_akademik = akademik.execute(
    "SELECT terisi FROM kelas WHERE id_kelas = 1").fetchone()[0]
terisi_keuangan = keuangan.execute(
    "SELECT terisi FROM kelas WHERE id_kelas = 1").fetchone()[0]

print('Staf TU Akademik membaca terisi =', terisi_akademik)
print('Staf TU Keuangan membaca terisi =', terisi_keuangan)
```

***Hasil:***

```
Staf TU Akademik membaca terisi = 25
Staf TU Keuangan membaca terisi = 25
```

- Staf TU Akademik menghitung koreksinya sendiri (+3 dari rekap WhatsApp) lalu commit.

```python
nilai_baru_akademik = terisi_akademik + 3
akademik.execute("UPDATE kelas SET terisi = ? WHERE id_kelas = 1",
                 (nilai_baru_akademik,))
akademik.commit()
tampilkan("SELECT * FROM kelas WHERE id_kelas = 1", akademik)
```

***Hasil:***

| **id_kelas** | **nama_matkul** | **kuota** | **terisi** |
| --- | --- | --- | --- |
| 1 | Teknologi Basis Data | 40 | 28 |

- Staf TU Keuangan menghitung berdasarkan nilai LAMA (25) yang ia baca sebelumnya (+5 dari rekap pembayaran), lalu commit.

```python
nilai_baru_keuangan = terisi_keuangan + 5
keuangan.execute("UPDATE kelas SET terisi = ? WHERE id_kelas = 1",
                 (nilai_baru_keuangan,))
keuangan.commit()
tampilkan("SELECT * FROM kelas WHERE id_kelas = 1", keuangan)
```

***Hasil:***

| **id_kelas** | **nama_matkul** | **kuota** | **terisi** |
| --- | --- | --- | --- |
| 1 | Teknologi Basis Data | 40 | 30 |

*Hasil akhir di database: terisi = 30, padahal seharusnya 25 + 3 + 5 = 33. Koreksi Staf TU Akademik (28) hilang total — tertimpa commit Staf TU Keuangan yang datang belakangan. Inilah Lost Update.*

*Solusi: gunakan pola UPDATE atomic (UPDATE kelas SET terisi = terisi + 3) seperti pada Pertemuan 9, bukan baca-hitung-tulis manual seperti di atas.*

### Skenario 2: Dirty Read

Staf TU menguji perubahan nama mata kuliah menjadi 'Teknologi Basis Data Lanjut', belum yakin sehingga belum commit. Dashboard akademik membaca nama tersebut di waktu yang sama.

- Buktikan dulu: secara default, SQLite TIDAK mengizinkan dirty read antar koneksi.

```python
staf_tu = sqlite3.connect(DB_FILE)
dashboard_default = sqlite3.connect(DB_FILE)

staf_tu.execute("BEGIN")
staf_tu.execute("UPDATE kelas SET nama_matkul = "
    "'Teknologi Basis Data Lanjut' WHERE id_kelas = 1")

tampilkan("SELECT nama_matkul FROM kelas WHERE id_kelas = 1",
          dashboard_default)
```

***Hasil (dashboard tetap melihat data lama/committed):***

| **nama_matkul** |
| --- |
| Teknologi Basis Data |

*staf_tu.rollback() lalu tutup kedua koneksi sebelum lanjut.*

- Untuk mereproduksi dirty read, gunakan shared cache + PRAGMA read_uncommitted pada sisi pembaca.

```python
staf_tu = sqlite3.connect(f'file:{DB_FILE}?cache=shared', uri=True)
dashboard = sqlite3.connect(f'file:{DB_FILE}?cache=shared', uri=True)
dashboard.execute('PRAGMA read_uncommitted = 1')

staf_tu.execute("BEGIN")
staf_tu.execute("UPDATE kelas SET nama_matkul = "
    "'Teknologi Basis Data Lanjut' WHERE id_kelas = 1")

nama_terbaca = dashboard.execute(
    "SELECT nama_matkul FROM kelas WHERE id_kelas = 1").fetchone()[0]
print('Dashboard membaca:', nama_terbaca)
```

***Hasil:***

```
Dashboard membaca: Teknologi Basis Data Lanjut
```

- Staf TU membatalkan perubahan (kurikulum belum disetujui), lalu periksa hasil akhir.

```python
staf_tu.rollback()
nama_final = dashboard.execute(
    "SELECT nama_matkul FROM kelas WHERE id_kelas = 1").fetchone()[0]
print('Nama matkul final di database :', nama_final)
print('Nama yang sempat dibaca dashboard:', nama_terbaca)
```

***Hasil:***

```
Nama matkul final di database : Teknologi Basis Data
Nama yang sempat dibaca dashboard: Teknologi Basis Data Lanjut
```

*Dashboard sempat menampilkan nama mata kuliah yang tidak pernah benar-benar berlaku, karena perubahan itu akhirnya dibatalkan. Inilah Dirty Read.*

### Non-Repeatable Read & Phantom Read: Mengapa Tidak Dipraktikkan?

Kedua anomali ini sengaja tidak dipraktikkan dengan kode karena SQLite, secara arsitektur, mencegahnya secara otomatis:

| **Mode SQLite** | **Perilaku saat transaksi lain menulis di tengah transaksi pembaca** |
| --- | --- |
| Rollback-journal (default) | Penulis diblokir (database is locked) selama transaksi pembaca masih terbuka |
| WAL (journal_mode=WAL) | Penulis tetap bisa commit, tapi pembaca tetap melihat snapshot stabil sampai transaksinya sendiri selesai |

Pada kedua mode tersebut, pembacaan kedua dalam satu transaksi akan selalu konsisten dengan pembacaan pertama — berbeda dengan MySQL yang mengizinkan anomali ini terjadi jika isolation level diatur ke Read Committed atau lebih rendah. Lihat materi kuliah untuk penjelasan lengkap.


---

## D. SOAL LATIHAN MANDIRI

Kerjakan soal-soal berikut pada notebook praktikum.

---

### Soal 1

> Ulangi simulasi Lost Update di atas untuk kelas baru 'Pemrograman Web' (kuota 30, terisi 10). Staf TU Akademik menambah 2 mahasiswa, Staf TU Keuangan menambah 4 mahasiswa pada saat hampir bersamaan. Buktikan update siapa yang hilang.

**Jawaban:**

Siapkan tabel dan data awal (kelas Pemrograman Web: kuota 30, terisi 10), lalu buka dua koneksi yang menunjuk ke file database yang sama.

```python
import sqlite3
import pandas as pd
import os

DB_FILE = 'krs_concurrency.db'
if os.path.exists(DB_FILE):
    os.remove(DB_FILE)

conn_setup = sqlite3.connect(DB_FILE)
conn_setup.execute('''
CREATE TABLE kelas (
    id_kelas INTEGER PRIMARY KEY,
    nama_matkul TEXT,
    kuota INTEGER,
    terisi INTEGER
)
''')
conn_setup.execute("INSERT INTO kelas VALUES (2, 'Pemrograman Web', 30, 10)")
conn_setup.commit()
conn_setup.close()

def tampilkan(sql, koneksi, params=()):
    return pd.read_sql_query(sql, koneksi, params=params)

akademik = sqlite3.connect(DB_FILE)
keuangan = sqlite3.connect(DB_FILE)
```

Kedua staf membaca nilai lama yang sama sebelum siapa pun menulis.

```python
terisi_akademik = akademik.execute(
    "SELECT terisi FROM kelas WHERE id_kelas = 2").fetchone()[0]
terisi_keuangan = keuangan.execute(
    "SELECT terisi FROM kelas WHERE id_kelas = 2").fetchone()[0]

print('Staf TU Akademik membaca terisi =', terisi_akademik)
print('Staf TU Keuangan membaca terisi =', terisi_keuangan)
```

***Hasil:***

```
Staf TU Akademik membaca terisi = 10
Staf TU Keuangan membaca terisi = 10
```

Staf TU Akademik menambah 2 mahasiswa lalu commit.

```python
nilai_baru_akademik = terisi_akademik + 2
akademik.execute("UPDATE kelas SET terisi = ? WHERE id_kelas = 2",
                 (nilai_baru_akademik,))
akademik.commit()
tampilkan("SELECT * FROM kelas WHERE id_kelas = 2", akademik)
```

***Hasil:***

| **id_kelas** | **nama_matkul** | **kuota** | **terisi** |
| --- | --- | --- | --- |
| 2 | Pemrograman Web | 30 | 12 |

Staf TU Keuangan menambah 4 mahasiswa berdasarkan nilai LAMA (10), lalu commit.

```python
nilai_baru_keuangan = terisi_keuangan + 4
keuangan.execute("UPDATE kelas SET terisi = ? WHERE id_kelas = 2",
                 (nilai_baru_keuangan,))
keuangan.commit()
tampilkan("SELECT * FROM kelas WHERE id_kelas = 2", keuangan)
```

***Hasil:***

| **id_kelas** | **nama_matkul** | **kuota** | **terisi** |
| --- | --- | --- | --- |
| 2 | Pemrograman Web | 30 | 14 |

**Kesimpulan:** Nilai akhir `terisi = 14`, padahal seharusnya 10 + 2 + 4 = 16. Update yang hilang adalah koreksi Staf TU Akademik (+2).

**Analisis:** Lost Update terjadi karena kedua staf menerapkan pola baca-hitung-tulis di aplikasi masing-masing, sehingga keduanya menghitung dari salinan nilai lama (10) dan menulis hasilnya sebagai nilai absolut. Database tidak mengetahui bahwa nilai 12 yang sudah di-commit Akademik seharusnya menjadi dasar perhitungan Keuangan, sehingga commit kedua menimpa commit pertama tanpa peringatan atau error. Dampaknya pada sistem KRS cukup serius, karena jumlah mahasiswa terisi menjadi lebih kecil dari kenyataan sehingga kuota kelas bisa terlampaui tanpa disadari.

---

### Soal 2

> Ulangi simulasi Lost Update, tetapi kali ini Staf TU Akademik melakukan commit() SETELAH Staf TU Keuangan. Apakah yang hilang berubah jadi koreksi Staf TU Keuangan? Jelaskan mengapa.

**Jawaban:**

Ulangi persiapan dan pembacaan nilai lama seperti Soal 1 (kedua staf sama-sama membaca `terisi = 10`), lalu urutan commit dibalik: Keuangan lebih dulu, Akademik belakangan.

```python
# Staf TU Keuangan commit lebih dulu (+4)
keuangan.execute("UPDATE kelas SET terisi = ? WHERE id_kelas = 2",
                 (terisi_keuangan + 4,))
keuangan.commit()
print(tampilkan("SELECT * FROM kelas WHERE id_kelas = 2", keuangan))

# Staf TU Akademik commit belakangan (+2), berdasarkan nilai lama (10)
akademik.execute("UPDATE kelas SET terisi = ? WHERE id_kelas = 2",
                 (terisi_akademik + 2,))
akademik.commit()
print(tampilkan("SELECT * FROM kelas WHERE id_kelas = 2", akademik))
```

***Hasil:***

Setelah Keuangan commit:

| **id_kelas** | **nama_matkul** | **kuota** | **terisi** |
| --- | --- | --- | --- |
| 2 | Pemrograman Web | 30 | 14 |

Setelah Akademik commit:

| **id_kelas** | **nama_matkul** | **kuota** | **terisi** |
| --- | --- | --- | --- |
| 2 | Pemrograman Web | 30 | 12 |

**Kesimpulan:** Ya, yang hilang berubah menjadi koreksi Staf TU Keuangan (+4). Nilai akhir menjadi 12, bukan 16.

**Analisis:** Penyebabnya adalah pola *last write wins*: pada pembaruan dengan nilai absolut, yang bertahan di database hanyalah tulisan dari commit terakhir, apa pun urutan pembacaannya. Karena kedua staf menghitung dari nilai lama yang sama (10), commit yang datang belakangan selalu menghapus efek commit sebelumnya. Dengan demikian, siapa yang kehilangan update ditentukan oleh urutan commit, bukan oleh siapa yang lebih dulu membaca, dan hasil akhirnya tidak dapat diprediksi dalam sistem yang sebenarnya.

---

### Soal 3

> Perbaiki skenario Lost Update pada soal 1 dengan memakai pola atomic UPDATE kelas SET terisi = terisi + 2 (atau + 4) seperti yang diajarkan di Pertemuan 9, alih-alih baca-hitung-tulis manual. Buktikan bahwa dengan pola ini, kedua koreksi staf TU tetap tersimpan (tidak ada yang hilang).

**Jawaban:**

Atur ulang data awal (Pemrograman Web: terisi 10), lalu masing-masing staf langsung menjalankan `UPDATE ... SET terisi = terisi + n` tanpa membaca nilai lama ke Python terlebih dahulu.

```python
akademik = sqlite3.connect(DB_FILE)
keuangan = sqlite3.connect(DB_FILE)

# Staf TU Akademik menambah 2 mahasiswa
akademik.execute("UPDATE kelas SET terisi = terisi + 2 WHERE id_kelas = 2")
akademik.commit()

# Staf TU Keuangan menambah 4 mahasiswa
keuangan.execute("UPDATE kelas SET terisi = terisi + 4 WHERE id_kelas = 2")
keuangan.commit()

tampilkan("SELECT * FROM kelas WHERE id_kelas = 2", keuangan)
```

***Hasil:***

| **id_kelas** | **nama_matkul** | **kuota** | **terisi** |
| --- | --- | --- | --- |
| 2 | Pemrograman Web | 30 | 16 |

**Pembuktian:** Nilai akhir `terisi = 16 = 10 + 2 + 4`, sehingga kedua koreksi staf TU tetap tersimpan dan tidak ada yang hilang.

**Analisis:** Pola atomic berhasil karena penambahan dihitung oleh database di dalam satu pernyataan terhadap nilai terbaru yang sudah di-commit, bukan terhadap salinan nilai lama di memori aplikasi. Dengan begitu, hasilnya sama untuk urutan commit apa pun (Akademik dulu atau Keuangan dulu). Pada SQLite, jika satu staf belum commit saat staf lain menjalankan UPDATE, staf kedua akan menunggu atau mendapat error `database is locked`, sehingga pembaruan tetap berjalan bergantian dan tidak saling menimpa.

---

### Soal 4

> Jelaskan dengan kata-kata sendiri: kenapa SQLite tidak membutuhkan SET TRANSACTION ISOLATION LEVEL seperti MySQL untuk mencegah Non-Repeatable Read, padahal secara default MySQL (Repeatable Read) pun sebenarnya sudah mencegahnya?

**Jawaban:**

MySQL (InnoDB) memakai *row-level locking* dan *multi-version concurrency control*, sehingga banyak transaksi boleh membaca dan menulis baris yang berbeda secara bersamaan. Karena itu MySQL menyediakan beberapa isolation level agar pengguna bisa memilih keseimbangan antara konsistensi dan kinerja. Pada level yang lebih rendah (misalnya Read Committed), Non-Repeatable Read dan Phantom Read memang bisa terjadi. Pada default MySQL, yaitu Repeatable Read, Non-Repeatable Read sudah dicegah, tetapi pengguna tetap bebas menaikkan atau menurunkan level tersebut.

SQLite berbeda secara arsitektur. Ia hanya mengizinkan satu penulis pada satu waktu untuk seluruh file database dan tidak menyediakan pengaturan isolation level yang bisa dipilih. Pada mode rollback-journal (default), penulis diblokir selama transaksi pembaca masih terbuka, sehingga data tidak mungkin berubah di tengah transaksi pembaca. Pada mode WAL, penulis boleh commit, tetapi pembaca tetap melihat snapshot stabil sampai transaksinya selesai.

**Analisis:** Perbedaan utamanya terletak pada filosofi desain: MySQL dirancang untuk konkurensi tinggi dengan isolation level yang dapat dikonfigurasi, sedangkan SQLite dirancang sederhana dengan perlindungan konsistensi yang sudah tertanam dalam mekanisme penguncian file atau snapshot-nya. Pada kedua mode SQLite, pembacaan kedua dalam satu transaksi selalu konsisten dengan pembacaan pertama, sehingga Non-Repeatable Read dan Phantom Read otomatis tidak dapat terjadi (setara dengan perilaku Serializable). Akibatnya, perintah `SET TRANSACTION ISOLATION LEVEL` tidak diperlukan, dan harga yang dibayar adalah kemampuan menangani banyak penulis bersamaan yang lebih terbatas dibanding MySQL.
