---
title: 210411100001_Achmad Baharuddin Akbar_Skip Gram

---

# 210411100001 - Achmad Baharuddin Akbar | Penambangan Web

# Word Embedding
Word embedding adalah representasi kata dalam bentuk vektor yang dapat menangkap konteks kata dalam dokumen.Teknik ini adalah bagian fundamental dari pemrosesan bahasa alami (NLP).

Dalam representasi word embedding, setiap kata diwakili oleh sebuah vektor dalam ruang vektor berdimensi tinggi (misalnya, 100, 200, 300 dimensi, dll.).

# Skip-Gram

![image](https://hackmd.io/_uploads/HJJW8X40C.png)

Pada dasarnya diagram ini menjelaskan bahwa model tersebut menggunakan jaringan saraf dengan satu lapisan tersembunyi (proyeksi) untuk memprediksi kata konteks dengan benar.

Inti dari skip-gram adalah: diberikan satu kata (kata target), model akan mencoba menebak kata-kata yang ada di sekitarnya (kata konteks).

**Contoh :**

"ibu saya pergi ke pasar menaiki motor"

1. Menentukan kata target dan kata-kata konteks. 
-  Setiap kata akan direpresentasikan sebagai vektor biner dengan panjang yang sama dengan jumlah kata dalam kalimat
2. Kata-kata unik (vocabulary) dalam kalimat ini:
"ibu","saya","pergi","ke","pasar","menaiki","motor"
Total kata unik : 7

Berikut representasi One Hot Encoding :

|  | ibu | saya | pergi | ke | pasar | menaiki | motor |
| --------- | --- | --- | --- | ------- | --------- | ------ | ----- |
| ibu       | 1   | 0   | 0   | 0       | 0         | 0      | 0     |
| saya       | 0   | 1   | 0   | 0       | 0         | 0      | 0     |
| pergi       | 0   | 0   | 1   | 0       | 0         | 0      | 0     |
| ke   | 0   | 0   | 0   | 1       | 0         | 0      | 0    |
| pasar | 0   | 0   | 0   | 0       | 1         | 0      | 0 |
| menaiki    | 0   | 0   | 0   | 0       | 0         | 1      | 0     |
| motor     | 0   | 0   | 0   | 0       | 0         | 0      | 1    |

3. Membuat pasangan kata target dan konteks

| Kata Target  | Kata Konteks         |
|--------------|----------------------|
| ibu          | saya                 |
| saya         | ibu, pergi           |
| pergi        | saya, ke             |
| ke           | pergi, pasar         |
| pasar        | ke, menaiki          |
| menaiki      | pasar, motor         |
| motor        | menaiki              |


## Membuat Word Embedding

Setelah mendapatkan pasangan kata, gunakan word embedding untuk mengonversi setiap kata menjadi vektor. Misalkan kita tentukan ukuran dimensi vektor embedding adalah 3

buat nilai embedding acak untuk setiap kata sebagai berikut :

| Kata         | Word Embedding       |
|--------------|----------------------|
| ibu          | [0.1,0.2,0.3]        |
| saya         | [0.4,0.5,0.6]        |
| pergi        | [0.7,0.8,0.9]        |
| ke           | [0.3,0.2,0.1]        |
| pasar        | [0.6,0.5,0.4]        |
| menaiki      | [0.9,0.8,0.7]        |
| motor        | [0.8,0.2,0.1]        |

---
### Iterasi ke 1
sekarang akan menghitung representasi untuk kata "saya" menggunakan tetangga kiri dan kanan yaitu 'ibu' dan 'pergi'
1. untuk "saya" dengan tetangga kiri "ibu":
        Vector "saya" = [0.4,0.5,0.6]
        Vector "ibu" = [0.1,0.2,0.3]
        
2. Untuk "saya" dengan tetangga kanan "pergi":
        Vector "pergi" = [0.7,0.8,0.9]

Vector saya = ![image](https://hackmd.io/_uploads/rJBZUQVC0.png)
     
Kesimpulan
Perhitungan di atas merupakan bagian dari iterasi pertama, di mana kita menggunakan pasangan kata-tetangga untuk membentuk representasi awal dari kata. Setelah beberapa iterasi, representasi vektor akan diperbarui dan menjadi lebih akurat dalam merepresentasikan hubungan semantik antar kata.

---
### Iterasi ke 2
Menggunakan contoh kata sama dengan kata target "saya" dan tetangganya "ibu" serta "pergi". Misalkan setelah iterasi pertama, kita menghitung error dan ingin memperbarui vektor embedding untuk kata "saya".

1. Vektor Awal dari Iterasi Pertama: Pada iterasi pertama, kita telah menghitung representasi sementara sebagai berikut:
ibu : [0.1,0.2,0.3]
saya: [0.4,0.5,0.6]
pergi : [0.7,0.8,0.9]

Perbarui vektor embedding untuk kata target "saya" dengan memperhitungkan error

Misalnya, untuk pasangan "saya" dengan tetangga "ibu" dan "pergi", kita perbarui embedding "saya" menggunakan rumus:

Rumus: vektor_saya=vektor_saya−learning rate×gradien error

1. Gradien error mengacu pada perbedaan antara prediksi model dan hasil yang diharapkan. Misalkan gradien yang dihasilkan adalah 0.05 untuk setiap dimensi vektor.
2. Pembaruan dilakukan berdasarkan gradient descent, di mana bobot embedding diubah sedikit untuk memperbaiki error. Misalkan kita menggunakan learning rate sebesar 0.1

Untuk perhitungannya

a. Pembaruan vektor "saya"

Dimensi pertama vector saya:
- vektor_saya_baru=0.4−(0.1×0.05)=0.395

Dimensi kedua vector saya:
- vektor_saya_baru=0.5−(0.1×0.05)=0.495

Dimensi Ketiga vector saya:
- vektor_saya_baru=0.6−(0.1×0.05)=0.595

Setelah pembaruan untuk pasangan pertama, vektor embedding untuk "saya" adalah:[0.395,0.495,0.595]


b. Pembaruan vektor "ibu"

Dimensi pertama vector ibu
- vektor_ibu_baru=0.1−(0.1×0.05)=0.095

Dimensi kedua vector ibu
- vektor_ibu_baru=0.2−(0.1×0.05)=0.195

Dimensi ketiga vector ibu
- vektor_ibu_baru=0.3−(0.1×0.05)=0.295

Hasil Vektor "ibu" setelah Iterasi Kedua =[0.095,0.195,0.295]

c. Pembaruan vektor "pergi"

Dimensi pertama vector pergi
- vektor_pergi_baru=0.7−(0.1×0.05)=0.695

Dimensi kedua vector pergi
- vektor_pergi_baru=0.8−(0.1×0.05)=0.795

Dimensi ketiga vector pergi
- vektor_pergi_baru=0.9−(0.1×0.05)=0.895

Setelah pembaruan untuk tetangga "pergi" =[0.695,0.795,0.895]

Rekapitulasi Vektor Embedding 
Setelah iterasi kedua, berikut adalah vektor embedding yang diperbarui untuk kata "saya", "ibu", dan "pergi":
- Vektor "saya": [0.395,0.495,0.595]
- Vektor "pergi": [0.695,0.795,0.895]
- Vektor "ibu": [0.095,0.195,0.295]

**Pembahasan**
Pada iterasi kedua, kita telah memperbarui vektor embedding untuk kata "saya", "ibu", dan "pergi" berdasarkan hasil dari iterasi pertama dengan menggunakan backpropagation dan pembaruan bobot (embedding).

---
### Iterasi ke 3
Pada iterasi ketiga, melanjutkan pembaruan vektor embedding untuk kata "saya", "ibu", dan "pergi" berdasarkan hasil dari iterasi kedua.

Vektor embedding setelah iterasi kedua:

Vektor "saya": [0.395,0.495,0.595]
Vektor "ibu": [0.095,0.195,0.295]
Vektor "pergi": [0.695,0.795,0.895]

Pembaruan pada Iterasi Ketiga
Untuk iterasi ketiga, kita asumsikan nilai gradien error tetap 0.05 per dimensi, dan learning rate tetap 0.1 .

a. Pembaruan Vektor "saya"

Dimensi pertama vektor "saya":
- vektor_saya_baru=0.395−(0.1×0.05)=0.39

Dimensi kedua vektor "saya":
- vektor_saya_baru=0.495−(0.1×0.05)=0.49

Dimensi ketiga vektor "saya":
- vektor_saya_baru=0.595−(0.1×0.05)=0.59

Jadi, setelah memperbarui vektor embedding untuk "saya", vektor "saya" menjadi:[0.39,0.49,0.59]

b. Pembaruan vector "ibu"

Dimensi pertama vektor "ibu":
- vektor_ibu_baru=0.095−(0.1×0.05)=0.09

Dimensi kedua vektor "ibu":
- vektor_ibu_baru=0.195−(0.1×0.05)=0.19

Dimensi ketiga vektor "ibu":
- vektor_ibu_baru=0.295−(0.1×0.05)=0.29

Setelah pembaruan, vektor embedding "ibu" menjadi: [0.09,0.19,0.29]

c. Pembaruan Vektor "pergi"

Dimensi pertama vektor "pergi":
- vektor_pergi_baru=0.695−(0.1×0.05)=0.69

Dimensi kedua vektor "pergi":
- vektor_pergi_baru=0.795−(0.1×0.05)=0.79

Dimensi ketiga vektor "pergi":
- vektor_pergi_baru=0.895−(0.1×0.05)=0.89

Setelah pembaruan, vektor embedding "pergi" menjadi: [0.69,0.79,0.89]

Rekapitulasi Vektor
Embedding Setelah Iterasi Ketiga
Setelah iterasi ketiga, berikut adalah vektor embedding yang diperbarui untuk kata "saya", "ibu", dan "pergi":

- Vektor "saya": [0.39,0.49,0.59]
- Vektor "ibu": [0.09,0.19,0.29]
- Vektor "pergi": [0.69,0.79,0.89]

**Pembahasan**
Pada iterasi ketiga, kita telah memperbarui vektor embedding untuk kata "saya", "ibu", dan "pergi" dengan menggunakan proses backpropagation yang didasarkan pada gradien error dari iterasi sebelumnya.