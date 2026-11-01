# Latihan: Membangun Decision Tree dari Data PlayTennis

Nama : ______________________________
NIM : ______________________________
Mata Kuliah : Data Mining (SD25-31001)
Topik : Supervised Learning (Classification), Decision Tree

---

## 1. Data Latih

Dataset **PlayTennis** berisi 14 data latih dengan 4 atribut dan 1 label (class).

| Day | Outlook | Temperature | Humidity | Wind | PlayTennis |
|:---:|:---:|:---:|:---:|:---:|:---:|
| D1 | Sunny | Hot | High | Weak | No |
| D2 | Sunny | Hot | High | Strong | No |
| D3 | Overcast | Hot | High | Weak | Yes |
| D4 | Rain | Mild | High | Weak | Yes |
| D5 | Rain | Cool | Normal | Weak | Yes |
| D6 | Rain | Cool | Normal | Strong | No |
| D7 | Overcast | Cool | Normal | Strong | Yes |
| D8 | Sunny | Mild | High | Weak | No |
| D9 | Sunny | Cool | Normal | Weak | Yes |
| D10 | Rain | Mild | Normal | Weak | Yes |
| D11 | Sunny | Mild | Normal | Strong | Yes |
| D12 | Overcast | Mild | High | Strong | Yes |
| D13 | Overcast | Hot | Normal | Weak | Yes |
| D14 | Rain | Mild | High | Strong | No |

**Keterangan istilah:**

- **Dataset** : kumpulan data latih (14 baris).
- **Atribut** : Outlook, Temperature, Humidity, Wind.
- **Label / class** : PlayTennis (nilainya Yes atau No).
- **Tipe data** : kategorikal (nilai berupa kategori, bukan angka).

**Jumlah tiap label:**

- Yes = 9 data (D3, D4, D5, D7, D9, D10, D11, D12, D13)
- No = 5 data (D1, D2, D6, D8, D14)
- Total = 14 data

---

## 2. Langkah 1: Menghitung Entropy Dataset Awal

Rumus Entropy:

$$
\text{Info}(D) = -\sum_{i=1}^{m} p_i \log_2(p_i)
$$

Dengan $p_i$ adalah proporsi data pada kelas ke-$i$.

Perhitungan:

$$
p_{Yes} = \frac{9}{14} = 0{,}643 \qquad p_{No} = \frac{5}{14} = 0{,}357
$$

$$
\text{Info}(D) = -\frac{9}{14}\log_2\left(\frac{9}{14}\right) - \frac{5}{14}\log_2\left(\frac{5}{14}\right)
$$

$$
= -(0{,}643)(-0{,}637) - (0{,}357)(-1{,}485)
$$

$$
= 0{,}410 + 0{,}530 = \mathbf{0{,}940}
$$

**Entropy dataset awal = 0,940**

---

## 3. Langkah 2: Menghitung Info dan Gain untuk Setiap Atribut

Rumus yang digunakan:

$$
\text{Info}_A(D) = \sum_{j=1}^{v} \frac{|D_j|}{|D|} \times I(D_j)
$$

$$
\text{Gain}(A) = \text{Info}(D) - \text{Info}_A(D)
$$

Keterangan notasi:

- $|D_j|$ : jumlah data pada cabang ke-$j$ dari atribut $A$.
- $|D|$ : jumlah seluruh data (14).
- $I(D_j)$ : entropy pada cabang ke-$j$.

Fungsi $I(\cdot)$ untuk pasangan (jumlah Yes, jumlah No) dihitung dengan rumus entropy di atas.

### 3.a Atribut Outlook

Pembagian data:

| Outlook | Yes | No | Jumlah | I(Yes, No) |
|:---:|:---:|:---:|:---:|:---:|
| Sunny | 2 (D9, D11) | 3 (D1, D2, D8) | 5 | 0,971 |
| Overcast | 4 (D3, D7, D12, D13) | 0 | 4 | 0 |
| Rain | 3 (D4, D5, D10) | 2 (D6, D14) | 5 | 0,971 |

