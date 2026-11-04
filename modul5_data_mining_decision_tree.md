# Modul Praktikum SD25-31001 Data Mining
## Modul 5. Supervised Learning: Decision Tree for Classification

Program Studi Sains Data, Fakultas Sains, Institut Teknologi Sumatera, 2025

---

## A. Konsep Dasar

Klasifikasi merupakan salah satu teknik data mining yang dapat digunakan untuk menganalisis sekumpulan data dengan membangun suatu model. Klasifikasi juga dapat diartikan sebagai pemrosesan untuk menemukan sebuah model atau fungsi yang menjelaskan dan mencirikan konsep atau kelas data, untuk kepentingan tertentu. Hasil dari klasifikasi dapat dijadikan dasar dalam pengambilan keputusan bagi data analysis. Pada modul ini, model yang digunakan adalah algoritma Decision Tree.

Decision Tree (pohon keputusan) adalah representasi grafis dari berbagai kemungkinan solusi terhadap suatu keputusan dan merupakan salah satu algoritma paling populer dalam pembelajaran terawasi (supervised learning). Algoritma ini mudah dipahami, mudah divisualisasikan, serta adaptif untuk berbagai jenis data. Decision Tree dapat digunakan untuk tugas klasifikasi maupun regresi, meskipun lebih sering diterapkan pada masalah klasifikasi. Algoritma ini bekerja dengan membagi dataset menjadi beberapa subset berdasarkan fitur tertentu hingga terbentuk struktur bercabang menyerupai pohon, di mana setiap cabang merepresentasikan suatu kondisi atau keputusan, dan setiap daun (leaf node) menunjukkan hasil akhir atau kelas prediksi, seperti pada Gambar 1.

```
Gambar 1. Model Decision Tree

                      [ age? ]
                /         |          \
            <=30       31..40         >40
              |           |             |
        [ student? ]    (yes)    [ credit rating? ]
          /      \                  /          \
        no       yes          excellent        fair
        |         |               |             |
      (no)      (yes)           (no)          (yes)
```

Dalam algoritma Decision Tree terdapat beberapa ukuran penting yaitu entropi, information gain, dan gini index. Ketiga ukuran ini digunakan untuk menilai seberapa baik suatu atribut dapat memisahkan data ke dalam kelompok yang lebih homogen. Dengan menghitung nilai-nilai tersebut, algoritma dapat memilih atribut terbaik pada setiap percabangan sehingga pohon keputusan yang terbentuk menjadi lebih akurat dan efisien dalam melakukan klasifikasi.

### 1) Entropy

Entropy adalah ukuran ketidakpastian atau tingkat "kemurnian" suatu himpunan data. Semakin kecil nilai entropy, distribusi semakin homogen dan node semakin murni (pure).

```
Info(D) = - sum_{i=1..m} p_i * log2(p_i)

*Info(D) = Entropy(D)
```

### 2) Information Gain

Information Gain digunakan untuk memilih atribut terbaik yang akan dijadikan node pembagi pada pohon keputusan. Gain(A) menggambarkan seberapa besar entropy berkurang akibat atribut A. Semakin besar, semakin bagus.

```
Gain(A) = Info(D) - Info_A(D)
```

### 3) Gini Index

Gini Index merupakan alternatif dari entropi untuk mengukur impurity (ketidakhomogenan) data.

```
Gini = 1 - sum_{i=1..C} (p_i)^2
```

Kelebihan utama Decision Tree adalah kemudahan interpretasi dan visualisasi. Struktur model yang menyerupai alur logika "jika-maka" (if-then rules) membuatnya mudah dipahami, bahkan oleh pengguna non-teknis. Selain itu, Decision Tree juga dapat menangani data numerik dan kategorikal, serta tidak membutuhkan banyak prapemrosesan data.

---

## B. Tujuan Praktikum

**I. Tujuan Instruksional Umum**

Praktikum bertujuan untuk menerapkan teori Decision Tree untuk klasifikasi pada penambangan data.

**II. Tujuan Instruksional Khusus**

1. Mahasiswa mampu menguasai konsep dasar Decision Tree.
2. Mahasiswa mampu menganalisis studi kasus menggunakan metode Decision Tree.
3. Mahasiswa mampu menampilkan visualisasi struktur pohon keputusan dan menganalisis hasil klasifikasi.

---

## C. Dataset dan Bahasa Pemrograman Python

Dataset yang digunakan adalah dataset bawaan (built-in) yang tersedia di Seaborn dan dapat langsung digunakan tanpa perlu mengunduh data eksternal.

**Tabel 1. Informasi dataset**

