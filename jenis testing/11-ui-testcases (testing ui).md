# 11 — UI Test Cases: AxxonNet (Middleware) & Panel (Frontend)

> **Tujuan.** Daftar test case UI yang bisa langsung dieksekusi untuk dua sistem:
> - **AxxonNet** = *middleware* (web client VMS Axxon One — daftar kamera, live view, archive, sumber event).
> - **Panel** = *frontend* (platform VSaaS / JASNITA — dashboard, laporan, monitoring yang menampilkan data dari AxxonNet).
>
> **Arsitektur yang diuji:** data mengalir **AxxonNet → Panel**. Karena itu pengujian dilakukan di tiga level: (A) frontend Panel, (B) middleware AxxonNet, (C) konsistensi end-to-end antara keduanya.
>
> Test case di sini melengkapi `03-test-case-design.md` dengan fokus **UI**, dan terhubung ke requirement di `01-requirement-analysis.md` (REQ-01…REQ-10). Hasil eksekusi dicatat di `05-test-execution-log.md`, bug di `06-defect-bug-log.md`.

---

## Legenda Status

| Simbol | Arti |
|---|---|
| ☐ | Belum dieksekusi (Not Executed) |
| ✅ | Pass |
| ❌ | Fail (catat bug di `06-defect-bug-log.md`) |
| ⚠️ | Blocked / tidak bisa diuji |

Setiap test case: isi kolom **Hasil** (✅/❌/⚠️) dan **Catatan** saat eksekusi.

---

## TAHAP A — UI PANEL (Frontend)

### A1. Login (REQ-01)

| ID | Langkah | Expected Result | Hasil | Catatan |
|---|---|---|---|---|
| UI-A1-01 | Login dengan email + password benar | Berhasil masuk ke dashboard | ✅ | |
| UI-A1-02 | Login dengan password salah | Muncul pesan error , tidak masuk | ✅ |<img width="423" height="377" alt="image" src="https://github.com/user-attachments/assets/5532c485-7eb2-4b3b-9d9c-ed0a6add8b59" /> |
| UI-A1-03 | Login dengan email/password kosong | Ada validasi, menunjukkan daerah yang belum diisi | ✅ | <img width="505" height="343" alt="image" src="https://github.com/user-attachments/assets/389f16d2-5ba5-4c8c-9285-cb9849d646a8" /> |
| UI-A1-04 | Klik Logout | Kembali ke halaman login| ✅ | |

### A2. Events Report (REQ-06)

| ID | Langkah | Expected Result | Hasil | Catatan |
|---|---|---|---|---|
| UI-A2-01 | Buka menu Events Report | Tabel tampil dengan kolom: Event Type, Event Timestamp, Camera Name, Event Value, Snapshot | ✅ | <img width="758" height="340" alt="image" src="https://github.com/user-attachments/assets/701e8726-57f6-4d2c-9543-20240c8887d2" /> |
| UI-A2-02 | Filter "Pilih Event" → klik Filter Data | Tabel update sesuai jenis event | ✅ | <img width="740" height="455" alt="image" src="https://github.com/user-attachments/assets/a670ec22-0fa9-4348-97e2-74acf27d4b3c" /> |
| UI-A2-03 | Filter "Pilih Kamera" → Filter Data | Hasil hanya kamera yang dipilih | ✅ |  <img width="740" height="455" alt="image" src="https://github.com/user-attachments/assets/a670ec22-0fa9-4348-97e2-74acf27d4b3c" /> |
| UI-A2-04 | Filter "Pilih Tanggal" → Filter Data | Hasil hanya dalam rentang tanggal | ✅ |  <img width="740" height="455" alt="image" src="https://github.com/user-attachments/assets/a670ec22-0fa9-4348-97e2-74acf27d4b3c" /> |
| UI-A2-05 | Perhatikan kolom Snapshot | Gambar event tampil | ✅ | event tampil, namun tidak selalu dalam keadaan bagus, kadang blurry, kadang duplikat, kadang di luar titik fokus |
| UI-A2-06 | Ganti "Show entries" ke 100 | Tabel menampilkan hingga 100 baris | ✅ | <img width="730" height="66" alt="image" src="https://github.com/user-attachments/assets/9ac8d2e1-3a9a-477c-9c20-b3cbc83fdab4" /> |
| UI-A2-07 | Klik "Unduh Data" | File Excel ter-download & bisa dibuka | ✅ | |
| UI-A2-08 | Bandingkan isi Excel dengan tabel | Isi Excel sama persis dengan tabel di UI | ✅ | |