Perhitungan $I$ per cabang:

$$
I(2,3) = -\frac{2}{5}\log_2\left(\frac{2}{5}\right) - \frac{3}{5}\log_2\left(\frac{3}{5}\right) = 0{,}529 + 0{,}442 = 0{,}971
$$

$$
I(4,0) = -\frac{4}{4}\log_2\left(\frac{4}{4}\right) - 0 = 0
$$

$$
I(3,2) = -\frac{3}{5}\log_2\left(\frac{3}{5}\right) - \frac{2}{5}\log_2\left(\frac{2}{5}\right) = 0{,}971
$$

Info Outlook:

$$
\text{Info}_{Outlook}(D) = \frac{5}{14}(0{,}971) + \frac{4}{14}(0) + \frac{5}{14}(0{,}971) = 0{,}694
$$

Gain Outlook:

$$
\text{Gain}(Outlook) = 0{,}940 - 0{,}694 = \mathbf{0{,}247}
$$

> Catatan: pada slide tertulis 0,246 karena pembulatan Info di tengah perhitungan. Nilai yang lebih presisi adalah 0,2467 (dibulatkan menjadi 0,247).

### 3.b Atribut Temperature

Pembagian data:

| Temperature | Yes | No | Jumlah | I(Yes, No) |
|:---:|:---:|:---:|:---:|:---:|
| Hot | 2 (D3, D13) | 2 (D1, D2) | 4 | 1 |
| Mild | 4 (D4, D10, D11, D12) | 2 (D8, D14) | 6 | 0,918 |
| Cool | 3 (D5, D7, D9) | 1 (D6) | 4 | 0,811 |

Perhitungan $I$ per cabang:

$$
I(2,2) = -\frac{2}{4}\log_2\left(\frac{2}{4}\right) - \frac{2}{4}\log_2\left(\frac{2}{4}\right) = 1
$$

$$
I(4,2) = -\frac{4}{6}\log_2\left(\frac{4}{6}\right) - \frac{2}{6}\log_2\left(\frac{2}{6}\right) = 0{,}390 + 0{,}528 = 0{,}918
$$

$$
I(3,1) = -\frac{3}{4}\log_2\left(\frac{3}{4}\right) - \frac{1}{4}\log_2\left(\frac{1}{4}\right) = 0{,}311 + 0{,}500 = 0{,}811
$$

Info Temperature:

$$
\text{Info}_{Temperature}(D) = \frac{4}{14}(1) + \frac{6}{14}(0{,}918) + \frac{4}{14}(0{,}811) = 0{,}911
$$

Gain Temperature:

$$
\text{Gain}(Temperature) = 0{,}940 - 0{,}911 = \mathbf{0{,}029}
$$

### 3.c Atribut Humidity

Pembagian data:

| Humidity | Yes | No | Jumlah | I(Yes, No) |
|:---:|:---:|:---:|:---:|:---:|
| High | 3 (D3, D4, D12) | 4 (D1, D2, D8, D14) | 7 | 0,985 |
| Normal | 6 (D5, D7, D9, D10, D11, D13) | 1 (D6) | 7 | 0,592 |

Perhitungan $I$ per cabang:

$$
I(3,4) = -\frac{3}{7}\log_2\left(\frac{3}{7}\right) - \frac{4}{7}\log_2\left(\frac{4}{7}\right) = 0{,}524 + 0{,}461 = 0{,}985
$$

$$
I(6,1) = -\frac{6}{7}\log_2\left(\frac{6}{7}\right) - \frac{1}{7}\log_2\left(\frac{1}{7}\right) = 0{,}191 + 0{,}401 = 0{,}592
$$

Info Humidity:

$$
\text{Info}_{Humidity}(D) = \frac{7}{14}(0{,}985) + \frac{7}{14}(0{,}592) = 0{,}788
$$

Gain Humidity:

$$
\text{Gain}(Humidity) = 0{,}940 - 0{,}788 = \mathbf{0{,}152}
$$

### 3.d Atribut Wind

Pembagian data:

