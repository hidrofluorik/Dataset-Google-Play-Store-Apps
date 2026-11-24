# Segmentasi Aplikasi Google Play Store Menggunakan Algoritma K-Means

Proyek Akhir Mata Kuliah Pembelajaran Mesin (INF62325)  
Kelas 5C · Program Studi S1 Informatika  
Universitas Islam Negeri Siber Syekh Nurjati Cirebon

## 1. Gambaran Proyek

Proyek ini menerapkan algoritma **K-Means** untuk mengelompokkan aplikasi pada Google Play Store berdasarkan karakteristik penilaian dan popularitas aplikasi.

**Pertanyaan analitik:**

> Bagaimana aplikasi pada Google Play Store dapat dikelompokkan berdasarkan karakteristik rating, jumlah ulasan, dan jumlah pemasangan, serta kelompok mana yang menunjukkan karakteristik aplikasi berkualitas?

## 2. Dataset

- **Nama paket:** Google Play Store Apps (5C-9)
- **Sumber:** Google Play Store Apps — Kaggle
- **Ukuran awal:** 10.841 baris dan 13 kolom
- **Kolom target:** Tidak ada, karena proyek menggunakan pembelajaran tanpa pengawasan (*unsupervised learning*) dengan K-Means.

### Fitur yang digunakan

| Kolom | Keterangan | Peran |
|---|---|---|
| `Rating` | Nilai penilaian aplikasi dari pengguna, dengan skala 1–5 | Fitur |
| `Reviews` | Jumlah ulasan pengguna terhadap aplikasi | Fitur |
| `Installs` | Jumlah atau kisaran pemasangan aplikasi | Fitur |

### Temuan eksplorasi awal

Berdasarkan pemeriksaan awal pada dataset:

- Terdapat 1.487 nilai kosong: 1.474 pada kolom `Rating` dan 13 pada kolom lainnya.
- Terdapat 483 baris duplikat.
- Ditemukan satu nilai `Rating` tidak valid, yaitu 19, karena rentang rating yang digunakan adalah 1–5.
- Kolom `Reviews` dan `Installs` perlu dipastikan bertipe numerik. Kolom `Installs` perlu dibersihkan dari tanda koma dan tanda tambah (`+`).

Berkas mentah disimpan sebagai `data/raw.csv` dan harus dipertahankan tanpa perubahan. Hasil penyiapan data bersama disimpan sebagai `data/data_bersih.csv`.

## 3. Pembagian Eksperimen

Setiap anggota mengerjakan satu varian eksperimen sesuai proposal kelompok.

| Anggota | Nama | Varian dan pekerjaan |
|---|---|---|
| 1 | Nurkhotimah | **Varian dasar:** K-Means menggunakan `Rating`, `Reviews`, dan `Installs` tanpa transformasi logaritma. Jumlah klaster ditentukan dengan metode Elbow untuk K = 2–10. |
| 2 | Sabbicarel Edward Piris | **Varian perlakuan data:** menerapkan transformasi `log1p` pada `Reviews` dan `Installs`, lalu membandingkan hasil klaster dengan data tanpa transformasi menggunakan parameter K-Means yang sama. |
| 3 | Mohamad Fariz Rachman Pratama | **Varian pemilihan fitur:** membandingkan fitur lengkap (`Rating`, `Reviews`, `Installs`) dengan kombinasi `Rating` dan `Installs`, menggunakan K = 3 dan prosedur lainnya yang sama. |

Setelah ketiga varian selesai, kelompok menyusun tabel perbandingan dan menentukan model yang akan digunakan pada tahap berikutnya.

## 4. Urutan Penyiapan Data

Penyiapan data dilakukan **satu kali oleh kelompok**. Ketiga anggota menggunakan berkas `data/data_bersih.csv` yang sama agar hasil eksperimen dapat dibandingkan secara adil.