**note:**
<img width="242" height="146" alt="image" src="https://github.com/user-attachments/assets/f9ae072d-1548-411b-9514-d3379037e5aa" /> pengaturan pemilihan harus dipilih semua dulu baru bisa, tidak bisa sendiri

### A3. CCTV — daftar kamera (REQ-04)

| ID | Langkah | Expected Result | Hasil | Catatan |
|---|---|---|---|---|
| UI-A3-01 | Buka menu CCTV | Daftar kamera tampil dengan kolom: Node name, Camera name, Status, Webhook, Last Updated, Last Detected Event, Action | ✅ | |
| UI-A3-02 | Search nama kamera | Hasil sesuai kata kunci | ✅ | <img width="732" height="380" alt="image" src="https://github.com/user-attachments/assets/0e226bdc-945c-4ebd-a35f-d526721bcf7f" /> |
| UI-A3-03 | Filter status (online/offline) | Hasil sesuai status | ✅ | |
| UI-A3-04 | Pagination ganti ke 10 | Tampil 10 baris per halaman | ✅ | <img width="755" height="98" alt="image" src="https://github.com/user-attachments/assets/64e4212a-5606-4066-8ecc-10e234593332" /> |
| UI-A3-05 | Pagination ganti ke 100 | Tampil 100 baris per halaman | ✅ | <img width="732" height="76" alt="image" src="https://github.com/user-attachments/assets/3c5b9cf4-6349-4bdc-91d7-85818c00764e" /> |
| UI-A3-06 | Klik "Sync Data" | Data ter-update, ada indikator loading/sukses | ✅ | <img width="752" height="468" alt="image" src="https://github.com/user-attachments/assets/ca4d5513-0f29-4620-92e2-c57bd32c3870" /> |
| UI-A3-07 | Klik tombol "Action" di satu baris | Aksi yang tersedia muncul | ✅ | livestream dan edit |
| UI-A3-08 | klik tombol livestream | dapat melihat isi livestream dan infonya | ✅ | <img width="883" height="302" alt="image" src="https://github.com/user-attachments/assets/778c5950-1f1b-41a5-b08a-dc8b09cb4336" /> |
| UI-A3-09 | Klik tombol edit | dapat mengedit isinya | ✅ | <img width="361" height="490" alt="image" src="https://github.com/user-attachments/assets/495b34bb-cde9-4aa5-986b-9ae15739d59c" /> |

### A4. Real-time Monitoring (REQ-05)

| ID | Langkah | Expected Result | Hasil | Catatan |
|---|---|---|---|---|
| UI-A4-01 | Pilih kamera → tampilkan live | Live stream tampil di card box | ✅ | <img width="286" height="188" alt="image" src="https://github.com/user-attachments/assets/2c05c143-26ea-4fee-a7bb-09d352633b9a" /> |
| UI-A4-02 | Kamera format HLS | Video jalan tanpa error | ⚠️ | video yang awalnya hls selalu berpindah menjadi mp4 agar bisa berputar|
| UI-A4-03 | Kamera format MP4 | Video jalan tanpa error | ✅ | berjalan kalau mp4, meskipun dipilih hls tidak berapa lama akan kembali ke mp4 |
| UI-A4-04 | Pilih beberapa kamera sekaligus | Semua card jalan tanpa freeze | ✅ | tidak dapat semua, tapi bisa sampai 11, kalau lebih bakal ada beberapa yang blank screen|

note: ada beberapa kamera yang bisa di axxonnet namun blank screen seperti kamera 11
### A5. Analytics Dashboard (REQ-02)