| Wind | Yes | No | Jumlah | I(Yes, No) |
|:---:|:---:|:---:|:---:|:---:|
| Weak | 6 (D3, D4, D5, D9, D10, D13) | 2 (D1, D8) | 8 | 0,811 |
| Strong | 3 (D7, D11, D12) | 3 (D2, D6, D14) | 6 | 1 |

Perhitungan $I$ per cabang:

$$
I(6,2) = -\frac{6}{8}\log_2\left(\frac{6}{8}\right) - \frac{2}{8}\log_2\left(\frac{2}{8}\right) = 0{,}311 + 0{,}500 = 0{,}811
$$

$$
I(3,3) = -\frac{3}{6}\log_2\left(\frac{3}{6}\right) - \frac{3}{6}\log_2\left(\frac{3}{6}\right) = 1
$$

Info Wind:

$$
\text{Info}_{Wind}(D) = \frac{8}{14}(0{,}811) + \frac{6}{14}(1) = 0{,}892
$$

Gain Wind:

$$
\text{Gain}(Wind) = 0{,}940 - 0{,}892 = \mathbf{0{,}048}
$$

### 3.e Rekap Gain Seluruh Atribut (Root)

| Atribut | Info_A(D) | Gain(A) |
|:---:|:---:|:---:|
| Outlook | 0,694 | **0,247** (tertinggi) |
| Temperature | 0,911 | 0,029 |
| Humidity | 0,788 | 0,152 |
| Wind | 0,892 | 0,048 |

**Kesimpulan:** atribut **Outlook** memiliki Information Gain tertinggi sehingga dipilih sebagai **root (node awal)**.

---

## 4. Langkah 3: Membentuk Cabang dari Root (Outlook)

Dari root Outlook terbentuk tiga cabang: **Sunny**, **Overcast**, dan **Rain**.

Aturan proses (sesuai materi): setiap cabang diproses terus selama masih terdapat lebih dari 1 kelas.

### 4.a Cabang Overcast

Data pada cabang Overcast:

| Day | Temperature | Humidity | Wind | PlayTennis |
|:---:|:---:|:---:|:---:|:---:|
| D3 | Hot | High | Weak | Yes |
| D7 | Cool | Normal | Strong | Yes |
| D12 | Mild | High | Strong | Yes |
| D13 | Hot | Normal | Weak | Yes |

Semua data berlabel **Yes**, entropy = 0 (pure node).

**Keputusan: Overcast menjadi daun (leaf) dengan hasil YES.** Tidak perlu diproses lebih lanjut.

### 4.b Cabang Sunny

Data pada cabang Sunny (5 data):

| Day | Temperature | Humidity | Wind | PlayTennis |
|:---:|:---:|:---:|:---:|:---:|
| D1 | Hot | High | Weak | No |
| D2 | Hot | High | Strong | No |
| D8 | Mild | High | Weak | No |
| D9 | Cool | Normal | Weak | Yes |
| D11 | Mild | Normal | Strong | Yes |

Entropy cabang Sunny (2 Yes, 3 No):

$$
I(Sunny) = I(2,3) = 0{,}971
$$

Perhitungan Gain untuk setiap atribut pada cabang Sunny:

**Atribut Temperature (pada cabang Sunny):**

| Temperature | Yes | No | Jumlah | I |
|:---:|:---:|:---:|:---:|:---:|
| Hot | 0 | 2 (D1, D2) | 2 | 0 |
| Mild | 1 (D11) | 1 (D8) | 2 | 1 |
| Cool | 1 (D9) | 0 | 1 | 0 |

$$
\text{Info}_{Temp}(Sunny) = \frac{2}{5}(0) + \frac{2}{5}(1) + \frac{1}{5}(0) = 0{,}400
$$

$$
\text{Gain}(Temperature \mid Sunny) = 0{,}971 - 0{,}400 = \mathbf{0{,}571}
$$

**Atribut Humidity (pada cabang Sunny):**

| Humidity | Yes | No | Jumlah | I |
|:---:|:---:|:---:|:---:|:---:|
| High | 0 | 3 (D1, D2, D8) | 3 | 0 |
| Normal | 2 (D9, D11) | 0 | 2 | 0 |