| | Dataset 1: Iris | Dataset 2: Penguins |
|---|---|---|
| Tujuan | Klasifikasi jenis bunga iris | Klasifikasi jenis penguin |
| Jumlah kelas (target) | 3 spesies: setosa, versicolor, virginica | 3 spesies: Adelie, Chinstrap, Gentoo |
| Jumlah fitur | 4 fitur numerik | 6 fitur utama (5 numerik + 1 kategorik) |
| Fitur | sepal_length, sepal_width, petal_length, petal_width | bill_length_mm, bill_depth_mm, flipper_length_mm, body_mass_g, sex, island |
| Target kolom | species | species |
| Jenis data | Semua numerik kecuali target | Campuran numerik dan kategorik |
| Jumlah sampel | 150 data | 333 data (rata-rata, bisa bervariasi tergantung versi) |

**Tabel 2. Daftar library dan deskripsi**

| Kode | Deskripsi |
|---|---|
| `import pandas as pd` | Library untuk manipulasi data berbentuk tabel (DataFrame). |
| `import numpy as np` | Library untuk operasi numerik dan array. |
| `import matplotlib.pyplot as plt` | Library untuk membuat grafik dan visualisasi data. |
| `import seaborn as sns` | Library untuk visualisasi data statistik. |
| `%matplotlib inline` | Menampilkan grafik. |
| `from sklearn.preprocessing import LabelEncoder` | Fungsi untuk mengubah data kategorik menjadi numerik. |
| `from sklearn.model_selection import train_test_split` | Fungsi untuk membagi data menjadi train dan test set. |
| `from sklearn.tree import DecisionTreeClassifier` | Model Decision Tree untuk klasifikasi. |
| `from sklearn.metrics import classification_report, confusion_matrix` | Metrik evaluasi untuk mengukur performa model. |
| `from sklearn.tree import plot_tree` | Fungsi untuk menampilkan visualisasi struktur pohon keputusan. |

---

## D. Implementasi Klasifikasi

Pada modul ini digunakan data iris untuk mengklasifikasikan jenis bunga iris. Langkah-langkahnya:

### 1. Import libraries

Library yang digunakan dijelaskan pada Tabel 2.

```python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns
%matplotlib inline

# for encoding
from sklearn.preprocessing import LabelEncoder

# for train test splitting
from sklearn.model_selection import train_test_split

# for decision tree object
from sklearn.tree import DecisionTreeClassifier

# for checking testing results
from sklearn.metrics import classification_report, confusion_matrix

# for visualizing tree
from sklearn.tree import plot_tree
```

### 2. Import data

Import dataset bawaan yang tersedia di Seaborn.

```python
# to know all the available dataset
sns.get_dataset_names()
```

Output (daftar dataset): anagrams, anscombe, attention, brain_networks, car_crashes, diamonds, dots, dowjones, exercise, flights, fmri, geyser, glue, healthexp, iris, mpg, penguins, planets, seaice, taxis, tips, titanic.

Pilih dan tampilkan data iris:

```python
# reading the data
df = sns.load_dataset('iris')
df.head()
```

```
   sepal_length  sepal_width  petal_length  petal_width species
0           5.1          3.5           1.4          0.2  setosa
1           4.9          3.0           1.4          0.2  setosa
2           4.7          3.2           1.3          0.2  setosa
3           4.6          3.1           1.5          0.2  setosa
4           5.0          3.6           1.4          0.2  setosa
```

Simpan data dengan nama file Iris.csv:

```python
df.to_csv('Iris.csv', index=False)
```

### 3. Data Analysis / Preprocessing

Exploratory Data Analysis (EDA) adalah proses menganalisis dan merangkum karakteristik utama kumpulan data, dengan tujuan memperoleh insight tentang struktur, korelasi, dan pola yang mendasari data. EDA membantu mengidentifikasi fitur penting, anomali, dan tren dalam data yang dapat menginformasikan analisis dan pemodelan lebih lanjut.

EDA biasanya melibatkan beberapa langkah penting:

- **Data cleaning and preparation**: penghapusan nilai yang hilang atau salah, transformasi variabel, dan penanganan outlier.
- **Analisis statistik**: penerapan metode matematika dan statistik pada data untuk mengidentifikasi fitur dan hubungan penting.
- **Data visualization**: pembuatan grafik, bagan, dan representasi visual lainnya untuk membantu mengidentifikasi pola, hubungan, dan anomali.

Pra-pemrosesan bertujuan menyiapkan data sedemikian rupa sehingga memungkinkan analisis dan pemodelan yang efektif serta menghilangkan bias atau kesalahan yang dapat memengaruhi hasil. Kamu bisa mengikuti langkah-langkah data preprocessing pada modul 1, 2, dan 3 data mining.

Dengan dataset iris, selesaikan langkah-langkah berikut dan tampilkan output pada setiap step.

#### a) Informasi dataset

Cek dimensi data; dataset ini memiliki 150 record dan 5 kolom.

```python
df.shape
# (150, 5)
```