| ID | Langkah | Expected Result | Hasil | Catatan |
|---|---|---|---|---|
| UI-A5-01 | Buka Analytics Dashboard | Semua widget metrik tampil | ✅ | |
| UI-A5-02 | Cek Total Camera (Active/Inactive) | Angka tampil & masuk akal | ✅ | saat menekan button active/inactive langsung membawa ke daftar kamera|
| UI-A5-03 | Cek Camera Health (Healthy/Degraded/Offline) | Nilai tampil | ✅ | <img width="251" height="135" alt="image" src="https://github.com/user-attachments/assets/ada606dd-eaf8-4f20-9f29-cfb51d9532be" /> |
| UI-A5-04 | Cek Total Events (Terkirim/Pending/Failed) | Nilai tampil | ✅ | <img width="254" height="137" alt="image" src="https://github.com/user-attachments/assets/688f192f-7dad-41fb-b3ef-c032a995bdde" /> |
| UI-A5-05 | Cek Top 10 Cameras by Event Count | Daftar tampil | ✅ | <img width="761" height="255" alt="image" src="https://github.com/user-attachments/assets/75dab31a-cbf5-45d0-92ee-cff6b4e5f94a" /> |
| UI-A5-06 | Cek Peak Activity Hours & Daily Event Trend | Grafik tampil, tidak NaN/error | ✅ | <img width="776" height="229" alt="image" src="https://github.com/user-attachments/assets/be2ac54d-e550-4b89-adb2-37e9cd3189ac" /> |
| UI-A5-07 | Cek Peak Event Distribution | Grafik tampil, tidak NaN/error | ✅ | <img width="772" height="212" alt="image" src="https://github.com/user-attachments/assets/43986b8e-f4a0-4dc1-9d04-46351855020d" /> |

### A6. Dashboard Summary (REQ-03)

| ID | Langkah | Expected Result | Hasil | Catatan |
|---|---|---|---|---|
| UI-A6-01 | Pilih kamera + periode → klik "Lihat Data" | Data tampil | ✅ | <img width="773" height="389" alt="image" src="https://github.com/user-attachments/assets/f3bce334-0b45-4bc4-ab8a-81ad748035da" /> |
| UI-A6-02 | Cek Total Kendaraan | Nilai tampil | ✅ | |
| UI-A6-03 | Cek Traffic per jam per tipe kendaraan | Grafik/tabel tampil | ✅ | <img width="755" height="181" alt="image" src="https://github.com/user-attachments/assets/ab532ae4-40b1-4e87-a3bf-c6e5f6a37fa7" /> |

### A7. Daily Counting Report (REQ-07 / REQ-07a)

| ID | Langkah | Expected Result | Hasil | Catatan |
|---|---|---|---|---|
| UI-A7-01 | Pilih format waktu (Harian/Mingguan/Bulanan) | Opsi berfungsi | ✅ | berhasil, namun kalau pilih week lalu pilih bagian tengah bulan dia akan menganggapnya sebagai week 1 |
| UI-A7-02 | Pilih kamera + tanggal | Tabel tampil (Camera name, Tanggal, hasil detektor) | ✅ | |
| UI-A7-03 | Cek kolom detektor dinamis | Kolom sesuai konfigurasi client (mis. Motor & Mobil) | ✅ | |

### A8. Account (REQ-08, NFR-04)

| ID | Langkah | Expected Result | Hasil | Catatan |
|---|---|---|---|---|
| UI-A8-01 | Lihat & ubah Profile (Nama, Email) | Perubahan tersimpan | ✅ | <img width="431" height="309" alt="image" src="https://github.com/user-attachments/assets/edc0373d-67ee-4a65-b693-b22e0be1a2a6" /> |
| UI-A8-02 | Ubah password dengan password lemah (<8 char / tanpa huruf besar / tanpa angka) | Ditolak dengan pesan validasi | ✅ | <img width="274" height="56" alt="image" src="https://github.com/user-attachments/assets/2af12764-f690-4135-93b6-92bb55299b83" /> |
| UI-A8-03 | Ubah password valid | Berhasil diubah | ✅ | <img width="534" height="392" alt="image" src="https://github.com/user-attachments/assets/77d7ae99-544d-4f8e-8801-da909859d12a" /> |
| UI-A8-04 | Logout | Kembali ke login | ✅ | |