$$
\text{Info}_{Humidity}(Sunny) = \frac{3}{5}(0) + \frac{2}{5}(0) = 0
$$

$$
\text{Gain}(Humidity \mid Sunny) = 0{,}971 - 0 = \mathbf{0{,}971}
$$

**Atribut Wind (pada cabang Sunny):**

| Wind | Yes | No | Jumlah | I |
|:---:|:---:|:---:|:---:|:---:|
| Weak | 1 (D9) | 2 (D1, D8) | 3 | 0,918 |
| Strong | 1 (D11) | 1 (D2) | 2 | 1 |

$$
\text{Info}_{Wind}(Sunny) = \frac{3}{5}(0{,}918) + \frac{2}{5}(1) = 0{,}951
$$

$$
\text{Gain}(Wind \mid Sunny) = 0{,}971 - 0{,}951 = \mathbf{0{,}020}
$$

**Rekap Gain pada cabang Sunny:**

| Atribut | Gain |
|:---:|:---:|
| Temperature | 0,571 |
| Humidity | **0,971** (tertinggi) |
| Wind | 0,020 |

**Keputusan: Humidity dipilih sebagai node berikutnya pada cabang Sunny.**

Cabang turunan dari Humidity:

- **High** : D1, D2, D8 semuanya No. Pure node, hasil = **NO**.
- **Normal** : D9, D11 semuanya Yes. Pure node, hasil = **YES**.

### 4.c Cabang Rain

Data pada cabang Rain (5 data):

| Day | Temperature | Humidity | Wind | PlayTennis |
|:---:|:---:|:---:|:---:|:---:|
| D4 | Mild | High | Weak | Yes |
| D5 | Cool | Normal | Weak | Yes |
| D6 | Cool | Normal | Strong | No |
| D10 | Mild | Normal | Weak | Yes |
| D14 | Mild | High | Strong | No |

Entropy cabang Rain (3 Yes, 2 No):

$$
I(Rain) = I(3,2) = 0{,}971
$$

Perhitungan Gain untuk setiap atribut pada cabang Rain:

**Atribut Temperature (pada cabang Rain):**

| Temperature | Yes | No | Jumlah | I |
|:---:|:---:|:---:|:---:|:---:|
| Mild | 2 (D4, D10) | 1 (D14) | 3 | 0,918 |
| Cool | 1 (D5) | 1 (D6) | 2 | 1 |

$$
\text{Info}_{Temp}(Rain) = \frac{3}{5}(0{,}918) + \frac{2}{5}(1) = 0{,}951
$$

$$
\text{Gain}(Temperature \mid Rain) = 0{,}971 - 0{,}951 = \mathbf{0{,}020}
$$

**Atribut Humidity (pada cabang Rain):**

| Humidity | Yes | No | Jumlah | I |
|:---:|:---:|:---:|:---:|:---:|
| High | 1 (D4) | 1 (D14) | 2 | 1 |
| Normal | 2 (D5, D10) | 1 (D6) | 3 | 0,918 |

$$
\text{Info}_{Humidity}(Rain) = \frac{2}{5}(1) + \frac{3}{5}(0{,}918) = 0{,}951
$$

$$
\text{Gain}(Humidity \mid Rain) = 0{,}971 - 0{,}951 = \mathbf{0{,}020}
$$

**Atribut Wind (pada cabang Rain):**

| Wind | Yes | No | Jumlah | I |
|:---:|:---:|:---:|:---:|:---:|
| Weak | 3 (D4, D5, D10) | 0 | 3 | 0 |
| Strong | 0 | 2 (D6, D14) | 2 | 0 |

$$
\text{Info}_{Wind}(Rain) = \frac{3}{5}(0) + \frac{2}{5}(0) = 0
$$

$$
\text{Gain}(Wind \mid Rain) = 0{,}971 - 0 = \mathbf{0{,}971}
$$

**Rekap Gain pada cabang Rain:**