```python
# getting information of dataset
df.info()
```

```
<class 'pandas.core.frame.DataFrame'>
RangeIndex: 150 entries, 0 to 149
Data columns (total 5 columns):
 #   Column        Non-Null Count  Dtype
---  ------        --------------  -----
 0   sepal_length  150 non-null    float64
 1   sepal_width   150 non-null    float64
 2   petal_length  150 non-null    float64
 3   petal_width   150 non-null    float64
 4   species       150 non-null    object
dtypes: float64(4), object(1)
memory usage: 6.0+ KB
```

Empat kolom pertama bertipe float dan kolom terakhir bertipe objek (str). Tidak ada nilai NaN, seperti yang terlihat setelah perintah pengecekan nilai kosong (`df.isnull().any()`), yang menghasilkan `False` untuk semua kolom (dtype: bool).

#### b) Deskripsi data

```python
df.describe()
```

```
       sepal_length  sepal_width  petal_length  petal_width
count    150.000000   150.000000    150.000000   150.000000
mean       5.843333     3.057333      3.758000     1.199333
std        0.828066     0.435866      1.765298     0.762238
min        4.300000     2.000000      1.000000     0.100000
25%        5.100000     2.800000      1.600000     0.300000
50%        5.800000     3.000000      4.350000     1.300000
75%        6.400000     3.300000      5.100000     1.800000
max        7.900000     4.400000      6.900000     2.500000
```

#### c) Distribusi data

```python
# let's plot pair plot to visualise the attributes all at once
sns.pairplot(data=df, hue='species')
plt.savefig("pne.png")
```

Output berupa pair plot (scatter plot dan distribusi KDE untuk setiap pasangan fitur, diwarnai berdasarkan species: setosa, versicolor, virginica).

Dapat kita lihat, **Setosa** selalu membentuk klaster yang berbeda.

#### d) Correlation matrix

Correlation matrix adalah tabel yang merangkum hubungan antara beberapa variabel dalam suatu kumpulan data. Ini menunjukkan koefisien korelasi antara setiap pasangan variabel, serta kekuatan dan arah hubungan antar variabel. Hal ini berguna untuk mengidentifikasi variabel yang berkorelasi tinggi dan memilih subset variabel untuk analisis lebih lanjut.

```python
# correlation matrix
# Exclude non-numeric columns before calculating correlation
numeric_df = df.select_dtypes(include=np.number)
sns.heatmap(numeric_df.corr(), annot=True, cmap='Blues')
plt.title('Correlation Matrix of Iris Dataset (Numeric Features)')
plt.savefig("one.png")
```

Output correlation matrix:

```
              sepal_length  sepal_width  petal_length  petal_width
sepal_length          1.00        -0.12          0.87         0.82
sepal_width          -0.12         1.00         -0.43        -0.37
petal_length          0.87        -0.43          1.00         0.96
petal_width           0.82        -0.37          0.96         1.00
```

Dari matriks korelasi terlihat bahwa:

- **Petal length** memiliki hubungan yang sangat kuat dengan **petal width**.
- **Sepal length** tidak memiliki hubungan dengan **sepal width**.

#### e) Label encoding

Label encoding adalah teknik pra-pemrosesan di mana data kategorikal diubah menjadi nilai numerik, agar kompatibel dengan operasi dan model matematika.

Pisahkan variabel target (y) dan fitur (X):

```python
target = df['species']
df1 = df.copy()
df1 = df1.drop('species', axis=1)
df1.shape
# (150, 4)
```

Sebaiknya jangan menghapus atau menambahkan kolom baru ke dataset asli. Buat salinannya, lalu modifikasi, sehingga jika hasilnya tidak sesuai harapan, kita masih memiliki data asli untuk memulai lagi dengan pendekatan yang berbeda.

```python
# Defining the attributes
X = df1
```

Sekarang lihat variabel target:

```python
target
```

```
0         setosa
1         setosa
2         setosa
3         setosa
4         setosa
         ...
145    virginica
146    virginica
147    virginica
148    virginica
149    virginica
150 rows x 1 columns, dtype: object
```

Target memiliki variabel kategoris, sehingga perlu dilakukan label encoding ke nilai numerik agar dapat diproses model.

```python
# label encoding
le = LabelEncoder()
target = le.fit_transform(target)
target
```

```
array([0, 0, 0, ..., 0,
       1, 1, 1, ..., 1,
       2, 2, 2, ..., 2])
```

Label yang didapat: setosa = 0, versicolor = 1, virginica = 2.

Simpan target:

```python
y = target
```

### 4. Split data

Membagi dataset menjadi data pelatihan dan pengujian.

```python
# Splitting the data - 80:20 ratio
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, random_state=42)

print("Training split input- ", X_train.shape)
print("Testing split input- ", X_test.shape)
```