Pada klastering tidak ada pemisahan data latih dan data uji, karena tidak ada label yang diuji. Pembersihan dilakukan sekali atas seluruh data: buang duplikat, pisahkan kolom identitas, buang kolom non-numerik, imputasi median, lalu pembatasan pencilan dengan IQR 1,5. Hasilnya disimpan sebagai `data_bersih.csv` dan dipakai ketiga varian apa adanya. Penskalaan tidak dilakukan pada tahap pembersihan, melainkan di dalam tiap varian dan sekali lagi saat model final dilatih: `StandardScaler` di-fit satu kali pada data bersih, lalu disimpan sebagai `scaler.pkl` dan dipakai aplikasi web dengan `transform`, bukan `fit_transform`. Pengodean kategori tidak dipakai, karena pada K-Means hanya fitur numerik yang dipakai.

**Penyesuaian untuk dataset ini:** sebelum fitur dipilih untuk pemodelan, `Reviews` dan `Installs` perlu dikonversi ke bentuk numerik. Tanda koma dan tanda tambah pada `Installs` dibersihkan terlebih dahulu. Pernyataan urutan kerja di atas merupakan draf berdasarkan sel 1.9 notebook Pertemuan 12 dan **harus diperiksa serta disesuaikan dengan langkah yang benar-benar dijalankan kelompok** sebelum dinyatakan final.

Transformasi `log1p` pada `Reviews` dan `Installs` merupakan perlakuan khusus Varian 2, bukan bagian dari berkas data bersih bersama.

## 5. Struktur Repository

```text
proyek-akhir-klastering/
├── README.md
├── data/
│   ├── raw.csv
│   ├── data_bersih.csv
│   ├── tabel_perbandingan.md
│   ├── tabel_perbandingan.csv
│   └── hasil_varian/
│       ├── varian1.json
│       ├── varian2.json
│       └── varian3.json
├── gambar/
│   ├── eda_distribusi.png
│   ├── eda_korelasi.png
│   ├── varian1_elbow.png
│   ├── varian2_silhouette.png
│   └── varian3_pemilihan_fitur.png
├── notebook/
│   ├── 01_penyiapan_data.ipynb
│   ├── 03_varian_1.ipynb
│   ├── 03_varian_2.ipynb
│   ├── 03_varian_3.ipynb
│   └── 04_perbandingan.ipynb
└── laporan/
    └── proposal.pdf
```

Berkas yang belum dihasilkan akan ditambahkan setelah tahap pengerjaannya selesai. Nama notebook akhir mengikuti ketentuan pada manual perkuliahan.

## 6. Checklist Luaran

### Pertemuan 12 — Penyiapan Data

- [ ] `data/raw.csv` tersedia dan tidak diubah.
- [ ] `data/data_bersih.csv` tersedia sebagai berkas di repository.
- [ ] `gambar/eda_distribusi.png` tersedia.
- [ ] `gambar/eda_korelasi.png` tersedia.
- [ ] Empat jawaban eksplorasi pada sel 1.6 sudah diisi.
- [ ] Pernyataan urutan kerja sudah diverifikasi dan sesuai dengan proses sebenarnya.
- [ ] Notebook disimpan sebagai `notebook/01_penyiapan_data.ipynb`.
- [ ] `data_bersih.csv` sudah dibagikan kepada seluruh anggota.

### Pertemuan 13 — Tiga Varian dan Perbandingan

- [ ] Hasil Varian 1: notebook, `varian1.json`, dan gambar Elbow.
- [ ] Hasil Varian 2: notebook, `varian2.json`, dan gambar Silhouette.
- [ ] Hasil Varian 3: notebook, `varian3.json`, dan gambar pemilihan fitur.
- [ ] Tabel perbandingan tiga varian tersedia dalam format Markdown dan CSV.
- [ ] Paragraf pemilihan model ditambahkan ke README setelah hasil ketiga varian dibandingkan.

## 7. Catatan

- Jangan mengubah `data/raw.csv`; simpan hasil pembersihan pada `data/data_bersih.csv`.
- Jangan membersihkan dataset secara terpisah untuk masing-masing anggota.
- Jangan menganggap berkas yang hanya tersimpan di Google Colab sudah masuk ke repository. Luaran harus diunggah dan di-commit ke repository.
- Perbarui checklist dan bagian urutan kerja setelah hasil pengerjaan benar-benar tersedia.