| Atribut | Gain |
|:---:|:---:|
| Temperature | 0,020 |
| Humidity | 0,020 |
| Wind | **0,971** (tertinggi) |

**Keputusan: Wind dipilih sebagai node berikutnya pada cabang Rain.**

Cabang turunan dari Wind:

- **Weak** : D4, D5, D10 semuanya Yes. Pure node, hasil = **YES**.
- **Strong** : D6, D14 semuanya No. Pure node, hasil = **NO**.

---

## 5. Langkah 4: Pohon Keputusan (Decision Tree) Akhir

```
                        [ Outlook ? ]
                       /      |       \
                   Sunny  Overcast    Rain
                    /         |         \
          [ Humidity ? ]    YES      [ Wind ? ]
              /     \                  /     \
          High     Normal           Weak    Strong
           |         |                |        |
          NO        YES              YES      NO
```

Penjelasan singkat:

- Node root adalah **Outlook**.
- Cabang **Sunny** dilanjutkan dengan **Humidity**.
- Cabang **Overcast** langsung menjadi daun **YES**.
- Cabang **Rain** dilanjutkan dengan **Wind**.

---

## 6. Rule (Aturan) dari Decision Tree

Setiap jalur dari root sampai daun dapat ditulis sebagai satu aturan IF-THEN:

- **R1** : IF Outlook = Sunny AND Humidity = High THEN PlayTennis = **No**
- **R2** : IF Outlook = Sunny AND Humidity = Normal THEN PlayTennis = **Yes**
- **R3** : IF Outlook = Overcast THEN PlayTennis = **Yes**
- **R4** : IF Outlook = Rain AND Wind = Weak THEN PlayTennis = **Yes**
- **R5** : IF Outlook = Rain AND Wind = Strong THEN PlayTennis = **No**

Atau dapat ditulis dalam satu rumus gabungan:

IF ((Outlook = Sunny) AND (Humidity = Normal)) OR (Outlook = Overcast) OR ((Outlook = Rain) AND (Wind = Weak)) THEN PlayTennis = **Yes**

---

## 7. Verifikasi Pohon pada Data Latih

Setiap data latih diuji kembali menggunakan rule di atas.

| Day | Outlook | Humidity | Wind | Aktual | Prediksi | Benar? |
|:---:|:---:|:---:|:---:|:---:|:---:|:---:|
| D1 | Sunny | High | Weak | No | No | ✓ |
| D2 | Sunny | High | Strong | No | No | ✓ |
| D3 | Overcast | High | Weak | Yes | Yes | ✓ |
| D4 | Rain | High | Weak | Yes | Yes | ✓ |
| D5 | Rain | Normal | Weak | Yes | Yes | ✓ |
| D6 | Rain | Normal | Strong | No | No | ✓ |
| D7 | Overcast | Normal | Strong | Yes | Yes | ✓ |
| D8 | Sunny | High | Weak | No | No | ✓ |
| D9 | Sunny | Normal | Weak | Yes | Yes | ✓ |
| D10 | Rain | Normal | Weak | Yes | Yes | ✓ |
| D11 | Sunny | Normal | Strong | Yes | Yes | ✓ |
| D12 | Overcast | High | Strong | Yes | Yes | ✓ |
| D13 | Overcast | Normal | Weak | Yes | Yes | ✓ |
| D14 | Rain | High | Strong | No | No | ✓ |

**Akurasi pada data latih = 14 / 14 = 100%**

---

## 8. Kode Python (Dapat Disalin ke Jupyter Notebook)

Kode berikut membangun ulang decision tree dengan metode ID3 (Information Gain) yang sama seperti perhitungan manual di atas, lalu menampilkan tabel gain, teks pohon, dan gambar pohon.