### A9. Master Data — Branches

| ID | Langkah | Expected Result | Hasil | Catatan |
|---|---|---|---|---|
| UI-A9-01 | Buka menu Branches | Data cabang tampil | ✅ | |

---

## TAHAP B — UI AXXONNET (Middleware)

### B1. Daftar Kamera & Web Client

| ID | Langkah | Expected Result | Hasil | Catatan |
|---|---|---|---|---|
| UI-B1-01 | Buka Web client | Semua kamera tampil di list (mis. `1.SBYBAP-CAM-A076-A`) | ✅ | |
| UI-B1-02 | Klik ikon sort A-Z | Urutan kamera menjadi terurut | ✅ | |
| UI-B1-03 | Ketik nama kamera di "Camera or layout" search | Kamera yang dicari muncul | ✅ | |
| UI-B1-04 | Klik bintang ★ pada kamera | Kamera masuk ke favorites | ✅ | |

### B2. Live View

| ID | Langkah | Expected Result | Hasil | Catatan |
|---|---|---|---|---|
| UI-B2-01 | Klik satu kamera | Video live tampil | ✅ | |
| UI-B2-02 | Cek header kamera | Nama, timestamp, resolusi (mis. 2560×1440), bitrate, indikator R (recording) benar | ✅ | |
| UI-B2-03 | Cek timestamp overlay di video | Sama dengan waktu sekarang | ✅ | <img width="165" height="45" alt="image" src="https://github.com/user-attachments/assets/951e5c3c-ca99-472d-bb4c-04426899f912" />
 |
| UI-B2-04 | Uji tombol in-frame | exporting archive, bookmark, zoom, mic berfungsi | ✅ | |

### B3. Layout / Grid — ⚠️ TITIK KRITIS PERFORMA 100 KAMERA

| ID | Langkah | Expected Result | Hasil | Catatan |
|---|---|---|---|---|
| UI-B3-01 | Ganti ke layout grid (2×2) | stream jalan bersamaan | ✅ | |
| UI-B3-02 | Buka grid besar (8×8 / banyak kamera) | Browser tidak lag/freeze/crash | ✅ | tidak crash namun tidak bisa melihat isi kameranya |
| UI-B3-03 | Biarkan grid aktif 10–15 menit, pantau Memory di DevTools | Tidak ada memory leak signifikan | ⚠️ | <img width="305" height="501" alt="image" src="https://github.com/user-attachments/assets/049fa28b-357e-4d79-a254-47ab22990cac" /> |
| UI-B3-04 | Ganti-ganti layout berkali-kali | Stream lama ikut berhenti (tidak menumpuk koneksi) | ✅ | |
| UI-B3-05 | Cek Network tab saat grid penuh | Jumlah koneksi stream wajar, tidak banyak yang gagal | ✅ | |

### B4. Archive / Playback

| ID | Langkah | Expected Result | Hasil | Catatan |
|---|---|---|---|---|
| UI-B4-01 | Masuk mode archive (eents) | Mode playback aktif | ✅ | |
| UI-B4-02 | Scrub timeline | Video rekaman jalan sesuai posisi | ✅ | |
| UI-B4-03 | Play/pause & loncat ke timestamp tertentu | Berfungsi | ✅ | |

### B5. Tab Navigasi Atas

| ID | Langkah | Expected Result | Hasil | Catatan |
|---|---|---|---|---|
| UI-B5-01 | Buka tab Faces | Halaman tampil tanpa error | ✅ | |
| UI-B5-02 | Buka tab License plates | Halaman tampil | ✅ | |
| UI-B5-03 | Buka tab Reports | Halaman tampil | ✅ | |
| UI-B5-04 | Buka tab User permissions | Halaman tampil | ✅ | |
| UI-B5-05 | Buka tab User Dashboards | Halaman tampil | ✅ | |
| UI-B5-06 | Buka tab User Licensing | Halaman tampil | ✅ | |
| UI-B5-07 | Buka tab User system settings | Halaman tampil | ✅ | |

