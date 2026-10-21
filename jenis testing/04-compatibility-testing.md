# Compatibility Testing — AxxonNet (Middleware) & Panel (Frontend)

> **Tujuan.** Memverifikasi sistem **tampil & berfungsi konsisten** di berbagai browser, resolusi layar, dan level zoom. Pertanyaan inti: *"Apakah hasilnya sama di lingkungan yang berbeda?"*
>
> **Apa yang diuji:** alur utama (login, CCTV, Events Report, live view) di Chrome/Edge/Firefox, pada resolusi & zoom berbeda.
>
> Terhubung ke `10-ui-testcases.md` TAHAP E. Temuan dicatat di `06-defect-bug-log.md`.

---

## Legenda Status

| Simbol | Arti |
|---|---|
| ☐ | Belum dieksekusi |
| ✅ | Normal di lingkungan ini |
| ❌ | Rusak/tidak berfungsi |
| ⚠️ | Minor issue (jalan, perlu perbaikan) |

---

## Apa yang harus dilakukan?

1. Siapkan beberapa browser: Chrome, Edge, Firefox (versi terbaru).
2. Jalankan alur utama di tiap browser.
3. Uji beberapa resolusi (laptop kecil, monitor besar) & level zoom.
4. Catat perbedaan tampilan/fungsi antar lingkungan + screenshot.

---

## TAHAP A — Lintas Browser

| ID | Lingkungan | Langkah | Expected Result | Hasil | Catatan |
|---|---|---|---|---|---|
| CMP-A-01 | Chrome (terbaru) | Jalankan alur utama | Semua fitur normal | ✅ | `11` UI-E-01 |
| CMP-A-02 | Edge (terbaru) | Jalankan alur utama | Semua fitur normal | ✅ | `11` UI-E-02 |
| CMP-A-03 | Firefox (terbaru) | Jalankan alur utama | Semua fitur normal | ❌ | penguji tidak memiliki firefox |
| CMP-A-04 | Tiap browser | Buka live view | Stream jalan di semua | ✅ | sama antar browser dan edge |
| CMP-A-05 | ui per browser | cek ui di browser masing-masing | font, teks, warna, ukurannya sama | ✅ | sama |

---

## TAHAP B — Resolusi & Zoom

| ID | Lingkungan | Langkah | Expected Result | Hasil | Catatan |
|---|---|---|---|---|---|
| CMP-B-01 | 1366×768 (laptop) | Cek layout alur utama | Tidak rusak/terpotong | ✅ | |
| CMP-B-02 | 1920×1080+ (monitor) | Cek layout | Proporsional, tidak rusak | ✅ | Bisa dioptimalkan (`11` UI-E-03) |
| CMP-B-03 | Zoom 80% / 100% / 150% | Cek keterbacaan | Tetap terbaca & rapi | ❌ | saat 100% ikon sidebar berpindah / tidak ditengah <img width="953" height="501" alt="image" src="https://github.com/user-attachments/assets/f8c69a92-6174-42fc-acb4-73ccccd3b5ea" /> bahkan ikon hamburgernya juga menghilang, baru normal saat 80% |

---

## TAHAP C — Perangkat (Opsional)

| ID | Lingkungan | Langkah | Expected Result | Hasil | Catatan |
|---|---|---|---|---|---|
| CMP-C-01 | Tablet/mobile browser | Buka Panel | Responsif / pesan arahan ke desktop | ❌ |  ada overlay halus saat model tablet <img width="289" height="149" alt="image" src="https://github.com/user-attachments/assets/9bc2fbed-90d9-4856-83ef-65c835cdb8aa" />, ikon yang menghilang (VIS-A2-01),  keluar box <img width="476" height="480" alt="image" src="https://github.com/user-attachments/assets/57704f58-0e38-4a58-a8a6-a1dfdd2e7730" /> logo jasnita di menu menghilang saat dia mode mobile dan tablet serta saat ditekan icon account pop up boxnya menutupi side bar, <img width="213" height="374" alt="image" src="https://github.com/user-attachments/assets/cb708b39-4668-4b53-a165-bc3c32ffff73" />, responsivitas masih sangat kurang. saat mobile gapnya terlalu kecil <img width="327" height="346" alt="image" src="https://github.com/user-attachments/assets/736d7ef5-916e-4856-bdfc-c68f5d0321ef" />, saat mobile/tablet baris harus digeser untuk melihat seluruh isinya serta angka page ditampakkan secara langsung dan mendorong tulisan "showing ... out ..." ke kiri <img width="320" height="201" alt="image" src="https://github.com/user-attachments/assets/d6e84272-08da-41f0-8b12-bd56634d69a6" /> |

---

**note:** Saat ini baru Chrome & Edge yang diverifikasi (keduanya normal). Firefox dan pengujian zoom belum dijalankan. Layout di monitor besar aman tetapi masih bisa dioptimalkan.

---

## Exit Criteria
- Alur utama berjalan normal di minimal 2 browser utama (Chrome, Edge).
- Layout tidak rusak pada resolusi umum.
- Keputusan dukungan mobile/Firefox terdokumentasi.