```python
# ===============================================================
# Decision Tree (ID3 - Information Gain) : Dataset PlayTennis
# ===============================================================
import math
from collections import Counter

import pandas as pd
import matplotlib.pyplot as plt

# ---------------------------------------------------------------
# 1. Membuat dataset
# ---------------------------------------------------------------
data = pd.DataFrame({
    'Day': [f'D{i}' for i in range(1, 15)],
    'Outlook':     ['Sunny', 'Sunny', 'Overcast', 'Rain', 'Rain', 'Rain', 'Overcast',
                    'Sunny', 'Sunny', 'Rain', 'Sunny', 'Overcast', 'Overcast', 'Rain'],
    'Temperature': ['Hot', 'Hot', 'Hot', 'Mild', 'Cool', 'Cool', 'Cool',
                    'Mild', 'Cool', 'Mild', 'Mild', 'Mild', 'Hot', 'Mild'],
    'Humidity':    ['High', 'High', 'High', 'High', 'Normal', 'Normal', 'Normal',
                    'High', 'Normal', 'Normal', 'Normal', 'High', 'Normal', 'High'],
    'Wind':        ['Weak', 'Strong', 'Weak', 'Weak', 'Weak', 'Strong', 'Strong',
                    'Weak', 'Weak', 'Weak', 'Strong', 'Strong', 'Weak', 'Strong'],
    'PlayTennis':  ['No', 'No', 'Yes', 'Yes', 'Yes', 'No', 'Yes',
                    'No', 'Yes', 'Yes', 'Yes', 'Yes', 'Yes', 'No'],
})
TARGET = 'PlayTennis'
ATTRIBUTES = ['Outlook', 'Temperature', 'Humidity', 'Wind']

print(data)
print()


# ---------------------------------------------------------------
# 2. Fungsi Entropy : Info(D) = -sum(p_i * log2(p_i))
# ---------------------------------------------------------------
def entropy(labels):
    n = len(labels)
    counts = Counter(labels)
    return -sum((c / n) * math.log2(c / n) for c in counts.values())


# ---------------------------------------------------------------
# 3. Fungsi Information Gain
#    Info_A(D) = sum(|Dj|/|D| * I(Dj))
#    Gain(A)   = Info(D) - Info_A(D)
# ---------------------------------------------------------------
def info_gain(df, attr, target=TARGET):
    base = entropy(df[target])
    info_a = 0.0
    for value, subset in df.groupby(attr):
        info_a += len(subset) / len(df) * entropy(subset[target])
    return base, info_a, base - info_a


# ---------------------------------------------------------------
# 4. Tabel Gain untuk root
# ---------------------------------------------------------------
print(f"Entropy dataset awal Info(D) = {entropy(data[TARGET]):.3f}\n")
print(f"{'Atribut':<14}{'Info_A(D)':>12}{'Gain(A)':>12}")
for a in ATTRIBUTES:
    _, info_a, gain = info_gain(data, a)
    print(f"{a:<14}{info_a:>12.3f}{gain:>12.3f}")
print()


# ---------------------------------------------------------------
# 5. Algoritma ID3 (rekursif)
# ---------------------------------------------------------------
def id3(df, attrs, target=TARGET):
    labels = df[target]

    # Kasus 1: semua data sudah satu kelas (pure node) -> daun
    if labels.nunique() == 1:
        return labels.iloc[0]

    # Kasus 2: tidak ada atribut tersisa -> ambil kelas mayoritas
    if not attrs:
        return labels.value_counts().idxmax()

    # Pilih atribut dengan Gain tertinggi
    gains = {a: info_gain(df, a, target)[2] for a in attrs}
    best = max(gains, key=gains.get)

    # Bangun cabang untuk setiap nilai atribut terbaik
    node = {best: {}}
    for value in df[best].unique():
        subset = df[df[best] == value]
        remaining = [a for a in attrs if a != best]
        node[best][value] = id3(subset, remaining, target)
    return node


tree = id3(data, ATTRIBUTES)
print("Decision Tree (format dictionary):")
print(tree)
print()


# ---------------------------------------------------------------
# 6. Menampilkan pohon dalam bentuk teks
# ---------------------------------------------------------------
def print_tree(node, indent=''):
    if not isinstance(node, dict):
        print(indent + '-> ' + str(node))
        return
    for attr, branches in node.items():
        for value, sub in branches.items():
            if isinstance(sub, dict):
                print(f"{indent}[{attr} = {value}]")
                print_tree(sub, indent + '    ')
            else:
                print(f"{indent}[{attr} = {value}] -> {sub}")


print("Pohon keputusan (teks):")
print_tree(tree)
print()


# ---------------------------------------------------------------
# 7. Fungsi prediksi dan verifikasi pada data latih
# ---------------------------------------------------------------
def predict(node, row):
    while isinstance(node, dict):
        attr = next(iter(node))
        node = node[attr][row[attr]]
    return node


data['Prediksi'] = data.apply(lambda r: predict(tree, r), axis=1)
data['Benar'] = data['Prediksi'] == data[TARGET]
print(data)
print(f"\nAkurasi data latih = {data['Benar'].sum()} / {len(data)} "
      f"= {data['Benar'].mean() * 100:.1f}%")


# ---------------------------------------------------------------
# 8. Menggambar pohon keputusan
# ---------------------------------------------------------------
def draw_tree(tree, title='Decision Tree - PlayTennis'):
    fig, ax = plt.subplots(figsize=(12, 7))
    ax.axis('off')

    def _draw(node, x, y, dx):
        if not isinstance(node, dict):
            color = 'lightgreen' if node == 'Yes' else 'salmon'
            ax.text(x, y, node, ha='center', va='center', fontsize=12,
                    bbox=dict(boxstyle='round,pad=0.4', fc=color, ec='black'))
            return
        attr = next(iter(node))
        ax.text(x, y, f"{attr}?", ha='center', va='center', fontsize=12,
                bbox=dict(boxstyle='round,pad=0.4', fc='lightblue', ec='black'))
        branches = node[attr]
        n = len(branches)
        for i, (value, sub) in enumerate(branches.items()):
            cx = x - dx * (n - 1) / 2 + i * dx
            cy = y - 1.6
            ax.plot([x, cx], [y - 0.15, cy + 0.15], 'k-', lw=1.2)
            ax.text((x + cx) / 2, (y + cy) / 2, str(value), ha='center',
                    va='center', fontsize=10,
                    bbox=dict(fc='white', ec='none'))
            _draw(sub, cx, cy, dx / 2.2)

    _draw(tree, 0, 0, 8)
    ax.set_title(title, fontsize=14)
    ax.margins(0.15)
    plt.show()


draw_tree(tree)
```

