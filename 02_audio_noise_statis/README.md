# 🎙️ Tugas 2 — Analisis Sinyal Suara & Noise Statis
**IF25-40305: Sistem Teknologi Multimedia (STM)** **Program Studi Teknik Informatika — Institut Teknologi Sumatera (ITERA)**

---

## 👤 Identitas Mahasiswa
* **Nama:** Muhammad Romadhon S
* **NIM:** 123140031

---

## 📌 Deskripsi Tugas
Proyek ini bertujuan untuk menganalisis karakteristik sinyal suara pembacaan berita yang direkam secara langsung di depan sumber **noise statis** (kipas angin), serta membuktikan fenomena distorsi **aliasing** pada proses penurunan laju sampel (*downsampling*). 

Eksperimen dan analisis di dalam *notebook* mencakup tiga tahapan utama:
1. **Akuisisi Audio & Ekstraksi Metadata:** Memuat sinyal asli tanpa mengubah laju sampel bawaan (`sr=None`) dan mengekstraksi parameter fisik sinyal.
2. **Visualisasi Audio 4 Dimensi:** Membedah sinyal menggunakan **Waveform** (domain waktu), **Spektrum FFT Terkalibrasi (dBFS)** (domain frekuensi), **Spektrogram STFT** (waktu-frekuensi linier), dan **Mel-Spektrogram** (skala perseptual pendengaran manusia).
3. **Eksperimen Resampling & Pembuktian Aliasing:** Membandingkan hasil *downsampling* dari 48.000 Hz ke 8.000 Hz antara metode **Naive Decimation (`y[::6]`)** tanpa filter dan metode **Resampling Standar DSP (`scipy.signal.resample_poly`)** dengan filter *anti-aliasing*.

---

## 🎧 Spesifikasi Akuisisi & Metadata Rekaman

| Parameter | Nilai / Keterangan |
| :--- | :--- |
| **Nama Berkas Asli** | `audio_original.wav` |
| **Sumber Noise Statis** | Kipas angin |
| **Perangkat Perekam** | Smartphone Xiaomi Redmi Note 10s (Aplikasi Voice Recorder bawaan) |
| **Jarak ke Sumber Noise** | $\pm 0,15$ meter |
| **Sumber Bacaan Berita** | [Otodriver — Changan Nevo Q05 SUV Listrik](https://otodriver.com/berita/2026/changan-nevo-q05-suv-listrik-yang-harganya-sudah-masuk-wilayah-lmpv-chaefdiempv) |
| **Laju Sampel Asli ($f_s$)** | `48.000 Hz` (Batas Nyquist = `24.000 Hz`) |
| **Jumlah Sampel** | `1.113.600 sampel` |
| **Durasi Rekaman** | `23,20 detik` |
| **Rentang Amplitudo** | `-0,4606` hingga `0,5250` |
| **Puncak Absolut** | `0,5250` (`-5,60 dBFS`) |

---

## 📊 Ringkasan Hasil Analisis

### 1. Visualisasi Audio 4 Dimensi
* **Waveform (Amplitudo vs Waktu):** Menggunakan ambang batas energi (*threshold*) sebesar **3,9 dB** (jendela 50 ms, geser 10 ms), terdeteksi proporsi durasi **vokal aktif sebesar 82,9%** dan **segmen hening (*noise* statis) sebesar 17,1%**. Pada segmen hening, gelombang tidak menyempit ke titik 0,0 melainkan menyisakan ketebalan konstan ($\pm 0,05$ hingga $\pm 0,08$) yang merepresentasikan *static noise floor*.
* **Spektrum FFT (dBFS Terkalibrasi):** Hasil deteksi puncak pada kurva segmen hening menunjukkan bahwa *noise* statis mendominasi area frekuensi rendah dengan **frekuensi dominan pada $\approx 117\text{ Hz}$** (sekitar **-38 dBFS**). Sementara itu, kurva vokal aktif mendominasi rentang frekuensi bicara (**80 Hz – 3.500 Hz**) dengan selisih energi 10–12 dB di atas kurva *noise*.
* **Spektrogram STFT (`n_fft=2048`, `hop_length=512`, $\Delta f = 23,4\text{ Hz}$):** Memperlihatkan perbedaan jelas antara *noise* statis yang tampil sebagai **pita garis horizontal konstan di sekitar 117 Hz** sepanjang 23,20 detik, dengan suara vokal yang tampil sebagai pola harmonik vertikal dinamis di rentang **100 Hz – 3.000 Hz** serta konsonan desis (*sibilance*) yang menjulang hingga **4.000 Hz – 7.500 Hz**.
* **Mel-Spektrogram (`128 bin Mel` vs `1025 bin frekuensi linier`):** Pemetaan ke skala Mel meregangkan porsi ruang visual pada frekuensi rendah–menengah (`0 – 2.048 Hz`), sehingga struktur pita vokal manusia terlihat jauh lebih padat, lebar, dan jelas sesuai kepekaan alami telinga manusia dibandingkan spektrogram linier.

### 2. Eksperimen Resampling & Pembuktian Aliasing ($48.000\text{ Hz} \rightarrow 8.000\text{ Hz}$)
Penurunan laju sampel dengan faktor desimasi $M = 6$ menghasilkan `185.600 sampel` dengan batas frekuensi Nyquist baru sebesar **4.000 Hz**:
* **Skenario A — Naive Decimation (`y[::6]`):** Mengambil setiap sampel ke-6 secara langsung tanpa penyaringan frekuensi tinggi. Akibatnya, energi desis konsonan dan *noise* statis di atas 4.000 Hz terpantul balik (*spectral folding*) ke rentang **2.000 Hz – 4.000 Hz**, membuat spektrogram tampak lebih kotor dan suara terdengar lebih kasar.
* **Skenario B — Resampling Standar DSP (`resample_poly`):** Menerapkan *Low-Pass Filter* (LPF) *anti-aliasing* sebelum membuang sampel. Area atas spektrogram (**3.500 Hz – 4.000 Hz**) tampak jauh lebih gelap/bersih dari pantulan frekuensi tinggi, dan kualitas suara yang dihasilkan tetap bersih tanpa distorsi *aliasing*.

---

## 📂 Struktur Berkas Repositori

```text
02_audio_noise_statis/
├── tugas_audio_noise_statis.ipynb   # Notebook utama berisi kode & analisis lengkap
├── tugas_audio_noise_statis.html    # Ekspor notebook dalam format HTML
├── tugas_audio_noise_statis.pdf     # Ekspor notebook dalam format PDF
├── audio_original.wav               # Rekaman asli (48.000 Hz, 23,20 detik)
├── audio_downsampled_naive.wav      # Hasil Skenario A: Naive Decimation (8.000 Hz)
├── audio_downsampled_clean.wav      # Hasil Skenario B: Resample Poly / Anti-Aliasing (8.000 Hz)
└── README.md                        # Dokumentasi repositori