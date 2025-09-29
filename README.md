# Tenki Weather

> Chatbot cuaca berbahasa Indonesia dengan karakter anime **Tenki‑Chan**. Dibangun dengan **Unity 6 (6000.2.3f1)** untuk **WebGL only**. Terintegrasi **OpenAI**, **WeatherAPI.com**, **ElevenLabs (TTS)**, dan **Batch CSV/XLSX**.

[![Unity 6000.2.3f1](https://img.shields.io/badge/Unity-6000.2.3f1-black?logo=unity)](#)
[![Platform WebGL](https://img.shields.io/badge/Platform-WebGL-blue)](#)
[![License MIT](https://img.shields.io/badge/License-MIT-green.svg)](#lisensi)
[![Production Ready](https://img.shields.io/badge/Status-Production--ready-brightgreen)](#)

**Live Demo:** **[https://tenki.live](https://tenki.live)**
**Repo:** **[https://github.com/muhammadIdhamMaarif/Tenki-Weather](https://github.com/muhammadIdhamMaarif/Tenki-Weather)**

![Banner Tenki Weather (placeholder)](IMG_HERO)

Tema/brand: **Light Blue Sky**

---

## Galeri Pratinjau (placeholder)

> Ganti path gambar/GIF berikut dengan aset Anda. Jika belum ada, biarkan *(placeholder)* supaya mudah dilengkapi nanti.

| Pratinjau                                                        | Keterangan                                            |
| ---------------------------------------------------------------- | ----------------------------------------------------- |
| ![Alur chat dan balasan cuaca (placeholder)](GIF_CHAT_FLOW)      | `<GIF_CHAT_FLOW>` — alur chat & balasan cuaca         |
| ![Cuplikan JSON plan dari OpenAI (placeholder)](GIF_INTENT_PLAN) | `<GIF_INTENT_PLAN>` — cuplikan JSON “plan”            |
| ![Pemutaran TTS Bahasa Indonesia (placeholder)](GIF_TTS)         | `<GIF_TTS>` — pemutaran suara Indonesia               |
| ![Unggah CSV/XLSX (placeholder)](GIF_UPLOAD_CSV)                 | `<GIF_UPLOAD_CSV>` — unggah CSV/XLSX & status progres |
| ![Proses paralel & retry (placeholder)](GIF_PROCESSING)          | `<GIF_PROCESSING>` — proses paralel & retry           |
| ![Unduh hasil CSV/XLSX (placeholder)](GIF_DOWNLOAD)              | `<GIF_DOWNLOAD>` — unduh CSV/XLSX hasil               |
| ![Aplikasi WebGL di browser (placeholder)](GIF_WEBGL)            | `<GIF_WEBGL>` — aplikasi berjalan di browser          |

**Screenshot UI (placeholder):**

* ![Beranda UI (placeholder)](IMG_UI_HOME)
* ![Kartu cuaca (placeholder)](IMG_UI_WEATHER_CARD)
* ![Dialog batch (placeholder)](IMG_UI_BATCH_DIALOG)

---

## Fitur

* **Intent Weather vs Chitchat** — Penentuan intent melalui **OpenAI** dengan **schema JSON-only** yang ketat.
* **Cuaca saat ini & ramalan (forecast)** — Menggunakan **WeatherAPI.com**; resolusi lokasi via nama (mis. *Kecamatan Dukun*) atau koordinat **lat,lon**.
* **TTS Bahasa Indonesia** — **ElevenLabs** menghasilkan audio, diputar dengan **AudioSource** di Unity.
* **Batch Processor (CSV/XLSX)** — Unggah **CSV/XLSX**, proses paralel dengan **worker-pool** (`maxConcurrency`), **retry/backoff**, dan ekspor **CSV/XLSX** (writer minimal). Termasuk **WebGL file‑picker & download bridge**.
* **Tenki‑Chan Avatar** — Menggunakan **Animation Rigging**, **uLipSync**, **Magica Cloth 2** untuk ekspresi, lip‑sync, dan simulasi kain.
* **Platform** — **WebGL only** (Unity 6 **6000.2.3f1**). Status **Production‑ready**. Lisensi **MIT**.
* **Opsional** — **Unity Services (Cloud Code/Relay)** sebagai jalur aman alternatif untuk menyembunyikan kunci API (tanpa tutorial di README ini).

---

## Arsitektur Singkat

```mermaid
flowchart LR
    UI[UI (Unity/WebGL)] -->|prompt user| OpenAI[OpenAI\n(JSON plan)]
    OpenAI -->|intent: weather| WeatherAPI[WeatherAPI.com\n(current/forecast)]
    WeatherAPI --> UI
    UI -->|teks final| ElevenLabs[ElevenLabs\n(TTS, Bahasa Indonesia)]
    ElevenLabs --> UI

    %% Jalur alternatif (opsional)
    UI -.-> CloudCode[Unity Cloud Code / Relay\n(opsional, proxy kunci API)] -.-> OpenAI
    UI -.-> CloudCode -.-> WeatherAPI
    UI -.-> CloudCode -.-> ElevenLabs

    %% Sorot skrip utama
    subgraph Scripts
      A[TenkiChatController\n(pipeline chat → cuaca → script → TTS)]
      B[BatchWeatherProcessor\n(worker pool, CSV/XLSX)]
    end
```

---

## Teknologi

| Komponen             | Peran                                      |
| -------------------- | ------------------------------------------ |
| Unity 6 (6000.2.3f1) | Engine & build WebGL                       |
| TextMeshPro          | UI teks berkualitas                        |
| Michsky DreamOS      | Komponen UI/UX modern (dashboard/controls) |
| Animation Rigging    | Rigging animasi Tenki‑Chan                 |
| uLipSync             | Lip‑sync real‑time untuk dialog            |
| Magica Cloth 2       | Simulasi kain/aksesori karakter            |
| OpenAI API           | Intent planning & perangkai naskah         |
| WeatherAPI.com       | Data cuaca (current/forecast)              |
| ElevenLabs           | TTS Bahasa Indonesia                       |

---

## Mulai Cepat

### Prasyarat

* **Unity 6 (6000.2.3f1)**.
* Akun & **API key** untuk **OpenAI**, **WeatherAPI.com**, **ElevenLabs**.
* Target **WebGL only**.

### Buka Proyek

```bash
# clone repo
git clone https://github.com/muhammadIdhamMaarif/Tenki-Weather.git
cd Tenki-Weather
# buka dengan Unity Hub, pilih Editor 6000.2.3f1
```

### Konfigurasi Kunci (Direct API)

Atur di **Inspector → TenkiChatController**:

| Field                                      | Contoh/Default                                 |
| ------------------------------------------ | ---------------------------------------------- |
| `OpenAIApiKey`                             | `sk-...`                                       |
| `WeatherApiKey`                            | `xxxxxxxxxxxxxxxxx`                            |
| `ElevenLabsApiKey`                         | `eleven-...`                                   |
| `ElevenLabsVoiceId`                        | `B8gJV1IhpuegLxdpXFOE` (contoh)                |
| `OpenAIChatUrl`                            | `https://api.openai.com/v1/chat/completions`   |
| `WeatherApiBaseUrl`                        | `https://api.weatherapi.com/v1`                |
| `ElevenLabsBaseUrl`                        | `https://api.elevenlabs.io/v1/text-to-speech/` |
| (Opsional) `UseSecureRelay`/`RelayBaseUrl` | arahkan ke proxy/Cloud Code Anda               |

> **Peringatan**: Mode **Direct API** menyimpan kunci di sisi klien **WebGL** (mudah diekstrak). Untuk produksi, gunakan **relay/Cloud Code** dan kuota terbatas/rotasi.

### Build WebGL (ringkas)

1. **File → Build Settings → WebGL** (Switch Platform).
2. **Player Settings**:

   * *Resolution & Presentation*: aktifkan **WebGL Compression** sesuai hosting, **Autoplay** audio mungkin **dibatasi**; siapkan pemicu suara via user gesture (klik/tap).
   * *Publishing Settings*: sesuaikan **Data Caching** & **Linker Target** untuk ukuran & kompatibilitas.
3. **Build** dan **deploy** ke hosting statis/CDN.

Catatan iOS/Safari: audio **butuh interaksi pengguna** sebelum `AudioSource.Play()` dapat terdengar.

---

## Batch Mode (CSV/XLSX)

### Format Kolom Masuk

Deteksi header (regex, case‑insensitive):

* **Nama**: `name|nama|kecamatan`
* **Latitude**: `lat|latitude|lintang`
* **Longitude**: `lon|lng|longitude|bujur`

### Cara Pakai

1. Klik **Upload** dan pilih berkas **.csv** atau **.xlsx**.
2. Tekan **Process** untuk mengambil cuaca tiap baris.
3. Unduh hasil dengan **Download CSV** atau **Download XLSX**.

Parameter kinerja:

* `maxConcurrency` (default **48**) — jumlah worker paralel (disarankan **32–64** untuk WebGL, sesuaikan jaringan/hosting).
* `maxRetries` (default **3**) — percobaan ulang untuk error sementara (429/5xx/timeouts) dengan **backoff + jitter**.

### Skema Keluaran

Kolom: `Name, Latitude, Longitude, Last Update, Suhu (°C), Kelembapan (%), Kondisi, Kecepatan Angin (kph), Arah Angin, Sinar UV`

Contoh (ringkas):

| Name            | Latitude | Longitude | Last Update      | Suhu (°C) | Kelembapan (%) | Kondisi       | Kecepatan Angin (kph) | Arah Angin | Sinar UV |
| --------------- | -------: | --------: | ---------------- | --------: | -------------: | ------------- | --------------------: | ---------- | -------: |
| Kecamatan Dukun |    -7.12 |    112.03 | 2025-09-01 09:30 |      28.5 |             75 | Partly cloudy |                  12.4 | E (90°)    |        7 |

> **Catatan:** Penulis **XLSX** di proyek ini **minimal** (sheet tunggal, teks/angka tanpa style). Cukup untuk analisis dasar & impor ke spreadsheet.

---

## Keamanan & Biaya

> **Peringatan**: **Kunci API di WebGL dapat dilihat** oleh pengguna (Network tab/Artifacts). Gunakan **kuota khusus**, **rotasi kunci**, dan pertimbangkan **relay/Cloud Code** untuk menyuntikkan kunci di server.

* **Rate limit**: OpenAI/WeatherAPI/ElevenLabs dapat mengembalikan **HTTP 429**. Lakukan retry/backoff.
* **Kesalahan server**: **5xx** — coba ulang otomatis (sudah diterapkan di batch).
* **Biaya**: perhatikan biaya LLM & TTS saat batch besar; gunakan `maxConcurrency` konservatif.

---

## FAQ & Troubleshooting

1. **Tidak ada suara yang keluar?**
   Browser (terutama iOS/Safari) memerlukan **user gesture** sebelum audio dapat diputar. Pastikan pengguna klik/tap tombol lebih dulu.
2. **CORS/HTTP diblokir?**
   Pastikan hosting mengizinkan request ke domain API yang digunakan atau route lewat **relay** dengan CORS benar.
3. **LLM mengembalikan JSON tidak valid?**
   Sistem memaksa **JSON‑only**; jika gagal, ulangi prompt atau aktifkan `VerboseLogging` untuk diagnosis.
4. **Lokasi tidak ditemukan/ambigu?**
   Gunakan format koordinat **`lat,lon`** (mis. `-7.98,112.63`) atau nama lokasi yang lebih spesifik.
5. **429 / Rate limit**
   Turunkan `maxConcurrency`, tambah `maxRetries`, dan jeda ulang (**backoff**) otomatis akan membantu.
6. **Audio patah‑patah di mobile**
   Nonaktifkan streaming audio TTS; proyek ini **membuffer** penuh untuk stabilitas WebGL.
7. **Ukuran build WebGL besar**
   Aktifkan kompresi, strip kode, dan optimalkan aset (gambar, rig, audio).
8. **Icon/cuaca tidak sesuai siang/malam**
   Pastikan penggunaan kode kondisi & flag `is_day` dari WeatherAPI saat memetakan sprite.

---

## Contoh Prompt Pengguna

1. “Cuaca **sekarang** di **Kecamatan Dukun** apa?”
2. “**Besok** di **Bandung** hujan nggak?”
3. “Minta **ramalan 3 hari** di **Malang**.”
4. “Koordinat **-7.98,112.63** cuacanya gimana?”
5. “Pakai **Fahrenheit** ya.”

---

## Roadmap

* Stream UI untuk respons LLM (typing/partial).
* **PWA** & offline cache ikon/kamus kondisi.
* Multi‑voice & kontrol **prosody** TTS.
* Mode **batch forecast** (multi‑hari) + grafik ringkas.
* **Relay/Cloud Code** template publik (tanpa kunci).

---

## Kontribusi

Kontribusi terbuka! Silakan buat **issue** atau **pull request** di repo:
[https://github.com/muhammadIdhamMaarif/Tenki-Weather](https://github.com/muhammadIdhamMaarif/Tenki-Weather)

> *Code of Conduct*: gunakan standar komunitas (mis. Contributor Covenant). Tambahkan berkas CoC jika diperlukan.

---

## Kredit & Lisensi

**Aset & Layanan Pihak Ketiga (tautan resmi):**

* **Unity** — [https://unity.com](https://unity.com)
* **TextMeshPro** — [https://docs.unity3d.com/Packages/com.unity.textmeshpro](https://docs.unity3d.com/Packages/com.unity.textmeshpro)
* **Michsky DreamOS** — *(tautan Asset Store resmi, tambahkan di sini)*
* **Animation Rigging** — [https://docs.unity3d.com/](https://docs.unity3d.com/)
* **uLipSync** — *(tautan resmi repositori, tambahkan di sini)*
* **Magica Cloth 2** — *(tautan Asset Store resmi, tambahkan di sini)*
* **OpenAI** — [https://openai.com](https://openai.com)
* **WeatherAPI.com** — [https://www.weatherapi.com](https://www.weatherapi.com)
* **ElevenLabs** — [https://elevenlabs.io](https://elevenlabs.io)

> **Disclaimer AI Art**: Pastikan hak pakai aset visual/AI art (termasuk model/voice) sesuai **komersial/publik** sebelum rilis.

**Lisensi**: Proyek ini berada di bawah **MIT License**. Lihat bagian [Lisensi](#lisensi).

---

## Lisensi

```
MIT License

Hak cipta (c) Pemilik Repo

Izin diberikan secara gratis, kepada siapa pun yang memperoleh salinan perangkat lunak ini dan file dokumentasi terkait ("Perangkat Lunak"), untuk berurusan dalam Perangkat Lunak tanpa batasan, termasuk tanpa batasan hak untuk menggunakan, menyalin, mengubah, menggabungkan, menerbitkan, mendistribusikan, melisensikan ulang, dan/atau menjual salinan Perangkat Lunak, serta untuk mengizinkan orang yang kepada siapa Perangkat Lunak disediakan untuk melakukannya, tunduk pada ketentuan berikut:

Pemberitahuan hak cipta di atas dan pemberitahuan izin ini harus disertakan dalam semua salinan atau bagian substansial dari Perangkat Lunak.

PERANGKAT LUNAK INI DISEDIAKAN "SEBAGAIMANA ADANYA", TANPA JAMINAN APA PUN, BAIK TERSURAT MAUPUN TERSIRAT, TERMASUK NAMUN TIDAK TERBATAS PADA JAMINAN DIPERDAGANGKAN, KESESUAIAN UNTUK TUJUAN TERTENTU DAN NONPELANGGARAN. DALAM KEADAAN APA PUN PARA PENULIS ATAU PEMEGANG HAK CIPTA TIDAK BERTANGGUNG JAWAB ATAS KLAIM, KERUSAKAN ATAU KEWAJIBAN LAINNYA, BAIK DALAM TINDAKAN KONTRAK, KESALAHAN ATAU LAINNYA, YANG TIMBUL DARI, DARI ATAU SEHUBUNGAN DENGAN PERANGKAT LUNAK ATAU PENGGUNAAN ATAU HAL LAIN DALAM PERANGKAT LUNAK.
```