**Output yang diharapkan (sesuai perhitungan manual):**

- Entropy awal = 0,940
- Gain Outlook = 0,247 (tertinggi), Humidity = 0,152, Wind = 0,048, Temperature = 0,029
- Struktur pohon: Outlook → Sunny: Humidity (High: No, Normal: Yes); Overcast: Yes; Rain: Wind (Weak: Yes, Strong: No)
- Akurasi data latih = 14 / 14 = 100,0%

---

## 9. Ringkasan Jawaban

1. Entropy dataset awal = **0,940**.
2. Information Gain tertinggi pada root adalah atribut **Outlook** (Gain = 0,247), sehingga Outlook menjadi **root**.
3. Cabang **Overcast** sudah murni (semua Yes), menjadi **daun YES**.
4. Cabang **Sunny** dipecah menggunakan **Humidity** (Gain = 0,971): High → No, Normal → Yes.
5. Cabang **Rain** dipecah menggunakan **Wind** (Gain = 0,971): Weak → Yes, Strong → No.
6. Pohon ini menghasilkan ketepatan **100%** pada data latih.

---

## Lampiran: Pengingat Rumus

$$
\text{Info}(D) = -\sum_{i=1}^{m} p_i \log_2(p_i)
$$

$$
\text{Info}_A(D) = \sum_{j=1}^{v} \frac{|D_j|}{|D|} \times I(D_j)
$$

$$
\text{Gain}(A) = \text{Info}(D) - \text{Info}_A(D)
$$

$$
I(a, b) = -\frac{a}{a+b}\log_2\left(\frac{a}{a+b}\right) - \frac{b}{a+b}\log_2\left(\frac{b}{a+b}\right)
$$