```
Training split input-  (120, 4)
Testing split input-  (30, 4)
```

### 5. Train model

Train model (pelatihan model) melibatkan penggunaan kumpulan data pelatihan untuk memperkirakan parameter model.

```python
# Defining the decision tree algorithm
dtree = DecisionTreeClassifier()
dtree.fit(X_train, y_train)

print('Decision Tree Classifer Created')
```

```
Decision Tree Classifer Created
```

Pada kode di atas, kita membuat sebuah objek dari kelas `DecisionTreeClassifier` dan menyimpannya ke dalam variabel `dtree`, sehingga objek tersebut dapat diakses menggunakan `dtree`. Selanjutnya, pohon keputusan dilatih menggunakan `X_train` dan `y_train`. Terakhir, dicetak pernyataan `Decision Tree Classifier Created` setelah pohon keputusan berhasil dibuat.

### 6. Predict result / Score model

Kita memperoleh akurasi sebesar 100% pada dataset pengujian yang berjumlah 30 data.

```python
# Predicting the values of test data
y_pred = dtree.predict(X_test)
print("Classification report - \n", classification_report(y_test, y_pred))
```

```
Classification report -
               precision    recall  f1-score   support

           0       1.00      1.00      1.00        10
           1       1.00      1.00      1.00         9
           2       1.00      1.00      1.00        11

    accuracy                           1.00        30
   macro avg       1.00      1.00      1.00        30
weighted avg       1.00      1.00      1.00        30
```

### 7. Evaluasi model

Plot confusion matrix:

```python
cm = confusion_matrix(y_test, y_pred)
plt.figure(figsize=(5, 5))

sns.heatmap(data=cm, linewidths=.5, annot=True, square=True, cmap='Blues')

plt.ylabel('Actual label')
plt.xlabel('Predicted label')

all_sample_title = 'Accuracy Score: {0}'.format(dtree.score(X_test, y_test))
plt.title(all_sample_title, size=15)

plt.savefig("one.png")
```

Output (Accuracy Score: 1.0):

```
                 Predicted label
                  0     1     2
Actual   0       10     0     0
label    1        0     9     0
         2        0     0    11
```

### 8. Visualization

Tampilkan plot dari pohon yang telah dibuat menggunakan perintah berikut.

```python
# Visualising the graph without the use of graphviz
plt.figure(figsize=(20, 20))
dec_tree = plot_tree(decision_tree=dtree, feature_names=df1.columns,
                     class_names=["setosa", "versicolor", "virginica"],
                     filled=True, precision=4, rounded=True)

plt.savefig("two.png")
```

Struktur pohon yang dihasilkan pada modul (data latih 120 sampel):

```
petal_width <= 0.8 ? (gini=0.6665, samples=120, value=[40,41,39])
|-- True : leaf setosa (gini=0.0, samples=40, value=[40,0,0])
`-- False: petal_length <= 4.75 ? (gini=0.4997, samples=80, value=[0,41,39])
    |-- True : petal_width <= 1.65 ? (gini=0.0526, samples=37, value=[0,36,1])
    |   |-- True : leaf versicolor (samples=36, value=[0,36,0])
    |   `-- False: leaf virginica  (samples=1,  value=[0,0,1])
    `-- False: petal_width <= 1.75 ? (gini=0.2055, samples=43, value=[0,5,38])
        |-- True : petal_length <= 4.95 ? (gini=0.5, samples=8, value=[0,4,4])
        |   |-- True : leaf versicolor (samples=2, value=[0,2,0])
        |   `-- False: petal_width <= 1.55 ? (gini=0.4444, samples=6, value=[0,2,4])
        |       |-- True : leaf virginica (samples=3, value=[0,0,3])
        |       `-- False: sepal_length <= 6.95 ? (gini=0.4444, samples=3, value=[0,2,1])
        |           |-- True : leaf versicolor (samples=2, value=[0,2,0])
        |           `-- False: leaf virginica  (samples=1, value=[0,0,1])
        `-- False: petal_length <= 4.85 ? (gini=0.0555, samples=35, value=[0,1,34])
            |-- True : sepal_width <= 3.1 ? (gini=0.4444, samples=3, value=[0,1,2])
            |   |-- True : leaf virginica  (samples=2, value=[0,0,2])
            |   `-- False: leaf versicolor (samples=1, value=[0,1,0])
            `-- False: leaf virginica (samples=32, value=[0,0,32])
```

Kita dapat melihat bagaimana pohon terbagi, nilai Gini Index (GI) pada setiap node, jumlah data dalam node tersebut, serta labelnya.

Catatan: pada kode asli modul, `class_names` tertulis `"vercicolor"` dan `"verginica"` (typo). Versi di atas sudah dikoreksi menjadi `"versicolor"` dan `"virginica"`.
