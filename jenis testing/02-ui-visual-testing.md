# UI / Visual Testing — AxxonNet (Middleware) & Panel (Frontend)

> **Tujuan.** Memverifikasi **kualitas tampilan**: layout, teks, ikon, warna, konsistensi, dan tidak adanya elemen rusak. Pertanyaan inti: *"Apakah tampilannya benar, rapi, dan konsisten?"* — bukan apakah berfungsi (itu Functional Testing).
>
> **Apa yang diuji:** elemen visual di tiap halaman Panel & AxxonNet, termasuk state (hover/disabled/loading), empty state, dan error state.
>
> Terhubung ke `10-ui-testcases.md`. Temuan dicatat di `06-defect-bug-log.md`.

---

## Legenda Status

| Simbol | Arti |
|---|---|
| ☐ | Belum dieksekusi (Not Executed) |
| ✅ | Pass (tampil benar & rapi) |
| ❌ | Fail (elemen rusak/typo/tidak konsisten) |
| ⚠️ | Warning (tampil tapi perlu perbaikan) |

---

## Apa yang harus dilakukan?

1. Buka tiap halaman, amati **detail visual** (bukan fungsi).
2. Periksa: typo, ikon hilang/broken, kolom tabel tidak rata, gambar broken, warna/font tidak seragam.
3. Picu state khusus: hover tombol, tombol disabled, loading, empty state, error state.
4. Bandingkan antar halaman — apakah gaya konsisten?
5. Lampirkan screenshot untuk setiap temuan.

---

## TAHAP A — Visual Panel (Panel)
Note: fungsionalitas sudah bagus, namun visualisasi dashboard terlalu boomer

### A1. Teks & Label

| ID | Langkah | Expected Result | Hasil | Catatan |
|---|---|---|---|---|
| VIS-A1-01 | Baca semua label menu & tombol | Tidak ada typo, bahasa konsisten | ⚠️ | ada gabungan inggris dan indonesia, tapi tidak ada typo <img width="200" height="100" alt="image" src="https://github.com/user-attachments/assets/6a9bafca-7d3f-4d88-b847-505ffeec51ff" /> |
| VIS-A1-02 | Cek judul halaman tiap menu | Judul benar & sesuai menu | ⚠️ | ada sedikit discrepency antara menu dan judul pada bagian kamera <img width="350" height="174" alt="image" src="https://github.com/user-attachments/assets/53884806-8850-4b0e-9430-5980f2b4b8bf" />, serta duplikasi dibagian user management |

### A2. Ikon, Tombol & Warna

| ID | Langkah | Expected Result | Hasil | Catatan |
|---|---|---|---|---|
| VIS-A2-01 | Cek semua ikon & tombol | Tampil semua, tidak broken | ❌ | logo jasnita di menu menghilang saat dia mode mobile dan tablet serta saat ditekan icon account pop up boxnya menutupi side bar, <img width="213" height="374" alt="image" src="https://github.com/user-attachments/assets/cb708b39-4668-4b53-a165-bc3c32ffff73" />, responsivitas masih sangat kurang |
| VIS-A2-02 | Cek state tombol (hover/disabled/loading) | Terlihat berbeda & jelas | ✅ | warna saat hover tampak jelas berbeda dengan saat tidak di hover |
| VIS-A2-03 | Bandingkan warna & font antar halaman | Seragam | ⚠️ (gray area) | warna pada tombol hover dengan tombol hover lainnya tidak seragam, kadang berubah, pada beberapa komponen seperti bg warnanya seragam, namun pada bar, tabel, ikon, tombol warna nya sering berbeda-beda|
| VIS-A2-04 | Cek indikator loading (Sync, Filter, Unduh) | Ada saat proses berjalan | ✅ | <img width="785" height="470" alt="image" src="https://github.com/user-attachments/assets/b02fa866-0284-4ed2-93d0-dd8406561593" /> |

### A3. Tabel & Data