### B6. Fullscreen & Lock

| ID | Langkah | Expected Result | Hasil | Catatan |
|---|---|---|---|---|
| UI-B6-01 | Klik fullscreen | Masuk & keluar fullscreen normal | ✅ | |
| UI-B6-02 | Klik ikon lock 🔒 | Fungsi unpin top panel bekerja sesuai | ✅ | |

---

## TAHAP C — END-TO-END (AxxonNet ↔ Panel) ⭐ PALING BERNILAI

> Menguji konsistensi data dari middleware ke frontend. Terkait REQ-09 / REQ-10.

| ID | Langkah | Expected Result | Hasil | Catatan |
|---|---|---|---|---|
| UI-C-01 | Catat 1 event di AxxonNet (kamera + timestamp + jenis), cari di Panel Events Report | Event yang sama muncul: timestamp, kamera, snapshot cocok | ✅ | khusus untuk archive, di axxonnet menggunakan utc yang memiliki 7 jam perbedaan waktu dengan wib yang ditampilkan oleh panel |
| UI-C-02 | Hitung jumlah kamera di AxxonNet vs Panel CCTV | Jumlah sama | ✅ | yang ditampilkan di axxonnet sebanyak 87 kamera yang dapat mendeteksi, sedang di panel terdapat 87+, hal ini dikarenakan sisa dari 87 itu tidak memiliki detektor |
| UI-C-03 | Set 1 kamera offline di AxxonNet | Panel CCTV menampilkan kamera itu offline | ❌ | salah satu kamera yang mati, 83, di axxonnet dianggap aktiv di panel, namun saat dilihat live streamnya, <img width="200" height="100" alt="image" src="https://github.com/user-attachments/assets/12a4bfc3-8e3a-4e42-a983-312cef5a7a36" /> |
| UI-C-04 | Bandingkan snapshot event AxxonNet vs Panel | Gambar sama | ✅ | |

---

## TAHAP D — Negatif / Edge Case

| ID | Langkah | Expected Result | Hasil | Catatan |
|---|---|---|---|---|
| UI-D-01 | Buka kamera yang offline | Tampil "no signal" dengan rapi, tidak crash (Panel & AxxonNet) | ✅ di axxonnet /❌ di panel | panel:  <img width="200" height="100" alt="image" src="https://github.com/user-attachments/assets/12a4bfc3-8e3a-4e42-a983-312cef5a7a36" />, axxonnet: <img width="200" height="100" alt="image" src="https://github.com/user-attachments/assets/6af94de6-49d2-462d-a233-353417a5091d" /> |
| UI-D-02 | Putus internet saat live view | Reconnect otomatis / pesan jelas | ✅ | |
| UI-D-03 | Filter tanggal yang tidak ada datanya | Tampil "data kosong", bukan error | ✅ | <img width="784" height="362" alt="image" src="https://github.com/user-attachments/assets/5f2b974a-c281-46d7-b01d-1623dd7325a7" /> |
| UI-D-04 | Diamkan hingga session timeout | Diminta login ulang dengan aman | ✅ | kembali ke halaman login |
| UI-D-05 | Unduh Excel saat data kosong | Tidak error (file kosong) | ✅ | |

---

## TAHAP E — Kompatibilitas

| ID | Langkah | Expected Result | Hasil | Catatan |
|---|---|---|---|---|
| UI-E-01 | Jalankan test utama di Chrome | Semua fitur normal | ✅ | |
| UI-E-02 | Jalankan di Edge | Semua fitur normal | ✅ | |
| UI-E-03 | Uji resolusi laptop & monitor besar | Layout tidak rusak (responsive) | ✅ | tidak serta merta rusak, tapi dapat lebih diperbaiki, tapi overall aman |

---
