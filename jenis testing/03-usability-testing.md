# Usability Testing — AxxonNet (Middleware) & Panel (Frontend)

> **Tujuan.** Mengukur seberapa **mudah & nyaman** sistem dipakai oleh user nyata. Pertanyaan inti: *"Apakah user bingung, tersesat, atau terhambat saat memakai sistem?"*
>
> **Apa yang diuji:** kejelasan alur, kualitas feedback, jumlah langkah, kejelasan status & pesan — dari sudut pandang user awam, bukan developer.
>
> Banyak temuan di sini berasal dari eksekusi UI (`10-ui-testcases.md`). Temuan dicatat sebagai saran perbaikan UX dan/atau bug di `06-defect-bug-log.md`.

---

## Legenda Status

| Simbol | Arti |
|---|---|
| ☐ | Belum dieksekusi |
| ✅ | Baik (user tidak terhambat) |
| ❌ | Bermasalah (user bingung/terhambat) |
| ⚠️ | Bisa diperbaiki (jalan, tapi kurang nyaman) |

---

## Apa yang harus dilakukan?

1. Posisikan diri sebagai **user awam**, bukan developer.
2. Coba selesaikan tugas nyata, misalnya: *"cari event mobil kemarin di kamera X lalu unduh."*
3. Amati titik di mana user mungkin **stuck**, salah paham, atau butuh menebak.
4. Catat jumlah langkah & apakah ada feedback yang hilang.
5. Tulis saran perbaikan di kolom Catatan.

---

## TAHAP A — Efisiensi Tugas (Task Efficiency)

| ID | Skenario | Expected Result | Hasil | Catatan |
|---|---|---|---|---|
| USA-A-01 | Cari 1 kamera di antara 100 kamera | Mudah & cepat lewat search | ✅ | search jelas (`11` UI-A3-02) |
| USA-A-02 | Filter Events Report dengan 1 kriteria saja | Bisa filter sebagian | ❌ | Harus pilih semua opsi dulu baru bisa (`11` note A2) |
| USA-A-03 | Unduh laporan event | Langkah minim & jelas | ✅ | ke events report lalu tekan tombol unduh data|
| USA-A-04 | Pilih rentang tanggal (date picker) untuk Daily Counting Report | Intuitif, tidak salah hitung | ❌ | Pilih tengah bulan dianggap Week 1 (`11` UI-A7-01) |
| USA-A-05 | Melihat summary data per kamera (hasil deteksi) |  Mudah & cepat lewat filter | ✅ |  |
| USA-A-06 | monitor livestream | jelas dan langsung tampil | ⚠️ | mudah diakses namun memakan waktu 46.55 detik untuk menampilkan livestream 1 kamera |
| USA-A-07 | melihat snapshot vehicle dari kamera dengan tanggal spesifik | menampilkan dalam jangka tersebut | ✅ |  |
| USA-A-08 | mengubah atau melihat suatu informasi di server (branch, user dan lainnya) | tersedia di page masing-masing | ✅ | terdapat tombol edit di kolom action untuk mengubah atau melihat informasinya |
| USA-A-09 | menambahkan kamera, role, branch maupun user  | dapat menambahkan di pagenya langsung | ✅ | terdapat tombol add di pojok kanan atas |
---

## TAHAP B — Kejelasan Status & Feedback

| ID | Skenario | Expected Result | Hasil | Catatan |
|---|---|---|---|---|
| USA-B-01 | Lihat status kamera online/offline sekilas | Jelas secara visual | ❌ | Kamera mati tampil "aktif" di Panel (`11` UI-C-03) |
| USA-B-02 | Login dengan password salah | Pesan error jelas & membantu | ✅ | `11` UI-A1-02 |
| USA-B-03 | Lakukan aksi (Sync/Simpan/Unduh) | Ada feedback sukses/gagal | ✅ | <img width="806" height="478" alt="image" src="https://github.com/user-attachments/assets/7033da35-7839-4baf-a6af-1000aadff6e7" /> |
| USA-B-04 | Stream kamera gagal tampil | Ada pesan, bukan layar kosong | ❌ | Blank screen (`11` UI-A4-04, kamera 11) |

---

## TAHAP C — Navigasi & Orientasi

| ID | Skenario | Expected Result | Hasil | Catatan |
|---|---|---|---|---|
| USA-C-01 | Berpindah antar menu | Jelas posisi user sekarang | ✅ | user tidak tersesat |
| USA-C-02 | Pahami arti ikon tanpa tooltip | Ikon cukup jelas sendiri | ⚠️(grey area) | beberapa dapat dipahami, namun ada beberapa icon yang mirip seperti events report dan daily counting report, panduan teks masih dibutuhkan |
| USA-C-03 | Klik "Active/Inactive" di Analytics | Perilaku sesuai ekspektasi | ⚠️ | Langsung membawa ke daftar kamera (`11` UI-A5-02) |
| USA-C-04 | keluar dari pop up box atau hasil snapshot | mudah keluar | ⚠️ | tombol untuk keluar yang berbentuk persegi panjang abu-abu terlalu kecil untuk ditekan <img width="616" height="309" alt="image" src="https://github.com/user-attachments/assets/a444d626-6f1a-4a4f-8404-85524e0a3b0c" /> |

---

**note:** Temuan usability paling berdampak: (1) filter Events Report yang memaksa pilih semua opsi dulu (USA-A-02), (2) date picker minggu yang salah hitung (USA-A-04), (3) status kamera offline yang tampil "aktif" (USA-B-01), dan (4) blank screen tanpa pesan saat stream gagal (USA-B-04). Semua menurunkan kenyamanan & kepercayaan user terhadap data.

---

## Exit Criteria
- Tugas umum bisa diselesaikan user awam tanpa bantuan.
- Setiap aksi penting memberi feedback yang jelas.
- Temuan usability terdokumentasi sebagai saran perbaikan UX.