| ID | Langkah | Expected Result | Hasil | Catatan |
|---|---|---|---|---|
| VIS-A3-01 | Cek alignment tabel Events Report & CCTV | Kolom rata, tidak tumpang tindih | ⚠️ (grey area) | saat mobile gapnya terlalu kecil <img width="327" height="346" alt="image" src="https://github.com/user-attachments/assets/736d7ef5-916e-4856-bdfc-c68f5d0321ef" />, saat mobile/tablet baris harus digeser untuk melihat seluruh isinya serta angka page ditampakkan secara langsung dan mendorong tulisan "showing ... out ..." ke kiri <img width="320" height="201" alt="image" src="https://github.com/user-attachments/assets/d6e84272-08da-41f0-8b12-bd56634d69a6" /> |
| VIS-A3-02 | Cek snapshot/gambar event | Tampil jelas, tidak broken | ⚠️ | Kadang blurry/duplikat/di luar fokus (`11` UI-A2-05) |
| VIS-A3-03 | Cek tampilan saat 100 baris di tabel | Rapi, tidak merusak layout | ⚠️ (grey area) | lihat catatan VIS-A3-01 |

### A4. State Khusus

| ID | Langkah | Expected Result | Hasil | Catatan |
|---|---|---|---|---|
| VIS-A4-01 | Picu empty state (filter tanpa data) | Pesan "data kosong" rapi | ✅ | `11` UI-D-03 |
| VIS-A4-02 | Picu error/kamera offline di Panel | Pesan rapi, bukan layar blank | ❌ | Panel blank, bukan "no signal" (`11` UI-D-01) |

---

## TAHAP B — Visual AxxonNet (Middleware)

| ID | Langkah | Expected Result | Hasil | Catatan |
|---|---|---|---|---|
| VIS-B-01 | Cek header kamera (nama, resolusi, bitrate, R) | Tampil benar & terbaca | ✅ | `11` UI-B2-02 |
| VIS-B-02 | Cek overlay timestamp di video | Terbaca, tidak terpotong | ✅ | `11` UI-B2-03 |
| VIS-B-03 | Cek tampilan grid saat banyak sel | Sel rapi walau isi tak selalu tampil | ⚠️ | `11` UI-B3-02 |
| VIS-B-04 | Cek "no signal" kamera offline | Tampil rapi | ✅ | `11` UI-D-01 (AxxonNet) |

---

## TAHAP C — Responsivitas Visual Panel

| ID | Langkah | Expected Result | Hasil | Catatan |
|---|---|---|---|---|
| VIS-C-01 | Resize window kecil↔besar | Elemen tidak pecah/terpotong | ❌ | ada overlay halus saat model tablet <img width="289" height="149" alt="image" src="https://github.com/user-attachments/assets/9bc2fbed-90d9-4856-83ef-65c835cdb8aa" />, ikon yang menghilang (VIS-A2-01),  keluar box <img width="476" height="480" alt="image" src="https://github.com/user-attachments/assets/57704f58-0e38-4a58-a8a6-a1dfdd2e7730" /> |
| VIS-C-02 | Zoom browser 80%/100%/150% | Tetap terbaca & rapi | ❌ | saat 100% ikon sidebar berpindah / tidak ditengah <img width="953" height="501" alt="image" src="https://github.com/user-attachments/assets/f8c69a92-6174-42fc-acb4-73ccccd3b5ea" /> bahkan ikon hamburgernya juga menghilang, baru normal saat 80%|

---

**note:** Dua temuan visual utama — (1) snapshot event sering blurry/duplikat (VIS-A3-02), dan (2) Panel menampilkan layar blank alih-alih pesan "no signal" saat kamera offline (VIS-A4-02). Keduanya memengaruhi kejelasan tampilan bagi user.

---

## Exit Criteria
- Tidak ada elemen broken atau typo di halaman utama.
- Empty state & error state tampil rapi dan informatif di semua menu.
- Tampilan konsisten antar halaman.
