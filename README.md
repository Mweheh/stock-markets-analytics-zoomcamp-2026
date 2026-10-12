# Stock Markets Analytics Zoomcamp 2026

Repo ini khusus **Stock Markets Analytics Zoomcamp 2026** dari [DataTalksClub](https://github.com/DataTalksClub/stock-markets-analytics-zoomcamp). Tidak dicampur dengan Machine Learning Zoomcamp.

## HW1 — Introduction and Financial Data Sources

- [Notebook solusi/reproduksi (belum dieksekusi)](hw01/hw01.ipynb)
- [Jawaban dan status validasi](hw01/answers.json)
- [Catatan implementasi](hw01/README.md)
- [Instruksi resmi](https://github.com/DataTalksClub/stock-markets-analytics-zoomcamp/blob/main/cohorts/2026/homework1.md)
- [Form submission](https://courses.datatalks.club/sma-zoomcamp-2026/homework/hw01)

| Soal | Materi | Jawaban submission |
| --- | --- | --- |
| Q1 | Tahun penambahan konstituen S&P 500 terbanyak | Belum diverifikasi |
| Q2 | Indeks dunia yang mengungguli S&P 500 YTD | Belum diverifikasi |
| Q3 | Median S&P 500 correction drawdown | Belum diverifikasi |
| Q4 | Reaksi harga AMZN terhadap earnings surprise | Belum diverifikasi |

Notebook mencakup pengambilan data, perhitungan dan ringkasan hasil. Karena sumber data pasar dapat berubah, output harus diperiksa sebelum diisi sebagai jawaban resmi.

## HW2 — Working with Data in Pandas

- [Notebook solusi/reproduksi (belum dieksekusi)](hw02/hw02.ipynb)
- [Jawaban dan status validasi](hw02/answers.json)
- [Catatan implementasi](hw02/README.md)
- [Instruksi resmi](https://github.com/DataTalksClub/stock-markets-analytics-zoomcamp/blob/main/cohorts/2026/homework2.md)
- [Form submission](https://courses.datatalks.club/sma-zoomcamp-2026/homework/hw02)

| Soal | Materi | Jawaban submission |
| --- | --- | --- |
| Q1 | Kategori withdrawn IPO bernilai tertinggi | Belum diverifikasi |
| Q2 | Median Sharpe ratio IPO 2025 | Belum diverifikasi |
| Q3 | Periode holding IPO optimal | Belum diverifikasi |
| Q4 | Profit strategi RSI < 30 | Belum diverifikasi |

Notebook mengikuti sumber `iposcoop.com` untuk IPO dan memakai data Parquet resmi kursus pada Q4. Data besar dan data Yahoo Finance tidak disalin ke repository. Sumber diperoleh ketika notebook dijalankan.

## HW3 — Categorical Features and Decision Trees

- [Notebook pendamping audit HW3](hw03/hw03.ipynb)
- [Google Colab yang telah dijalankan pengguna](https://colab.research.google.com/drive/1i0mKulJ-JJ4ApGKL1rwjaqHejR3wRDKd?usp=sharing)
- [Jawaban serta bukti output yang disalin](hw03/answers.json)
- [Panduan audit Q3](hw03/Q3_AUDIT.md)
- [Penjelasan dan jawaban eksploratif Q5](hw03/README.md)
- [Instruksi resmi](https://github.com/DataTalksClub/stock-markets-analytics-zoomcamp/blob/main/cohorts/2026/homework3.md)
- [Form submission](https://courses.datatalks.club/sma-zoomcamp-2026/homework/hw03)

| Soal | Hasil Colab yang dilaporkan | Status |
| --- | --- | --- |
| Q1 | **0.025** (`month_wom_October_w4`) | Cocok dengan opsi |
| Q2 | **0.588** (`pred3_manual_dgs10_5`) | Cocok dengan opsi |
| Q3 | **1243** | **TIDAK cocok** dengan opsi 271/571/1171/1571; perlu audit |
| Q4 | **4** (`TEST precision = 0.62917`) | Cocok dengan opsi |
| Q5 | Proposal indikator tambahan | Draft eksploratif tersedia |

Angka HW3 berasal dari output Colab yang diberikan pengguna, **belum diperiksa ulang dari file notebook asli**. Notebook dalam repo merupakan *audit companion*, bukan salinan dari Colab yang telah dieksekusi.

## Menjalankan ulang

Gunakan Python yang kompatibel dengan dependensi di `requirements.txt` atau Google Colab. Jalankan notebook HW1/HW2 sesuai urutan dan periksa perubahan sumber. HW3 perlu **notebook modul resmi 2026** untuk menciptakan `new_df` dan prediksi; notebook audit di repo tidak berdiri sendiri.

```bash
python -m pip install -r requirements.txt
```

**Status reproduksibilitas:** notebook HW1/HW2 baru disiapkan dan belum dieksekusi; output dan pilihan jawaban belum diverifikasi. HW3 memiliki output yang dilaporkan, tetapi Q3 masih bermasalah. Repository ini **tidak otomatis melakukan submission**.
