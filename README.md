# 🎓 IF25-40305: Sistem Teknologi Multimedia (STM)
**Program Studi Teknik Informatika — Fakultas Teknologi Industri**  
**Institut Teknologi Sumatera (ITERA)**

Repositori ini merupakan dokumentasi terpusat untuk seluruh pengerjaan tugas, eksperimen praktikum (*hands-on*), serta analisis mandiri pada mata kuliah **Sistem Teknologi Multimedia (IF25-40305)**.

---

## 👨‍🏫 Informasi Identitas

| Kategori | Keterangan |
| :--- | :--- |
| **Mata Kuliah** | Sistem Teknologi Multimedia (STM) |
| **Kode Mata Kuliah** | IF25-40305 |
| **Dosen Pengampu** | **Martin Clinton Toshima Manullang, S.T., M.T., Ph.D.** |
| **Nama Mahasiswa** | Muhammad Romadhon S |
| **NIM** | 123140031 |
| **Institusi** | Institut Teknologi Sumatera (ITERA) |

---

## 📚 Daftar Tugas yang Ada di Repository

Berikut adalah daftar direktori tugas yang terdapat di dalam repositori ini (klik pada tautan folder untuk melihat kode *notebook* dan laporan analisis masing-masing tugas):

| No | Nama Tugas / Modul | Direktori | Topik & Fokus Pembahasan | Status |
| :-: | :--- | :--- | :--- | :-: |
| 2 | **Tugas 2 — Analisis Sinyal Suara & Noise Statis** | [`./02_audio_noise_statis`](./02_audio_noise_statis) | Akuisisi audio, Visualisasi 4 Dimensi (Waveform, FFT dBFS, STFT, Mel-Spektrogram), serta Eksperimen Resampling & Pembuktian Aliasing. | ✅ Selesai |

---

## 🔍 Ringkasan Tugas yang Telah Dikerjakan

### 📌 [Tugas 2: Analisis Sinyal Suara & Noise Statis](./02_audio_noise_statis)
Menganalisis rekaman suara pembacaan berita (`audio_original.wav`, 48.000 Hz, durasi 23,20 detik) yang direkam menggunakan *smartphone* Xiaomi Redmi Note 10s pada jarak $\pm 0,15\text{ m}$ di depan sumber *noise* statis (kipas angin):
* **Akuisisi & Ekstraksi Metadata:** Perekaman menghasilkan rentang amplitudo `-0,4606` hingga `0,5250` dengan puncak absolut `-5,60 dBFS` (bebas dari distorsi *clipping*).
* **Visualisasi Audio 4 Dimensi:**
  * **Waveform:** Memisahkan segmen vokal aktif (`82,9%`) dan segmen hening/*noise floor* (`17,1%`) menggunakan ambang energi `3,9 dB`.
  * **Spektrum FFT (dBFS):** Mengidentifikasi frekuensi dominan *noise* statis kipas angin pada **$\approx 117\text{ Hz}$** (`-38 dBFS`) dan pita vokal utama pada **80 Hz – 3.500 Hz**.
  * **Spektrogram STFT:** Memetakan *noise* statis sebagai garis horizontal konstan di `117 Hz` sepanjang waktu dan vokal sebagai struktur harmonik vertikal yang dinamis.
  * **Mel-Spektrogram:** Memetakan frekuensi fisik ke skala perseptual Mel (`128 bin Mel`) sehingga struktur *formants* vokal terlihat jauh lebih padat dan jelas sesuai kepekaan telinga manusia.
* **Eksperimen Resampling & Aliasing ($48\text{ kHz} \rightarrow 8\text{ kHz}$):** Membuktikan munculnya pantulan frekuensi palsu (*aliasing*) pada metode *Naive Decimation* (`y[::6]`) di rentang `2.000 – 4.000 Hz`, serta membuktikan efektivitas peredaman *aliasing* menggunakan filter *Low-Pass* pada `scipy.signal.resample_poly`.

---

## 📂 Struktur Direktori Repositori

```text
📦 IF25-40305-Sistem-Teknologi-Multimedia
 ┣ 📂 02_audio_noise_statis/             # Direktori Tugas 2: Analisis Audio & Noise Statis
 ┃ ┣ 📜 tugas_audio_noise_statis.ipynb   # Jupyter Notebook utama Tugas 2
 ┃ ┣ 📜 tugas_audio_noise_statis.html    # Hasil ekspor HTML
 ┃ ┣ 📜 tugas_audio_noise_statis.pdf     # Hasil ekspor PDF
 ┃ ┣ 🔊 audio_original.wav               # Rekaman suara asli (48 kHz)
 ┃ ┣ 🔊 audio_downsampled_naive.wav      # Audio hasil Naive Decimation (8 kHz)
 ┃ ┣ 🔊 audio_downsampled_clean.wav      # Audio hasil Resample Poly berfilter (8 kHz)
 ┃ ┗ 📜 README.md                        # Dokumentasi khusus Tugas 2
 ┗ 📜 README.md                          # Dokumentasi utama repositori (berkas ini)
```

---

## 🛠️ Persiapan Lingkungan & Instalasi

Seluruh tugas pemrosesan sinyal dan multimedia pada repositori ini dijalankan menggunakan **Python 3** dan **Jupyter Notebook / JupyterLab**.

1. **Clone repositori ini:**
   ```bash
   git clone <url-repositori-kamu>
   cd <nama-folder-repositori>
   ```

2. **Instal pustaka Python yang dibutuhkan:**
   ```bash
   pip install numpy matplotlib scipy librosa soundfile ipython jupyter
   ```

3. **Jalankan Jupyter Notebook:**
   ```bash
   jupyter notebook
   ```
   Pilih folder tugas yang ingin dibuka (misalnya `02_audio_noise_statis/tugas_audio_noise_statis.ipynb`), lalu jalankan seluruh sel kode (*Run All*).

---
*Disusun oleh **Muhammad Romadhon S (123140031)** — Teknik Informatika ITERA.*
