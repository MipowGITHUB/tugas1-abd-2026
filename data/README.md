# Data Tugas 1

## Dataset yang Dipilih

| Item | Isi |
|---|---|
| Nama dataset | `akhyar016/dgph-covid19-social-media` |
| Sumber | [Hugging Face Datasets](https://huggingface.co/datasets/akhyar016/dgph-covid19-social-media) (diunggah oleh `akhyar016`; konten asli berasal dari Reddit) |
| Lisensi/ketentuan pakai | Tidak dicantumkan pada dataset card. Konten berasal dari Reddit sehingga tunduk pada ketentuan Reddit; dipakai hanya untuk keperluan akademik tugas ini. |
| Ukuran | 9.604.309 baris x 9 kolom; ±4,9 GiB (5.272.680.031 byte) dalam 36 shard parquet |
| Periode data | Sampel yang dianalisis: 2020-01-28 s.d. 2022-12-16. Rentang penuh belum diperiksa (hanya sampel). |
| Unit analisis | Satu baris = satu submission atau komentar Reddit bertema COVID-19 |

> **Catatan:** data utama berasal dari subreddit komunitas lokal dan tematik (mayoritas AS, juga Inggris, Jerman, Australia, Brasil), bukan data khusus Indonesia.

## Skema Kolom

| Kolom | Tipe | Keterangan |
|---|---|---|
| `id` | string | ID unik konten, berprefiks sumber (mis. `reddit_t3_...`) |
| `content` | list<struct{text, title}> | Isi teks dan judul |
| `timestamp` | timestamp[ns, UTC] | Waktu posting |
| `source` | string | Subreddit asal (mis. `reddit_coronavirusga`) |
| `url` | string | Path konten di Reddit |
| `parent_id` | string | ID induk (untuk komentar) |
| `content_type` | string | `reddit_submission` atau `reddit_comment` |
| `submission_id` | string | ID submission asal thread |
| `metadata` | string (JSON) | Skor, jumlah komentar, subreddit, penulis, dll. |

## Strategi Data di Repository Ini

Dataset penuh **tidak diunduh dan tidak di-commit** (batas file GitHub 100 MB). Yang disimpan di repository:

| File | Isi | Di Git? |
|---|---|---|
| `data/preview/dgph_preview_sample.parquet` | Sampel ±10.000 baris (3,1 MB), diambil lewat API `datasets-server` Hugging Face dari 100 blok yang tersebar merata | Ya |
| `data/processed/dgph_bersih.parquet` | Hasil cleaning sampel (9.753 baris), dibuat oleh `02_data_cleaning.ipynb` | Tidak (di-ignore) |
| `data/raw/*.parquet` | Shard penuh bila diunduh manual | Tidak (di-ignore) |

Sampel berasal dari blok baris berurutan, **bukan sampel acak**, sehingga jumlah baris per subreddit atau per bulan tidak merepresentasikan volume sebenarnya.

## Urutan Menjalankan Notebook

1. `notebooks/01_data_profiling.ipynb`: mengambil metadata dan membuat sampel preview (butuh internet; dilewati bila preview sudah ada).
2. `notebooks/02_data_cleaning.ipynb`: cleaning dan validasi, menghasilkan `data/processed/dgph_bersih.parquet`.
3. `notebooks/03_eda_and_insights.ipynb`: analisis temporal, 6 visualisasi, dan insight.

## Tempat Mencari Dataset

Pilih dataset Indonesia yang legal digunakan, dapat didokumentasikan sumbernya, dan memenuhi batas ukuran tugas.

| Situs | Kegunaan |
|---|---|
| [Satu Data Indonesia](https://data.go.id/) | Portal data terbuka lintas instansi pemerintah Indonesia. |
| [Badan Pusat Statistik](https://www.bps.go.id/) | Statistik sosial, ekonomi, kependudukan, dan data wilayah. |
| [BMKG Data Online](https://dataonline.bmkg.go.id/) | Data cuaca, iklim, gempa bumi, dan observasi meteorologi. |
| [Hugging Face Datasets](https://huggingface.co/datasets) | Dataset publik yang dapat dicari berdasarkan topik, bahasa, atau ukuran. |
| [Kaggle Datasets](https://www.kaggle.com/datasets) | Katalog dataset publik; periksa lisensi dan dokumentasi pembuatnya. |
| [Google Dataset Search](https://datasetsearch.research.google.com/) | Mesin pencari untuk menemukan dataset dari berbagai portal. |

## Cara Memperoleh Data

**Sampel (default, otomatis):** jalankan `notebooks/01_data_profiling.ipynb`; sampel dibuat ke `data/preview/`.

**Dataset penuh (opsional, lokal):** unduh shard parquet dari halaman Hugging Face di atas
(daftar URL tiap shard dicetak oleh notebook 01) ke `data/raw/` tanpa mengubah isinya. Notebook 02 otomatis memakai
`data/raw/*.parquet` bila ada. Catat nama file dan checksum bila tersedia.
s
## Aturan Penyimpanan

- Jangan commit dataset mentah atau hasil olahan berukuran besar ke Git.
- File pada `data/raw/` adalah data asli dan tidak boleh diubah.
- Simpan hasil transformasi yang dapat direproduksi pada `data/processed/`.
